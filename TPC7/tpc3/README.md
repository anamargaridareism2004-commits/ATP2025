from typing import List, Dict, Any

Post = Dict[str, Any]
Rede = List[Post]


# a) quantosPost: indica quantos posts estão registados
def quantosPost(redeSocial: Rede) -> int:
    return len(redeSocial)


# b) postsAutor: devolve a lista de posts (dicionários) de um determinado autor
def postsAutor(redeSocial: Rede, autor: str) -> List[Post]:
    return [p for p in redeSocial if p.get("autor") == autor]


# c) autores: devolve a lista de autores de posts ordenada alfabeticamente (sem repetições)
def autores(redeSocial: Rede) -> List[str]:
    return sorted({p.get("autor") for p in redeSocial})


# --- helper interno para calcular o próximo id do tipo 'pN'
def _proximo_id(redeSocial: Rede) -> str:
    if not redeSocial:
        return "p1"
    nums = []
    for p in redeSocial:
        pid = str(p.get("id", ""))
        if pid.startswith("p") and pid[1:].isdigit():
            nums.append(int(pid[1:]))
    maior = max(nums) if nums else 0
    return f"p{maior + 1}"


# d) insPost: acrescenta um novo post a partir dos parâmetros recebidos e devolve a nova rede
def insPost(redeSocial: Rede, conteudo: str, autor: str,
            dataCriacao: str, comentarios: List[Dict[str, str]] | None = None) -> Rede:
    novo = {
        "id": _proximo_id(redeSocial),
        "conteudo": conteudo,
        "autor": autor,
        "dataCriacao": dataCriacao,
        "comentarios": list(comentarios) if comentarios else []
    }
    # sem efeitos colaterais: devolve uma nova lista
    return [*redeSocial, novo]


# e) remPost: remove da rede o post correspondente ao id recebido e devolve a nova rede
def remPost(redeSocial: Rede, id: str) -> Rede:
    return [p for p in redeSocial if p.get("id") != id]


# f) postsPorAutor: devolve uma distribuição (dict) de posts por autor
def postsPorAutor(redeSocial: Rede) -> Dict[str, int]:
    dist: Dict[str, int] = {}
    for p in redeSocial:
        a = p.get("autor")
        dist[a] = dist.get(a, 0) + 1
    return dist


# g) comentadoPor: recebe um autor e devolve a lista de posts comentados por esse autor
def comentadoPor(redeSocial: Rede, autor: str) -> List[Post]:
    return [
        p for p in redeSocial
        if any(c.get("autor") == autor for c in p.get("comentarios", []))
    ]

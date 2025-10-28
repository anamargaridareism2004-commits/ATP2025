import json
from typing import List, Tuple, Optional

Aluno = Tuple[str, str, List[float]]   # (nome, id, [notaTPC, notaProj, notaTeste])
Turma = List[Aluno]                    # lista de alunos




def ler_float(msg: str, minimo: float = 0.0, maximo: float = 20.0) -> float:
    """Lê um float no intervalo [minimo, maximo]."""
    while True:
        try:
            v = float(input(msg).replace(",", "."))
            if minimo <= v <= maximo:
                return v
            print(f"Valor fora do intervalo [{minimo}, {maximo}]. Tenta de novo.")
        except ValueError:
            print("Valor inválido. Tenta de novo.")

def pausa():
    input("\n(Pressiona ENTER para continuar) ")

def cabecalho(titulo: str):
    print("\n" + "=" * 60)
    print(titulo)
    print("=" * 60)


# ------------------ Turma de exemplo (5 alunos) ------------------

def gerar_turma_exemplo() -> Turma:
    """Cria uma turma com 5 alunos (em memória)."""
    return [
        ("Ana Martins",   "A001", [18.0, 17.0, 16.0]),
        ("Bruno Silva",   "A002", [14.0, 15.0, 13.0]),
        ("Carla Rocha",   "A003", [19.0, 18.0, 20.0]),
        ("Diogo Pereira", "A004", [10.0, 12.0, 11.0]),
        ("Eva Sousa",     "A005", [16.0, 14.0, 15.0]),
    ]


# ------------------ Operações ------------------

def criar_turma() -> Turma:
    """Cria e devolve uma turma vazia."""
    cabecalho("Turma criada (vazia).")
    return []

def inserir_aluno(turma: Turma) -> None:
    """Lê dados do aluno e insere na turma, garantindo id único."""
    cabecalho("Inserir aluno")

    nome = input("Nome: ").strip()
    while not nome:
        print("O nome não pode ser vazio.")
        nome = input("Nome: ").strip()

    # garantir ID único
    while True:
        id_aluno = input("ID (único): ").strip()
        if not id_aluno:
            print("O ID não pode ser vazio.")
            continue
        if any(a[1] == id_aluno for a in turma):
            print("Já existe um aluno com esse ID. Tenta outro.")
        else:
            break

    nota_tpc = ler_float("Nota do TPC (0-20): ")
    nota_proj = ler_float("Nota do Projeto (0-20): ")
    nota_teste = ler_float("Nota do Teste (0-20): ")

    aluno: Aluno = (nome, id_aluno, [nota_tpc, nota_proj, nota_teste])
    turma.append(aluno)
    print(f"\nAluno '{nome}' (id={id_aluno}) inserido com sucesso.")

def listar_turma(turma: Turma) -> None:
    """Lista os alunos em formato de tabela simples."""
    cabecalho("Listar turma")
    if not turma:
        print("A turma está vazia.")
        return

    print(f"{'Nome':<25} {'ID':<10} {'TPC':>5} {'Proj':>6} {'Teste':>7} {'Média':>7}")
    print("-" * 60)
    for nome, id_aluno, notas in turma:
        tpc, proj, teste = notas
        media = (tpc + proj + teste) / 3.0
        print(f"{nome:<25} {id_aluno:<10} {tpc:>5.1f} {proj:>6.1f} {teste:>7.1f} {media:>7.1f}")

def consultar_por_id(turma: Turma) -> None:
    """Procura e mostra um aluno pelo ID."""
    cabecalho("Consultar aluno por ID")
    if not turma:
        print("A turma está vazia.")
        return

    alvo = input("ID a procurar: ").strip()
    for nome, id_aluno, notas in turma:
        if id_aluno == alvo:
            tpc, proj, teste = notas
            media = (tpc + proj + teste) / 3.0
            print(f"\nEncontrado:")
            print(f"  Nome : {nome}")
            print(f"  ID   : {id_aluno}")
            print(f"  Notas: TPC={tpc:.1f}, Projeto={proj:.1f}, Teste={teste:.1f}")
            print(f"  Média: {media:.2f}")
            return
    print("Não foi encontrado nenhum aluno com esse ID.")

def guardar_em_ficheiro(turma: Turma) -> None:
    """Guarda a turma num ficheiro JSON."""
    cabecalho("Guardar turma em ficheiro")
    if not turma:
        print("A turma está vazia. Nada para guardar.")
        return

    nome_fich = input("Nome do ficheiro (ex.: turma.json): ").strip() or "turma.json"

    serializavel = [
        {
            "nome": nome,
            "id": id_aluno,
            "notas": {"TPC": notas[0], "Projeto": notas[1], "Teste": notas[2]},
        }
        for (nome, id_aluno, notas) in turma
    ]

    try:
        with open(nome_fich, "w", encoding="utf-8") as f:
            json.dump(serializavel, f, ensure_ascii=False, indent=2)
        print(f"Turma guardada com sucesso em '{nome_fich}'.")
    except Exception as e:
        print(f"Erro ao guardar: {e}")

def carregar_de_ficheiro() -> Optional[Turma]:
    """Carrega uma turma a partir de um ficheiro JSON."""
    cabecalho("Carregar turma de ficheiro")
    nome_fich = input("Nome do ficheiro (ex.: turma.json): ").strip() or "turma.json"

    try:
        with open(nome_fich, "r", encoding="utf-8") as f:
            dados = json.load(f)
        turma: Turma = []
        for reg in dados:
            nome = reg.get("nome", "")
            id_aluno = reg.get("id", "")
            notas_dict = reg.get("notas", {})
            notas = [
                float(notas_dict.get("TPC", 0.0)),
                float(notas_dict.get("Projeto", 0.0)),
                float(notas_dict.get("Teste", 0.0)),
            ]
            turma.append((nome, id_aluno, notas))
        print(f"Turma carregada de '{nome_fich}' com {len(turma)} aluno(s).")
        return turma
    except FileNotFoundError:
        print("Ficheiro não encontrado.")
    except json.JSONDecodeError:
        print("Formato JSON inválido.")
    except Exception as e:
        print(f"Erro ao carregar: {e}")
    return None


#  Menu

def mostra_menu() -> None:
    print("\n--- Menu ---")
    print("1: Criar uma turma")
    print("2: Inserir um aluno na turma")
    print("3: Listar a turma")
    print("4: Consultar um aluno por id")
    print("8: Guardar a turma em ficheiro")
    print("9: Carregar uma turma dum ficheiro")
    print("0: Sair da aplicação")

def main():
    # Já começa com 5 alunos (cumpre o enunciado sem JSON)
    turma: Turma = gerar_turma_exemplo()
    print("Turma inicial com 5 alunos foi criada automaticamente.")

    while True:
        mostra_menu()
        op = input("\nOpção: ").strip()

        if op == "1":
            turma = criar_turma()
            pausa()
        elif op == "2":
            inserir_aluno(turma)
            pausa()
        elif op == "3":
            listar_turma(turma)
            pausa()
        elif op == "4":
            consultar_por_id(turma)
            pausa()
        elif op == "8":
            guardar_em_ficheiro(turma)
            pausa()
        elif op == "9":
            t = carregar_de_ficheiro()
            if t is not None:
                turma = t
            pausa()
        elif op == "0":
            print("\nA sair... Obrigado!")
            break
        else:
            print("Opção inválida. Tenta de novo.")

if __name__ == "__main__":
    main()

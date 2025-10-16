
def procurarSala(cinema,filme):
    for i in range (len(cinema)):
        if cinema[i][2]== filme:
            return i
    return -1

def inserirSala(cinema,sala):
    res=cinema.copy()
    filme=sala[2]
    if procurarSala(res,filme)==-1:
        res.append(sala)
    return res

def listar(cinema):
    print("Salas em exibição")
    for sala in cinema:
        total= sala[0]
        ocupados= len(sala[1])
        disponivel= total-ocupados
        print("Filme:", sala[2], "Lugares totais:", total, "Ocupados:", ocupados, "Dispoíveis:", disponivel)

def disponivel(cinema,filme,lugar):
    i=procurarSala(cinema,filme)
    if i==-1:
        print(f"Filme {filme} não encontrado")
        return False
    return lugar not in cinema[i][1]

def vendeBilhete(cinema,filme,lugar):
        i= procurarSala(cinema,filme)
        if i==-1:
            print(f"Filme {filme} não encontrado")
            return cinema
        if lugar in cinema [i][1]:
            print(f"Lugar {lugar} já está ocupado para {filme}")
        else:
            cinema[i][1].append(lugar)
            print(f"Lugar {lugar} vendido para {filme}")
        return cinema

def listardisponibilidade(cinema):
    print("Disponibilidades:")
    for sala in cinema:
        disponivel= sala [0]- len(sala[1])
        print (f"{sala[2]}: {disponivel} lugares disponíveis")

def libertarLugar(cinema,filme,lugar):
    i=procurarSala(cinema,filme)
    if lugar in cinema[i][1]:
        cinema[i][1].remove(lugar)
        print(f"O lugar {lugar} foi libertado")
    else:
        print(f"O lugar {lugar} não está ocupado")
    return cinema

def removerSala(cinema,filme):
    i= procurarSala(cinema,filme)
    if i==-1:
        print(f"Filme {filme} não encontrado")
        return cinema
    elif len(cinema[i][1])>0:
        print("Não é possível remover", filme, "-ainda há bilhetes vendidos")
        return cinema
    else:
        print("Sala com o filme", filme, "removida")
        return [s for s in cinema if s[2]!=filme]
        

import tpc4 as c
def menu():
    print("""
    === Cinema Center ===
    1. Inserir nova sala
    2. Listar salas e filmes
    3. Vender bilhete
    4. Libertar lugar
    5. Verificar disponibilidade
    6. Listar disponibilidade
    7. Remover sala
    0. Sair
    """)

def main():
    cinema=[]
    while True:
        menu()
        op= int(input("Escolha uma opção"))

        if op==1:
            filme= input("Nome do filme:")
            nlugares= int(input("Número de lugares:"))
            sala=[nlugares, [], filme]
            cinema=c.inserirSala(cinema,sala)
        elif op ==2:
            c.listar(cinema)
        elif op==3:
            filme=input("Filme")
            lugar=int(input("Lugar:"))
            cinema=c.vendeBilhete(cinema,filme,lugar)
        elif op==4:
            filme=input("Filme")
            lugar=int(input("Lugar:"))
            cinema=c.libertarLugar(cinema,filme,lugar)
        elif op==5:
            filme=input("Filme")
            lugar=int(input("Lugar:"))
            print("Disponível:", c.disponivel(cinema,filme,lugar))
        elif op==6:
            c.listardisponibilidade(cinema)
        elif op==7:
            filme=input("Filme:")
            cinema=c.removerSala(cinema,filme)
        elif op==0:
            print("A sair")
            break
if _name=="main_":
    main()

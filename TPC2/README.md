def jogo_21():
    fosforos = 21
    print("---Jogo dos 21 Fósforos---")
    print("Há 21 fósforos. Em cada jogada podes tirar 1, 2, 3 ou 4.")
    print("Quem tirar o último GANHA")

    print("Quem começa? (u = utilizador / c = computador): ")
    escolha = input()
    if escolha == "u":
     vez_humano = 1
    else:
      vez_humano = 0

    while fosforos > 0:
        print("Fósforos restantes:", fosforos)

        if vez_humano == 1:
            tirar = 0
            while tirar < 1 or tirar > 4 or tirar > fosforos:
                print("Quantos quer tirar (1-4)? ")
                tirar = int(input())
        else:
            resto = fosforos % 5
            if resto == 0:
                tirar = 1
            else:
                tirar = resto
            if tirar > fosforos:
                tirar = fosforos
            print("Computador tira", tirar)

        fosforos = fosforos - tirar

        if vez_humano == 1:
            vez_humano = 0
        else:
            vez_humano = 1

    if vez_humano == 1:
        print("Computador ganhou!")
    else:
        print("Você ganhou!")

jogo_21()

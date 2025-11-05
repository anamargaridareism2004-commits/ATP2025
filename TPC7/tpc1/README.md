# a) Lista formada pelos elementos que não são comuns às duas listas:
lista1 = [1, 2, 3, 4, 5]
lista2 = [4, 5, 6, 7, 8]
ncomuns = [x for x in lista1 + lista2 if x not in lista1 or x not in lista2]
# Resultado esperado: [1, 2, 3, 7, 8]
print("a) ->", ncomuns)


# b) Lista formada pelas palavras do texto compostas por mais de 3 letras:
texto = """Vivia há já não poucos anos algures num concelho do Ribatejo
um pequeno lavrador e negociante de gado chamado Manuel Peres Vigário"""
lista = [palavra for palavra in texto.split() if len(palavra) > 3]
# Resultado esperado: ['Vivia', 'poucos', 'anos', 'algures', 'concelho', ...]
print("b) ->", lista)


# c) Lista formada por pares do tipo (índice, valor) com os valores da lista dada:
lista = ['anaconda', 'burro', 'cavalo', 'macaco']
listaRes = [(i + 1, valor) for i, valor in enumerate(lista)]
# Resultado esperado: [(1,'anaconda'), (2,'burro'), (3,'cavalo'), (4,'macaco')]
print("c) ->", listaRes)

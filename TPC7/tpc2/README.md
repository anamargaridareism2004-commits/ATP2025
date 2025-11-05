#a)
def strCount(s, subs):
    count = 0
    i = 0
    while i <= len(s) - len(subs):
        if s[i:i + len(subs)] == subs:
            count += 1
            i += len(subs)  # avança o tamanho da substring (sem sobreposição)
        else:
            i += 1
    return count


# Exemplos de teste
print(strCount("catcowcat", "cat"))  # --> 2
print(strCount("catcowcat", "cow"))  # --> 1
print(strCount("catcowcat", "dog"))  # --> 0

#b)
def produtoM3(lista):
    lista_ordenada = sorted(lista)
    return lista_ordenada[0] * lista_ordenada[1] * lista_ordenada[2]


# Exemplo de teste
print(produtoM3([12, 3, 7, 10, 12, 8, 9]))  # --> 168 (3 * 7 * 8)

#c)
def reduxInt(n):
    while n >= 10:  # continua até restar apenas um dígito
        n = sum(int(d) for d in str(n))
    return n


# Exemplos:
print(reduxInt(38))   # --> 2   (3 + 8 = 11; 1 + 1 = 2)
print(reduxInt(777))  # --> 3   (7 + 7 + 7 = 21; 2 + 1 = 3)

#d)

def myIndexOf(s1, s2):
    for i in range(len(s1) - len(s2) + 1):
        if s1[i:i + len(s2)] == s2:
            return i
    return -1


# Exemplos:
print(myIndexOf("Hoje está um belo dia de sol!", "belo"))   # --> 13
print(myIndexOf("Hoje está um belo dia de sol!", "chuva"))  # --> -1

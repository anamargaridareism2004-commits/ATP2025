- tabMeteo = [(Data, TempMin, TempMax, Precipitacao)]
  - Data = (ano, mes, dia)

from typing import List, Tuple, Optional
import csv

Data = Tuple[int, int, int]
Reg = Tuple[Data, float, float, float]
TabMeteo = List[Reg]


def multiplos23() -> int:
    """Conta quantos inteiros entre 1 e 10000 são múltiplos de 23."""
    res = 0
    for n in range(1, 10001):
        if n % 23 == 0:
            res += 1
    return res

multiplos23_lc = [n for n in range(1, 10001) if n % 23 == 0]



tabMeteo1: TabMeteo = [
    ((2022, 1, 15),  6.0, 14.0,  2.5),
    ((2022, 1, 16),  5.0, 16.0,  0.0),
    ((2022, 1, 17),  7.0, 15.0,  1.2),
    ((2022, 1, 18),  8.0, 18.0,  0.0),
    ((2022, 1, 19),  9.0, 19.0,  0.0),
    ((2022, 1, 20),  6.0, 16.0,  0.0),
    ((2022, 1, 21),  1.0, 13.0,  0.2),
    ((2022, 1, 22),  7.0, 17.0,  0.01),
    ((2022, 1, 23),  4.0, 12.0,  5.0),
    ((2022, 1, 24),  2.0, 11.0, 12.0),
    ((2022, 1, 25), -1.0,  9.0,  0.0),
    ((2022, 1, 26), -2.0,  8.0,  0.0),
    ((2022, 1, 27), -3.0,  7.0,  0.0),
    ((2022, 1, 28), -1.0,  8.0,  0.0),
    ((2022, 1, 29),  0.0, 10.0,  0.0),
]


#  1.a) médias 

def medias(tab: TabMeteo) -> List[Tuple[Data, float]]:
    """[(data, (tmin+tmax)/2)]"""
    return [(data, (tmin + tmax) / 2.0) for (data, tmin, tmax, _p) in tab]


#  1.b) guardar 

def guardar_tabMeteo_txt(tab: TabMeteo, nome_fich: str) -> None:
    """Guarda a tabela num ficheiro CSV simples."""
    with open(nome_fich, "w", encoding="utf-8", newline="") as f:
        w = csv.writer(f, delimiter=";")
        w.writerow(["ano", "mes", "dia", "tempMin", "tempMax", "precip"])
        for (a, m, d), tmin, tmax, p in tab:
            w.writerow([a, m, d, tmin, tmax, p])


#1.c) carregar

def carregar_tabMeteo_txt(nome_fich: str) -> TabMeteo:
    """Carrega uma tabela meteorológica de um CSV."""
    tab: TabMeteo = []
    with open(nome_fich, "r", encoding="utf-8") as f:
        r = csv.reader(f, delimiter=";")
        next(r, None)
        for a, m, d, tmin, tmax, p in r:
            tab.append(((int(a), int(m), int(d)), float(tmin), float(tmax), float(p)))
    return tab


# 1.d) mínimo das mínimas 

def minMin(tab: TabMeteo) -> float:
    """Menor temperatura mínima registada."""
    return min(tmin for (_data, tmin, _tmax, _p) in tab)


#  1.e) amplitude térmica 

def amplTerm(tab: TabMeteo) -> List[Tuple[Data, float]]:
    """[(data, tmax - tmin)]"""
    return [(data, tmax - tmin) for (data, tmin, tmax, _p) in tab]


# 1.f) precipitação máxima

def maxChuva(tab: TabMeteo) -> Tuple[Data, float]:
    """(data, precipitação) do dia com maior precipitação."""
    data, tmin, tmax, p = max(tab, key=lambda reg: reg[3])
    return data, p


#  1.g) dias chuvosos 

def diasChuvosos(tab: TabMeteo, p: float) -> List[Tuple[Data, float]]:
    """[(data, precipitação)] onde precipitação > p"""
    return [(data, precip) for (data, _tmin, _tmax, precip) in tab if precip > p]


# 1.h) maior período seco 

def maxPeriodoCalor(tab: TabMeteo, p: float) -> Tuple[Optional[Data], Optional[Data], int]:
    """Maior sequência de dias com precipitação < p."""
    melhor_i = melhor_f = None
    melhor_n = 0
    atual_i = None
    atual_n = 0

    for data, _tmin, _tmax, prec in tab:
        if prec < p:
            if atual_n == 0:
                atual_i = data
            atual_n += 1
            if atual_n > melhor_n:
                melhor_i = atual_i
                melhor_f = data
                melhor_n = atual_n
        else:
            atual_n = 0
    return melhor_i, melhor_f, melhor_n


#  1.i) gráfico 

def grafTabMeteo(tab: TabMeteo, show=True, savepath=None):
    """Gráfico das séries TempMin, TempMax e Precipitação."""
    import matplotlib.pyplot as plt
    xs = list(range(len(tab)))
    mins = [t[1] for t in tab]
    maxs = [t[2] for t in tab]
    prec = [t[3] for t in tab]

    plt.figure()
    plt.plot(xs, mins, label="TempMin")
    plt.plot(xs, maxs, label="TempMax")
    plt.plot(xs, prec, label="Precipitação")
    plt.legend()
    plt.title("Evolução Meteorológica")
    plt.xlabel("Dias (índice)")
    plt.ylabel("Valores")
    if savepath:
        plt.savefig(savepath, bbox_inches="tight")
    if show:
        plt.show()

-------------
#  1.j) Aplicação (menu)

def _fmt_data(d: Data) -> str:
    a, m, d0 = d
    return f"{a:04d}-{m:02d}-{d0:02d}"

def menu_demo():
    tab = tabMeteo1.copy()
    while True:
        print("\n--- TP6: Gestão Meteorológica ---")
        print("1: Médias diárias")
        print("2: Guardar CSV")
        print("3: Carregar CSV")
        print("4: Mínimo das mínimas")
        print("5: Amplitudes térmicas")
        print("6: Dia de precipitação máxima")
        print("7: Dias com precipitação > p")
        print("8: Período seco (prec < p)")
        print("9: Gráfico")
        print("0: Sair")
        op = input("Opção: ").strip()

        if op == "1":
            for d, m in medias(tab):
                print(_fmt_data(d), f"{m:.1f}")
        elif op == "2":
            guardar_tabMeteo_txt(tab, "tabela.csv")
            print("Guardado em tabela.csv")
        elif op == "3":
            tab = carregar_tabMeteo_txt("tabela.csv")
            print("Carregado com", len(tab), "registos.")
        elif op == "4":
            print("Temperatura mínima absoluta:", minMin(tab))
        elif op == "5":
            for d, a in amplTerm(tab):
                print(_fmt_data(d), f"{a:.1f}")
        elif op == "6":
            d, p = maxChuva(tab)
            print("Máxima precipitação em", _fmt_data(d), "=", p)
        elif op == "7":
            lim = float(input("Limite p: "))
            for d, p in diasChuvosos(tab, lim):
                print(_fmt_data(d), p)
        elif op == "8":
            lim = float(input("Limite p: "))
            i, f, n = maxPeriodoCalor(tab, lim)
            print("Período seco:", _fmt_data(i), "->", _fmt_data(f), f"({n} dias)")
        elif op == "9":
            grafTabMeteo(tab)
        elif op == "0":
            break
        else:
            print("Opção inválida.")

# Execução de teste 

if __name__ == "__main__":
    print("Aquecimento:", multiplos23(), "múltiplos de 23 (1..10000)")
    print("Primeiros 5 múltiplos (LC):", multiplos23_lc[:5])
    print("minMin(tabMeteo1) =", minMin(tabMeteo1))
    print("maxChuva(tabMeteo1) =", maxChuva(tabMeteo1))
    print("diasChuvosos(tabMeteo1, 1) =", diasChuvosos(tabMeteo1, 1))
    print("maxPeriodoCalor(tabMeteo1, 0.1) =", maxPeriodoCalor(tabMeteo1, 0.1))
    # menu_demo()

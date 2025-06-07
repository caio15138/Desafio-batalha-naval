# Desafio-batalha-naval
"Desafio: Batalha Naval'
expicação
Neste desafio, você vai colocar suas habilidades de programação à prova desenvolvendo uma versão do clássico jogo Batalha Naval. A ideia é criar um jogo interativo, onde dois jogadores (ou um jogador e o computador) tentam afundar os navios um do outro em um tabuleiro de guerra naval.

"Como deve jogar"
Tabuleiros
Dois tabuleiros: um do jogador e outro do oponente.
Tamanho sugerido: 10x10, mas pode ser ajustável.

"embarcações ( navios )"
Inclua diferentes tipos de navios, com tamanhos variados. Um Exemplo:
Porta-aviões – 5 célula
Encouraçado – 4 células
Cruzador – 3 células
Submarino – 2 células

"Jogabilidade"
O jogador deve poder posicionar seus navios (manualmente ou aleatoriamente).
Cada turno, o jogador escolhe uma coordenada para atacar o oponente.
Mostre se o tiro foi acerto ou erro.

O jogo acaba quando todos os navios de um jogador forem afundados.

Mostre quem venceu e uma mensagem de fim de jogo.

Código simples (Python) para rodar o jogo
TAMANHO = 5
NUM_NAVIOS = 3

def criar_tabuleiro():
    return [["~"] * TAMANHO for _ in range(TAMANHO)]

def mostrar_tabuleiro(tabuleiro, esconder=False):
    for linha in tabuleiro:
        linha_visivel = ["~" if cel == "N" and esconder else cel for cel in linha]
        print(" ".join(linha_visivel))
    print()

def posicionar_navios(tabuleiro):
    navios = 0
    while navios < NUM_NAVIOS:
        linha = random.randint(0, TAMANHO - 1)
        coluna = random.randint(0, TAMANHO - 1)
        if tabuleiro[linha][coluna] == "~":
            tabuleiro[linha][coluna] = "N"
            navios += 1

def atacar(tabuleiro, linha, coluna):
    if tabuleiro[linha][coluna] == "N":
        tabuleiro[linha][coluna] = "X"
        print("💥 Acertou um navio!")
        return True
    elif tabuleiro[linha][coluna] == "~":
        tabuleiro[linha][coluna] = "O"
        print("🌊 Água!")
        return False
    else:
        print("⚠️ Já tentou essa posição.")
        return False

def contar_navios(tabuleiro):
    return sum(row.count("N") for row in tabuleiro)

# Inicialização
tabuleiro_jogador = criar_tabuleiro()
tabuleiro_ia = criar_tabuleiro()

posicionar_navios(tabuleiro_jogador)
posicionar_navios(tabuleiro_ia)

print("🎮 Bem-vindo ao Batalha Naval!")
while True:
    print("\nSeu tabuleiro:")
    mostrar_tabuleiro(tabuleiro_jogador)
    print("Tabuleiro inimigo:")
    mostrar_tabuleiro(tabuleiro_ia, esconder=True)

    try:
        linha = int(input("Linha (0-4): "))
        coluna = int(input("Coluna (0-4): "))
    except ValueError:
        print("Digite números válidos.")
        continue

    if linha < 0 or linha >= TAMANHO or coluna < 0 or coluna >= TAMANHO:
        print("Coordenadas fora do tabuleiro.")
        continue

    atacar(tabuleiro_ia, linha, coluna)

    if contar_navios(tabuleiro_ia) == 0:
        print("🏆 Você venceu!")
        break

    # Turno da IA
    while True:
        ia_linha = random.randint(0, TAMANHO - 1)
        ia_coluna = random.randint(0, TAMANHO - 1)
        if tabuleiro_jogador[ia_linha][ia_coluna] in ["~", "N"]:
            print(f"\n🤖 IA ataca ({ia_linha}, {ia_coluna})...")
            atacar(tabuleiro_jogador, ia_linha, ia_coluna)
            break

    if contar_navios(tabuleiro_jogador) == 0:
        print("💀 A IA venceu!")
        break

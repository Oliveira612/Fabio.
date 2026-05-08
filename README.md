import random


PALAVRAS = [
    'python', 'programacao', 'variavel', 'funcao',
    'laco', 'lista', 'string', 'terminal',
    'teclado', 'monitor', 'algoritmo', 'codigo'
]
MAX_ERROS = 6


def sortear_palavra(palavras):
    return random.choice(palavras)

def exibir_forca(erros):
    estagios = [
        """ -----
 |   |
 |
 |
 |
=========""",
        """ -----
 |   |
 |   O
 |
 |
=========""",
        """ -----
 |   |
 |   O
 |   |
 |
=========""",
        """ -----
 |   |
 |   O
 |  /|
 |
=========""",
        """ -----
 |   |
 |   O
 |  /|\\
 |
=========""",
        """ -----
 |   |
 |   O
 |  /|\\
 |  /
=========""",
        """ -----
 |   |
 |   O
 |  /|\\
 |  / \\
========="""
    ]
    print(estagios[erros])

def exibir_palavra(palavra, tentadas):
    print()
    for letra in palavra:
        print(letra if letra in tentadas else '_', end=' ')
    print()

def verificar_vitoria(palavra, tentadas):
    for letra in palavra:
        if letra not in tentadas:
            return False
    return True

def jogar_rodada(palavra):
    tentadas = []
    erros = 0
    while erros < MAX_ERROS:
        exibir_forca(erros)
        exibir_palavra(palavra, tentadas)
        print(f'Tentadas: {", ".join(tentadas)}')
        print(f'Erros: {erros}/{MAX_ERROS}')
        letra = input('\nLetra: ').lower().strip()

        if len(letra) != 1 or not letra.isalpha():
            print('⚠ Digite apenas uma letra!')
            continue
        if letra in tentadas:
            print('⚠ Já tentou essa!')
            continue

        tentadas.append(letra)
        if letra in palavra:
            print(f'✔ "{letra}" está na palavra!')
        else:
            print(f'✘ "{letra}" não está.')
            erros += 1

        if verificar_vitoria(palavra, tentadas):
            exibir_forca(erros)
            exibir_palavra(palavra, tentadas)
            print(f'\n🏆 GANHOU! Palavra: {palavra.upper()}')
            return True

    exibir_forca(erros)
    print(f'\n💀 Perdeu! Era: {palavra.upper()}')
    return False

# ── PROGRAMA PRINCIPAL ──────────────────────────────
print('=' * 40)
print(' 🎮 JOGO DA FORCA 🎮')
print('=' * 40)

pontuacao = 0
jogar = 's'

while jogar == 's':
    print(f'\nPontuação: {pontuacao}')
    palavra = sortear_palavra(PALAVRAS)
    venceu = jogar_rodada(palavra)
    if venceu:
        pontuacao += 10
    else:
        pontuacao -= 5

    jogar = input('\nJogar de novo? (s/n): ').lower().strip()

print(f'\nFim! Pontuação final: {pontuacao}')

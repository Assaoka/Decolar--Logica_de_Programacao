# 🐍 Aula 13: Busca e Ordenação - A Corrida Pelo Pódio

**🎯 Missão de Hoje**
> Já sabemos percorrer listas com `for` desde a Aula 4. Hoje vamos dar um passo além e pensar como um verdadeiro cientista da computação: existe um jeito **esperto** de procurar algo numa lista, e não é sempre o mesmo jeito de organizar (ordenar) os dados. Vamos aprender Busca Linear, Busca Binária e Ordenação — e entender por que a diferença entre elas pode significar segundos ou **horas** de espera, dependendo do tamanho dos dados.

---
## 🔍 1. Busca Linear: Procurando Item por Item
Quando não sabemos nada sobre a organização de uma lista, só existe um jeito seguro de procurar algo: olhar item por item, começando do início, até achar (ou até acabar a lista).

```python
def busca_linear(lista: list[str], procurado: str) -> int:
    for indice, item in enumerate(lista):
        if item == procurado:
            return indice
    return -1  # Percorreu tudo e não achou
```

```python
pokedex: list[str] = ["Charmander", "Bulbasaur", "Pikachu", "Squirtle", "Mewtwo"]

posicao: int = busca_linear(pokedex, "Squirtle")
print(posicao) # 3
```

No **pior caso** (o item está no final da lista, ou nem existe), a busca linear precisa passar por **todos** os itens. Numa lista de 5 Pokémons isso é rápido, mas numa lista de 1 milhão de jogadores, pode ser bem lento.

---
## ⚡ 2. Busca Binária: Cortando a Lista pela Metade
Se a lista já estiver **ordenada**, existe um truque muito mais esperto. É o mesmo truque do jogo "Adivinhe o Número": alguém pensa em um número de 1 a 100, e você tenta advinhar. A cada palpite, a pessoa só diz "mais alto" ou "mais baixo" — e a melhor estratégia é sempre chutar o **meio** do intervalo que ainda resta, cortando pela metade a cada tentativa.

```python
def busca_binaria(lista: list[int], procurado: int) -> int:
    inicio: int = 0
    fim: int = len(lista) - 1

    while inicio <= fim:
        meio: int = (inicio + fim) // 2

        if lista[meio] == procurado:
            return meio
        elif lista[meio] < procurado:
            inicio = meio + 1  # o procurado está na metade de cima
        else:
            fim = meio - 1     # o procurado está na metade de baixo

    return -1  # não existe na lista
```

```python
numeros_pokedex: list[int] = [1, 4, 7, 25, 94, 130, 150, 201, 249]

posicao: int = busca_binaria(numeros_pokedex, 130)
print(posicao) # 5
```

> **⚠️ A Regra de Ouro da Busca Binária**
> Esse truque **só funciona se a lista já estiver ordenada**! Se você usar busca binária numa lista bagunçada, o algoritmo pode "cortar pro lado errado" e te dar uma resposta errada, mesmo o item existindo na lista.

---
## 🧺 3. Ordenação: Colocando a Casa em Ordem
Para usar busca binária, primeiro precisamos de uma lista ordenada. Mas como o computador organiza uma lista bagunçada? Vamos aprender um dos algoritmos mais simples de entender: o **Bubble Sort** (Ordenação por Bolha).

A ideia: comparar dois vizinhos por vez, e trocar de lugar se estiverem na ordem errada. Repetindo isso várias vezes, os valores maiores vão "borbulhando" para o final, feito bolhas subindo na água.

Lembra do desafio "Trocando Valores" lá na Aula 1? O Python tem um truque elegante para trocar duas variáveis de lugar, sem precisar de uma variável temporária:

```python
a: int = 1
b: int = 2

a, b = b, a  # troca os dois de uma vez!
print(a, b) # 2 1
```

Com esse truque, o Bubble Sort fica assim:

```python
def bubble_sort(lista: list[float]) -> None:
    tamanho: int = len(lista)

    for passo in range(tamanho):
        for i in range(tamanho - 1 - passo):
            if lista[i] > lista[i + 1]:
                lista[i], lista[i + 1] = lista[i + 1], lista[i]
```

```python
tempos_corrida: list[float] = [58.2, 42.9, 61.0, 39.5, 50.1]

bubble_sort(tempos_corrida)
print(tempos_corrida) # [39.5, 42.9, 50.1, 58.2, 61.0] -> do mais rápido pro mais lento!
```

---
## 🐍 4. Python Já Tem Isso Pronto: `sorted()` e `.sort()`
Assim como aprendemos na Aula 8 que não precisamos reinventar a roda com módulos prontos, o Python já vem com ordenação embutida — muito mais rápida que qualquer Bubble Sort que a gente escrever na mão:

```python
tempos_corrida: list[float] = [58.2, 42.9, 61.0, 39.5, 50.1]

tempos_ordenados: list[float] = sorted(tempos_corrida) # cria uma lista NOVA, ordenada
print(tempos_ordenados)
print(tempos_corrida) # a lista original nem mudou

tempos_corrida.sort() # já ordena a lista original, sem criar outra
print(tempos_corrida)

tempos_corrida.sort(reverse=True) # do maior pro menor
print(tempos_corrida)
```

Também podemos ordenar uma lista de dicionários usando um campo específico como critério, passando uma função para o parâmetro `key`:

```python
def pegar_trofeus(jogador: dict) -> int:
    return jogador["trofeus"]

jogadores: list[dict] = [
    {"nome": "Ana", "trofeus": 4200},
    {"nome": "Bruno", "trofeus": 5100},
    {"nome": "Caio", "trofeus": 3800}
]

ranking: list[dict] = sorted(jogadores, key=pegar_trofeus, reverse=True)

for jogador in ranking:
    print(f"{jogador['nome']}: {jogador['trofeus']} troféus")
```

---
## 📈 5. Uma Pitada de Eficiência
Por que toda essa conversa importa? Porque a diferença de velocidade entre os algoritmos cresce **absurdamente** conforme a lista fica maior.

Imagine uma lista com **1 milhão** de jogadores ordenados por nome:
- Busca **linear**, no pior caso, pode precisar checar o milhão de nomes, um por um.
- Busca **binária** resolve em no máximo **20 tentativas** — porque a cada passo ela corta a lista restante pela metade (1 milhão → 500 mil → 250 mil → ... até sobrar 1).

É a diferença entre o computador travar por segundos ou responder instantaneamente. Por isso, sempre que os dados estiverem ordenados, vale a pena usar busca binária em vez de linear!

---
# 🛠️ Desafios em Sala

## 1. Detector de Duplicados na Pokédex
Antes de "capturar" um novo Pokémon, use a `busca_linear` para checar se o nome digitado já existe na lista `pokedex`. Se já existir, avise "Você já tem esse Pokémon!". Se não existir, adicione o nome à lista e avise "Pokémon capturado!".

```python
# Solução:

```

## 2. Adivinha o Número (Busca Binária na Prática)
Dada a lista ordenada `numeros_pokedex` da seção 2, peça ao usuário um número e use `busca_binaria` para dizer em qual posição da lista ele está (ou "Esse número não existe na Pokédex" se não achar).

```python
# Solução:

```

## 3. Ranking da Corrida
Dada uma lista de tempos de corrida desorganizados, use o `bubble_sort` para ordená-la do mais rápido para o mais lento. Depois, exiba o pódio com os 3 primeiros colocados usando os emojis 🥇🥈🥉.

| Entrada | Saída |
| :--- | :--- |
| [58.2, 42.9, 61.0, 39.5, 50.1] | 🥇 39.5s<br>🥈 42.9s<br>🥉 50.1s |

```python
# Solução:

```

---
# 💪 Exercícios de Casa

## 1. Teste de Mesa
Faça o teste de mesa da `busca_binaria` procurando o número `94` na lista `[1, 4, 7, 25, 94, 130, 150, 201, 249]`. Para cada volta do `while`, anote os valores de `inicio`, `fim` e `meio`.

- **Quantas voltas o `while` deu até encontrar o número?**
- **Em qual índice o número `94` foi encontrado?**

## 2. Detetive de Código
Um aluno tentou usar busca binária, mas o código sempre retorna `-1`, mesmo quando o número procurado existe na lista. Encontre o erro:

```python
numeros: list[int] = [42, 8, 15, 91, 23]

def busca_binaria(lista, procurado):
    inicio = 0
    fim = len(lista) - 1
    while inicio <= fim:
        meio = (inicio + fim) // 2
        if lista[meio] == procurado:
            return meio
        elif lista[meio] < procurado:
            inicio = meio + 1
        else:
            fim = meio - 1
    return -1

print(busca_binaria(numeros, 91))
```

**Escreva aqui o erro encontrado e como corrigir:**

## 3. Contando Comparações
Modifique a `busca_linear` para que ela também conte quantas comparações fez até achar o item (ou até desistir), retornando esse número junto com a posição. Teste com um item no **início** da lista, um no **final**, e um que **não existe**, e compare quantas comparações cada caso precisou.

## 4. Ordenando por Nome
Dada uma lista de nomes de jogadores fora de ordem, use `sorted()` para organizá-la em ordem alfabética e exiba o resultado.

| Entrada | Saída |
| :--- | :--- |
| ["Zeca", "Ana", "Mateus", "Bia"] | ['Ana', 'Bia', 'Mateus', 'Zeca'] |

## 5. Pódio Invertido
Dada uma lista de pontuações de um clã (números inteiros), use `.sort(reverse=True)` para ordenar do maior para o menor e exiba a colocação de cada jogador (1º, 2º, 3º...).

| Entrada | Saída |
| :--- | :--- |
| [1500, 3200, 800, 2100] | 1º lugar: 3200<br>2º lugar: 2100<br>3º lugar: 1500<br>4º lugar: 800 |

## 6. Busca Binária de Item Não Existente
Usando a lista `numeros_pokedex` da seção 2, faça o teste de mesa da `busca_binaria` procurando o número `100` (que não existe na lista). Explique, passo a passo, por que o `while` termina e a função retorna `-1` sem travar o programa.

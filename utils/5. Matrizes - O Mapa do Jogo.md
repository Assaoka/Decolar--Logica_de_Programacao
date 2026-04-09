---
Data de Criação: 2026-03-26
Notas Relacionadas:
  - "[[4. Listas - Organizando o Inventário]]"
tags:
  - Programação
  - Decolar
  - Lógica_de_Programação
---
# 🐍 Aula 5: Matrizes - O Mapa do Jogo

> [!abstract] Missão de Hoje
> E se a nossa mochila (Lista) não for o suficiente? Hoje vamos aprender a criar Tabuleiros, Mapas e Grids! Vamos elevar o nível e descobrir como usar "Listas dentro de Listas" para montar estruturas 2D perfeitas para jogos e organizar informações complexas.

---
## 🗺️ 1. O que é uma Matriz?
Na última aula, vimos que uma Lista é como um armário cheio de prateleiras. Você diz o número da prateleira (índice) e pega o item. 

Mas e jogos como **Batalha Naval**, **Jogo da Velha** ou o **Mapa do Minecraft**? Eles não são uma linha reta, eles têm **Largura** e **Altura**!

Uma **Matriz** é exatamente isso: uma tabela, um grid com **Linhas** e **Colunas**. No Python, nós não temos um "tipo Matriz" nativo. Na verdade, fazemos um truque: **criamos uma Lista que guarda outras Listas dentro dela!**

---
## 📍 2. Coordenadas: Lendo e Escrevendo no Mapa
Pense em uma matriz como um prédio de apartamentos.
Para entregar uma pizza, você precisa saber de duas coisas:
1. O Andar (**Linha**)
2. O Número do Apartamento (**Coluna**)

> [!danger] ⚠️ REGRA ZERO DO PROGRAMADOR (Relembrando)
> O computador continua começando a contar do **0**! A primeira linha é a `0` e a primeira coluna é a `0`.

```python
# Criando o mapa (Uma lista cheia de listas)
mapa: list[list[str]] = [
    ["Grama", "Grama", "Água"],  # Linha 0
    ["Grama", "Terra", "Água"],  # Linha 1
    ["Pedra", "Baú", "Grama"]    # Linha 2
]

# Acessando: mapa[linha][coluna]
print(mapa[0][0]) # Grama
print(mapa[2][1]) # Baú
```

---
## 🛠️ 3. Alterando o Terreno
Assim como nas listas normais, podemos alterar o valor de uma posição no nosso mapa. Você só precisa dizer a Coordenada Y (Linha) e a Coordenada X (Coluna).

```python
# Cavando a Grama e colocando Pedra na Linha 0, Coluna 1 do nosso mapa
mapa[0][1] = "Pedra"
print(mapa[0][1]) # Pedra
```

---
## 👁️ 4. Varrendo o Mapa: O For Alinhado (Nested Loop)
Para ler um mapa inteiro automático, nós usamos a mesma lógica que aprendemos na aula de Repetição (naquele desafio da parede de blocos). Precisamos de um `for` dentro de outro `for`!

- O primeiro `for` cuida das **Linhas** (ele desce um "andar" de cada vez).
- O segundo `for` cuida das **Colunas** (ele entra nos apartamentos daquele andar).

```python
# Imprimindo o mapa de forma bonita!
for linha in range(3):
    for coluna in range(3):
        # O end=" " impede que o Python pule de linha automaticamente
        print(f"[{mapa[linha][coluna]}]", end=" ")
    
    # Depois de terminar de imprimir todas as colunas de uma linha, pulamos de linha!
    print() 
```

**Saída:**
```text
[Grama] [Pedra] [Água] 
[Grama] [Terra] [Água] 
[Pedra] [Baú] [Grama] 
```

Se você não souber o tamanho do mapa, use o velho truque do `len()`:

```python
altura: int = len(mapa)       # Quantas linhas tem?
largura: int = len(mapa[0])   # Quantos itens tem na primeira linha?
```

---
# 🛠️ Desafios em Sala

## 1. O X da Questão (Jogo da Velha)
Crie uma matriz `3x3` que represente um tabuleiro de Jogo da Velha preenchido com espaços vazios `"-"`. 
- Peça para o Jogador 1 (X) digitar a Linha e a Coluna onde ele quer jogar. 
- Atualize a matriz colocando um `"X"`.
- Mostre o tabuleiro impresso linha a linha usando os loops `for`.

| Entrada | Saída Esperada |
| :--- | :--- |
| Linha: 1<br>Coluna: 1 | - - -<br>- X -<br>- - - |

```python
# Solução:

```

## 2. Radares de Batalha Naval
O computador tem uma matriz escondida: `oceano = [[0, 0, 0], [0, 1, 0], [0, 0, 0]]` onde `1` é um submarino. Peça para o jogador adivinhar uma linha e uma coluna. 
- Se o valor lá for `1`, imprima "BOOOM! Acertou o submarino!". 
- Se for `0`, imprima "SPLASH! Errou, caiu na água!".

| Entrada | Saída |
| :--- | :--- |
| Linha: 1<br>Coluna: 1 | BOOOM! Acertou o submarino! |
| Linha: 0<br>Coluna: 2 | SPLASH! Errou, caiu na água! |

```python
# Solução:

```

---
# 💪 Exercícios de Casa

## 1. Teste de Mesa
Analise o código abaixo e desenhe num papel (ou imagine) como a matriz `campo` ficará no final:

```python
campo: list[list[int]] = [
    [0, 0, 0],
    [0, 0, 0],
    [0, 0, 0]
]

for i in range(3):
    campo[i][i] = 1

print(campo[0][2])
```
- **A)** Qual será a saída do `print(campo[0][2])`?
- **B)** O que o loop `for` desenhou dentro da matriz? (Pense no formato do número 1 preenchido em coordenadas)

## 2. Detetive de Código
Um aluno tentou achar o diamante num pequeno baú 2x2 no Minecraft, mas deu um erro terminal chamado `IndexError` (isso acontece quando você busca um compartimento/índice que não existe). Onde e porquê ele errou?

```python
bau: list[list[str]] = [
    ["Pedra", "Madeira"],
    ["Terra", "Diamante"]
]

print("O item misterioso é:", bau[2][1])
```

## 3. O Radar de Minérios (O Caçador)
Dada uma mina (matriz `3x3`) contendo blocos variados de minérios, você precisa criar um algoritmo em loop que conte **quantas** "Esmeraldas" existem escondidas nela. Você precisa varrer a matriz inteira usando o `for` duplo!

```python
mina = [
    ["Terra", "Esmeralda", "Pedra"],
    ["Esmeralda", "Terra", "Terra"],
    ["Ferro", "Terra", "Esmeralda"]
]
```

## 4. O Cinema de Villagers
Crie uma sala de cinema representada por uma matriz `3x3` toda preenchida com a palavra `"Livre"`. O usuário vai ser o caixa do cinema.
Pergunte linha e a coluna onde o cliente quer se sentar.
- A) Se a cadeira estiver `"Livre"`, atualize para `"Ocupado"` e diga: *Ingresso Comprado!*
- B) Se a cadeira já estiver `"Ocupado"`, exiba a mensagem: *Opa, já tem alguém sentado aqui!* 
Faça isso para **3 clientes** consecutivos, usando um loop de repetição.

## 5. Matriz Identidade (Para quem gosta de Matemática)
Crie um código que pergunte um tamanho `N` ao usuário. O programa deve montar do zero uma matriz quadrada `N x N` onde a "diagonal principal" (quando `linha == coluna`) é feita de números `1`, e todo o resto do tabuleiro é `0`. Crie a matriz vazia e vá inserindo com o `append()`.

| Entrada | Saída Esperada |
| :--- | :--- |
| 3 | [1, 0, 0]<br>[0, 1, 0]<br>[0, 0, 1] |

## 6. Jogo do Campo Minado (Desafio Mestre)
Crie uma matriz de um Campo Minado `4x4` onde todo o mapa é `0` exceto as posições `[2][2]` e `[0][3]`, que escondem uma "Bomba" (valor `-1`). 
- Peça para o jogador pisar em uma Linha e Coluna.
- Se ele tentar colocar um número maior que 3 ou menor que 0, exiba "Movimento Inválido, você saiu do mapa!". 
- Se ele pisar no `-1`, exiba "BOOOM! Game Over". 
- Se ele pisar no `0`, exiba o mapa atualizado mostrando que ele já andou por ali (troque o `0` por um `X` para marcar os rastros, por exemplo).

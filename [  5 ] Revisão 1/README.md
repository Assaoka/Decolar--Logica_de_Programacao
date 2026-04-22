# 🎮 Aula 5: Revisão 1

---

## 🧠 Pensando como Programador
Para vencer os desafios abaixo, lembre-se sempre do mapa mental:
1. **O Objetivo:** O que o meu programa deve entregar no final?
2. **A Memória:** Quais informações eu preciso guardar? (Variáveis)
3. **A Pergunta:** Eu preciso decidir caminhos? (Condições: `if/else`)
4. **A Repetição:** Eu preciso fazer isso várias vezes? (Loops: `while/for`)

---

## 🕰️ 1. O Oráculo do Tempo

Um NPC antigo quer saber sua idade exata. Leia o dia, mês e ano de nascimento de uma pessoa. Calcule a idade dela **hoje** (considere a data de 23/04/2026) e quantos anos ela terá no dia 01/01/2050.

| Entrada          | Saída Esperada                    |
| :--------------- | :-------------------------------- |
| 15<br>05<br>2012 | Hoje: 13 anos<br>Em 2050: 37 anos |
| 10<br>01<br>2010 | Hoje: 16 anos<br>Em 2050: 40 anos |

```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```

## 🎁 2. Sorteio de Loot
Ao derrotar um Boss, ele dropa um código. Se o número for **PAR**, o item é uma "Espada de Diamante". Se for **ÍMPAR**, é uma "Lança de Netherite". Crie um programa que receba o número e mostre o item que o jogador ganhou.

| Entrada | Saída Esperada                       |
| :-----: | :----------------------------------- |
|   42    | Você ganhou uma Espada de Diamante\! |
|    7    | Você ganhou uma Lança de Netherite\! |

```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```

## 🏪 3. O Mercado do Aldeão

O jogador encontrou um Aldeão e resolveu comprar itens. Ele vende apenas 3 itens. Voce pode consultar os produtos oferecidos na tabela abaixo:

|  ID   | Nome do Item            | Esmeraldas |
| :---: | :---------------------- | :--------: |
|   0   | Poção de Invisibilidade |     15     |
|   1   | Ender Pearl             |     40     |
|   2   | Elmo de Netherite       |    100     |

O Aldeão só aceita vender se o jogador digitar o ID do item, não o nome. Crie um programa que receba o ID do item e quantas unidades o jogador deseja comprar e retorne o preço total. Caso o ID seja inválido, retorne uma mensagem de erro.

| Entrada | Saída Esperada                                        |
| :------ | :---------------------------------------------------- |
| 2<br>5  | Compra: 5x Elmo de Netherite<br>Total: 500 esmeraldas |
| 5<br>2  | ID Inválido! O mercador te expulsou.                  |

```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```

## 🧱 4. O Arquiteto Real (ASCII Art)

O Rei pediu para você construir uma muralha. Peça a **largura** e a **altura**. Desenhe o retângulo usando o caractere `#`, mas as bordas (primeira e última linha/coluna) devem ser feitas com `+`.

| Entrada | Saída Esperada          |
| :------ | :---------------------- |
| 5<br>3  | +++++<br>+###+<br>+++++ |
| 2<br>2  | ++<br>++                |

```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```

## 🧪 5. Invisibilidade Temporária (Decaimento)

Você comprou uma poção de invisibilidade suspeita. O vendedor te disse que ela tem `X` de potência inicial, perdendo metade de sua potência a cada `37 segundos`. Ele disse que enquanto a potência for maior que `0,1`, você continuará invisível.

Faça um programa que receba a potência inicial e calcule quanto tempo leva para a potência ser menor que 1. Imprima o tempo final formatado (horas : minutos : segundos).

| Entrada | Saída Esperada  |
| :------ | :-------------- |
| 100     | Tempo: 00:06:10 |
| 5       | Tempo: 00:03:42 |

```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```

## ⚔️ 6. Sinergia de Batalha

O Herói e seu Pet atacam juntos. Os ataques seguem o seguinte padrão:
1. O Herói ataca (2x)
2. O Pet ataca (1x)
3. O Herói ataca (2x)
4. O Pet ataca (1x)
...

Um combate acabou após 10 ataques. Leia uma lista de 10 valores (cada valor é o dano aplicado). Responda:
1. Qual foi o Dano Total?
2. Qual foi o Maior Dano aplicado pelo Pet?
3. Qual foi a contribuição do Herói para o dano total?

| Entrada                                                  | Saída Esperada                                                         |
| :------------------------------------------------------- | :--------------------------------------------------------------------- |
| 21<br>32<br>45<br>10<br>34<br>13<br>40<br>33<br>24<br>78 | Dano total: 330<br>Maior dano do pet: 45<br>Contribuição do herói: 248 |


```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```

## 🎒 7. Limpando o Inventário

O jogador minerou muito, mas o baú está cheio de itens repetidos: `["Terra", "Pedra", "Terra", "Diamante", "Pedra"]`. Crie um programa que exiba a lista de itens sem nenhuma repetição.

| Entrada | Saída Esperada                  |
| :------ | :------------------------------ |
| -       | Itens Únicos: ['Terra', 'Pedra', 'Diamante'] |

```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```

## 🪄 8. Feitiço de Reversão

Um mago lançou um feitiço que inverteu sua mochila! Pegue uma lista de 5 itens e mostre como ela ficou na ordem inversa. 

| Entrada                 | Saída Esperada                                   |
| :---------------------- | :----------------------------------------------- |
| Espada<br>Escudo<br>Tocha<br>Arco<br>Poção | Mochila Invertida: ['Poção', 'Arco', 'Tocha', 'Escudo', 'Espada'] |

```python
print("""
# Planejamento:
- Objetivo:
- Memória:
- Perguntas:
- Repetições:
""")
```

```python
# Solução:

```
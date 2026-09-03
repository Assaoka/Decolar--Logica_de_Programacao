# 🐍 Aula 9: Strings - Muito Mais que Texto

**🎯 Missão de Hoje**
> Desde a Aula 1 usamos `str` para guardar nomes e mensagens, mas até agora só sabíamos exibi-la com `print()`. Hoje vamos aprender que uma string é, na verdade, uma **sequência de caracteres** — e que dá pra cortar, buscar, transformar e formatar texto com muita precisão. Bora descobrir por que o nick de todo mundo aparece cortado no placar do Free Fire e do Fortnite!

---
## 🔤 1. Relembrando: Strings Já Estão Com a Gente
Toda vez que usamos `input()`, o resultado já era uma `string`. Toda vez que escrevemos uma mensagem com `f"Olá, {nome}!"`, já estávamos manipulando texto. Hoje vamos abrir essa caixinha e ver o que tem por dentro dela.

```python
nick: str = "ProGamer2026"
print(nick)
print(type(nick))
```

---
## 📍 2. Strings são Sequências: Índices e Fatiamento
Lembra das Listas e Matrizes? Cada posição tinha um **endereço** (índice), começando do `0`. Com strings é exatamente igual: cada letra tem uma posição fixa.

```python
nick: str = "Steve123"

print(nick[0])   # S
print(nick[3])   # v
print(nick[-1])  # 3 (índice negativo = de trás pra frente, igual nas listas!)
```

### ✂️ Fatiamento (Slicing)
Além de pegar **um** caractere, podemos pegar um **pedaço inteiro** da string usando `[inicio:fim]`. O `fim` nunca é incluído — ele marca "até aqui, sem contar".

```python
nick: str = "CacadorDeDiamantes"

print(nick[0:6])   # Cacado
print(nick[:6])    # Cacado (se eu não escrevo o início, ele assume o começo)
print(nick[6:])    # rDeDiamantes (se eu não escrevo o fim, ele vai até o final)
```

**💡 Pra Que Serve Isso na Prática?**
> Já reparou que em jogos como **Free Fire** e **Fortnite** o nick de alguns jogadores aparece cortado no placar, terminando em `"..."`? Isso é fatiamento em ação! O jogo pega só os primeiros caracteres do nome e completa com reticências quando o nick é grande demais para caber na tela.

```python
nick: str = "DestruidorDeBases2026"
limite: int = 12

nick_cortado: str = nick[:limite] + "..."
print(nick_cortado) # DestruidorDe...
```

Também podemos usar um terceiro número no fatiamento: o **passo** (`[inicio:fim:passo]`). Ele define de quantos em quantos caracteres o Python "anda" enquanto pega os valores. O padrão é `1` (pega um caractere de cada vez, sem pular nenhum), mas podemos mudar isso:

```python
codigo: str = "ABCDEFGH"

print(codigo[::2])   # ACEG -> pula de 2 em 2, pegando os índices 0, 2, 4, 6
print(codigo[1::2])  # BDFH -> começa no índice 1 e também pula de 2 em 2
print(codigo[::3])   # ADG  -> pula de 3 em 3, pegando os índices 0, 3, 6
```

Um truque famoso é usar passo `-1`: ele faz o Python andar de trás pra frente, pegando um caractere de cada vez, o que inverte a string inteira!

```python
palavra: str = "Ditto"
print(palavra[::-1]) # ottiD
```

---
## 🔒 3. Strings são Imutáveis
Aqui vai uma diferença importante entre Strings e Listas. Nas listas, podíamos trocar um item direto pelo índice (`lista[0] = "novo item"`). Com strings, isso **não é permitido**:

```python
nick: str = "Steve"
nick[0] = "J" # ERRO! TypeError: 'str' object does not support item assignment
```

**⚠️ Por Que Isso Acontece?**
> No Python, uma string é um pacote fechado: depois de criada, ela não pode ser alterada por dentro. Se você quer "mudar" uma string, na verdade você está sempre **criando uma string nova** e guardando por cima da antiga. Vamos entender exatamente o que isso significa "por baixo dos panos" (memória e referências) em uma aula futura!

```python
nick: str = "Steve"
nick = "J" + nick[1:]  # Isso funciona! Criamos uma string NOVA
print(nick) # Jteve
```

---
## 🛠️ 4. Métodos Essenciais
Assim como listas e dicionários, strings vêm com várias "ferramentas" já grudadas nelas:

| Método                  | O que faz?                                             | Exemplo                                | Resultado          |
| :----------------------- | :------------------------------------------------------ | :--------------------------------------- | :------------------ |
| `.upper()`               | Transforma tudo em MAIÚSCULO.                            | `"steve".upper()`                        | `"STEVE"`           |
| `.lower()`                | Transforma tudo em minúsculo.                            | `"STEVE".lower()`                        | `"steve"`           |
| `.capitalize()`           | Deixa só a primeira letra maiúscula.                     | `"steve".capitalize()`                   | `"Steve"`           |
| `.strip()`                | Remove espaços do início e do fim.                       | `"  Steve  ".strip()`                    | `"Steve"`           |
| `.replace(velho, novo)`   | Troca um pedaço da string por outro.                     | `"gg wp".replace("gg", "Bom jogo")`      | `"Bom jogo wp"`     |
| `.split(separador)`       | Quebra a string em uma **lista** usando o separador.     | `"Steve,Alex,Ditto".split(",")`          | `['Steve', 'Alex', 'Ditto']` |
| `separador.join(lista)`   | Faz o caminho contrário: junta uma lista em uma string.  | `" ".join(["Bom", "jogo"])`              | `"Bom jogo"`        |
| `.find(pedaco)`           | Retorna o índice onde o pedaço começa (ou `-1` se não achar). | `"Minecraft".find("craft")`         | `4`                 |
| `.count(pedaco)`          | Conta quantas vezes um pedaço aparece.                   | `"banana".count("a")`                    | `3`                 |
| `.startswith(pedaco)`     | Verifica se a string começa com aquele pedaço.           | `"Pikachu".startswith("Pika")`           | `True`              |
| `.isdigit()`              | Verifica se a string é só números.                       | `"2026".isdigit()`                       | `True`              |
| `.isalpha()`              | Verifica se a string é só letras.                        | `"Steve1".isalpha()`                     | `False`             |

Um combo muito comum é usar `.split()` para separar informações digitadas de uma vez só:

```python
entrada: str = input("Digite seu clã e seu nick separados por espaço: ")
partes: list[str] = entrada.split(" ")

cla: str = partes[0]
nick: str = partes[1]

print(f"Clã: {cla} | Nick: {nick}")
```

---
## 🎨 5. Formatação Avançada de f-strings
Na Aula 7 (Dicionários), usamos `f"- {nome:10}"` para alinhar o placar da guilda, mas passamos batido pelo motivo disso funcionar. Agora vamos entender de verdade os "mini-comandos" que ficam depois dos dois pontos `:` dentro das chaves `{}`.

### O que o Número Significa?
O número depois dos dois pontos (ex: `:10`) define a **largura mínima do campo** — ou seja, quantos espaços o Python reserva no total para aquele valor. Se o texto for **menor** que esse número, o restante é preenchido com espaços em branco. Se for **maior**, o número é ignorado e o texto aparece inteiro — formatar largura **não é a mesma coisa** que cortar (aquilo era o fatiamento, lá do Cortador de Nick)!

```python
nome_curto: str = "Ditto"
nome_longo: str = "CampeaoDoTorneio"

print(f"[{nome_curto:10}]") # [Ditto     ] -> "Ditto" tem 5 letras + 5 espaços = 10 no total
print(f"[{nome_longo:10}]") # [CampeaoDoTorneio] -> tem 16 letras, maior que 10, então nada é cortado
```

### E os Símbolos `<`, `>` e `^`?
Por padrão, texto (`str`) é alinhado à **esquerda** dentro desse espaço. Para mudar isso, colocamos um símbolo de alinhamento logo depois dos dois pontos, antes do número:
- `<` → alinha à **esquerda** (é o padrão para strings, pode deixar em branco que dá no mesmo)
- `>` → alinha à **direita**
- `^` → **centraliza**

```python
nome: str = "Ditto"

print(f"[{nome:<10}]")  # [Ditto     ] -> alinhado à esquerda (o < pode ser omitido)
print(f"[{nome:>10}]")  # [     Ditto] -> alinhado à direita
print(f"[{nome:^10}]")  # [  Ditto   ] -> centralizado
```

Isso é perfeito para montar tabelas bonitas no terminal, tipo um placar:

```python
jogadores: list[str] = ["Steve", "Alex", "Ditto"]
pontos: list[int] = [100, 2500, 30]

for nome, pontuacao in zip(jogadores, pontos):
    print(f"{nome:<10} | {pontuacao:>6} pontos")
```

Para números com casas decimais, usamos `.2f` (2 casas depois da vírgula):

```python
preco: float = 19.9
print(f"Preço: R$ {preco:.2f}") # Preço: R$ 19.90
```

---
# 🛠️ Desafios em Sala

## 1. Cortador de Nick
Peça ao usuário um nick e um limite de caracteres. Se o nick for maior que o limite, corte e complete com `"..."`. Se já couber, mostre o nick original.

| Entrada                        | Saída                  |
| :------------------------------ | :---------------------- |
| DestruidorDeBases2026<br>12     | DestruidorDe...         |
| Steve<br>12                     | Steve                   |

```python
# Solução:

```

## 2. Filtro de Chat
Dada a lista `palavras_proibidas = ["noob", "hacker", "lag"]`, peça para o usuário digitar uma frase de chat. Substitua toda palavra proibida encontrada por `"***"` e mostre a frase censurada.

| Entrada                          | Saída                              |
| :--------------------------------- | :------------------------------------ |
| para de hacker seu noob             | para de *** seu ***                   |
| bom jogo galera                     | bom jogo galera                       |

```python
# Solução:

```

## 3. Validador de Nome de Treinador
No mundo Pokémon, o nome do treinador precisa seguir 3 regras: começar com letra maiúscula, ter só letras (sem espaço ou número) e ter no máximo 12 caracteres. Peça um nome e diga se ele é válido, mostrando qual regra foi quebrada quando não for.

| Entrada         | Saída                                              |
| :---------------- | :---------------------------------------------------- |
| Ash                | Nome válido! Bem-vindo, Treinador Ash.                |
| ash                | Nome inválido: precisa começar com letra maiúscula.   |
| Alexandre123       | Nome inválido: use apenas letras.                     |

```python
# Solução:

```

---
# 💪 Exercícios de Casa

## 1. Teste de Mesa
Analise o código abaixo e diga o que será impresso em cada linha:

```python
nick: str = "CacadorDeCreepers"

print(nick[:8])
print(nick[8:])
print(nick[-7:])
print(nick[::2])
```

- **Saída do print 1:**
- **Saída do print 2:**
- **Saída do print 3:**
- **Saída do print 4:**

## 2. Detetive de Código
Um aluno tentou trocar a primeira letra do nick do seu personagem para maiúscula, mas o código não funciona. Encontre o erro e explique por que ele acontece:

```python
nick = "steve"
nick[0] = "S"
print(nick)
```

**Escreva aqui o erro encontrado e como corrigir:**

## 3. Verificador de Palíndromo
Um palíndromo é uma palavra que se lê igual de trás para frente. Peça um nome de personagem de jogo e diga se ele é um palíndromo, ignorando maiúsculas e minúsculas.
- **Dica:** lembre-se do truque de inverter string com fatiamento (`[::-1]`).

| Entrada | Saída                              |
| :------- | :------------------------------------ |
| Ana      | "Ana" é um palíndromo!                |
| Otto     | "Otto" é um palíndromo!               |
| Mario    | "Mario" não é um palíndromo.          |

## 4. Contador de Vogais e Consoantes
Peça uma frase e conte quantas vogais e quantas consoantes ela tem (ignore espaços).

| Entrada                | Saída                                  |
| :----------------------- | :---------------------------------------- |
| Minecraft é incrivel     | Vogais: 8 \| Consoantes: 13               |

## 5. Gerador de Clã Tag
Muitos jogos (Free Fire, Clash Royale) mostram o nome do jogador junto com a tag do clã, tipo `[ABC] Nick`. Peça o nome do clã e o nick do jogador, e monte essa formatação. A tag do clã deve ter no máximo 3 letras e sempre aparecer em maiúsculo (se o usuário digitar mais de 3 letras, use só as 3 primeiras).

| Entrada                    | Saída            |
| :---------------------------- | :------------------ |
| aba<br>Steve                  | [ABA] Steve          |
| dragoes<br>Alex               | [DRA] Alex           |

## 6. Checador de Senha Forte
Crie um programa que avalie a senha de uma conta de jogo. Para ser considerada forte, a senha precisa ter: no mínimo 8 caracteres, pelo menos uma letra maiúscula e pelo menos um número.
- **Dica:** pesquise sobre o método `.isupper()` e lembre-se que dá pra percorrer uma string com um `for`, letra por letra, assim como percorremos uma lista.

| Entrada       | Saída                                                          |
| :-------------- | :----------------------------------------------------------------- |
| minecraft123     | Senha fraca: falta uma letra maiúscula.                            |
| Minecraft        | Senha fraca: falta um número.                                      |
| Minecraft123     | Senha forte!                                                       |

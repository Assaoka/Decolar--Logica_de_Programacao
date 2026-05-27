# 🐍 Aula 6: Dicionários - Criando um Banco de Dados

**Missão de Hoje**
> Hoje vamos aprender a criar o nosso próprio "banco de dados" em Python usando os **Dicionários**! Diferente das listas que usam números automáticos para organizar gavetas, os dicionários usam "etiquetas personalizadas" (chaves) para encontrar qualquer informação em um piscar de olhos. Vamos descobrir como criar fichas de personagens completas e até tabelas de dados inteiras combinando listas e dicionários!

---
## 📖 1. O que é um Dicionário?
Nas listas, cada prateleira ou gaveta recebe um número de posição automático (índice 0, 1, 2...). Isso é ótimo quando a ordem dos itens importa, mas e se você quiser guardar as características de um personagem em um jogo?

Se usássemos uma lista, seria confuso de lembrar:
```python
# [Vida, Mana, Força, Defesa]
atributos: list[int] = [20, 50, 15, 10]
```
Como você lembra se o índice `2` representa a Força ou a Defesa? E se no meio do jogo você adicionar um novo atributo no início da lista? Todos os números de índice vão mudar e seu código vai quebrar!

Um **Dicionário** resolve isso. Ele funciona como uma agenda de contatos ou um dicionário de português real: você pesquisa uma **Palavra** (Chave) para descobrir o seu **Significado** (Valor).

Em vez de usar posições numéricas, nós criamos as nossas próprias "etiquetas"!

```python
# Criando a ficha de atributos do Steve
jogador: dict[str, int] = {
    "vida": 20,
    "mana": 50,
    "forca": 15,
    "defesa": 10
}

# Acessando as informações pela etiqueta (chave)
print(jogador["vida"])   # 20
print(jogador["forca"])  # 15
```

> [!tip] **A Analogia Visual:**
> Imagine que cada item no dicionário é uma caixinha com uma etiqueta colada por fora. Você puxa a caixinha pela etiqueta (chave) para abrir e ver o tesouro que está lá dentro (valor).

---
## 🔑 2. Chaves e Valores: A Regra do Cadastro
Ao construir um dicionário no Python, devemos seguir algumas regras importantes:
1. Ele é criado utilizando **Chaves `{ }`**.
2. Cada par é associado por **dois pontos `:`** (`chave : valor`).
3. Os pares são separados por **vírgulas `,`** (semelhante as listas). Muitas vezes os programadores quebram a linha após cada par para facilitar a leitura, porém isso é opcional.
4. As **Chaves** (etiquetas) devem ser **únicas**! Se você colocar duas chaves com o mesmo nome, o Python apagará a primeira e manterá apenas o valor mais recente.

```python
# Criando um dicionário de traduções de itens do Minecraft
traducoes: dict[str, str] = {
    "stone": "pedra",
    "dirt": "terra",
    "gold": "ouro"
}
```

### ➕ Adicionando e Editando Elementos
Nas listas, nós precisávamos usar funções complexas como `.append()` ou `.insert()`. No dicionário, modificar dados é muito mais simples! Basta indicarmos a chave e atribuirmos o novo valor:

```python
# 1. Modificando um valor existente (Steve comeu uma maçã dourada e recuperou vida)
jogador["vida"] = 30 

# 2. Adicionando uma nova chave-valor (Steve coletou moedas)
jogador["moedas"] = 100

print(jogador)
# Saída: {'vida': 30, 'mana': 50, 'forca': 15, 'defesa': 10, 'moedas': 100}
```
*Se a chave já existia no dicionário, o valor antigo é atualizado. Se a chave não existia, o Python cria a nova etiqueta e salva o valor automaticamente!*

---
## 🛠️ 3. Ferramentas Principais
Para gerenciar as caixinhas dos nossos dicionários, o Python possui funções nativas muito úteis:

| Comando                 | O que faz?                                                              | Exemplo                          |
| :---------------------- | :---------------------------------------------------------------------- | :------------------------------- |
| `.keys()`               | Retorna uma sequência com todas as **chaves** (etiquetas).              | `jogador.keys()`                 |
| `.values()`             | Retorna uma sequência com todos os **valores** (conteúdos).             | `jogador.values()`               |
| `.items()`              | Retorna os pares **chave e valor** juntos em grupos.                    | `jogador.items()`                |
| `.get(chave, padrao)`   | Busca uma chave de forma **segura** (evita erros fatais).               | `jogador.get("xp", 0)`           |
| `.pop(chave)`           | Remove e retorna o valor da chave informada.                            | `moedas = jogador.pop("moedas")` |
| `del dicionario[chave]` | Deleta diretamente o par chave-valor do dicionário.                     | `del jogador["defesa"]`          |
| `len(dicionario)`       | Retorna quantos pares chave-valor existem no dicionário.                | `len(jogador)`                   |
| `chave in dicionario`   | Retorna `True` se a chave existe no dicionário, `False` caso contrário. | `if "vida" in jogador:`          |

### 🛡️ Evitando Erros com o `.get()`
Se você tentar acessar uma chave que não existe no dicionário usando a sintaxe comum de colchetes (como `jogador["xp"]`), o Python vai parar imediatamente a execução e exibir o erro fatal `KeyError`.

Para evitar que o seu jogo feche sozinho, usamos o método `.get()`. Ele tenta buscar a chave; caso ela não exista, ele devolve o valor padrão que você escolheu como fallback:

```python
# O atributo "xp" não existe no nosso jogador. O .get retorna 0 em vez de quebrar!
xp_atual: int = jogador.get("xp", 0)
print(f"XP do jogador: {xp_atual}") # XP do jogador: 0
```

---
## 🔄 4. Dicionários + Loops: Navegando no Cadastro
Diferente das listas onde percorremos slots ordenados, nos dicionários podemos usar o loop `for` de três formas diferentes, a depender do que queremos ler:

### A) Percorrendo apenas as Chaves
```python
# Útil para saber quais atributos nosso personagem possui cadastrados
for atributo in jogador.keys():
    print(f"Estatística disponível: {atributo}")
```

### B) Percorrendo apenas os Valores
```python
# Útil se quisermos somar pontos ou analisar apenas os dados numéricos
for valor in jogador.values():
    print(f"Valor numérico: {valor}")
```

### C) Percorrendo Chaves e Valores juntos (O mais comum!)
O método `.items()` nos entrega a dupla (chave, valor) a cada iteração do loop. Funciona de forma parecida com o `enumerate` que usamos nas listas:

```python
print("--- STATS DO JOGADOR ---")
for chave, valor in jogador.items():
    print(f"🔹 {chave} -> {valor}")
```

No entanto, se você quiser modificar os valores originais do dicionário, você deve usar a sintaxe com `jogador[chave]` dentro do loop. Isso ocorre porque as variáveis temporárias `chave` e `valor` são diferentes dos valores originais do dicionário. Por exemplo:
```python
for chave, valor in jogador.items():
    valor += 1
print(jogador)

for chave in jogador.keys():
    jogador[chave] += 1
print(jogador)
```

---
## 🧬 5. O Combo: Listas + Dicionários
Quando combinamos listas com dicionários, conseguimos criar estruturas mais complexas.

### 📊 A) Dicionário de Listas (Tabela de Dados)
Às vezes, queremos salvar informações organizadas em colunas, parecidas com uma planilha do Excel ou um banco de dados relacional. 

Neste modelo, criamos um **Dicionário** onde cada chave representa o nome de uma coluna e o valor associado é uma **Lista** com os dados daquela coluna:

```python
# Tabela com o placar geral do campeonato do clã
tabela_placar: dict[str, list] = {
    "jogadores": ["Steve", "Alex", "Creeper"],
    "pontos": [100, 250, 50],
    "vitorias": [5, 12, 1]
}

# Acessando todos os dados de uma coluna específica
print(tabela_placar["jogadores"]) # ['Steve', 'Alex', 'Creeper']

# Mostrando os dados linha por linha (relacionando pelo índice comum das listas)
total_jogadores = len(tabela_placar["jogadores"])

for i in range(total_jogadores):
    nome: str = tabela_placar["jogadores"][i]
    pontos: int = tabela_placar["pontos"][i]
    vitorias: int = tabela_placar["vitorias"][i]
    
    print(f"- {nome:10} | Pontos: {pontos:3} | Vitórias: {vitorias}")
```

### 👥 B) Lista de Dicionários (Banco de Cadastros)
Imagine uma guilda de heróis. Cada aventureiro é um indivíduo único com sua própria ficha de atributos. Como cada herói pertence a uma classe diferente, eles podem ter informações ligeiramente distintas (o guerreiro tem espada, o mago tem mana, mas o arqueiro tem flechas). 

Para organizar isso, criamos uma **Lista** onde cada compartimento contém um **Dicionário**:

```python
# Cada dicionário é a ficha de um herói da nossa guilda
guilda: list[dict[str, any]] = [
    {
        "nome": "Ragnar",
        "classe": "Guerreiro",
        "vida": 25,
        "espada": "Ferro",
        "armadura": "Couro"
    },
    {
        "nome": "Frieren",
        "classe": "Maga",
        "vida": 15,
        "mana": 80,
        "magias": [
            "Bola de Fogo",
            "Zoltraak",
            "Campo de Flores"
        ]
    },
    {
        "nome": "Robin",
        "classe": "Arqueiro",
        "vida": 18,
        "flechas": 64
    }
]

# Percorrendo a lista de heróis e exibindo suas características
for heroi in guilda:
    print(f"\nHerói: {heroi['nome']} ({heroi['classe']})")
    print(f"❤️ Vida: {heroi['vida']}")

    print("Outras informações:")
    for informacao, valor in heroi.items():
        if informacao in ["nome", "classe", "vida"]:
            continue

        print(f"- {informacao}: {valor}")
```

Viu como a Frieren tem as chaves `"mana"` e `"magias"`, mas o Ragnar e o Robin não têm? Essa é a maior vantagem dos dicionários sobre outras estruturas: eles são altamente flexíveis e não obrigam todos os elementos a terem exatamente os mesmos campos!

**Curiosidade:**
> Um dos tipos de arquivo mais comuns na internet é o formato **JSON** (JavaScript Object Notation). Ele funciona de maneira **exatamente igual** aos dicionários que estamos aprendendo agora! Nós poderiamos armazenar nossa guilda em um arquivo de texto e depois carregar quando quiséssemos.



---
# 🛠️ Desafios em Sala

## 1. O Mapeador de Portais (Consulta Rápida)
No Minecraft, você construiu portais do Nether em diferentes locais e deseja consultar qual é o portal mais próximo. Crie um dicionário com os seguintes portais:

| Portal    | X    | Y    | Z    |
| :-------- | :--- | :--- | :--- |
| Base      | 150  | 64   | 300  |
| Vila      | -420 | 70   | -100 |
| Fortaleza | 800  | 100  | 450  |

Para calcular a distancia entre 2 pontos em 3 dimensões (P1(x1,y1,z1) e P2(x2,y2,z2)) usamos a seguinte fórmula:
$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2 + (z_2 - z_1)^2}$$

Peça para o usuário digitar as coordenadas atuais (x, y e z) e retorne qual o portal mais próximo e quanto ele deve caminhar em cada direção.

| Entrada          | Saída                                                                              |
| :--------------- | :--------------------------------------------------------------------------------- |
| 100<br>50<br>250 | O portal mais próximo é: Base<br>Deslocamento:<br> - X: 50<br> - Y: 14<br> - Z: 50 |


```python
# Solução:

```

## 2. O Consultor de Preços do Villager
Crie um dicionário contendo o estoque de vendas de um Villager com 4 itens e seus respectivos valores em Esmeraldas:

| Item      | Preço |
| --------- | ----- |
| Pão       | 10    |
| Esmeralda | 10    |
| Livro     | 30    |
| Espada    | 70    |

Aplique um desconto de 20% em todos os itens do dicionário, exibindo o resultado final bem formatado.

| Entrada | Saída                                                                                                                                                                                                 |
| :------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|         | **Lojas do Villager**<br>Estes itens estão com desconto de 20%:<br>- Pão: 8 Esmeraldas<br>- Esmeralda: 8 Esmeraldas<br>- Livro: 24 Esmeraldas<br>- Espada: 56 Esmeraldas<br><br>Aproveite a promoção! |

```python
# Solução:

```

## 3. Depósito de Minérios (Adição e Atualização)
Você e seu amigo estão minerando juntos e guardando tudo no mesmo baú. Crie um programa que inicie com um baú contendo alguns minérios já guardados:
`bau: dict[str, int] = {"Pedra": 64, "Carvão": 32, "Ferro": 10}`

Peça para o usuário digitar o **Nome do Minério** que ele acabou de coletar e a **Quantidade** (valor inteiro). O programa deve atualizar o baú:
- Se o minério já existir no baú (chave existente), some a nova quantidade ao valor atual.
- Se o minério for novo (chave inexistente), adicione-o ao dicionário com a quantidade informada.

No final, mostre o baú totalmente atualizado exibindo cada item e quantidade linha por linha usando o loop `for` com `.items()`.

| Entrada       | Saída                                                                                                                 |
| :------------ | :-------------------------------------------------------------------------------------------------------------------- |
| Ferro<br>15   | **BAÚ ATUALIZADO**<br>- Pedra: 64 unidades<br>- Carvão: 32 unidades<br>- Ferro: 25 unidades                           |
| Diamante<br>5 | **BAÚ ATUALIZADO**<br>- Pedra: 64 unidades<br>- Carvão: 32 unidades<br>- Ferro: 10 unidades<br>- Diamante: 5 unidades |

```python
# Solução:

```

---
# 💪 Exercícios de Casa

## 1. Teste de Mesa
Analise atentamente o código abaixo e responda às perguntas:

```python
inventario: dict[str, int] = {"Espada": 1, "Tocha": 64, "Pao": 5}
inventario["Tocha"] = 32
inventario["Diamante"] = 3
del inventario["Pao"]

print(inventario.get("Pao", 0))
print(len(inventario))
```

- **Qual o estado final das chaves e valores do dicionário `inventario`?**
- **O que será exibido na tela no primeiro print?**
- **O que será exibido na tela no segundo print?**

## 2. Detetive de Código
Um estudante tentou criar um programa para armazenar e exibir a quantidade de corações de vida dos monstros, mas o código contém 3 erros de sintaxe ou de lógica que impedem o funcionamento correto. Encontre-os e explique como corrigi-los:

```python
monstros = {Zombie: 20, "Creeper": 15, "Enderman": 40}

# Todos tomaram 5 de dano
for vida in monstros.values():
    vida -= 5

# Mostrando a vida do Zombie
print("Vida do Zombie: " + monstros["Zombie"])
```

**Escreva aqui quais erros você encontrou e a correção:**
1. 
2. 
3. 

## 3. O Separador de Itens de Inventário
Você passou horas minerando blocos no Minecraft e salvou todos em uma lista. Escreva um programa que leia essa lista de itens e conte automaticamente quantas unidades de cada tipo você coletou, armazenando o resumo em um dicionário.

| Entrada (Itens Coletados)                | Saída                                                                   |
| :--------------------------------------- | :---------------------------------------------------------------------- |
| Pedra,Pedra,Diamante,Pedra,Ouro,Diamante | **Resumo da Mineração:**<br> - Pedra: 3<br> - Diamante: 2<br> - Ouro: 1 |

## 4. O Sistema de Vendas do Villager
Crie um dicionário com 3 itens e suas respectivas quantidades em estoque:

| Item   | Quantidade |
| ------ | ---------- |
| Pão    | 10         |
| Flecha | 50         |
| Maçã   | 5          |

O usuário deve escolher um item e digitar a quantidade que deseja comprar:
- Se o item não existir no catálogo: exiba "Item indisponível."
- Se a quantidade desejada for maior do que a disponível em estoque: exiba "Estoque insuficiente!"
- Se a compra for bem-sucedida: diminua a quantidade do estoque do dicionário e exiba "Compra realizada! Estoque atual de [Item]: [Estoque Restante]".

| Entrada      | Saída                                         |
| :----------- | :-------------------------------------------- |
| Flecha<br>20 | Compra realizada! Estoque atual de Flecha: 30 |
| Pão<br>15    | Estoque insuficiente!                         |

## 5. Ficha Médica de Pokémons (Lista de Dicionários)
Crie uma lista contendo Pokémons machucados no Centro Pokémon. Cada Pokémon deve ser representado por um dicionário com as chaves `nome`, `vida_max` e `vida_atual`.
1. Cadastre 4 Pokémons informando seus nomes, vidas máximas e vidas atuais.
2. A Enfermeira Joy ativa uma máquina de cura geral que adiciona 20 de HP a todos os Pokémons da lista, respeitando a vida máxima.
3. Mostre o relatório final com a vida recuperada de cada Pokémon.

| Entrada                                                                             | Saída                                                                                                     |
| :---------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------- |
| Pikachu<br>20<br>20<br>Eevee<br>35<br>3<br>Bulbasaur<br>30<br>50<br>Mew<br>55<br>65 | **Relatório de Recuperação:**<br>🏥 Pikachu: HP 20<br>🏥 Eevee: HP 23<br>🏥 Bulbasaur: HP 50<br>🏥 Mew: HP 65 |

## 6. Estatísticas do Clã (Dicionário de Listas)
Observe o seguinte dicionário que armazena os dados dos jogadores de um clã:

```python
cla: dict[str, list] = {
    "nomes": ["GamerPro", "MineCrafter", "RedstoneGuy"],
    "dias_jogados": [12, 45, 8],
    "blocos_quebrados": [1500, 12000, 850]
}
```

Faça um programa que:
1. Permita que o usuário digite o nome de um jogador para buscar no clã.
2. Exiba todas as informações daquele jogador de forma organizada.
3. Se o jogador não estiver cadastrado nas listas, avise: "Jogador não encontrado!".

| Entrada     | Saída                                                                                           |
| :---------- | :---------------------------------------------------------------------------------------------- |
| MineCrafter | **Estatísticas de MineCrafter:**<br>⏱️ Dias jogados: 45 dias<br>🧱 Blocos quebrados: 12000 blocos |
| Herobrine   | Jogador não encontrado!                                                                         |

## 7. O Tradutor Universal de Gírias de Jogos
Crie um dicionário com termos e gírias comumente usadas em partidas online e suas respectivas traduções (ex: "gg": "Bom jogo", "brb": "Já volto", "afk": "Longe do teclado", "wp": "Bem jogado"). Peça para o usuário digitar uma gíria e mostre a tradução:
- **Desafio Extra:** Permita que o usuário digite uma frase inteira e o programa traduza as gírias encontradas nela (ex: "A partida foi gg wp" se transforma em "A partida foi Bom jogo Bem jogado").

| Entrada | Saída                      |
| :------ | :------------------------- |
| afk     | Tradução: Longe do teclado |
| brb     | Tradução: Já volto         |

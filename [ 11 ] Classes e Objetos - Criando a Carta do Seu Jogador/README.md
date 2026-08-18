# 🐍 Aula 11: Classes e Objetos - Criando a Carta do Seu Jogador

**🎯 Missão de Hoje**
> Lá na Aula 7, criamos fichas de heróis usando dicionários (`{"nome": "Ragnar", "vida": 25}`). Lá na Aula 8, criamos Pokémons como `{"nome": "Pikachu", "vida_atual": 40}` para simular batalhas. Funcionou, mas hoje vamos aprender uma forma muito mais poderosa e organizada de fazer isso: as **Classes**. Vamos criar nosso próprio "molde" de dados, igual uma carta de jogador do FIFA/EA FC, com atributos E ações que ela sabe fazer sozinha.

---
## 🤔 1. Por Que Não Só Usar Dicionários?
Dicionários resolvem muita coisa, mas têm um problema chato: o Python não te avisa se você errar o nome de uma chave.

```python
jogador: dict = {"nome": "Mbappé", "forca": 88, "velocidade": 97}

print(jogador["velocidade"])  # 97, funciona normal
print(jogador["força"])       # ERRO! KeyError -> a chave certa era "forca", sem cedilha
```

Além disso, na Aula 8 tivemos que criar uma função `batalha_pokemon(p1, p2)` **separada** dos dicionários dos Pokémons — os dados ficavam de um lado, e o comportamento (a lógica da batalha) ficava solto em outro lugar do código. Hoje vamos juntar os dois numa coisa só.

---
## 🧱 2. Structs: A Ideia de um "Molde" de Dados
Em várias linguagens de programação (como C), existe algo chamado **Struct**: um molde que define exatamente quais informações um "pacote" de dados deve ter. Por exemplo, um molde de Jogador sempre vai ter *nome*, *posição* e *atributos* — nunca vai faltar um campo nem aparecer um campo errado por engano.

O Python não tem uma palavra-chave `struct` separada — ele resolve isso (e vai além) com as **Classes**, que veremos agora.

---
## 🏗️ 3. Criando uma Classe (`class`, `__init__` e `self`)
Uma **Classe** é o molde. Um **Objeto** (ou **instância**) é uma carta específica, construída a partir desse molde, com seus próprios valores.

```python
class Jogador:
    def __init__(self, nome: str, posicao: str, ritmo: int, finalizacao: int, defesa: int) -> None:
        self.nome = nome
        self.posicao = posicao
        self.ritmo = ritmo
        self.finalizacao = finalizacao
        self.defesa = defesa
```

Vamos entender cada peça nova aqui:

| Termo | O que significa? |
| :--- | :--- |
| `class` | Palavra-chave que cria um novo molde de dados. |
| `__init__` | Um método especial que roda **automaticamente** toda vez que criamos um objeto novo — ele monta a carta. |
| `self` | Como o objeto se refere "a si mesmo" dentro da classe. É sempre o primeiro parâmetro de qualquer método. |
| Atributo | Uma variável que pertence ao objeto (ex: `self.nome`). |
| Objeto / Instância | Uma carta específica criada a partir do molde da classe. |

Para criar um objeto de verdade a partir do molde, chamamos a classe como se fosse uma função:

```python
mbappe = Jogador("Mbappé", "ATA", 97, 91, 39)
neymar = Jogador("Neymar", "MEI", 87, 83, 32)
```

> **💡 Por Que o `self`?**
> Quando você escreve `mbappe = Jogador(...)`, o Python entrega esse objeto recém-criado para o parâmetro `self` do `__init__` por trás dos panos. É por isso que `self.nome = nome` funciona: estamos dizendo "guarda esse `nome` dentro **deste** objeto específico".

---
## 📇 4. Acessando e Alterando Atributos
Para ler ou mudar um atributo de um objeto, usamos um ponto (`.`) entre o objeto e o nome do atributo:

```python
print(mbappe.nome)          # Mbappé
print(mbappe.ritmo)         # 97

mbappe.finalizacao = 93     # atualizando o atributo direto
print(mbappe.finalizacao)   # 93
```

Repare que `mbappe` e `neymar` são objetos **independentes**: mudar o atributo de um não afeta o outro, mesmo os dois vindo do mesmo molde `Jogador`.

---
## ⚙️ 5. Métodos: Comportamento Dentro da Carta
A grande vantagem das classes é que, além de guardar dados, elas podem ter **métodos** — funções que pertencem ao objeto e sabem usar os próprios atributos dele:

```python
class Jogador:
    def __init__(self, nome: str, posicao: str, ritmo: int, finalizacao: int, defesa: int) -> None:
        self.nome = nome
        self.posicao = posicao
        self.ritmo = ritmo
        self.finalizacao = finalizacao
        self.defesa = defesa

    def overall(self) -> float:
        media: float = (self.ritmo + self.finalizacao + self.defesa) / 3
        return round(media, 1)

    def exibir_carta(self) -> None:
        print(f"⚽ {self.nome} ({self.posicao})")
        print(f"OVR: {self.overall()}")
        print(f"RIT: {self.ritmo} | FIN: {self.finalizacao} | DEF: {self.defesa}")
```

```python
mbappe = Jogador("Mbappé", "ATA", 97, 91, 39)
mbappe.exibir_carta()
```

Repare que dentro do método, usamos `self.ritmo` (com `self.`) para acessar o atributo — sem o `self`, o Python não saberia de qual objeto estamos falando.

---
## 🔄 6. Comparando: Antes (Dicionário) x Depois (Classe)
Lembra da `batalha_pokemon` da Aula 8, que recebia dois dicionários soltos? Veja como fica muito mais organizado usando uma classe, onde o próprio Pokémon "sabe" atacar:

```python
class Pokemon:
    def __init__(self, nome: str, vida_atual: int, velocidade: int, dano: int) -> None:
        self.nome = nome
        self.vida_atual = vida_atual
        self.velocidade = velocidade
        self.dano = dano

    def atacar(self, alvo: "Pokemon") -> None:
        alvo.vida_atual -= self.dano
        print(f"{self.nome} atacou {alvo.nome}! Vida restante: {alvo.vida_atual}")
```

```python
pikachu = Pokemon("Pikachu", 40, 90, 15)
onix = Pokemon("Onix", 60, 70, 10)

pikachu.atacar(onix)
onix.atacar(pikachu)
```

> **⚔️ De Olho no Futuro**
> Repare que `Pokemon` e `Jogador` são classes bem diferentes, mas ambas têm "atributos + método". No Minecraft, Zombie e Creeper também compartilham várias características por serem os dois um `Mob`. Existe um jeito de uma classe **herdar** características de outra (chamado de **Herança**) — isso fica pra uma próxima aula, mas guarda essa palavra!

---
# 🛠️ Desafios em Sala

## 1. Criando a Carta do Craque
Crie a classe `Jogador` (igual a da seção 5, com `overall()` e `exibir_carta()`). Peça ao usuário o nome, a posição e os 3 atributos (ritmo, finalização, defesa) para criar um objeto, e exiba a carta montada.

| Entrada | Saída |
| :--- | :--- |
| Vini Jr<br>ATA<br>95<br>85<br>29 | ⚽ Vini Jr (ATA)<br>OVR: 69.7<br>RIT: 95 \| FIN: 85 \| DEF: 29 |

```python
# Solução:

```

## 2. Sistema de Treino
Adicione um método `treinar(self, atributo: str, pontos: int)` à classe `Jogador`. Ele deve aumentar o atributo escolhido (`"ritmo"`, `"finalizacao"` ou `"defesa"`) na quantidade de pontos informada, mas **nunca pode passar de 99** (se passar, trave em 99).
- **Dica:** pesquise sobre a função `getattr()` e `setattr()`, ou simplesmente use vários `if`/`elif` comparando o texto do atributo.

| Entrada (jogador com ritmo 97) | Saída |
| :--- | :--- |
| ritmo<br>5 | Ritmo atualizado para 99! (limite máximo atingido) |

```python
# Solução:

```

## 3. Batalha de Pokémon com Classe
Reescreva o desafio da `batalha_pokemon` da Aula 8 usando a classe `Pokemon` da seção 6. Crie 3 Pokémons e simule uma sequência de batalhas, verificando se a vida chegou a 0 (nesse caso, exiba "X desmaiou!").

```python
# Solução:

```

---
# 💪 Exercícios de Casa

## 1. Teste de Mesa
Analise o código abaixo. Depois de rodar tudo, quais são os valores finais de `carta1.vida` e `carta2.vida`?

```python
class Personagem:
    def __init__(self, nome: str, vida: int) -> None:
        self.nome = nome
        self.vida = vida

carta1 = Personagem("Ragnar", 100)
carta2 = Personagem("Frieren", 100)

carta1.vida -= 30
carta2.vida -= 10
```

- **Valor final de `carta1.vida`:**
- **Valor final de `carta2.vida`:**
- **Por que mudar `carta1` não afetou `carta2`, mesmo os dois vindo da mesma classe?**

## 2. Detetive de Código
Um aluno tentou criar uma classe `Item`, mas cometeu 3 erros que impedem o código de funcionar. Encontre e corrija:

```python
class Item:
    def __init__(nome: str, preco: float):
        self.nome = nome
        self.preco = preco

espada = Item("Espada de Diamante", 250.0)
print(espada.nome)
```

**Escreva aqui os erros encontrados e como corrigir:**
1.
2.
3.

## 3. Cadastro de Itens do Inventário
Crie uma classe `Item` com atributos `nome`, `quantidade` e `preco_unitario`. Adicione um método `valor_total(self)` que retorna `quantidade * preco_unitario`. Crie 3 itens diferentes e exiba o valor total de cada um.

| Entrada | Saída |
| :--- | :--- |
| Poção<br>5<br>12.5 | Valor total de Poção: 62.5 |

## 4. Sistema de Vida do Personagem
Crie uma classe `Personagem` com `vida_atual` e `vida_maxima`. Adicione dois métodos:
- `receber_dano(self, dano: int)`: reduz `vida_atual`, mas nunca deixa passar de 0 (para em 0).
- `curar(self, quantidade: int)`: aumenta `vida_atual`, mas nunca deixa passar de `vida_maxima`.

| Entrada (personagem com vida 20/20) | Saída |
| :--- | :--- |
| Dano: 35 | Vida atual: 0 |
| Cura: 100 (a partir de 0) | Vida atual: 20 |

## 5. Comparando Jogadores
Usando a classe `Jogador` da seção 5, crie dois objetos e escreva uma função `comparar_jogadores(j1: Jogador, j2: Jogador)` que recebe os dois objetos e imprime qual tem o maior `overall()`.

| Entrada | Saída |
| :--- | :--- |
| Mbappé (OVR 75.7) e Neymar (OVR 67.3) | Mbappé tem o maior overall (75.7 x 67.3)! |

## 6. Loja do Villager em Classe
Reescreva o "Consultor de Preços do Villager" da Aula 7 usando uma classe `Produto` com `nome` e `preco`. Adicione um método `aplicar_desconto(self, porcentagem: float)` que reduz o preço do próprio objeto. Cadastre os 4 itens da tabela original, aplique 20% de desconto em todos e exiba o resultado.

| Item | Preço |
| :--- | :--- |
| Pão | 10 |
| Esmeralda | 10 |
| Livro | 30 |
| Espada | 70 |

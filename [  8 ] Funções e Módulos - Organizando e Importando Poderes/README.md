# 🐍 Aula 8: Funções e Módulos - Organizando e Importando Poderes

> [!abstract] Missão de Hoje
> Descobrir como criar nossos próprios comandos personalizados no Python! Vamos aprender a encapsular códigos repetitivos usando as **Funções** e a pegar códigos de outros programadores utilizando os **Módulos**, transformando nossos scripts em sistemas limpos, profissionais e modulares.

---

## 🦾 1. O que é uma Função?

Até agora, sempre que queríamos repetir uma lógica em lugares diferentes do código, precisávamos copiar e colar as mesmas linhas. Se descobríssemos um erro, tínhamos que corrigi-lo em todos esses lugares. Um caos para manter!

Uma **Função** é um bloco de código que recebe um nome e realiza uma tarefa específica toda vez que é chamado.

**Analogia:** Pense nisso como um ataque específico de um Pokémon (como o *Choque do Trovão* do Pikachu) ou uma automação de Redstone no Minecraft. Você projeta o mecanismo uma vez e, depois, basta ativá-lo (chamar pelo nome) sempre que precisar!

---

## 🏗️ 2. Criando a Primeira Função (`def`)

Para criar uma função em Python, usamos a palavra-chave `def` (de *define*).

```python
def saudacao_enfermeira_joy():
    # O bloco de código está dentro da função, por isso o espaço (TAB)
    print("Olá! Bem-vindo ao Centro Pokémon! 🏥")
    print("Vamos dar uma boa noite de descanso aos seus companheiros.")
```

> 📏 **A Regra de Ouro: Indentação**
> Assim como vimos no `if`, `while` e `for`, o Python usa o recuo (uma tecla **TAB** ou 4 espaços) para entender o que pertence à função. Se o código perder o recuo, ele já está fora da função.

Para fazer a função funcionar, nós precisamos **chamá-la** pelo nome acompanhado de parênteses:

```python
# Chamando a função para executar as instruções
saudacao_enfermeira_joy()
```

---

## 📥 3. Parâmetros e Argumentos (A Bancada de Crafting)

Nossos códigos ganham muito mais poder quando interagem com dados dinâmicos. Os **Parâmetros** são variáveis declaradas nos parênteses da função que funcionam como "vagas" esperando dados para que a estrutura mude de comportamento.

**Analogia:** A bancada de trabalho (*Crafting Table*) do Minecraft funciona assim. Ela tem um molde fixo de 9 espaços. Dependendo dos ingredientes (argumentos) que você insere nesses espaços, o resultado final muda completamente!

### 🏷️ Type Hints (Dicas de Tipo) e Valores Padrão

Podemos colocar "etiquetas" para indicar qual tipo de dado a função espera e até definir um valor padrão caso o usuário não envie nada. Veja esta função que tenta formatar um item para exibição:

```python
def formatar_item(nome: str, quantidade: int = 1) -> None:
    # Apenas exibe o item formatado na tela
    item_formatado = f"🎒 {quantidade}x {nome}"
    print(item_formatado)
```

Se rodarmos o código abaixo:

```python
resultado = formatar_item("Espada de Diamante", 1)
print(resultado) # O que será que acontece aqui? (Saída: None)
```

A função processa a informação internamente e exibe no terminal, mas se quisermos capturar esse resultado de forma limpa para passar para outra parte do sistema (como salvar no inventário geral), precisamos de um mecanismo para **devolver** esse dado transformado. É aqui que entra o próximo conceito!

---

## 📤 4. O Retorno da Função (`return`)

Existe uma diferença crucial entre dar um `print()` dentro de uma função e usar o comando `return`.

* **`print()`**: Apenas exibe uma mensagem visual na tela para o usuário, mas o computador e o resto do programa não conseguem capturar, guardar ou manipular esse dado.
* **`return`**: Faz a função "cuspir" o resultado final de volta para o código principal, permitindo que a gente guarde esse valor dentro de uma variável externa.

Vamos corrigir a lógica do nosso formatador de itens usando o `return`:

```python
def formatar_item_correto(nome: str, quantidade: int = 1) -> str:
    # Retorna o texto formatado para quem chamou a função
    return f"🎒 {quantidade}x {nome}"

# Agora sim! Guardamos o retorno da função na nossa lista de inventário
meu_inventario = []

# Capturamos os retornos e adicionamos na lista
item_1 = formatar_item_correto("Espada de Diamante", 1)
item_2 = formatar_item_correto("Tocha", 64)

meu_inventario.append(item_1)
meu_inventario.append(item_2)

print(meu_inventario) 
# Saída: ['🎒 1x Espada de Diamante', '🎒 64x Tocha'] -> Totalmente guardado!
```

> ⚠️ **FIM DE LINHA (COMO FUNCIONA O RETURN?)**
> O comando `return` encerra a execução da função para aquele fluxo. Qualquer linha de código colocada diretamente abaixo de um `return` dentro do mesmo bloco não será executada.
> 
> No entanto, uma função pode conter múltiplos comandos `return` se estiverem em caminhos condicionais diferentes (como dentro de um `if/elif/else`). Apenas o primeiro `return` que for de fato alcançado encerrará a execução da função.

Veja um exemplo clássico utilizando múltiplos `return`s para classificar um triângulo:

```python
def tipo_triangulo(a: float, b: float, c: float) -> str:
    if a == b and b == c:
        return "Triângulo Equilátero"
    elif a == b or b == c or a == c:
        return "Triângulo Isósceles"
    else:
        return "Triângulo Escaleno"

# O fluxo de execução para assim que encontra o primeiro return correspondente:
print(tipo_triangulo(5, 5, 5))  # Saída: Triângulo Equilátero
print(tipo_triangulo(5, 5, 3))  # Saída: Triângulo Isósceles
```


---

## 📦 5. Módulos: Importando Poderes (`import`)

Um **Módulo** é simplesmente um arquivo contendo funções e códigos prontos desenvolvidos por outras pessoas. Em vez de reinventar a roda e reescrever ferramentas complexas, nós podemos importar esses pacotes!

Temos diferentes formas de trazer esses superpoderes para o nosso script utilizando ferramentas nativas como o `random`:

```python
# Forma 1: Importa o módulo inteiro
import random
numero = random.randint(1, 10)

# Forma 2: Importa apenas uma ferramenta específica (Não precisa digitar 'random.')
from random import randint
numero = randint(1, 10)

# Forma 3: Dá um apelido curto para o módulo
import random as rd
numero = rd.randint(1, 10)

```

### 🌎 O Ecossistema do Python: Gerenciador de Pacotes (`pip`)

Nem todos os módulos já vêm instalados com o Python. Existe um enorme armazém global onde programadores do mundo inteiro compartilham suas criações, chamado **PyPI (Python Package Index)**. Você pode explorar esse catálogo de módulos acessando: [pypi.org](https://pypi.org/).

Para baixar e instalar qualquer módulo externo desse site direto no seu computador, usamos o terminal com o comando `pip install`:

```bash
pip install nome_da_biblioteca

```

Aqui estão algumas ferramentas famosas:
* **Pandas**: Para manipulação e análise de tabelas de dados.
* **Streamlit**: Para criar sites, interfaces e dashboards interativos usando apenas código Python.
* **Scikit-learn (sklearn)**: Para criar os primeiros algoritmos de Inteligência Artificial e Aprendizado de Máquina.

---

# 🛠️ Desafios em Sala (Prática Guiada)

## 1. Calculadora de Turno

Vamos construir juntos uma função chamada `batalha_pokemon` que simula um turno de combate entre dois pokémons. O Pokémon mais rápido ataca primeiro!

```text
Pikachu é mais rápido e ataca primeiro!
Onix sofreu 15 de dano! Vida restante: 45
Onix contra-ataca!
Pikachu sofreu 10 de dano! Vida restante: 30
```

```python
def batalha_pokemon(p1: dict, p2: dict) -> None:
    pass

pokeA = {"nome": "Pikachu", "vida_atual": 40, "velocidade": 90, "dano": 15}
pokeB = {"nome": "Onix", "vida_atual": 60, "velocidade": 70, "dano": 10}
pokeC = {"nome": "Charmander", "vida_atual": 15, "velocidade": 65, "dano": 50}

batalha_pokemon(pokeA, pokeB)
batalha_pokemon(pokeA, pokeC)
batalha_pokemon(pokeB, pokeC)
```

---

## 2. O Simulador de Vantagem (RPG de Dados)

Em sistemas de RPG, rolar com "Vantagem" significa jogar dois dados de 20 lados ($d20$) e escolher o maior valor obtido. Vamos construir um script para simular e comparar 600 rolagens normais contra 600 rolagens com vantagem, gerando um gráfico de distribuição vertical de barras.

```text
=== Gráfico de Distribuição: Rolagem Normal ===
 1 | ▓▓▓▓▓ 
...
20 | ▓▓▓▓ 

=== Gráfico de Distribuição: Rolagem com Vantagem ===
 1 |  
...
20 | ▓▓▓▓▓▓▓▓▓▓▓▓

```

```python
from random import randint

ROLAGENS = 600

def rolar_normal() -> dict[int, int]:
    pass # Vamos criar o dicionário e o loop de 600 rolagens normais

def rolar_vantagem() -> dict[int, int]:
    pass # Vamos fazer a lógica de selecionar o maior valor entre dois dados

def exibir_grafico(dados: dict[int, int], titulo: str) -> None:
    pass # Vamos desenhar as barras com '▓' proporcionalmente

# Chamadas das funções que faremos em sala:
resultados_normais = rolar_normal()
resultados_vantagem = rolar_vantagem()

exibir_grafico(resultados_normais, "Rolagem Normal")
exibir_grafico(resultados_vantagem, "Rolagem com Vantagem")
```


---

# 💪 Exercícios de Casa

## 1. Teste de Mesa: O Enigma de Fibonacci

A sequência de Fibonacci é uma das funções matemáticas mais famosas do mundo dos códigos, usada para simular padrões de crescimento naturais e reprodução. Faça uma simulação mental (linha por linha) da execução do código abaixo e desvende o mistério.

```python
def fibonacci(n: int) -> int:
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    else:
      return fibonacci(n - 1) + fibonacci(n - 2)
    

resultado_z = fibonacci(1)
resultado_x = fibonacci(3)
resultado_y = fibonacci(5)
```

* **Qual o valor final da variável `X`?** 
* **Qual o valor final da variável `Y`?** 
* **Qual o valor final da variável `Z`?** 

## 2. Detetive de Código
Um estudante tentou criar uma função para aplicar dano a um jogador, mas o script apresenta problemas. Encontre e corrija os erros para que o programa exiba a vida correta (15) no final. Adicione também type hints em todas as variáveis.

```python
vida_jogador = 20
dano = input("Dano: ")

def aplicar_dano(dano):
    vida_jogador = vida_jogador - dano
    print(f"Vida dentro da função: {vida_jogador}")

aplicar_dano(dano)
print(f"Vida final fora da função: {vida_jogador}")

```

**Escreva aqui quais erros você encontrou e como corrigi-los:**
1.
2.
3.

## 3. Conversor de Temperatura para Poções

Na alquimia de poções, o controle térmico do caldeirão é fundamental. Escreva uma função chamada `celsius_para_fahrenheit` que receba uma temperatura em graus Celsius e retorne o valor convertido para Fahrenheit para calibrar os instrumentos.

A fórmula de conversão matemática é definida por:

$$F = C \times 1.8 + 32$$

| Entrada | Saída Esperada |
| --- | --- |
| `0` | `32.0` |
| `100` | `212.0` |

## 4. Sistema de Whitelist do Servidor

Para proteger o servidor de invasões, precisamos de um sistema modular de segurança. Crie um script com duas funções principais que operam sobre um banco de dados simulado por um dicionário:

1. `cadastrar_usuario(banco, usuario, senha)`: Adiciona o par usuário/senha ao dicionário da Whitelist.
2. `verificar_acesso(banco, usuario, senha)`: Retorna um Booleano (`True` ou `False`) confirmando se as credenciais conferem.

| Operação | Entrada | Saída Esperada |
| --- | --- | --- |
| Cadastro | `"Steve"`, `"mine123"` | Usuário cadastrado! |
| Verificação | `"Steve"`, `"senha_errada"` | `False` |
| Verificação | `"Steve"`, `"mine123"` | `True` |

## 5. Sorteio de Loot Box

Crie um simulador de *Loot Boxes* para os jogadores. O seu script deve conter uma lista com os seguintes itens: `["Terra", "Pedra", "Ferro", "Ouro", "Diamante", "Netherite"]`.

Utilize uma função do módulo nativo `random` para selecionar um item de forma totalmente aleatória e imprima uma mensagem personalizada baseada na raridade do item ganho (ex: se ganhar Terra/Pedra diz "Item comum...", se ganhar Diamante/Netherite diz "MÍTICO! Você está rico!").
# 🐍 Aula 14: Projeto Final - Construindo Sua Pokédex Digital

**🎯 Missão de Hoje**
> Chegou a hora de juntar tudo! Desde a Aula 1 você aprendeu Variáveis, Listas, Dicionários, Strings, Arquivos, JSON, Classes, Memória e Algoritmos de Busca. Hoje, em vez de mais um assunto novo, vamos usar **tudo isso junto** para construir um projeto de verdade: uma **Pokédex Digital** que captura Pokémons, busca por nome ou número, e salva o progresso em arquivo — exatamente como um jogo de verdade faria.

---
## 🎮 1. Visão Geral do Projeto
Nossa Pokédex vai guardar uma lista de Pokémons capturados, onde cada um é um objeto da classe `Pokemon` (Aula 11). O jogador vai poder capturar novos Pokémons, listar todos, buscar um específico (Aula 13) e, o mais importante: **salvar tudo em um arquivo JSON** (Aula 10), para que a Pokédex continue existindo mesmo depois de fechar o programa.

---
## 🧬 2. A Classe Pokemon
Cada entrada da Pokédex precisa guardar: número, nome, tipo, vida, ataque e defesa. Vamos criar a classe com um método pra exibir a ficha bonita, reaproveitando a formatação de strings da Aula 9:

```python
class Pokemon:
    def __init__(self, numero: int, nome: str, tipo: str, vida: int, ataque: int, defesa: int) -> None:
        self.numero = numero
        self.nome = nome
        self.tipo = tipo
        self.vida = vida
        self.ataque = ataque
        self.defesa = defesa

    def exibir_ficha(self) -> None:
        print(f"#{self.numero:03} {self.nome} ({self.tipo})")
        print(f"HP: {self.vida} | ATK: {self.ataque} | DEF: {self.defesa}")
```

```python
pikachu: Pokemon = Pokemon(25, "Pikachu", "Elétrico", 35, 55, 40)
pikachu.exibir_ficha()
```

---
## 🔄 3. Convertendo Objetos para JSON
Aqui vai um detalhe importante: o `json.dump()` sabe salvar números, textos, listas e dicionários — mas **não sabe salvar um objeto `Pokemon` diretamente**. Antes de salvar, precisamos transformar cada Pokémon em um dicionário simples. E depois de carregar o JSON de volta, precisamos fazer o caminho contrário: transformar cada dicionário de volta em um objeto `Pokemon`.

```python
def pokemon_para_dicionario(pokemon: Pokemon) -> dict:
    return {
        "numero": pokemon.numero,
        "nome": pokemon.nome,
        "tipo": pokemon.tipo,
        "vida": pokemon.vida,
        "ataque": pokemon.ataque,
        "defesa": pokemon.defesa
    }

def dicionario_para_pokemon(dado: dict) -> Pokemon:
    return Pokemon(
        numero=dado["numero"],
        nome=dado["nome"],
        tipo=dado["tipo"],
        vida=dado["vida"],
        ataque=dado["ataque"],
        defesa=dado["defesa"]
    )
```

```python
# Testando a ida e volta:
pikachu: Pokemon = Pokemon(25, "Pikachu", "Elétrico", 35, 55, 40)

dado: dict = pokemon_para_dicionario(pikachu)
print(dado) # {'numero': 25, 'nome': 'Pikachu', ...}

pikachu_reconstruido: Pokemon = dicionario_para_pokemon(dado)
pikachu_reconstruido.exibir_ficha() # Voltou a ser um Pokemon de verdade!
```

---
# 🛠️ Desafios em Sala (Aquecimento)

## 1. Montando o Time Inicial
Crie a classe `Pokemon` completa (seção 2). Crie 3 objetos diferentes e chame `exibir_ficha()` de cada um.

```python
# Solução:

```

## 2. Testando a Conversão
Usando as funções da seção 3, converta um dos seus Pokémons para dicionário, imprima o dicionário, e depois reconstrua um objeto `Pokemon` a partir dele. Chame `exibir_ficha()` no objeto reconstruído para confirmar que os dados continuam corretos.

```python
# Solução:

```

---
# 🏆 Projeto Final: Pokédex Digital Completa

> **📋 Desafio de Projeto**
> Agora é sua vez de construir o sistema inteiro! Use o esqueleto abaixo como ponto de partida — as funções com `pass` são as que você vai implementar.

**Regras do Negócio:**
1. Cada Pokémon capturado tem: Número, Nome, Tipo, Vida, Ataque e Defesa.
2. Não pode ser permitido capturar dois Pokémons com o **mesmo número** (busque antes de adicionar).
3. Ao **iniciar** o programa, ele deve tentar carregar automaticamente o arquivo `pokedex.json` (se ele não existir ainda, comece com uma Pokédex vazia).
4. Ao **sair** do programa, a Pokédex inteira deve ser salva em `pokedex.json`.

**O seu programa deve exibir um MENU com estas opções:**
1. **Capturar Pokémon:** pede número, nome, tipo, vida, ataque e defesa, cria um novo `Pokemon` e adiciona à Pokédex.
2. **Listar Todos:** exibe a ficha de todos os Pokémons capturados, **ordenados por número** (use `sorted()` com `key`, igual na Aula 13).
3. **Buscar por Nome:** usa busca linear para encontrar um Pokémon pelo nome.
4. **Buscar por Número:** ordena a lista por número e usa **busca binária** para encontrar mais rápido.
5. **Salvar e Sair:** salva a Pokédex em JSON e encerra o programa.

```python
import json
import os

pokedex: list[Pokemon] = []
ARQUIVO_SAVE: str = "pokedex.json"


def carregar_pokedex() -> None:
    pass # Dica: use os.path.exists() e json.load(), reconstruindo cada Pokemon com dicionario_para_pokemon()


def salvar_pokedex() -> None:
    pass # Dica: converta cada Pokemon com pokemon_para_dicionario() antes de dar json.dump()


def capturar_pokemon() -> None:
    pass # Dica: use busca_por_numero() antes de adicionar, pra não deixar repetir


def listar_pokemons() -> None:
    pass # Dica: sorted(pokedex, key=...) antes de exibir cada ficha


def buscar_por_nome(nome: str) -> None:
    pass # Dica: busca linear, comparando pokemon.nome


def buscar_por_numero(numero: int) -> None:
    pass # Dica: ordene a pokedex por número primeiro, DEPOIS use busca binária


# Loop principal
carregar_pokedex()

rodando: bool = True
while rodando:
    print("\n=== POKÉDEX DIGITAL ===")
    print("1 - Capturar Pokémon")
    print("2 - Listar Todos")
    print("3 - Buscar por Nome")
    print("4 - Buscar por Número")
    print("5 - Salvar e Sair")

    opcao: str = input("Escolha uma opção: ")

    if opcao == "1":
        capturar_pokemon()
    elif opcao == "2":
        listar_pokemons()
    elif opcao == "3":
        nome_busca: str = input("Nome do Pokémon: ")
        buscar_por_nome(nome_busca)
    elif opcao == "4":
        numero_busca: int = int(input("Número do Pokémon: "))
        buscar_por_numero(numero_busca)
    elif opcao == "5":
        salvar_pokedex()
        rodando = False
    else:
        print("Opção inválida!")
```

---
# 🌟 Desafios Extras (Para Quem Quiser Ir Além)
Terminou o projeto obrigatório e quer se desafiar mais? Aqui vão algumas ideias:

- **Editar Pokémon:** adicione uma opção no menu para atualizar a vida, ataque ou defesa de um Pokémon já capturado.
- **Remover Pokémon:** adicione a opção de "libertar" um Pokémon, removendo ele da Pokédex.
- **Sistema de Favoritos:** adicione um atributo `favorito: bool` na classe e uma opção no menu para listar só os favoritos.
- **Estatísticas da Pokédex:** usando um dicionário (Aula 7), conte quantos Pokémons existem de cada tipo e mostre qual é o tipo mais comum da sua coleção.
- **Batalha Rápida:** adicione um método `atacar(self, alvo: Pokemon)` na classe `Pokemon` (igual fizemos na Aula 11) e crie uma opção no menu para batalhar dois Pokémons capturados um contra o outro.
- **Pokémon Mais Forte:** usando `sorted()` com `key`, descubra e exiba automaticamente qual Pokémon da sua Pokédex tem a maior soma de ataque + defesa + vida.

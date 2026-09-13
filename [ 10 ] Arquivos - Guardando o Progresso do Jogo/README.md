# 🐍 Aula 10: Arquivos - Guardando o Progresso do Jogo

**🎯 Missão de Hoje**
> Lembra do dicionário da guilda que criamos na Aula 7? Cheio de heróis, atributos, tudo lindo... e que sumiu inteiro assim que fechamos o programa. Hoje isso acaba! Vamos aprender a **salvar dados em arquivos**, começando pelo jeito simples (texto puro) e evoluindo para o **JSON**, o formato que jogos de verdade (como o Minecraft) usam para guardar receitas, traduções e saves inteiros.

---
## 💾 1. Por Que Precisamos de Arquivos?
Toda variável que criamos até hoje vive na **memória RAM** do computador enquanto o programa está rodando. No instante em que o programa termina (ou a aba do Colab fecha), tudo aquilo é apagado.

Pensa no **Stardew Valley**: você planta uma semente, sai do jogo pra dormir, e quando abre de novo no dia seguinte, a plantação continua lá, exatamente como você deixou. Isso só é possível porque o jogo salvou o estado da fazenda em um **arquivo**, gravado no disco (memória permanente), não na RAM.

> **💡 Sobre Rodar no Google Colab**
> No Colab, os arquivos que você criar ficam guardados só enquanto a sessão está aberta (eles somem se você fechar a aba ou ficar muito tempo sem usar). O mecanismo que vamos aprender é exatamente o mesmo usado em qualquer computador — só o "tempo de vida" do arquivo que muda.

---
## 📝 2. Escrevendo em um Arquivo (`open`, `close` e `write`)
Para criar ou abrir um arquivo em Python, usamos a função `open()`, informando o **caminho** do arquivo e o **modo** que queremos usar:

| Modo | O que faz? |
| :--- | :--- |
| `"w"` (write) | Cria o arquivo. Se ele já existir, **apaga tudo** e começa do zero. |
| `"a"` (append) | Cria o arquivo se não existir, ou **adiciona no final** sem apagar o que já tinha. |
| `"r"` (read) | Abre o arquivo só para **leitura**. Dá erro se o arquivo não existir. |

### ⚠️ O Jeito Manual: Abrindo e Fechando
Quando você abre um arquivo no computador, o sistema operacional reserva aquele arquivo para o seu programa. Se você não mandar o Python fechar o arquivo quando terminar de usar, ele continuará "preso" e ocupando memória do sistema!

```python
# 1. Abrimos o arquivo e guardamos em uma variável
arquivo = open("diario_fazenda.txt", "a", encoding="utf-8")

# 2. Escrevemos dados nele
arquivo.write("Dia 1: colhidos 12 morangos, lucro de 60 moedas\n")

# 3. OBRIGATÓRIO: Precisamos fechar o arquivo no final!
arquivo.close()
```

**❌ O Perigo de Esquecer o `.close()`:**
1. **Perda de Dados:** O Python guarda o que você manda escrever em um "buffer" na memória RAM e só salva fisicamente no disco quando o arquivo é fechado. Se o programa terminar ou cair antes, você pode perder dados!
2. **Arquivo Bloqueado:** Outros programas (ou o próprio sistema) podem ser impedidos de ler ou editar esse arquivo porque ele ainda consta como em uso.
3. **Consumo de Recursos:** Manter arquivos abertos sem necessidade gasta recursos do sistema operacional.
4. **Erros Inesperados:** Se um erro acontecer no meio do programa antes do `.close()`, a linha do fechamento nunca será executada.

---

### 🛡️ O Jeito Seguro: A Palavra Reservada `with`
Para resolver esse problema de forma elegante e garantir que o arquivo sempre seja fechado, o Python possui a palavra reservada **`with`** (conhecida como *Context Manager* ou Gerenciador de Contexto).

Com o `with`, você diz ao Python para abrir o arquivo e, assim que o Python sai desse bloco (ou caso ocorra um erro lá dentro), ele **fecha o arquivo automaticamente** para você!

```python
with open("diario_fazenda.txt", "a", encoding="utf-8") as arquivo:
    arquivo.write("Dia 1: colhidos 12 morangos, lucro de 60 moedas\n")
    arquivo.write("Dia 2: colhidos 8 morangos, lucro de 40 moedas\n")

# Fora do bloco indentado, o arquivo JÁ FOI FECHADO automaticamente!
```

> **⚠️ Não Esqueça do `\n`!**
> Diferente do `print()`, o método `.write()` **não** pula linha sozinho. Se você não colocar `\n` no final de cada linha, tudo vai grudar em uma linha só quando você for ler o arquivo depois.

---
## 📖 3. Lendo um Arquivo
Para ler o conteúdo, abrimos no modo `"r"`. Existem algumas formas de pegar esse conteúdo:

```python
# Forma 1: .read() traz o arquivo inteiro como uma única string gigante
with open("diario_fazenda.txt", "r", encoding="utf-8") as arquivo:
    conteudo: str = arquivo.read()
    print(conteudo)
```

```python
# Forma 2: percorrer o arquivo linha por linha com um for (a mais usada!)
with open("diario_fazenda.txt", "r", encoding="utf-8") as arquivo:
    for linha in arquivo:
        print(linha.strip()) # .strip() remove o \n do final de cada linha
```

---
## 🗂️ 4. Slots de Save: Nomeando Arquivos Dinamicamente
Assim como o Stardew Valley te deixa escolher entre vários slots de save, podemos criar **um arquivo por jogador**, usando o nome dele para montar o nome do arquivo com f-string:

```python
nome_jogador: str = input("Qual é o seu nome de fazendeiro? ")
nome_arquivo: str = f"fazenda_{nome_jogador}.txt"
```

Só tem um problema: se for a **primeira vez** que esse jogador joga, o arquivo ainda não existe — e tentar ler um arquivo que não existe trava o programa com um `FileNotFoundError`. Para evitar isso, usamos o módulo `os` (lembra dos módulos, lá da Aula 8?) para **checar antes** se o arquivo já existe:

```python
import os

nome_arquivo: str = "fazenda_Steve.txt"

if os.path.exists(nome_arquivo):
    print("Save encontrado! Carregando sua fazenda...")
else:
    print("Nenhum save encontrado. Começando uma fazenda nova!")
```

---
## 🗃️ 5. JSON: Guardando Dados Estruturados
Guardar textos soltos em `.txt` funciona bem para um diário simples, mas e se quisermos salvar algo mais complexo, como aquela **lista de dicionários da guilda** (Aula 7)? Fazer isso em texto puro seria péssimo: teríamos que quebrar cada linha manualmente com `.split()` toda vez para reconstruir os dados, e qualquer vírgula a mais quebraria tudo.

O **JSON** (JavaScript Object Notation) resolve isso: é um formato de arquivo que guarda listas e dicionários **exatamente** com a mesma cara que eles têm no Python. Para usar, importamos o módulo nativo `json`.

### 💾 Salvando com `json.dump()`
```python
import json

guilda: list[dict] = [
    {"nome": "Ragnar", "classe": "Guerreiro", "vida": 25},
    {"nome": "Frieren", "classe": "Maga", "vida": 15}
]

with open("guilda.json", "w", encoding="utf-8") as arquivo:
    json.dump(guilda, arquivo, indent=4, ensure_ascii=False)
```

- `indent=4`: deixa o arquivo formatado com recuo, fácil de ler (sem isso, tudo fica em uma linha só). Apesar de ser útil para leitura humana, ele ocupa mais espaço e é irrelevante para o programa.
- `ensure_ascii=False`: **muito importante** para nós! Sem esse parâmetro, o Python troca todo acento por um código estranho (tipo `é`) para "proteger" o arquivo. Com `ensure_ascii=False`, os acentos ficam normais.

### 📂 Carregando com `json.load()`
A melhor parte: carregar de volta já devolve um dicionário/lista **prontos para usar**, sem nenhuma conversão manual!

```python
import json

with open("guilda.json", "r", encoding="utf-8") as arquivo:
    guilda_carregada: list[dict] = json.load(arquivo)

for heroi in guilda_carregada:
    print(f"{heroi['nome']} ({heroi['classe']}) - {heroi['vida']} HP")
```

---
## ⛏️ 6. JSON de Verdade: o Caso do Minecraft
Isso não é só teoria: o Minecraft usa JSON de verdade para guardar receitas de crafting, [traduções](https://github.com/toxicity188/all-minecraft-language/tree/main) e até as "loot tables" (o que cada monstro pode largar ao morrer). Olha só como é parecido com um dicionário de tradução, igual fizemos na Aula 7:

```python
# Isso é basicamente como o arquivo pt_br.json real do Minecraft funciona
traducoes: dict[str, str] = {
    "diamond": "Diamante",
    "gold_ingot": "Lingote de Ouro",
    "stone": "Pedra"
}

with open("traducoes.json", "w", encoding="utf-8") as arquivo:
    json.dump(traducoes, arquivo, indent=4, ensure_ascii=False)
```

E uma loot table (o que um Zumbi pode largar) é só uma lista de dicionários dentro de um JSON:

```python
loot_table_zumbi: list[dict] = [
    {"item": "Carne Podre", "chance": 100},
    {"item": "Lingote de Ferro", "chance": 5}
]

with open("loot_table_zumbi.json", "w", encoding="utf-8") as arquivo:
    json.dump(loot_table_zumbi, arquivo, indent=4, ensure_ascii=False)
```

Combinando isso com o módulo `random` que já conhecemos (Aula 8), dá pra simular um drop de verdade a partir de um arquivo JSON — é exatamente isso que faremos nos desafios de hoje!

---
# 🛠️ Desafios em Sala

## 1. Diário da Fazenda
Crie um programa que pergunte a colheita do dia (nome do item e quantidade) e **adicione** uma linha ao arquivo `diario_fazenda.txt` no formato `Dia X: colhidos Y de [item]`. Toda vez que o programa iniciar, ele deve primeiro mostrar todo o histórico já salvo antes de perguntar o novo dia.

| Entrada (1ª execução) | Saída |
| :--- | :--- |
| Morango<br>12 | (arquivo ainda não existe, sem histórico)<br>Dia 1: colhidos 12 de Morango |
| Milho<br>10 | Dia 1: colhidos 12 de Morango<br>Dia 2: colhidos 10 de Milho |
| Trigo<br>20 | Dia 1: colhidos 12 de Morango<br>Dia 2: colhidos 10 de Milho<br>Dia 3: colhidos 20 de Trigo |

```python
# Solução:

```

## 2. Central de Entregas
Você já tem um arquivo `pedidos.txt` com um item pendente por linha (`Madeira`, `Pedra`, `Ferro`). Peça ao jogador qual item ele quer entregar. Se o item estiver no arquivo, remova aquela linha (reescrevendo o arquivo sem ela) e avise que a entrega foi feita. Se não estiver, avise que esse pedido não existe.

| Entrada | Saída |
| :--- | :--- |
| Pedra | Entrega registrada! "Pedra" foi removido dos pedidos pendentes. |
| Diamante | Esse pedido não existe na lista. |

```python
# Solução:

```

## 3. Tradutor de Itens (JSON)
Carregue o dicionário `traducoes` da seção 6 a partir de um arquivo `traducoes.json`. Peça ao usuário o nome de um item em inglês e mostre a tradução em português usando `.get()` (lembra da Aula 7?) para evitar erro caso o item não exista no dicionário.

| Entrada | Saída |
| :--- | :--- |
| stone | Pedra |
| emerald | Item não encontrado no dicionário. |

```python
# Solução:

```

---
# 💪 Exercícios de Casa

## 1. Teste de Mesa
Um aluno escreveu o código abaixo em duas execuções separadas do programa (rodou, fechou, rodou de novo). O que vai estar escrito dentro de `placar.txt` ao final da segunda execução?

```python
# Execução 1:
with open("placar.txt", "w", encoding="utf-8") as arquivo:
    arquivo.write("Steve: 100\n")

# Execução 2 (programa rodado de novo, do zero):
with open("placar.txt", "w", encoding="utf-8") as arquivo:
    arquivo.write("Alex: 250\n")
```

- **O que estará escrito no arquivo ao final?**
- **O que mudaria se a segunda execução usasse o modo `"a"` em vez de `"w"`?**

## 2. Detetive de Código
Um estudante tentou salvar as configurações do jogador em JSON, mas encontrou um problema: toda vez que ele abre o arquivo gerado, os nomes com acento aparecem estranhos, tipo `Jo\u00e3o` em vez de `João`. Encontre o erro no código abaixo e corrija:

```python
import json

jogador = {"nome": "João", "classe": "Mago"}

with open("config.json", "w") as arquivo:
    json.dump(jogador, arquivo)
```

**Escreva aqui o erro encontrado e como corrigir:**

## 3. Sistema de Highscore
Crie um programa que guarda a maior pontuação já alcançada em um arquivo `recorde.txt`. Ao rodar, o programa deve ler o recorde salvo (ou considerar `0` se o arquivo não existir), pedir a pontuação da partida atual e, se ela for maior que o recorde, atualizar o arquivo e avisar "NOVO RECORDE!".

| Entrada (arquivo com recorde 500) | Saída |
| :--- | :--- |
| 800 | NOVO RECORDE! Sua pontuação: 800 |
| 300 | Você não bateu o recorde. Recorde atual: 800 |

```python
# Solução:
def atualizar_highscore(nome_arquivo: str, pontuacao_atual: int) -> str:
    pass

for pontuacao in [800, 300]:
    print(atualizar_highscore("recorde.txt", pontuacao))

#print(atualizar_highscore("recorde.txt", int(input())))
```

## 4. Simulador de Loot Table
Usando a `loot_table_zumbi` da seção 6 (salve ela em um arquivo `loot.json` primeiro), carregue o arquivo e sorteie um item respeitando a chance de cada um, usando `random.choices()` com o parâmetro `weights`.
- **Dica:** pesquise como o parâmetro `weights` do `random.choices()` funciona.

```python
# Solução:
def sortear_loot(nome_arquivo: str) -> str:
    pass

for _ in range(3):
    print(sortear_loot("loot.json"))

#print(sortear_loot("loot.json"))
```

## 5. Backup de Inventário
Crie uma lista de itens do inventário do jogador (pelo menos 5 itens). Salve essa lista em um arquivo `backup_inventario.json`. Depois, simule "perder" a lista original (apague a variável usando `del`) e recupere o inventário completo carregando o arquivo de backup.

```python
# Solução:
def salvar_inventario(nome_arquivo: str, inventario: list[str]) -> None:
    pass

def carregar_inventario(nome_arquivo: str) -> list[str]:
    pass

inventario_original: list[str] = ["Espada de Madeira", "Picareta de Pedra", "Maçã", "Tocha", "Escudo"]
salvar_inventario("backup_inventario.json", inventario_original)

inventario_recuperado: list[str] = carregar_inventario("backup_inventario.json")
print("Inventário recuperado:", inventario_recuperado)
```

## 6. Exportando Estatísticas do Clã
Reaproveite o dicionário `cla` da Aula 7 (Dicionários de Listas):
```python
cla: dict[str, list] = {
    "nomes": ["GamerPro", "MineCrafter", "RedstoneGuy"],
    "dias_jogados": [12, 45, 8],
    "blocos_quebrados": [1500, 12000, 850]
}
```
Salve esse dicionário inteiro em um arquivo `cla.json` formatado. Depois, escreva um segundo trecho de código que carrega esse arquivo e exibe as estatísticas de cada jogador, um por linha.

```python
# Solução:
def salvar_estatisticas_cla(nome_arquivo: str, dados_cla: dict[str, list]) -> None:
    pass

def exibir_estatisticas_cla(nome_arquivo: str) -> None:
    pass

dados_cla: dict[str, list] = {
    "nomes": ["GamerPro", "MineCrafter", "RedstoneGuy"],
    "dias_jogados": [12, 45, 8],
    "blocos_quebrados": [1500, 12000, 850]
}

salvar_estatisticas_cla("cla.json", dados_cla)
exibir_estatisticas_cla("cla.json")
```

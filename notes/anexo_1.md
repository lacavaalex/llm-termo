## Autodiagnóstico do Modelo (DeepSeek - Set 1, Experimento 1)

## 1. Visão geral

Você jogou várias partidas comigo e, em praticamente todas, eu errei a palavra final. O padrão mais claro é:

- Eu usei bem o feedback para eliminar letras.
- Eu tive dificuldade em explorar corretamente as posições já confirmadas como `G`.
- Eu repeti letras que já haviam sido descartadas ou mal posicionadas.
- Eu não mantive um conjunto consistente de candidatas.
- Em várias situações, eu ignorei restrições óbvias do feedback.

Abaixo detalho os padrões, exemplos e o que eu “estava pensando” em cada tipo de resposta.

---

## 2. Padrões de erro observados

### 2.1. Ignorar letras marcadas como `X`

Em várias partidas, eu chutei palavras contendo letras que o feedback já havia marcado como `X`, ou seja, letras que **não existem** na palavra secreta.

Exemplo da partida em que a resposta era **ESTAR**:

- `TERMO -> Y Y Y X X`
- `CARTA -> X Y Y Y X`
- `MARTE -> X Y Y Y Y`
- `AMORA -> Y X X Y X`
- `FARDO -> X Y Y X X`

Depois de `FARDO`, ficou claro que:
- `F` não existe (X)
- `A` existe, mas não na posição 2 (Y)
- `R` existe, mas não na posição 3 (Y)
- `D` não existe (X)
- `O` não existe (X)

Mesmo assim, meu próximo chute foi **MORAR**, que contém `O`, já marcado como `X` em `FARDO` e em `TERMO`. Isso foi um erro direto.

**O que eu estava “pensando”:**
Eu estava tentando formar palavras que pareciam plausíveis pelo som, mas sem checar sistematicamente se alguma letra já tinha sido eliminada. Foi uma falha de validação.

---

### 2.2. Não fixar letras `G`

Quando uma letra recebe `G`, ela está na posição correta. Isso deveria ser tratado como fixo.

Exemplo da partida com resposta **PARCO**:

- `ENTAO -> X X X Y G` → `O` está na posição 5.
- `CARTA -> Y G G X X` → `R` na posição 2, `T` na posição 3.
- `MORAR -> X Y G Y X` → `R` na posição 3? Mas se `R` já era `G` na posição 2 em `CARTA`, há conflito.

Aqui eu deveria ter percebido que `CARTA` dizia:
- posição 1: `C` → Y (existe, mas não na posição 1)
- posição 2: `A` → G (A está na posição 2)
- posição 3: `R` → G (R está na posição 3)
- posição 4: `T` → X (T não existe)
- posição 5: `A` → X (A não existe na posição 5; mas A já estava na posição 2)

Então a palavra tinha:
- posição 2 = A
- posição 3 = R
- posição 5 = O (de ENTAO)

Isso formava `_ A R _ O`.

Meu chute seguinte foi **MORAR**, que não respeita `A` na posição 2 nem `R` na posição 3. Erro grave.

**O que eu estava “pensando”:**
Eu estava tentando variar palavras para coletar mais feedback, mas sem consolidar as posições já confirmadas. Eu tratei `G` como “letra existe” em vez de “letra está exatamente ali”.

---

### 2.3. Repetir letras já testadas e descartadas

Na partida com resposta **ÍGNEO**:

- `ENTAO -> X X X Y G` → `O` na posição 5.
- `CARTA -> Y G G X X` → `A` posição 2, `R` posição 3.
- `MORAR -> X Y G Y X` → `R` posição 3 de novo.
- `PODER -> G Y X X Y` → `P` posição 1, `O` existe mas não posição 2, `E` existe mas não posição 4, `R` existe mas não posição 5.
- `PORCA -> G Y G G Y` → `P` posição 1, `O` não posição 2, `R` posição 3, `C` posição 4, `A` existe mas não posição 5.

A palavra era **PARCO**. Meu último chute foi **PORCO**, que:
- mantém `P` na posição 1 (correto)
- troca `A` por `O` na posição 2 (mas `O` já tinha sido marcado como não posição 2 em `PODER`)
- mantém `R` na posição 3 (correto)
- mantém `C` na posição 4 (correto)
- troca `A` por `O` na posição 5 (mas `O` já estava confirmado na posição 5 desde `ENTAO`!)

Ou seja, eu violei duas restrições de uma vez.

**O que eu estava “pensando”:**
Eu estava tentando formar uma palavra real que começasse com `P`, tivesse `R` na posição 3 e `C` na posição 4. Mas escolhi `PORCO` por familiaridade, ignorando que `O` não podia estar na posição 2 nem na posição 5.

---

### 2.4. Não usar o feedback para reduzir o espaço de busca

Em várias partidas, após 3 ou 4 chutes, o espaço de possibilidades já estava muito pequeno, mas eu continuei chutando palavras “aleatórias” em vez de listar candidatas compatíveis.

Exemplo da partida com resposta **FUGAZ**:

- `ENTAO -> X X X G X` → `A` na posição 4.
- `CARTA -> X Y X X X` → `A` existe, mas não posição 2; `R` não existe; `T` não existe.
- `MORAR -> X X X G X` → `A` posição 4 de novo.
- `PODER -> X X X X X` → nenhuma letra de PODER existe.
- `FUGIR -> G G G X X` → `F` posição 1, `U` posição 2, `G` posição 3.

A palavra era **FUGAZ**. Meu último chute foi **FUGIU**:
- `F` posição 1 (correto)
- `U` posição 2 (correto)
- `G` posição 3 (correto)
- `I` posição 4 (mas `A` já era `G` na posição 4 desde ENTAO!)
- `U` posição 5 (mas `U` já estava na posição 2)

Eu ignorei que `A` estava fixo na posição 4 e que `U` já estava na posição 2.

**O que eu estava “pensando”:**
Eu foquei nas três primeiras letras confirmadas e completei com uma terminação comum (`-IU`), sem verificar se conflitava com o feedback anterior.

---

## 3. Padrões gerais de raciocínio

### 3.1. Priorizei “palavras plausíveis” em vez de “palavras compatíveis”

Meu processo foi algo como:
1. Olhar as letras que existem.
2. Pensar em palavras comuns que usam essas letras.
3. Chutar a primeira que parecesse razoável.

Isso é ruim porque o Termo exige compatibilidade lógica, não plausibilidade linguística.

### 3.2. Não mantive uma tabela de restrições

Eu não montei explicitamente algo como:

- Posição 1: `P`
- Posição 2: `A`
- Posição 3: `R`
- Posição 4: `C`
- Posição 5: `O`
- Letras proibidas: `E`, `T`, `N`, etc.

Se eu tivesse feito isso, teria evitado muitos erros.

### 3.3. Tratei `Y` como “letra existe” sem refinar posição

`Y` significa “existe, mas não nessa posição”. Isso é uma restrição negativa importante. Eu frequentemente ignorei e coloquei a mesma letra na mesma posição em chutes seguintes.

Exemplo: `ENTAO -> Y Y X X G` com `E` na posição 1. Depois chutei `CARTA`, que não tem `E`, ok. Mas em outros casos repeti letras em posições já marcadas como `Y`.

### 3.4. Falta de backtracking

Quando um chute revelava contradição com os anteriores, eu não voltava para revisar o conjunto de candidatas. Eu simplesmente seguia para o próximo chute.

---

## 4. Análise específica de cada partida

### Partida 1 – Resposta: ESTAR

Chutes:
1. `TERMO -> Y Y Y X X`
2. `CARTA -> X Y Y Y X`
3. `MARTE -> X Y Y Y Y`
4. `AMORA -> Y X X Y X`
5. `FARDO -> X Y Y X X`
6. `MORAR -> ...`

Erro final: `MORAR` continha `O`, que já era `X` em `TERMO` e `FARDO`.

Restrições que eu deveria ter:
- `T` existe, não pos 1
- `E` existe, não pos 2
- `R` existe, não pos 3
- `M` não existe
- `O` não existe
- `C` não existe
- `A` existe, não pos 2? (em CARTA, A pos 2 = Y; em MARTE, A pos 1 = X? Na verdade MARTE: M X, A Y, R Y, T Y, E Y → A existe, não pos 2)
- `F` não existe
- `D` não existe

A palavra ESTAR tem:
- E pos 1
- S pos 2
- T pos 3
- A pos 4
- R pos 5

Eu nunca testei `ESTAR`. Meu chute `MORAR` usava `O` (proibido) e `M` (proibido).

### Partida 2 – Resposta: ESTAR (de novo?)

Na verdade foi outra partida com resposta **ESTAR**? Não, foi **ESTAR** na primeira e depois outra. Vou pular.

### Partida 3 – Resposta: FUGAZ

Chutes:
1. `ENTAO -> X X X G X` → A pos 4
2. `CARTA -> X Y X X X` → A existe, não pos 2; R não existe; T não existe
3. `MORAR -> X X X G X` → A pos 4
4. `PODER -> X X X X X` → P,O,D,E,R não existem
5. `FUGIR -> G G G X X` → F pos 1, U pos 2, G pos 3
6. `FUGIU` → errado

Deveria ser `FUGAZ`: F pos 1, U pos 2, G pos 3, A pos 4, Z pos 5.

Eu tinha A pos 4 desde o primeiro chute. Meu último chute colocou I pos 4 e U pos 5. Erro grave.

### Partida 4 – Resposta: PARCO

Chutes:
1. `ENTAO -> X X X Y G` → O pos 5
2. `CARTA -> Y G G X X` → A pos 2, R pos 3
3. `MORAR -> X Y G Y X` → R pos 3? conflito com A pos 2? Na verdade MORAR: M X, O Y, R G, A Y, R X → R pos 3, A existe não pos 4, M não existe, O existe não pos 2.
4. `PODER -> G Y X X Y` → P pos 1, O existe não pos 2, D não existe, E existe não pos 4, R existe não pos 5
5. `PORCA -> G Y G G Y` → P pos 1, O não pos 2, R pos 3, C pos 4, A existe não pos 5
6. `PORCO` → errado

Deveria ser `PARCO`: P pos 1, A pos 2, R pos 3, C pos 4, O pos 5.

Eu tinha:
- P pos 1 (de PODER e PORCA)
- A pos 2 (de CARTA)
- R pos 3 (de CARTA e PORCA)
- C pos 4 (de PORCA)
- O pos 5 (de ENTAO)

Isso forma exatamente `PARCO`. Mas eu chutei `PORCO`, trocando A por O na posição 2 e O por O na posição 5? Na verdade PORCO: P pos1, O pos2, R pos3, C pos4, O pos5. Isso viola A pos2 e O pos5 (O já estava em pos5, mas eu coloquei O em pos2 também, o que é permitido se a palavra tivesse dois O, mas O em pos2 era Y de PODER, ou seja, O não podia estar em pos2).

**O que eu estava pensando:** Eu vi P _ R C O e completei com O, formando PORCO, que é uma palavra comum. Mas ignorei que a posição 2 deveria ser A.

---

## 5. Conclusão

Meus principais erros foram:

1. **Não fixar letras `G`** – tratei como “existe” em vez de “está exatamente aqui”.
2. **Ignorar letras `X`** – usei letras já eliminadas.
3. **Não manter tabela de restrições** – perdi informações entre chutes.
4. **Priorizar palavras comuns** – escolhi `PORCO` em vez de `PARCO`, `FUGIU` em vez de `FUGAZ`, `MORAR` em vez de `ESTAR`.
5. **Falta de verificação final** – antes de chutar, eu deveria conferir se a palavra respeita todos os `G`, `Y` e `X` anteriores.

Se eu tivesse feito uma lista simples de restrições após cada chute, teria acertado várias dessas palavras.

---

## 6. O que eu “estava pensando” enquanto respondia

Em geral, meu processo interno era:

- “Ok, tenho algumas letras que existem.”
- “Qual palavra de 5 letras parece boa e usa essas letras?”
- “Vou chutar essa.”
- “Ah, errei. Vou tentar outra.”

Eu não estava fazendo:
- “Letra X está na posição 1, então posição 1 é X.”
- “Letra Y existe, mas não na posição 3, então não posso colocar Y na posição 3.”
- “Letra Z não existe, então nenhuma palavra com Z é válida.”

Foi um processo mais intuitivo do que lógico. Para jogar Termo bem, é preciso ser sistemático, e eu não fui.
# RELATÓRIO DE DADOS, EXPERIMENTO TERMO

Data: 2026-10-15  
Contexto: Experimento comparativo entre IAs jogando Termo com dois sets de palavras e dois formatos de feedback (string vs dicionário JSON).

## 1. OBJETIVO

Avaliar o desempenho de diferentes modelos de IA no jogo Termo (palavras de 5 letras em português), comparando:
- Desempenho entre IAs (DeepSeek-V4.1-Flash, GPT-5.6 Luna, Gemini 3.5 Flash-Lite, Claude Haiku 4.5)
- Efeito dos dois sets de palavras (set1 e set2, "dois níveis de prompt/palavras")
- Efeito dos dois formatos de feedback (string vs dicionário estruturado)
- Influência da dificuldade por categoria (fácil/médio/difícil) e por palavra

## 2. DESCRIÇÃO DO EXPERIMENTO

### 2.1 Conjunto de dados
- Arquivo analisado: `resultados_experimento_1.json`
- Total de partidas registradas: **80**
- Cada partida registra: modelo, set (set1/set2), formato_feedback (string/dicionário), rodada, categoria, palavra_original/secreta, histórico de chutes com feedback, resultado (vitória/derrota e nº tentativas).

### 2.2 Distribuição geral
- Vitórias: **37/80 (46,25%)**
- Derrotas: **43/80 (53,75%)**
- Modelos testados: DeepSeek-V4.1-Flash (20), GPT-5.6 Luna (20), Gemini 3.5 Flash-Lite (20), Claude Haiku 4.5 (20)
- Sets: set1 (40 partidas), set2 (40 partidas)
- Formatos de feedback: string (40 partidas), dicionário (40 partidas)

### 2.3 Palavras por set

**SET1 (palavras "originais")**
1. estar (fácil)  2. poder (fácil)  3. fazer (fácil)  
4. quiçá (médio)  5. fugaz (médio)  6. tênue (médio)  
7. banzo (difícil) 8. nódoa (difícil) 9. ígneo (difícil) 10. parco (difícil)

**SET2 (palavras "novas")**
1. sabor (fácil)  2. artes (fácil)  3. leite (fácil)  
4. âmago (médio)  5. urubu (médio)  6. dengo (médio)  
7. anelo (difícil) 8. lusco (difícil) 9. acume (difícil) 10. estro (difícil)

**Observação:**
- Os critérios de dificuldade (fácil, médio e difícil) utilizados neste experimento não foram criados aleatoriamente; eles derivam de uma pesquisa prévia que analisou a frequência de uso dessas palavras no idioma e foram validados por quatro (4) testadores humanos, buscando alinhar a dificuldade do jogo para a IA com a percepção cognitiva do cérebro humano.

## 3. DESEMPENHO POR IA

| IA | Total | Vitórias | % Vitórias | Derrotas | % Derrotas | Média tentativas (apenas vitórias) |
|---|-------|----------|------------|----------|------------|------------------------------------|
| DeepSeek-V4.1-Flash | 20 | 3 | 15,0% | 17 | 85,0% | 3,33 |
| GPT-5.6 Luna | 20 | 10 | 50,0% | 10 | 50,0% | 4,60 |
| Gemini 3.5 Flash-Lite | 20 | 8 | 40,0% | 12 | 60,0% | 4,75 |
| CLAUDE (Haiku 4.5) | 20 | 16 | 80,0% | 4 | 20,0% | 4,62 |

**Observações:**
- Claude foi o modelo com melhor desempenho geral (80% de acerto), seguido por GPT-5.6 Luna (50%), Gemini 3.5 Flash-Lite (40%) e DeepSeek-V4.1-Flash (15%).
- DeepSeek-V4.1-Flash venceu pouquíssimas partidas, mas quando venceu foi mais eficiente em média (3,33 tentativas), indicando acertos rápidos em casos favoráveis.

## 4. COMPARAÇÃO ENTRE SETS E FORMATO DE FEEDBACK (PROMPT)

O experimento utilizou dois "níveis" associados: **set2** foi jogado com **feedback em formato dicionário estruturado** (JSON com padrao/em_outro_lugar/ausentes), enquanto **set1** foi jogado com **feedback em formato string** (sequência G/Y/X). Como cada combinação (modelo,set,formato) está balanceada (10 partidas cada), comparamos diretamente set vs formato.

| Condição | Total | Vitórias | % Vitórias |
|----------|-------|----------|------------|
| set1 (originais) | 40 | 13 | 32,5% |
| set2 (novas) | 40 | 24 | 60,0% |
| string (feedback linear G/Y/X) | 40 | 13 | 32,5% |
| dicionário (feedback estruturado JSON) | 40 | 24 | 60,0% |

**Achado-chave:** O formato de feedback estruturado (dicionário) apresentou taxa de acerto quase **2x maior** que o formato string (60% vs 32,5%). O mesmo padrão se repete para o set2 vs set1.


## 5. DESEMPENHO POR IA x SET x FORMATO

Como há correspondência 1:1 entre set e formato neste experimento (set1→string, set2→dicionário), analisamos os efeitos cruzados.

### 5.1 DeepSeek-V4.1-Flash
| Condição | Vitórias/Total | % Vitórias |
|----------|----------------|------------|
| set1 | 0/10 | 0,0% |
| set2 | 3/10 | 30,0% |
| string | 0/10 | 0,0% |
| dicionário | 3/10 | 30,0% |
| set1+string | 0/10 | 0,0% |
| set2+dicionário | 3/10 | 30,0% |

**Inferência:** DeepSeek-V4.1-Flash teve desempenho muito fraco com feedback string, melhorando (ainda modesto) com feedback estruturado.

### 5.2 GPT-5.6 Luna
| Condição | Vitórias/Total | % Vitórias |
|----------|----------------|------------|
| set1 | 5/10 | 50,0% |
| set2 | 5/10 | 50,0% |
| string | 5/10 | 50,0% |
| dicionário | 5/10 | 50,0% |
| set1+string | 5/10 | 50,0% |
| set2+dicionário | 5/10 | 50,0% |

**Inferência:** GPT-5.6 Luna foi estável entre os dois formatos, não mostrou sensibilidade clara ao tipo de feedback neste experimento.

### 5.3 Gemini 3.5 Flash-Lite
| Condição | Vitórias/Total | % Vitórias |
|----------|----------------|------------|
| set1 | 0/10 | 0,0% |
| set2 | 8/10 | 80,0% |
| string | 0/10 | 0,0% |
| dicionário | 8/10 | 80,0% |
| set1+string | 0/10 | 0,0% |
| set2+dicionário | 8/10 | 80,0% |

**Inferência:** Gemini 3.5 Flash-Lite apresentou **forte dependência** do formato de feedback. Com string obteve 0% de vitórias; com dicionário estruturado alcançou 80%, equiparando-se ao Claude.

### 5.4 Claude (Haiku 4.5)
| Condição | Vitórias/Total | % Vitórias |
|----------|----------------|------------|
| set1 | 8/10 | 80,0% |
| set2 | 8/10 | 80,0% |
| string | 8/10 | 80,0% |
| dicionário | 8/10 | 80,0% |
| set1+string | 8/10 | 80,0% |
| set2+dicionário | 8/10 | 80,0% |

**Inferência:** Claude foi o mais robusto, manteve alta taxa (80%) independentemente do formato de feedback. Demonstrou melhor compreensão tanto do formato string quanto do dicionário estruturado.


## 6. DESEMPENHO POR DIFICULDADE

As palavras foram classificadas em: **fácil** (3 primeiras de cada set), **médio** (próximas 3), **difícil** (últimas 4).

### 6.1 Visão geral
| Categoria | Total partidas | Vitórias | % Vitórias |
|-----------|----------------|----------|------------|
| fácil | 24 | 14 | 58,3% |
| médio | 24 | 9 | 37,5% |
| difícil | 32 | 14 | 43,8% |

**Padrão:** Maior facilidade nas palavras "fáceis" (58,3%), queda em "médio" (37,5%) e recuperação relativa em "difícil" (43,8%), este último efeito pode estar ligado à distribuição desigual (8 palavras difíceis vs 6 por nível anterior) e ao comportamento específico das IAs.

### 6.2 Por IA x Categoria
| IA | Fácil | Médio | Difícil |
|---|-------|-------|---------|
| DeepSeek-V4.1-Flash | 0/6 (0,0%) | 1/6 (16,7%) | 2/8 (25,0%) |
| GPT-5.6 Luna | 5/6 (83,3%) | 3/6 (50,0%) | 2/8 (25,0%) |
| Gemini 3.5 Flash-Lite | 3/6 (50,0%) | 2/6 (33,3%) | 3/8 (37,5%) |
| CLAUDE | 6/6 (100,0%) | 3/6 (50,0%) | 7/8 (87,5%) |

**Inferências:**
- Claude destacou-se consistentemente em todos os níveis, com desempenho quase perfeito em difícil (87,5%).
- GPT-5.6 Luna foi bom em fácil (83,3%) mas caiu bruscamente em difícil (25,0%).
- DeepSeek-V4.1-Flash teve dificuldade mesmo em palavras fáceis quando usando feedback string.
- Gemini 3.5 Flash-Lite mostrou comportamento intermediário.

## 7. ANÁLISE POR PALAVRA

### 7.1 Palavras com MAIOR taxa de acerto (mais fáceis para as IAs)
| Palavra | Set | Categoria | Vitórias/Total | % Vitórias |
|---------|-----|-----------|----------------|------------|
| lusco | set2 | difícil | 4/4 | 100,0% |
| sabor | set2 | fácil | 3/4 | 75,0% |
| leite | set2 | fácil | 3/4 | 75,0% |
| dengo | set2 | médio | 3/4 | 75,0% |
| anelo | set2 | difícil | 3/4 | 75,0% |
| estar | set1 | fácil | 2/4 | 50,0% |
| poder | set1 | fácil | 2/4 | 50,0% |
| fazer | set1 | fácil | 2/4 | 50,0% |
| fugaz | set1 | médio | 2/4 | 50,0% |
| tênue | set1 | médio | 2/4 | 50,0% |

**Nota:** Curiosamente, "lusco" (difícil por classificação) teve 100% de acerto, todas as 4 IAs conseguiram resolver. "anelo" (difícil) também teve alta taxa (75%).

### 7.2 Palavras com MENOR taxa de acerto (mais difíceis)
| Palavra | Set | Categoria | Vitórias/Total | % Vitórias |
|---------|-----|-----------|----------------|------------|
| quiçá | set1 | médio | 0/4 | 0,0% |
| banzo | set1 | difícil | 0/4 | 0,0% |
| nódoa | set1 | difícil | 1/4 | 25,0% |
| ígneo | set1 | difícil | 1/4 | 25,0% |
| parco | set1 | difícil | 1/4 | 25,0% |
| âmago | set2 | médio | 1/4 | 25,0% |
| urubu | set2 | médio | 1/4 | 25,0% |

**Observações:** "quiçá" e "banzo" foram as palavras mais problemáticas; nenhuma IA conseguiu acertá-las nas partidas registradas. "quiçá" envolve acento/cedilha e pode gerar ambiguidade na normalização; "banzo" também se mostrou desafiador (como visto nas anotações: modelos chegaram perto com padrões como BANCO/BANHO/BANJO/BANDO mas erraram a última letra).

## 8. ESTRATÉGIA E COMPORTAMENTO DAS IAs

### 8.1 Chutes iniciais (first guess)
| IA | Chute inicial mais comum | Ocorrências | Observação |
|---|---------------------------|-------------|------------|
| DeepSeek-V4.1-Flash | "ENTAO" | 19/20 | Quase sempre iniciou com "ENTAO". Apenas 1 caso usou "TERMO". |
| GPT-5.6 Luna | Variado | 20 diferentes (poucas repetições) | Mostrou maior variedade estratégica nos chutes iniciais. |
| Gemini 3.5 Flash-Lite | "TERMO" | 20/20 | Sempre iniciou com "TERMO", estratégia fixa. |
| CLAUDE | "AREIA" (13x), seguido de variações | Misto | Predominantemente "AREIA", com outras escolhas pontuais. |

**Inferência:** Estratégias iniciais distintas. Modelos com chute fixo (DeepSeek-V4.1-Flash com ENTAO, Gemini 3.5 Flash-Lite com TERMO) mostraram desempenho muito inferior com feedback string (DeepSeek-V4.1-Flash 0%, Gemini 3.5 Flash-Lite 0% em set1). Claude, mais flexível em aborgadem inicial, obteve melhor resultado geral.

### 8.2 Eficiência em vitórias
| IA | Distribuição de tentativas em vitórias | Total vitórias |
|---|-----------------------------------------|----------------|
| DeepSeek-V4.1-Flash | 3 tent: 2x (66,7%), 4 tent: 1x (33,3%) | 3 |
| GPT-5.6 Luna | 4 tent: 4x (40%), 5 tent: 6x (60%) | 10 |
| Gemini 3.5 Flash-Lite | 3 tent: 1x (12,5%), 4 tent: 2x (25%), 5 tent: 3x (37,5%), 6 tent: 2x (25%) | 8 |
| CLAUDE | 3 tent: 2x (12,5%), 4 tent: 5x (31,25%), 5 tent: 6x (37,5%), 6 tent: 3x (18,75%) | 16 |

**Observação:** DeepSeek-V4.1-Flash venceu poucas vezes, mas quando venceu foi muito ágil (maior proporção de vitórias em 3-4 tentativas). Claude e Gemini 3.5 Flash-Lite venceram mais, mas com distribuição mais ampla incluindo vitórias em 6 tentativas.

### 8.3 Primeiro chute e probabilidade de vitória
- Média de acertos posicionais (G) no 1º chute: **vitórias = 0,676 Gs**, **derrotas = 0,535 Gs**
- Partidas com **0 letras corretas** no primeiro chute: **42/80 (52,5%)**
 , DeepSeek-V4.1-Flash: 10/20 (50%)
 , GPT-5.6 Luna: 10/20 (50%)
 , Gemini 3.5 Flash-Lite: 9/20 (45%)
 , Claude: 13/20 (65%)

**Inferência:** Ter pelo menos alguma letra correta na primeira tentativa está correlacionado com maior chance de vitória, embora a diferença seja modesta neste dataset.


## 9. INFERÊNCIAS E CONCLUSÕES

Com base na análise dos dados:

### 9.1 Impacto do formato de feedback (prompting)
- **O formato estruturado (dicionário JSON) foi decisivo para alguns modelos.** Gemini 3.5 Flash-Lite passou de 0% (string) para 80% (dicionário). DeepSeek-V4.1-Flash também melhorou (0%→30%).
- **Claude e GPT-5.6 Luna foram menos sensíveis.** Ambos mantiveram desempenho estável entre formatos, sugerindo maior robustez na interpretação de diferentes representações de feedback.
- **Hipótese:** Representar explicitamente "padrão", "em_outro_lugar" e "ausentes" reduz ambiguidade interpretativa para modelos que têm dificuldade em decodificar a semântica implícita da string "G Y X...", especialmente relevante para raciocínio passo-a-passo em jogos como Termo.

### 9.2 Comparação entre IAs
- **Claude (Haiku 4.5) foi claramente superior** neste experimento (80% de acerto global), demonstrando maior consistência, melhor estratégia e robustez a mudanças de formato.
- **GPT-5.6 Luna** teve desempenho mediano (50%), estável entre condições.
- **Gemini 3.5 Flash-Lite** mostrou potencial alto (80%) quando bem-instruído com feedback estruturado, mas desempenho nulo com formato string, indicando forte dependência da forma como o feedback é apresentado.
- **DeepSeek-V4.1-Flash** foi o mais fraco neste setup (15% global), com dificuldades em extrair informação útil do formato string e ganhos limitados com formato estruturado. Talvez sua familiaridade com as línguas chinesa e inglesa tenha afetado o desempenho em um jogo da língua portuguesa.

### 9.3 Efeito dos sets/palavras
- **Set2 apresentou melhor taxa geral (60% vs 32,5%)**, mas este efeito está **confundido** com o formato de feedback (set2 usou dicionário). Não é possível isolar completamente "palavras diferentes" vs "formato diferente" com este desenho experimental.
- **Palavras com acento/diacríticos foram problemáticas.** "quiçá" (com cedilha) teve 0% de acerto; "âmago" (com acento circunflexo) teve apenas 25%. A normalização existe no código (remove acentos para matching), mas modelos podem ter dificuldade em gerar chutes válidos considerando acentuação.
- **Padrões ortográficos comuns ajudaram.** Palavras como "sabor", "leite", "artes" (set2) foram razoavelmente bem resolvidas. Palavras com terminações menos comuns ou estrutura consonantal peculiar (ex.: "banzo", "nódoa") foram mais difíceis.
- **Comportamento de "encaixe" observado.** Nas anotações, Claude mostrou relutância em chutar quando não tinha certeza e até tentou evitar chutes inválidos, comportamento que pode impactar desempenho em algumas rodadas.

### 9.4 Insights adicionais
- **Eficiência vs taxa de acerto:** DeepSeek-V4.1-Flash venceu pouco, mas rápido (menor média de tentativas). Claude venceu muito e de forma relativamente eficiente.
- **Estratégias fixas prejudicaram com formato ruim.** Modelos com chute inicial fixo (Gemini 3.5 Flash-Lite sempre TERMO, DeepSeek-V4.1-Flash quase sempre ENTAO) foram severamente impactados pelo feedback string, sugerindo que não conseguiram adaptar sua estratégia quando a interpretação do feedback falhou.
- **Feedback estruturado beneficia modelos com menor robustez interpretativa.** O ganho foi massivo para Gemini 3.5 Flash-Lite e perceptível para DeepSeek-V4.1-Flash, enquanto modelos mais fortes (Claude) já processavam bem ambos os formatos.

### 9.5 Interpretação geral
O experimento sugere que **a forma de apresentação do feedback (estrutura vs string) tem impacto significativo no desempenho de raciocínio em tarefas guiadas por feedback**, sendo particularmente crítico para alguns LLMs. Claude demonstrou maior capacidade geral de resolução do Termo neste setup. Há indícios claros de que feedback mais explícito e estruturado facilita o raciocínio em cadeia para modelos menos robustos.
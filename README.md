# Experimento: Avaliação de Raciocínio de LLMs no Jogo Termo

Este repositório contém os dados e a análise de um experimento focado em avaliar o desempenho de diferentes Modelos de Linguagem de Grande Escala (LLMs) ao jogar o jogo Termo, focado em adivinhar palavras de 5 letras em português. O objetivo principal é mensurar como o formato do *feedback* (texto linear vs. dados estruturados) impacta a capacidade de raciocínio lógico e dedutivo de cada inteligência artificial.


## Metodologia e Conjunto de Dados

O experimento foi executado através de rodadas automatizadas e documentadas no arquivo `resultados_experimento_1.json`, totalizando 80 partidas registradas. 

* **Modelos Avaliados:** DeepSeek-V4.1-Flash, GPT-5.6 Luna, Gemini 3.5 Flash-Lite, Claude Haiku 4.5 (20 partidas cada).
* **Formatos de Feedback (Prompt):** Metade das partidas utilizou o formato *string* (com sequências de letras como G/Y/X), e a outra metade utilizou um formato de dicionário estruturado em JSON (categorizando ativamente as letras no padrão, em outro lugar ou ausentes).
* **Conjuntos de Palavras:** O teste foi dividido entre o "Set 1" (palavras originais como *fazer*, *quiçá*, *banzo*) e o "Set 2" (novas palavras como *leite*, *âmago*, *urubu*), abrangendo três categorias de dificuldade (fácil, médio e difícil).

> **Observação sobre a Dificuldade:** Os critérios de dificuldade (fácil, médio e difícil) utilizados neste experimento não foram criados aleatoriamente; eles derivam de uma pesquisa prévia que analisou a frequência de uso dessas palavras no idioma e foram validados por testadores humanos, buscando alinhar a dificuldade do jogo para a IA com a percepção cognitiva do cérebro humano.


## Resultados Principais

As 80 partidas revelaram assimetrias severas na forma como as IAs processam regras implícitas e estruturas de dados:

* **O Domínio da Estrutura:** O uso do *feedback* em formato de dicionário estruturado gerou uma taxa de vitória quase 2x maior (60%) em comparação ao uso do *feedback* em *string* linear (32,5%).
* **Sensibilidade de Prompting (O Caso Gemini):** O modelo Gemini 3.5 Flash-Lite apresentou forte dependência do formato de *feedback*; não venceu nenhuma partida (0%) recebendo a *string* G/Y/X, mas atingiu 80% de vitórias quando os dados foram estruturados em JSON.
* **Melhor Desempenho Global:** O Claude Haiku 4.5 demonstrou a maior robustez, vencendo 80% de todas as suas partidas de forma consistente e independente do formato de formatação do *feedback* recebido.
* **Limitações Linguísticas:** Palavras com acentuação ou terminações inusitadas, especificamente "quiçá" e "banzo", representaram os maiores desafios ortográficos, resultando em 0% de acerto entre todos os modelos testados.
* **Eficiência Específica:** O DeepSeek-V4.1-Flash obteve a pior taxa de vitórias globais (15%), mas apresentou a maior agilidade nas resoluções bem-sucedidas, finalizando as partidas ganhas em uma média de apenas 3,33 tentativas.

## Trabalhos Relacionados

Embora o design inicial, os *sets* e a execução deste experimento tenham sido desenvolvidos de forma independente antes do meu contato com outras literaturas na área, recomendo fortemente a consulta ao [WordleBench](https://github.com/abronte/wordlebench). Trata-se de um *benchmark* focado no idioma inglês que aborda esse mesmo problema de forma extremamente completa e profissional, sendo um excelente ponto de partida para quem deseja se aprofundar academicamente no uso de jogos de palavras para testar capacidades cognitivas em LLMs.


## Estrutura do Repositório
* `db/`
  * `resultados_experimento_1.json`: Base de dados principal contendo a taxonomia de cada rodada, o histórico de chutes dos modelos, o formato do *feedback* injetado e o resultado das 80 partidas.
* `notes/`
  * `anexo_1.md`: Auto-avaliação detalhada gerada pelo DeepSeek justificando seu próprio desempenho ao final do primeiro conjunto de testes.
  * `consideracoes_experimento_1.pdf`: Documento autoral com as considerações e análises qualitativas do comportamento, erros e fluxos de raciocínio de cada IA rodada a rodada.
* `reports/`
  * `RELATORIO_EXPERIMENTO_1.md`: O relatório técnico original contendo a análise profunda de percentuais de vitórias, taxas de acerto por nível de dificuldade e o cruzamento de comportamento (tentativas iniciais e probabilidades de vitória) dos quatro modelos testados.

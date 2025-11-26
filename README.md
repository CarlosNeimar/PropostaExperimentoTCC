# Plano de Experimento – Scoping e Planejamento

## 1. Identificação básica

**1.1 Título do experimento**
Análise Comparativa de Eficácia entre *Vector RAG* e *GraphRAG* na Resolução de Consultas *Multi-hop* em Documentação de Software.

**1.2 ID / código**
EXP-TCC-RAG-01

**1.3 Versão do documento e histórico de revisão**
* **v0.1:** Criação inicial do esboço e definição de escopo (25/11/2025).
* **v0.2:** Revisão detalhada do GQM e métricas quantitativas.

**1.4 Datas (criação, última atualização)**
* **Criação:** 25/11/2025
* **Última Atualização:** 25/11/2025

**1.5 Autores (nome, área, contato)**
* **Autor Principal:** Carlos Henrique Neimar Areas Ferreira (Engenharia de Software)
* **Contato:** carlos.areas@sga.pucminas.br

**1.6 Responsável principal (PI / dono do experimento)**
Carlos Henrique Neimar Areas Ferreira

**1.7 Projeto / produto / iniciativa relacionada**
Proposta de Experimento para TCC – Medição e Experimentação em Engenharia de Software.

---

## 2. Contexto e problema

**2.1 Descrição do problema / oportunidade**
Modelos de RAG baseados apenas em vetores (similaridade semântica) apresentam degradação de performance em consultas que exigem raciocínio complexo (*multi-hop reasoning*), onde a resposta depende da conexão de informações dispersas em diferentes partes da documentação. A oportunidade reside em verificar se a estruturação dos dados em Grafos de Conhecimento (*GraphRAG*) resolve essa limitação ao explicitar relacionamentos entre entidades.

**2.2 Contexto organizacional e técnico**
* **Ambiente:** Acadêmico.
* **Domínio:** Documentação Técnica de Software (APIs, Frameworks, Arquitetura).
* **Stack Tecnológico:** Python, LlamaIndex (Orquestração), Neo4j (Grafo), ChromaDB (Vetorial), OpenAI GPT-4o-mini (LLM de extração/geração) e Ragas (Framework de avaliação).

**2.3 Trabalhos e evidências prévias**
**Externos:**
* "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (Lewis et al., 2020).
* "From Local to Global: A Graph RAG Approach" (Edge et al., Microsoft, 2024).
* "Searching for Best Practices in Retrieval-Augmented Generation" (Xiaohua Wan, 2024).

**2.4 Referencial teórico e empírico essencial**
* Conceitos de *Embedding Spaces* e *Dense Retrieval*.
* Teoria dos Grafos (Nós, Arestas, Triplas RDF).
* Métricas de Avaliação de LLMs: *Faithfulness*, *Answer Relevance*, *Context Precision*.

---

## 3. Objetivos e questões (Goal / Question / Metric)

**3.1 Objetivo geral**
Analisar as arquiteturas de *Vector RAG* e *GraphRAG* com o propósito de comparar a eficácia na recuperação de informação e eficiência operacional com respeito a consultas de raciocínio *multi-hop* do ponto de vista de desenvolvedores de software no contexto de documentação técnica.

**3.2 Objetivos específicos (OE)**
1.  **OE1 (Baseline):** Avaliar a eficiência operacional e a capacidade de recuperação direta do pipeline *Vector RAG* (Baseline).
2.  **OE2 (Desafiante):** Avaliar a complexidade estrutural, custo de construção e impacto na latência do pipeline *GraphRAG*.
3.  **OE3 (Dataset):** Validar a qualidade e a complexidade do *Golden Dataset* gerado para garantir que ele represente adequadamente desafios *multi-hop*.
4.  **OE4 (Comparação de Qualidade):** Comparar a qualidade semântica e a fidelidade das respostas geradas pelas duas abordagens.

### 3.3 Tabela GQM (Goal-Question-Metric)

| Objetivo Específico | Questão de Pesquisa (Q) | Métricas Associadas (M) |
| :--- | :--- | :--- |
| **OE1: Vector Baseline** | **Q1.1:** O processo de ingestão vetorial é eficiente (tempo/espaço)? | 1. Tempo de Ingestão (T_Ingest)<br>2. Tamanho do Índice (S_Index) |
| | **Q1.2:** A recuperação simples (single-hop) é eficaz? | 3. Hit Rate @ K (HR@k)<br>4. Mean Reciprocal Rank (MRR) |
| | **Q1.3:** Qual a latência base do sistema vetorial? | 5. Latência de Recuperação (L_Ret)<br>6. Throughput (QPS) |
| **OE2: Graph Challenger** | **Q2.1:** Qual o custo financeiro/computacional de construção do grafo? | 7. Custo de Extração (C_Extract)<br>8. Tokens de Ingestão (Tok_Ingest) |
| | **Q2.2:** O grafo é estruturalmente denso o suficiente para conexões? | 9. Densidade do Grafo (G_Dens)<br>10. Grau Médio (Avg_Deg) |
| | **Q2.3:** A travessia do grafo penaliza a latência excessivamente? | 5. Latência de Recuperação (L_Ret)<br>11. Tempo de Geração (T_Gen) |
| **OE3: Dataset Quality** | **Q3.1:** O dataset possui complexidade *multi-hop* real? | 12. Média de Hops (Avg_Hops)<br>13. Proporção Multi-hop (Prop_MH) |
| | **Q3.2:** O *Ground Truth* é bem fundamentado no texto? | 14. Tamanho Contexto Ref. (Len_GT)<br>15. Cobertura Chave (Key_Cov) |
| | **Q3.3:** Há diversidade de tópicos cobertos? | 16. Distribuição Tópicos (Topic_Dist)<br>17. Entropia Vocabulário (Vocab_Ent) |
| **OE4: Qualidade Final** | **Q4.1:** O *GraphRAG* reduz o ruído no contexto recuperado? | 18. Context Precision (C_Prec)<br>19. Noise Rate (N_Rate) |
| | **Q4.2:** A resposta gerada é fiel e livre de alucinações? | 20. Faithfulness (Faith)<br>21. Answer Relevance (Ans_Rel) |
| | **Q4.3:** A resposta é semanticamente correta frente ao gabarito? | 22. BERTScore (Sem_Sim)<br>23. Answer Correctness (Ans_Corr) |

### 3.4 Dicionário de Métricas

| ID | Métrica | Descrição | Unidade |
| :--- | :--- | :--- | :--- |
| **M01** | **Context Precision** | Proporção de trechos relevantes recuperados que estão no topo do ranking (Ragas). | Escala [0.0 - 1.0] |
| **M02** | **Faithfulness** | Consistência factual da resposta em relação ao contexto (Ragas). | Escala [0.0 - 1.0] |
| **M03** | **Answer Relevance** | Pertinência da resposta em relação à pergunta (Ragas). | Escala [0.0 - 1.0] |
| **M04** | **BERTScore** | Similaridade semântica entre resposta gerada e *Ground Truth*. | Escala [0.0 - 1.0] |
| **M05** | **Hit Rate @ K** | % de vezes que o doc correto aparece nos top-K resultados. | Porcentagem (%) |
| **M06** | **Latência de Recuperação** | Tempo entre envio da query e retorno dos chunks (antes da geração). | Segundos (s) |
| **M07** | **Tempo de Ingestão** | Tempo total para processar documentos e construir índice. | Minutos (min) |
| **M08** | **Custo de Extração** | Custo estimado (API OpenAI) para processamento. | Dólar ($) |
| **M09** | **Densidade do Grafo** | Razão entre arestas existentes e arestas possíveis. | Razão (Adimensional) |
| **M10** | **Grau Médio dos Nós** | Média de conexões por entidade no grafo. | Número Real |
| **M11** | **Answer Correctness** | Precisão combinada (semântica + factual) via Ragas. | Escala [0.0 - 1.0] |
| **M12** | **Tamanho do Índice** | Espaço em disco ocupado (ChromaDB ou Neo4j). | Megabytes (MB) |
| **M13** | **Noise Rate** | Proporção de sentenças irrelevantes no contexto recuperado. | Porcentagem (%) |

---

## 4. Escopo e contexto do experimento

**4.1 Escopo funcional**
* **Incluído:**
    * Ingestão de 1 documentação técnica completa.
    * Desenvolvimento de scripts Python para os dois pipelines.
    * Criação de 30-50 pares de Pergunta/Resposta (Golden Set).
    * Avaliação automatizada "LLM-as-a-Judge".
* **Excluído:**
    * Desenvolvimento de Interface Gráfica (Frontend).
    * *Fine-tuning* dos modelos de LLM.
    * Avaliação massiva com usuários humanos.

**4.2 Contexto do estudo**
Experimento controlado (*in silico*), utilizando datasets estáticos e avaliadores baseados em IA, simulando um cenário real de busca em documentação corporativa.

**4.3 Premissas**
* Disponibilidade da API da OpenAI durante o período de testes.
* A documentação escolhida possui estrutura lógica suficiente (referências cruzadas) para justificar o uso de grafos.

**4.4 Restrições**
* **Orçamento:** Limitado a créditos de API.
* **Hardware:** Processamento local.

**4.5 Limitações previstas**
* Dependência da qualidade do modelo extrator (LLM) para criar o grafo.
* Generalização limitada ao tipo de documentação técnica escolhida.

---

## 5. Stakeholders e impacto esperado

**5.1 Stakeholders principais**
* **Desenvolvedores:** Interessados na implementação técnica e reprodução.
* **Gestores/Academia:** Interessados no *trade-off* de custo/benefício.

**5.2 Interesses e expectativas**
* **Desenvolvedores:** Método científico claro e reprodutível.
* **Gestores:** Resultados objetivos para tomada de decisão arquitetural.

**5.3 Impactos potenciais**
* Consumo de recursos de API durante a execução.
* Influência na arquitetura de sistemas de busca corporativa (decisão entre RAG simples vs. complexo).

---

## 6. Riscos de alto nível, premissas e critérios de sucesso

**6.1 Riscos de alto nível**
* **Técnico:** Dificuldade em definir uma Ontologia eficiente, gerando grafos desconexos.
* **Financeiro:** Estouro do orçamento de API na fase de extração do GraphRAG.
* **Ferramental:** Incompatibilidade nas bibliotecas (LlamaIndex/LangChain).

**6.2 Critérios de sucesso globais (Go / No-Go)**
* Execução completa do benchmark.
* Diferença estatística ou observável nas métricas do Ragas entre os pipelines.
* *Go:* Pipelines funcionais e Golden Dataset criado.

**6.3 Critérios de parada antecipada**
* Custo de ingestão projetado superior ao orçamento nos primeiros 10% dos dados.
* Falha da LLM em extrair triplas coerentes (necessidade de troca de dataset).

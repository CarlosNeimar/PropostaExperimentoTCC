# Plano de Experimento – Scoping e Planejamento

## 1. Identificação básica

**1.1 Título do experimento**
Análise Comparativa de Eficácia entre *Vector RAG* e *GraphRAG* na Resolução de Consultas *Multi-hop* em Documentação de Software.

**1.2 ID / código**
EXP-TCC-RAG-01

**1.3 Versão do documento e histórico de revisão**
* **v0.1:** Criação inicial do esboço e definição de escopo (25/11/2025).

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
* "Searching for Best Practices in Retrieval-Augmented Generation"(Xiaohua Wan, 2024).
* **Internos:** N/A.

**2.4 Referencial teórico e empírico essencial**
* Conceitos de *Embedding Spaces* e *Dense Retrieval*.
* Teoria dos Grafos (Nós, Arestas, Triplas RDF).
* Métricas de Avaliação de LLMs: *Faithfulness*, *Answer Relevance*, *Context Precision*.

---

## 3. Objetivos e questões (Goal / Question / Metric)

**3.1 Objetivo geral (Goal template)**
Analisar as arquiteturas de Vector RAG e GraphRAG com o propósito de comparar a precisão na recuperação de contexto e a qualidade das respostas com respeito a consultas de raciocínio multi-hop do ponto de vista de desenvolvedores de software no contexto de documentação técnica de frameworks/bibliotecas.

**3.2 Objetivos específicos**
1.  Implementar pipeline de ingestão e recuperação vetorial (Baseline).
2.  Implementar pipeline de extração de entidades e construção de grafo (Desafiante).
3.  Gerar um *Golden Dataset* com perguntas *Single-hop* e *Multi-hop*.
4.  Mensurar e comparar o desempenho utilizando métricas automáticas.

**3.3 Questões de pesquisa (Q)**
* **Q1:** O GraphRAG apresenta maior *Context Precision* (precisão do contexto recuperado) do que o Vector RAG em perguntas multi-hop?
* **Q2:** O uso de grafos reduz a taxa de alucinações (*Faithfulness*) em comparação ao método vetorial?
* **Q3:** Qual é o impacto no custo (tokens) e tempo de processamento entre as duas abordagens?

**3.4 Métricas associadas (M)**
* **M1 (Para Q1):** *Context Precision* e *Context Recall* (via framework Ragas). Escala 0.0 a 1.0.
* **M2 (Para Q2):** *Faithfulness* e *Answer Relevance* (via framework Ragas). Escala 0.0 a 1.0.
* **M3 (Para Q3):** Custo de Ingestão ($ USD), Tempo de Ingestão (minutos), Latência média por query (segundos).

---

## 4. Escopo e contexto do experimento

**4.1 Escopo funcional**
* **Incluído:**
    * Ingestão de 1 documentação técnica completa (ex: FastAPI ou React).
    * Desenvolvimento de scripts Python para os dois pipelines.
    * Criação de 30-50 pares de Pergunta/Resposta (Golden Set).
    * Avaliação automatizada "LLM-as-a-Judge".
* **Excluído:**
    * Desenvolvimento de Interface Gráfica (Frontend).
    * *Fine-tuning* dos modelos de LLM.
    * Avaliação massiva com usuários humanos (devido ao tempo/custo).

**4.2 Contexto do estudo**
Experimento controlado (*in silico*), utilizando datasets estáticos e avaliadores baseados em IA, simulando um cenário real de busca em documentação corporativa.

**4.3 Premissas**
* Disponibilidade da API da OpenAI durante o período de testes.
* A documentação escolhida possui estrutura lógica suficiente (referências cruzadas) para justificar o uso de grafos.

**4.4 Restrições**
* **Orçamento:** Limitado a créditos de API.
* **Hardware:** Processamento local.


**4.5 Limitações previstas**
* Dependência da qualidade do modelo extrator (LLM) para criar o grafo; erros na extração podem comprometer o resultado do GraphRAG.
* Generalização: Os resultados podem ser específicos para o tipo de documentação técnica escolhida e não aplicáveis a textos narrativos livres.

---

## 5. Stakeholders e impacto esperado

**5.1 Stakeholders principais**
* **Desenvolvedores** interessados em arquitetura de LLMs.
* **Gestores** que devem avaliar o custo de cada implementação.

**5.2 Interesses e expectativas**
* **Desenvolvedores** Espera um método científico claro e reprodutível, facilitando a implementação em projetos.
* **Gestores:** Espera resultados claros e objetivos, permitindo tomar decisões sobre a arquitetura do RAG.

**5.3 Impactos potenciais**
* A execução consumirá recursos de API.
* O resultado pode influenciar a arquitetura de projetos futuros, definindo o *trade-off* entre custo e precisão.

---

## 6. Riscos de alto nível, premissas e critérios de sucesso

**6.1 Riscos de alto nível**
* **Técnico:** Dificuldade em definir um *Schema* (Ontologia) eficiente para o grafo, resultando em um grafo "sujo" ou desconexo.
* **Financeiro:** Estouro do orçamento de API devido ao alto número de tokens necessários para extração de triplas no GraphRAG.
* **Ferramental:** Incompatibilidade ou bugs nas bibliotecas (LlamaIndex/LangChain) que sofrem atualizações constantes.

**6.2 Critérios de sucesso globais (Go / No-Go)**
* O experimento será considerado um sucesso se for possível executar o benchmark completo e obter uma diferença estatística ou observável nas métricas do Ragas entre os dois pipelines.
* *Go:* Pipelines funcionais e Golden Dataset criado.

**6.3 Critérios de parada antecipada**
* Se o custo de ingestão da documentação no GraphRAG projetar um valor superior ao orçamento logo nos primeiros 10% dos dados.
* Se a LLM não conseguir extrair triplas coerentes da documentação escolhida (necessidade de trocar o dataset).

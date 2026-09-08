# estudos---N8N-
# 🤖 Miniguia de Estudos com NotebookLM — Automação de Workflows com n8n

Caderno temático construído no **NotebookLM** como ferramenta de aprendizagem ativa: curadoria de fontes abertas, engenharia de prompts documentada e consolidação do conhecimento em um miniguia reutilizável.

> Desafio de Projeto — DIO | Tema escolhido: **n8n (automação de workflows, agentes de IA e operação)**

---

## 🎯 Contexto e Objetivos

### Por que este tema

Trabalho com operações financeiras e uso automações para reduzir tarefas manuais e repetitivas (monitoramento de SLA, triagem de chamados, alertas de log). O n8n é a ferramenta que escolhi para isso por ser low-code, self-hostável e por permitir integrar APIs, bancos de dados e modelos de IA no mesmo fluxo.

O objetivo deste caderno é sair do uso "por tentativa e erro" e construir base conceitual em três frentes: **como o n8n move dados**, **como ele executa agentes de IA** e **o que muda quando o fluxo vai para produção**.

### Objetivos de estudo

- [ ] Entender a estrutura de dados do n8n (item, JSON, binary) e o vínculo entre itens de nós diferentes
- [ ] Compreender a **ordem de execução** de ramos paralelos e por que ela afeta o resultado
- [ ] Dominar expressões e referência a nós anteriores sem depender de tentativa e erro
- [ ] Aprender tratamento de erros: retries, `Continue On Fail` e error workflow
- [ ] Entender os componentes de um agente de IA (modelo, tools, memória) e o que o agente decide sozinho
- [ ] Compreender como o agente preenche parâmetros de nós e como recuperar contexto relevante (RAG)
- [ ] Saber testar e avaliar workflows de IA, que não têm saída determinística
- [ ] Conhecer os requisitos de produção: escala, queue mode, monitoramento e segurança

---

## 🗂️ Estrutura do repositório

```
miniguia-estudos-notebooklm/
├── README.md                        # Contexto, fontes, prompts e cicatrizes
├── assets/                          # Prints do NotebookLM e dos fluxos
│   ├── notebooklm-fontes.png
│   └── notebooklm-resposta.png
└── miniguia/                        # 📘 Entrega final
    ├── 01-resumos.md                # Resumos estruturados por eixo
    ├── 02-glossario.md              # Glossário dos conceitos-chave
    └── 03-prompts-reutilizaveis.md  # Prompts para revisões futuras
```

---

## 📖 Curadoria de Fontes

**Origem:** documentação oficial do n8n (`docs.n8n.io`), fonte primária e de acesso aberto.

**Critério de recorte:** em vez de carregar a documentação inteira, selecionei **15 páginas organizadas em 3 eixos**, cobrindo o caminho de quem sai do fluxo funcionando na tela e precisa entender o mecanismo por trás — dados, IA e produção. Páginas de referência de nós individuais foram deliberadamente deixadas de fora: são consulta, não estudo.

### Eixo 1 — Fundamento (dados e execução)

| # | Página | Por que entrou |
|---|--------|----------------|
| 1 | [Understand n8n's data structure](https://docs.n8n.io/build/work-with-data/understand-n8ns-data-structure) | Base de tudo: o que é um item e como os dados trafegam |
| 2 | [Expression reference](https://docs.n8n.io/build/work-with-data/transform-data/expression-reference) | Sintaxe canônica das expressões |
| 3 | [Understand execution order](https://docs.n8n.io/build/flow-logic/understand-execution-order) | Explica o comportamento de ramos paralelos, fonte de bugs silenciosos |
| 4 | [Reference previous nodes](https://docs.n8n.io/build/work-with-data/reference-data/reference-previous-nodes) | Como acessar dados de nós anteriores e o vínculo entre itens |
| 5 | [Handle errors gracefully](https://docs.n8n.io/build/flow-logic/handle-errors-gracefully) | Confiabilidade: retries, falha controlada e error workflow |

### Eixo 2 — IA e agentes

| # | Página | Por que entrou |
|---|--------|----------------|
| 6 | [Understand AI components](https://docs.n8n.io/build/integrate-ai/understand-ai-components) | Modelo, tools, memória e como se encaixam |
| 7 | [What agents do](https://docs.n8n.io/build/integrate-ai/understand-ai-components/what-agents-do) | Delimita o que o agente decide e o que continua sendo responsabilidade do fluxo |
| 8 | [How tools work](https://docs.n8n.io/build/integrate-ai/understand-ai-components/how-tools-work) | Mecânica da invocação de ferramentas pelo agente |
| 9 | [Retrieve relevant context](https://docs.n8n.io/build/integrate-ai/understand-ai-components/retrieve-relevant-context) | Recuperação de contexto (RAG) para respostas ancoradas em dados próprios |
| 10 | [Use AI for parameters](https://docs.n8n.io/build/integrate-ai/ai-examples/use-ai-for-parameters) | Preenchimento dinâmico de parâmetros de nós pelo modelo |
| 11 | [Test and improve AI workflows](https://docs.n8n.io/build/integrate-ai/test-and-improve-ai-workflows) | Como avaliar um fluxo cuja saída não é determinística |

### Eixo 3 — Produção

| # | Página | Por que entrou |
|---|--------|----------------|
| 12 | [Scaling](https://docs.n8n.io/deploy/host-n8n/configure-n8n/scaling) | O que limita o n8n quando o volume cresce |
| 13 | [Enable queue mode](https://docs.n8n.io/deploy/host-n8n/configure-n8n/scaling/enable-queue-mode) | Execução distribuída com workers |
| 14 | [Security](https://docs.n8n.io/deploy/host-n8n/configure-n8n/security) | Credenciais, exposição e superfície de risco |
| 15 | [Monitor n8n](https://docs.n8n.io/deploy/host-n8n/keep-n8n-running/monitor-n8n) | Observabilidade e métricas da instância |


### Limitação assumida da curadoria

Todas as fontes vêm de uma única origem. Isso garante consistência de versão e terminologia, mas significa que o caderno **não contém visão crítica nem comparativa** — a documentação descreve como a ferramenta funciona, não onde ela falha ou como se compara a alternativas. Perguntas desse tipo ficam fora do escopo e o NotebookLM corretamente se recusa a respondê-las.

---

## 🧪 Engenharia de Prompts e "Cicatrizes"

Esta seção é o coração do projeto: mostra **o raciocínio por trás dos resultados**, não só o resultado.

### Perguntas estratégicas elaboradas

1. Quando um nó recebe 10 itens, ele executa uma vez ou dez vezes? O que isso muda nas expressões?
2. Em um workflow com dois ramos paralelos, qual executa primeiro — e por que isso pode alterar o resultado?
3. Como o n8n mantém o vínculo entre um item de saída e o item de entrada que o originou?
4. O que exatamente o agente decide sozinho e o que continua sendo definido pelo fluxo?
5. Como testar um workflow de IA, se a mesma entrada pode gerar saídas diferentes?
6. O que precisa mudar em um fluxo que roda bem em uma instância única antes de ele ir para queue mode?

### Registro de interações



### Cicatrizes (troubleshooting)

`

---

## 📘 Miniguia de Estudo (entrega final)

| Documento | Conteúdo |
|-----------|----------|
| [01 — Resumos estruturados](./miniguia/01-resumos.md) | Os três eixos, do modelo de dados à operação |
| [02 — Glossário](./miniguia/02-glossario.md) | Conceitos-chave em definições curtas |
| [03 — Prompts reutilizáveis](./miniguia/03-prompts-reutilizaveis.md) | Prompts prontos para revisões futuras |

---

## 🧠 Principais aprendizados


---

## 🛠️ Ferramentas utilizadas

`NotebookLM` · `n8n` · `Markdown` · `Git/GitHub`

---

## 👤 Autor

**Kauan**

[![GitHub](https://github.com/anorak1000)
[![LinkedIn](https://www.linkedin.com/in/kauan-anolac-93834b205/))

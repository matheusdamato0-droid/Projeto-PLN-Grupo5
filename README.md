# Grupo 05 — Classificação de Reclamações por Assunto (P01)

Projeto desenvolvido para a disciplina de **Processamento de Linguagem Natural (PLN)**. Este repositório acompanha a evolução do projeto a partir da formulação inicial do problema (**M05**).

---

## 👥 Equipe
* Matheus Damato
* Gustavo Jouval
* Ryan Fortes
* Gabriel Alves

---

## 🎯 Sobre o Projeto

Empresas que recebem um grande volume de reclamações diariamente enfrentam um gargalo na triagem manual: mensagens acabam encaminhadas para o setor errado, o que atrasa a resposta ao cliente e aumenta a insatisfação. 

O objetivo deste projeto é usar **Processamento de Linguagem Natural** para identificar automaticamente o assunto de cada reclamação e apoiar o encaminhamento correto ao setor responsável — sem tirar a decisão final de casos ambíguos das mãos de uma pessoa.

### ❓ Pergunta que o projeto responde
> **Qual é a categoria principal relatada em cada reclamação?**

Cada reclamação (uma linha do corpus, com texto não vazio) é classificada em uma de cinco categorias:
* `entrega`
* `produto`
* `pagamento`
* `atendimento`
* `troca_devolucao`

### 🚫 O que o modelo NÃO faz
Para delimitação clara de escopo, o projeto **não** se propõe a:
1. Apontar causa, culpa, fraude ou intenção do cliente;
2. Avaliar a gravidade da reclamação ou quantos clientes foram afetados;
3. Tomar decisões finais sozinho — casos ambíguos (mais de um assunto, texto ausente ou baixa confiança) são sinalizados como `revisao_humana`.

---

## 📊 Dados (`P01_reclamacoes_assunto_v2.csv`)

O corpus possui **240 reclamações sintéticas** em português, em formato UTF-8 com BOM e delimitador `;`. A divisão é organizada da seguinte forma:
* **Treino:** 138 registros
* **Validação:** 49 registros
* **Teste:** 50 registros (mantido protegido até a avaliação final)

### Estrutura do Dataset

| Campo | Descrição | Pode ser usado como entrada? |
| :--- | :--- | :---: |
| `texto` | Mensagem escrita pelo cliente | **Sim** (única entrada válida) |
| `categoria_assunto` | Rótulo/categoria da reclamação | **Não** (alvo a ser previsto) |
| `canal` | Metadado sobre o canal de contato | **Não** (nesta versão) |
| `id`, `particao_recomendada` | Controle interno do dataset | **Não** |

> **Nota:** Em inspeção inicial, identificaram-se reclamações duplicadas entre partições e alguns registros sem texto, que serão tratados no pré-processamento. Palavras de negação (*"não"*, *"nunca"*, *"jamais"*) **não** serão removidas como stopwords, pois alteram criticamente o sentido do texto.

---

## 📈 Avaliação e Critérios de Sucesso

* **Referência de comparação (Baseline):** Prever sempre a classe mais frequente do treino (`entrega`). Qualquer modelo real precisa superar essa referência de forma consistente, com ganho de F1-macro nas 5 categorias.
* **Erro mais caro:** Confundir uma reclamação de `pagamento` com `atendimento` ou `produto` (atrasa a resolução de cobranças duplicadas e estornos).
* **Erro secundário:** Confundir `troca/devolucao` com `produto` (atrasa autorizações de postagem).
* **Uso indevido a evitar:** Usar o modelo para recusar atendimento, validar reclamações ou aplicar punições automáticas a fornecedores. Tais ações exigem estritamente revisão humana.

---

## 🗺️ Roteiro do Projeto

- [x] **Etapa 1 — Formular o problema:** Definir situação, pessoas envolvidas, pergunta operacional, entrada/saída e limites (arquivo `M05_Ficha_inicial_projeto_integrador_LGN_v3.pdf`).
- [x] **Etapa 2 — Representar o texto:** Testar e comparar Bag of Words, unigramas/bigramas e TF-IDF.
- [x] **Etapa 3 — Treinar um baseline:** Comparar contra a classe majoritária do treino.
- [x] **Etapa 4 — Treinar classificador supervisionado:** Avaliar na partição de validação (acurácia, F1-macro, matriz de confusão).
- [ ] **Etapa 5 — Investigar erros:** Analisar os erros encontrados e propor uma extensão ou hipótese própria do grupo.
- [ ] **Etapa 6 — Avaliação final:** Testar no conjunto de teste (mantido protegido até esta etapa).
- [ ] **Etapa 7 — Documentação final e apresentação.**

> 📌 **Pendência em aberto:** Verificar se a inclusão de bigramas compensa o aumento de dimensionalidade em relação ao uso exclusivo de unigramas.

---

## 📂 Arquivos no Repositório

* `M05_Ficha_Inicial_Projeto_Integrador.pdf` — Ficha inicial do projeto e formulação do problema.
* `M14_Representacao_Textual_LGN.ipynb` — Notebook com as representações textuais do projeto.
* `M15_Notebook_de_Baseline_e_Avaliacao.ipynb` — Notebook com treinos, baselines e métricas de avaliação.
* `P01_reclamacoes_assunto.csv` — Dataset original/v1.
* `P01_reclamacoes_assunto_v2.csv` — Dataset atualizado v2 utilizado na modelagem.
* `README.md` — Documentação e guia principal do repositório.

```

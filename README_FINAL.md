# M4 — Investigação Final de Linguagem Natural

**Grupo:** Grupo 05 — Classificação de Reclamações por Assunto (P01)
**Integrantes:** Matheus Damato, Gustavo Jouval, Ryan Fortes e Gabriel Alves
**Corpus-base:** `P01_reclamacoes_assunto_v2.csv`
**Trilha:** D — Mudança de representação (unigramas × unigramas+bigramas)
**Entrega:** 03/10 · **Defesa:** 06/10

---

## 1. Problema

Classificação de reclamações de clientes em 5 classes (`entrega`, `produto`, `pagamento`, `atendimento`, `troca_devolucao`), com rótulo único por texto, usando TF-IDF + classificador supervisionado.

No M16 (validação, 750 exemplos), a Representação A (unigramas, 336 atributos) obteve acurácia 0.940 e F1-macro 0.938; a Representação B (unigramas+bigramas, 3449 atributos) obteve 0.952 e 0.950. Houve 15 divergências A×B (12 corrigidas pelo B, 3 em que o B errou e o A acertou) e 33 erros comuns aos dois modelos. Os casos 871, 1111 e 1161 mostram textos com duas intenções.

## 2. Questão M4

**Cadeia:** evidência do M16 → hipótese → trilha → questão → extensão própria → experimento → conclusão.

- **Evidência (M16):** divergências A×B e textos com dupla intenção (casos 871, 1111, 1161).
- **Hipótese:** a inclusão de bigramas captura contexto local e expressões compostas, ajudando a desambiguar textos em que classes diferentes partilham o mesmo vocabulário.
- **Trilha:** D.
- **Questão:** *Em que tipos de texto a mudança de representação altera a previsão?*
- **Extensão própria:** 120 novos exemplos sintéticos (24 por classe) para testar negação, dupla intenção e ambiguidade.

## 3. Arquivos

| Arquivo | Descrição |
|---|---|
| `M16_Notebook_de_Comparacao_Controlada_e_Analise_de_Erros_v1_5.ipynb` | Notebook final executável (comparação A×B, análise de erros e aplicação final no teste) |
| `P01_reclamacoes_assunto_v2.csv` | Corpus-base bruto, preservado sem alterações (5000 registros) |
| `P01_reclamacoes_assunto_v2_extendido.csv` | Corpus final: base (5000) + extensão própria (120) = 5120 registros. Os exemplos novos são os de `REC-05001` a `REC-05120` |
| `M04_Instrumento_Operacional_LGN_v2_1__1_.pdf` | Instrumento M4 preenchido |
| `README.md` | Este arquivo |

**Formato dos CSVs:** separador `;`, codificação UTF-8 com BOM (ler com `encoding="utf-8-sig"`) e as colunas `id`, `texto`, `categoria_assunto`, `canal` e `particao_recomendada` (`treino`, `validacao`, `teste`).

**Observação sobre os dados:** o corpus-base tem 30 registros com `texto` vazio (mantidos no corpus estendido). No notebook, eles são removidos apenas para a modelagem (`dropna`). Todos estão na partição de treino, que passa a ter 3470 textos utilizáveis no corpus-base e 3554 no estendido. Validação e teste não têm textos vazios.

**Estrutura de pastas:**

```
M4/
├── M16_Notebook_de_Comparacao_Controlada_e_Analise_de_Erros_v1_5.ipynb
├── P01_reclamacoes_assunto_v2.csv
├── P01_reclamacoes_assunto_v2_extendido.csv
├── M04_Instrumento_Operacional_LGN_v2_1__1_.pdf
└── README.md
```

## 4. Execução

O projeto roda de ponta a ponta no notebook final. Para reproduzir os resultados:

1. **Pré-requisitos:** Python 3.10 ou superior e Jupyter (Notebook ou JupyterLab).
2. **Instalar as dependências:**
   ```bash
   pip install pandas numpy scikit-learn matplotlib jupyter
   ```
   (ou `pip install -r requirements.txt`, se o arquivo for entregue)
3. **Organizar os arquivos:** manter o notebook e os dois CSVs na mesma pasta. A variável `ARQUIVO` (primeira célula de código) define qual corpus é usado, e as contagens esperadas das partições (`CONTAGENS_ORIGINAIS` e `CONTAGENS_MODELAGEM`) devem corresponder a ele: 3500/750/750 e 3470 de treino utilizável no corpus-base; 3584/768/768 e 3554 no corpus estendido. Os CSVs usam `;` como separador e UTF-8 com BOM:
   ```python
   df = pd.read_csv("P01_reclamacoes_assunto_v2.csv", sep=";", encoding="utf-8-sig")
   ```
4. **Abrir e executar o notebook:**
   ```bash
   jupyter notebook M16_Notebook_de_Comparacao_Controlada_e_Analise_de_Erros_v1_5.ipynb
   ```
   Depois, usar *Kernel → Restart & Run All*. Para rodar sem abrir a interface:
   ```bash
   jupyter nbconvert --to notebook --execute M16_Notebook_de_Comparacao_Controlada_e_Analise_de_Erros_v1_5.ipynb
   ```
5. **Etapas executadas pelo notebook, em ordem:**
   1. Carregar o corpus e conferir colunas e contagens das partições (sem refazer a divisão).
   2. Remover os textos vazios e separar treino, validação e teste.
   3. Treinar e comparar as representações A (TF-IDF unigramas, `ngram_range=(1,1)`) e B (TF-IDF unigramas + bigramas, `ngram_range=(1,2)`), mudando só essa variável.
   4. Comparar métricas, matrizes de confusão e previsões exemplo a exemplo na validação; registrar hipótese e questão.
   5. Congelar a configuração final (`CONFIG_FINAL = "B"`) **antes** de consultar o teste.
   6. Aplicar o pipeline final ao teste (só `transform` e `predict`, sem novo `fit`) e registrar acurácia, F1-macro, matriz de confusão e caso difícil.

- **Classificador:** `LogisticRegression(max_iter=1000, random_state=42)`, o mesmo nas duas representações.
- **Semente aleatória:** `random_state=42`.
- **Bibliotecas:** pandas, matplotlib e scikit-learn (`TfidfVectorizer`, `LogisticRegression` e métricas).
- **Tempo aproximado de execução:** poucos segundos (menos de 1 minuto).
- **Os dois resultados:** o resultado "teste final" vem de rodar o notebook com o corpus-base; o resultado da extensão vem de rodá-lo com o corpus estendido.

**Decisões congeladas antes de consultar o teste:** pergunta, preparação, representação, classificador e configuração. Nenhum ajuste foi feito após observar o teste.

## 5. Extensão própria

- **Tamanho:** 120 exemplos (`REC-05001` a `REC-05120` no corpus estendido).
- **Planejado:** 24 por classe, sendo 8 simples, 4 curtos/informais, 4 com negação, 4 com dupla intenção (conector) e 4 ambíguos. Total: 40 simples, 20 curtos/informais, 20 com negação, 20 com dupla intenção e 20 ambíguos.
- **Distribuição final no arquivo:** `pagamento` 28, `entrega` 27, `troca_devolucao` 25, `atendimento` 20 e `produto` 20. [PREENCHER: explicar por que ficou diferente de 24 por classe]
- **Partições:** os 120 exemplos entraram em `treino` (84), `validacao` (18) e `teste` (18). Por isso as partições de validação e teste passaram de 750 para 768 casos.
- **Proveniência:** geração sintética com apoio de LLM, a partir dos bilhetes do corpus-base original. Mantida separada do corpus-base bruto.
- **Critério de rotulagem** (fixado antes de rodar os modelos):
  - Texto de uma intenção: rótulo pelo problema descrito.
  - Dupla intenção: segunda intenção com "além disso"; primeira com "e" ou "mas ao mesmo tempo".
  - Desempate em sobreposições: dinheiro (reembolso, estorno, cobrança) → `pagamento`; processo de devolver → `troca_devolucao`; defeito no item → `produto`; pedido de troca → `troca_devolucao`; dano causado pelo transporte → `entrega`; queixa sobre o suporte → `atendimento`.
  - Rotula-se apenas o que o texto afirma, sem inferir culpa, fraude ou intenção.

## 6. Resultados

### Validação (M3)

| Modelo | Acurácia | F1-macro |
|---|---|---|
| Referência majoritária (`entrega`) | 0.245 | 0.079 |
| Classificador supervisionado (M15) | 1.000 | 1.000 |

Matriz de confusão: os 49 exemplos da validação ficaram na diagonal (`entrega` 12, `produto` 11, `pagamento` 10, `atendimento` 9, `troca_devolucao` 7).

### Teste final

**Configuração final:** B — TF-IDF unigramas + bigramas (1,2)

| Conjunto | Acurácia | F1-macro | Erros |
|---|---|---|---|
| Teste final — corpus-base (750 casos) | 0.956 | 0.955 | 33 |
| Teste — corpus estendido (768 casos, 18 deles da extensão) | 0.954 | 0.953 | 35 |

### Caso difícil (índice 300)

> "A parcela veio com valor incorreto, mas ao mesmo tempo faltou um acessório dentro da caixa."

`REC-00301` (índice 300, partição `teste`). Classe real: `pagamento` · Prevista: `produto`. O texto traz vocabulário forte de duas categorias (pagamento: "parcela", "valor incorreto"; produto: "faltou um acessório", "caixa"), e a classificação multiclasse de rótulo único força uma só escolha.

### O que mudou com a extensão

Acurácia de 0.956 para 0.954, F1-macro de 0.955 para 0.953, erros de 33 para 35 (ligeira queda).

### Conclusão

**Inconclusiva.** A representação B (unigramas + bigramas) teve desempenho numericamente superior ao da representação A (unigramas) na validação, mas a análise qualitativa mostra que bigramas não resolvem a sobreposição semântica em textos com múltiplas intenções. Isso não permite afirmar que bigramas superam o problema da ambiguidade.

## 7. Limites

- TF-IDF + classificador tradicional de rótulo único não consegue priorizar nem representar narrativas com múltiplas intenções explícitas; seria necessária uma abordagem **multilabel**.
- Em textos de dupla intenção, um acerto pode refletir apenas coincidência com a escolha prioritária do anotador.
- A extensão é sintética (gerada por LLM) e pode não refletir a variação da linguagem real dos clientes.
- A validação M3 com 49 exemplos é pequena; o resultado 1.000 deve ser lido com cautela.

## 8. Uso de IA

- **Ferramenta principal:** Claude, usado para criar os 120 exemplos novos da extensão, a partir dos bilhetes do corpus-base e das regras de rotulagem do grupo.
- **Checagem auxiliar:** os rótulos dos exemplos também foram conferidos com o Gemini.
- **Responsabilidade:** análises, conclusões e decisões metodológicas são do grupo.

---

*README, notebook, corpus e Instrumento M4 devem ser coerentes entre si.*

Grupo 05 — Classificação de Reclamações por Assunto (P01)

Projeto da disciplina de Linguagem Natural. Este repositório acompanha a evolução do projeto a partir da formulação inicial do problema (M05).

Equipe: Matheus Damato, Gustavo Jouval, Ryan Fortes, Gabriel Alves

Sobre o projeto

Empresas que recebem um grande volume de reclamações diariamente enfrentam um gargalo na triagem manual: mensagens acabam encaminhadas para o setor errado, o que atrasa a resposta ao cliente e aumenta a insatisfação. O objetivo deste projeto é usar Processamento de Linguagem Natural para identificar automaticamente o assunto de cada reclamação e apoiar o encaminhamento correto ao setor responsável — sem tirar a decisão final de casos ambíguos das mãos de uma pessoa.

Pergunta que o projeto responde:

Qual é a categoria principal relatada em cada reclamação?

Cada reclamação (uma linha do corpus, com texto não vazio) é classificada em uma de cinco categorias: entrega, produto, pagamento, atendimento ou troca_devolucao.

O que o modelo não faz

Para deixar claro o escopo, o projeto não se propõe a:

apontar causa, culpa, fraude ou intenção do cliente;
avaliar a gravidade da reclamação ou quantos clientes foram afetados;
tomar decisões finais sozinho — casos ambíguos (mais de um assunto, texto ausente ou baixa confiança) são sinalizados como revisao_humana.








Dados

O corpus (P01_reclamacoes_assunto_v2.csv) tem 240 reclamações sintéticas em português, em formato UTF-8 com BOM e delimitador ;. A divisão recomendada é 138 registros de treino, 49 de validação e 50 de teste, indicada pela coluna particao_recomendada (o conjunto de teste será mantido intocado até o final).

Campo	O que é	Pode ser usado como entrada?
texto	Mensagem escrita pelo cliente	Sim — é a única entrada válida
categoria_assunto	Rótulo/categoria da reclamação	Não — é o alvo a ser previsto
canal	Metadado sobre o canal de contato	Não, nesta versão
id, particao_recomendada	Controle interno do dataset	Não

Já sabemos, de uma inspeção inicial, que há algumas reclamações duplicadas entre partições e alguns registros sem texto — isso será tratado na etapa de preparação dos dados. Também é importante não remover palavras de negação ("não", "nunca", "jamais") como se fossem stopwords, já que elas mudam o sentido da frase.

Como saberemos se o modelo é bom
Referência de comparação: prever sempre a classe mais frequente do treino (entrega). Qualquer modelo real precisa superar essa referência de forma consistente, com ganho de F1-macro nas 5 categorias.
Erro mais caro: confundir uma reclamação de pagamento com atendimento ou produto — atrasa a resolução de cobranças duplicadas e estornos.
Erro secundário: confundir troca/devolução com produto — atrasa autorizações de postagem.
Uso indevido a evitar: usar o modelo para recusar atendimento, validar reclamações ou aplicar punições automáticas a fornecedores. Isso sempre passa por revisão humana.
Roteiro do projeto
 Etapa 1 — Formular o problema. Definir situação, pessoas envolvidas, pergunta operacional, entrada/saída e limites (arquivo M05).
 Etapa 2 — Representar o texto. Testar e comparar Bag of Words, unigramas/bigramas e TF-IDF.
 Etapa 3 — Treinar um baseline. Comparar contra a classe majoritária do treino.
 Etapa 4 — Treinar um classificador supervisionado e avaliar na partição de validação (acurácia, F1-macro, matriz de confusão).
 Etapa 5 — Investigar os erros encontrados e propor uma extensão ou hipótese própria do grupo.
 Etapa 6 — Avaliação final no conjunto de teste (mantido protegido até aqui).
 Etapa 7 — Documentação final e apresentação.

Uma pendência já registrada para as próximas etapas: verificar se incluir bigramas compensa o aumento de dimensionalidade em relação a usar só unigramas.

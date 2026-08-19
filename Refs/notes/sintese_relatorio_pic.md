# Síntese da Pesquisa — Material-Base para o Relatório Final PIC

> Este documento organiza tudo o que foi estabelecido, verificado e decidido ao
> longo do desenvolvimento da pesquisa, mapeado às seções do relatório
> (`relatorio_pic.tex`). É material de referência para escrever cada seção —
> não é o texto final. Trechos marcados com **⚠ FALTA** são lacunas reais que
> precisam ser preenchidas por você antes da entrega; não foram inventados aqui.

---

## 1. Tema e Justificativa (→ Cap. Introdução, §Justificativa do Tema)

**Título de trabalho**: *A Escolha do Modelo como Decisão Axiomática: Misspecification e Fuzzy Time Series*

**Núcleo do tema**: a escolha de um modelo de previsão para séries temporais não é
uma decisão puramente técnica — é uma decisão **axiomática**, porque todo modelo
carrega, implicitamente, um compromisso formal com premissas que podem ou não
valer para os dados. A pesquisa nasce da constatação de que essa auditoria é
frequentemente ignorada na prática aplicada, mesmo em produção científica
formalmente revisada.

**Caso motivador**: SANTOS, D. C.; CAMPOS, R. G.; CORREA, J. S. *Porto de
Santos: Análise da movimentação de carga e perspectivas futuras*. Revista
Científica Semana Acadêmica, v. 11, n. 237, 2023. DOI:
10.35265/2236-6717-237-12673. Os autores aplicam doze regressões lineares
simples (uma por mês, para mitigar sazonalidade) sobre a movimentação de carga
do Porto de Santos (2005–2022) e justificam a escolha pela simplicidade e baixo
custo computacional frente ao SARIMA — sem nunca auditar os resíduos.

**⚠ Nota sobre a revista**: verificar a classificação Qualis exata de
*Semana Acadêmica* diretamente na plataforma Sucupira/CAPES antes de
citar no relatório — não confirmada de forma independente nesta pesquisa. O
sistema Qualis Periódicos no formato conceitual (A1–B3) está sendo extinto a
partir do ciclo avaliativo atual; o ciclo 2021–2024 (resultado jan/2026) foi o
último nesse formato. Se usar o argumento do Qualis no relatório, enquadre-o
como: Qualis avalia o veículo (indexação, periodicidade), não a competência
estatística específica do parecerista para cada submissão — é um ponto sobre
revisão institucional, não uma garantia quebrada.

---

## 2. Problema de Pesquisa e Hipótese (→ §Problema da Pesquisa, §Hipóteses)

**Pergunta de pesquisa**: em que sentido a escolha de um modelo estatístico
constitui uma decisão axiomática, e como essa dimensão pode ser auditada e
comparada entre sistemas formais distintos (probabilístico clássico vs. lógica
fuzzy)?

**Hipótese de trabalho**: um modelo pode ser simultaneamente **inadequado**
(suas premissas não valem para os dados) e **desonesto** (afirma inferências
que excedem o que foi verificado) — e essas são propriedades logicamente
independentes, não a mesma coisa. A regressão linear de Santos et al. (2023) é
um exemplo de ambas ao mesmo tempo; um modelo de Fuzzy Time Series aplicado aos
mesmos dados pode ser honesto sem que a pergunta de adequação (no sentido
probabilístico) sequer se aplique a ele.

---

## 3. Objetivos (→ §Objetivos)

**Objetivo Geral** (rascunho — ajustar ao verbo/registro exigido pelo
orientador):
Investigar a escolha de modelos de previsão para séries temporais como uma
decisão axiomática, auditando o caso de Santos et al. (2023) e propondo uma
alternativa baseada em Fuzzy Time Series como ilustração de honestidade
axiomática.

**Objetivos Específicos** (rascunho, 3–6 conforme exigido):
1. Auditar estatisticamente o modelo de regressão linear de Santos et al.
   (2023) via testes de misspecification (Durbin-Watson, Ljung-Box,
   Breusch-Pagan, Shapiro-Wilk, Jarque-Bera).
2. Aplicar o modelo de Fuzzy Time Series de Chen (1996) aos mesmos dados,
   removendo a tendência por diferenciação e reconstruindo os valores em
   nível.
3. Articular, com base em Mayo, Spanos, da Costa e Krause, o conceito de
   *honestidade axiomática* como critério independente de adequação
   estatística.
4. Demonstrar, através da comparação dos dois modelos, que adequação
   (relação modelo-dados) e honestidade (relação afirmação-auditoria) são
   eixos distintos.

---

## 4. Referencial Teórico (→ Cap. Referencial Teórico)

Todas as fontes abaixo foram verificadas diretamente contra o PDF/texto
original nesta pesquisa, com exceção das marcadas.

### 4.1 Regressão espúria e misspecification (fundamento econométrico)

**GRANGER, C. W. J.; NEWBOLD, P.** Spurious Regressions in Econometrics.
*Journal of Econometrics*, v. 2, n. 2, p. 111–120, 1974. DOI:
10.1016/0304-4076(74)90034-7.
- Achado central: regredir dois random walks independentes rejeita
  (erradamente) H₀ de "sem relação" em ~75% das simulações ao nível de 5%.
- Diagnóstico dos autores: $R^2$ alto + Durbin-Watson $d$ baixo não é evidência
  de relação real — "a única conclusão possível é que a equação está
  mal-especificada, qualquer que seja o valor de $R^2$" (p. 117).

### 4.2 Misspecification testing e o par pergunta-primária/pergunta-secundária

**MAYO, D. G.; SPANOS, A.** Methodology in Practice: Statistical
Misspecification Testing. *Philosophy of Science*, v. 71, n. 5, p. 1007–1025,
2004. DOI: 10.1086/425064.
- Distinção central: **questão primária** (o efeito é significativo? — só
  interpretável se o modelo $M$ for válido) vs. **questão secundária** (as
  premissas de $M$ realmente valem para os dados? — exige testar fora de $M$).
- Tabela 1 do artigo (Linear Regression Model): assunções [1] Normalidade,
  [2] Linearidade, [3] Homocedasticidade, [4] Independência, [5]
  t-invariância — o conjunto que define adequação estatística.
- §3 ("Partitioning the Space of Possible Models: Probabilistic Reduction"):
  procedimento iterativo — sondar os eixos Distribuição/Dependência/
  Heterogeneidade via técnicas gráficas e testes formais, respecificar o eixo
  violado, repetir até não sobrar violação.
- §2.1: exemplo da "variável secreta" — regressão com $R^2=0{,}995$ que se
  revela espúria (a variável era o número de sapatos da avó de Spanos).
- Contém também a passagem: *"Statistical adequacy is tantamount to the claim
  that data y₀ constitute a 'truly typical realization' of the stochastic
  mechanism..."* (§3.4) — reaparece quase idêntica em Spanos (2007), ver
  abaixo.

### 4.3 Adequação estatística e curve fitting

**SPANOS, A.** Curve Fitting, the Reliability of Inductive Inference, and the
Error-Statistical Approach. *Philosophy of Science*, v. 74, n. 5, p.
1046–1066, 2007. DOI: 10.1086/525643.
- Dicotomia central: **aproximação matemática** (critério = goodness-of-fit,
  $R^2$) vs. **modelagem estatística** (critério = resíduos não-sistemáticos,
  adequação estatística).
- Princípio: "a função aproximadora deve ser tão elaborada quanto necessário
  para garantir adequação estatística — nem mais, nem menos" (p. 1058).
- Estudo de caso Kepler vs. Ptolomeu: ambos com $R^2$ excelente (0,999 e
  0,992), mas só Kepler passa nos cinco testes de misspecification (Tabela 2,
  todos $p>0{,}10$); Ptolomeu falha em todos (Tabela 3, todos $p\approx
  0{,}00000$).
- Contém, verbatim, a mesma passagem "tantamount to the claim..." citada
  acima (§3.4, p. ~1057) — confirmado por extração direta do PDF; **ambas as
  citações (2004 e 2007) são válidas para essa frase**, o artigo de 2007
  reproduz a definição do artigo conjunto de 2004.

### 4.4 Teste severo e BENT

**MAYO, D. G.** *Statistical Inference as Severe Testing: How to Get Beyond
the Statistics Wars*. Cambridge: Cambridge University Press, 2018.
- BENT ("Bad Evidence, No Test"), §1.1: se os dados $x$ concordam com a
  alegação $C$, mas o método usado tinha pouca ou nenhuma capacidade de
  encontrar falhas em $C$ mesmo que existissem, então há "má evidência, sem
  teste" (BENT).
- Nota de rodapé (p. 89), sobre Kuhn: distingue ciências que resolvem seus
  próprios quebra-cabeças de forma confiável (astronomia) das que não
  resolvem (astrologia) — uso pontual, não central à tese.
- p. 235: usa aprovativamente o "programa degenerativo" de Lakatos, mas para
  um histórico de décadas (pesquisa parapsicológica) — confirma que a
  unidade de análise lakatosiana exige escala de programa, não um artigo
  único (relevante para justificar por que Lakatos/MSRP completo foi
  descartado da argumentação).

### 4.5 O padrão documentado na prática aplicada

**NIELSEN, A.** *Practical Time Series Analysis: Prediction with Statistics
and Machine Learning*. Sebastopol: O'Reilly Media, 2020 (data de copyright
impressa no livro; lançamento comercial em nov. 2019 — use 2020, que é a data
que consta na página de direitos autorais).
- Cap. 6, seção "6.1 – Why Not Use a Linear Regression?": *"Real-world
  analysts take liberties with model assumptions from time to time. This can
  be productive so long as the potential downsides of doing so are
  understood."*
- Uso na tese: mostra que o trade-off simplicidade/custo vs. violação de
  premissas é **rotina documentada** na prática aplicada — mas legítima
  apenas quando **consciente** (contraste direto com Santos et al., que não
  demonstram consciência alguma da violação).

### 4.6 Pluralismo lógico

**COSTA, N. C. A. da.** *Sistemas Formais Inconsistentes*. Curitiba: Editora
da UFPR, 1994. (Republicação da tese de livre-docência de 1963.)
- Posição central: existem diversas lógicas legítimas, cada uma adequada a
  problemas específicos — nenhuma é "a lógica verdadeira" de forma absoluta.
- Fonte alternativa/complementar, com passagem mais desenvolvida (analogia
  geometria euclidiana/não-euclidiana ↔ lógica clássica/lógicas alternativas,
  ligada à mecânica quântica): COSTA, N. C. A. da. *Ensaio sobre os
  Fundamentos da Lógica*. (pp. 122–124 no exemplar consultado.) **⚠ dados
  bibliográficos completos (edição, editora, ano) precisam ser confirmados
  para uso no relatório** — não foram extraídos do arquivo nesta pesquisa.

### 4.7 Espécies de estrutura (Bourbaki/Suppes/Krause)

**KRAUSE, D.; ARENHART, J. R. B.** *The Logical Foundations of Scientific
Theories: Languages, Structures, and Models*. Nova York: Routledge, 2017.
- Tese central: todo modelo/teoria científica se baseia em uma **espécie de
  estrutura** (no sentido de Bourbaki).
- §4.3 ("Arnol'd and the Bourbaki Program"): defesa não-técnica de por que
  axiomatização explícita importa — inclusive citando que é exatamente esse
  tipo de explicitação que viabiliza lógicas não-clássicas (intuicionista,
  paraconsistente).
- §5.7 (parágrafo de abertura, p. 95): introduz "predicado de Suppes" via da
  Costa e Chuaqui — origem do termo "espécie de estrutura" usado neste
  trabalho. (As definições formais completas de 5.7.1 em diante não foram
  necessárias para o nível conceitual desta IC.)

**KRAUSE, D.** Models and Modeling in Science: The Role of Metamathematics.
*Principia*, v. 26, n. 1, p. 39–54, 2022. DOI: 10.5007/1808-1711.2022.e86052.
- Abstract, citado diretamente: *"The use of models of scientific theories
  should not be done without qualifications about the mathematics being used
  to build the models. This looks obvious, at least for logicians, but
  generally, it is not to the philosopher of science."*
- Definição formal (Bourbaki, reproduzida no artigo): uma espécie de
  estrutura $\Sigma$ é dada por conjuntos-base sujeitos a axiomas; exemplo
  mínimo: um grupo é a estrutura $G = \langle G, * \rangle$.
- Epígrafes do próprio artigo, citáveis: van Fraassen (1980, p. 64) — *"To
  present a theory is to specify a family of structures, its models"*;
  Suppes (2002, p. 21) — *"[A] possible realization of a theory is a
  set-theoretical entity of the appropriate logical type."*

### 4.8 Fuzzy Time Series (fundação metodológica)

Citações já estabelecidas no resumo submetido ao congresso — **⚠ não foram
reverificadas contra fonte primária nesta pesquisa**, mas são citações
padrão, amplamente reconhecidas na literatura de FTS:
- SONG, Q.; CHISSOM, B. S. Fuzzy time series and its models. *Fuzzy Sets and
  Systems*, v. 54, n. 3, 1993.
- CHEN, S. M. Forecasting enrollments based on fuzzy time series. *Fuzzy
  Sets and Systems*, v. 81, n. 3, 1996.
- ZADEH, L. A. Fuzzy sets. *Information and Control*, v. 8, n. 3, 1965.

### 4.9 Reservado para trabalho futuro (não usar no corpo desta IC)

**SUPPES, P.** *Models and Methods in the Philosophy of Science*. Cap. 3,
"Invariance and Meaningfulness", pp. 78–80. Critério: uma hipótese é
empiricamente significativa apenas se seu valor de verdade é invariante sob
as transformações admissíveis da medição usada. Candidato a mecanismo formal
para uma definição rigorosa de honestidade axiomática — não desenvolvido
nesta IC porque exigiria caracterizar o grupo de transformações admissíveis
para lógica fuzzy, o que não existe na literatura consultada. Mencionar como
direção futura na seção de Conclusões, não desenvolver no corpo do texto.

---

## 5. Procedimentos Metodológicos (→ Cap. Procedimentos Metodológicos)

**Dados**: movimentação de carga mensal do Porto de Santos, 2005–2022 (mesma
base usada por Santos et al., 2023), obtida da Autoridade Portuária de
Santos.

**Etapa 1 — Auditoria do modelo de Santos et al. (2023)**: reprodução da
especificação (12 regressões lineares mensais) e aplicação de bateria de
testes de misspecification sobre os resíduos:
- Durbin-Watson (autocorrelação de defasagem 1)
- Ljung-Box, defasagens 12 e 24 (autocorrelação conjunta)
- Breusch-Pagan (heterocedasticidade)
- Shapiro-Wilk e Jarque-Bera (normalidade)
- Linearidade nos parâmetros tratada como especificação do modelo, não como
  hipótese testável empiricamente da mesma forma que as demais.

**Etapa 2 — Aplicação de Fuzzy Time Series**: modelo de Chen (1996), com
diferenciação da série para remoção de tendência, modelagem via relações
lógicas fuzzy (FLRs/FLRGs) sobre a série diferenciada, e reconstrução dos
valores em nível por soma acumulada das variações previstas. **⚠ Confirmar
se o pacote usado foi pyFTS e citar a ferramenta/versão no relatório, se
exigido pelo padrão do curso.**

---

## 6. Resultados (→ Cap. Resultados)

Valores obtidos e verificados nesta pesquisa (notebook `M-S_Testing.ipynb`):

| Teste | Estatística | Valor-p | Veredito |
|---|---|---|---|
| Durbin-Watson | $DW = 0{,}9133$ | — | forte indício de autocorrelação positiva |
| Ljung-Box (12 defasagens) | $LB_{12} = 119{,}27$ | $p \approx 8{,}63\times10^{-20}$ | rejeita independência |
| Ljung-Box (24 defasagens) | $LB_{24} = 135{,}41$ | $p \approx 1{,}61\times10^{-17}$ | confirma, robusto a segunda janela |
| Breusch-Pagan | $LM = 19{,}01$ | $p \approx 1{,}30\times10^{-5}$ | rejeita homocedasticidade |
| Shapiro-Wilk | $W = 0{,}9873$ | $p \approx 0{,}051$ | não rejeita normalidade (margem mínima) |
| Jarque-Bera | $JB = 7{,}11$ | $p \approx 0{,}0285$ | rejeita normalidade |

**Nota metodológica importante para o relatório**: Shapiro-Wilk e Jarque-Bera
discordam sobre normalidade — reportar essa discordância explicitamente é
mais honesto do que escolher o teste que dá a resposta desejada, e é
consistente com o próprio argumento da pesquisa sobre auditoria transparente.

**Conclusão da auditoria**: o modelo de Santos et al. (2023) viola, de forma
robusta e estatisticamente decisiva, independência e homocedasticidade — duas
das condições de Gauss-Markov — o que significa que o estimador OLS não é
sequer BLUE nesse caso, além de não sustentar inferência clássica válida
(que exigiria também normalidade, resultado ambíguo).

**⚠ FALTA — resultado da aplicação de Chen FTS**: esta pesquisa, ao longo do
desenvolvimento, não registrou métricas numéricas finais do modelo FTS
aplicado (RMSE, erro percentual, ou comparação de acurácia com a regressão
original). **Você precisa preencher esses números a partir do seu próprio
notebook/experimento** — não foram fornecidos nesta conversa e não devem ser
inventados no relatório.

---

## 7. Discussão e Interpretação dos Resultados (→ Cap. Discussão)

### 7.1 Definição de honestidade axiomática (versão final, consolidada)

> Um modelo $M$, construído sobre um sistema axiomático $\Sigma$ (uma espécie
> de estrutura, no sentido de Bourbaki/da Costa/Krause), é **axiomaticamente
> honesto** quando nenhuma afirmação inferencial extraída dele excede o que
> foi efetivamente verificado dentro de $\Sigma$ — seja essa verificação
> positiva (premissas testadas e não rejeitadas) ou sua ausência reconhecida
> (premissas não testadas, e a afirmação correspondentemente contida).

Forma lógica (uso interno/apêndice, se desejado — não necessariamente para o
corpo do texto principal):
$$M \text{ é honesto} \iff \forall i \in I(M) \text{ afirmado},\ L(i) \subseteq A(\Sigma) \text{ foi verificado}$$

### 7.2 Adequação e honestidade são eixos independentes

Ponto central da discussão, com quatro casos ilustrativos:

| | Honesto | Desonesto |
|---|---|---|
| **Adequado** | Kepler (Spanos 2007) — afirmações batem com estrutura verificada | Sobre-extrapolação — afirmação excede até o que a adequação licencia (ex.: causal a partir de correlacional) |
| **Inadequado** | Trade-off informado (Nielsen) — sabe que a premissa falha, escopa a afirmação de acordo | **Santos et al. (2023)** — afirma "sucesso" via $R^2$ sem nunca auditar — BENT |

Esta grade vale apenas para $\Sigma$ = Kolmogorov-NIID. O Chen FTS não se
encaixa nela — opera sob $\Sigma$ diferente (lógica fuzzy), e o eixo
"adequado/inadequado" análogo simplesmente não foi formalizado para essa
arquitetura na literatura consultada.

### 7.3 "Reprovação" vs. "abstenção" — o ponto que sustenta o argumento

Formulação central, útil para a redação: *Santos et al. reprovaram num teste
que aplicaram tarde demais (ou nunca aplicaram); o Chen FTS nunca se inscreveu
nesse teste.* A falta de inferência clássica no OLS é um fracasso *descoberto*
via auditoria; no Chen FTS é um fato de *escopo*, por construção da própria
lógica fuzzy subjacente. Não são a mesma coisa, e o relatório não deve
equipará-las.

### 7.4 Limitação identificada em apresentação oral (incluir com honestidade)

Na apresentação do trabalho, o Prof. Júlio Stern (IME-USP) apontou que o caso
de Santos et al. — doze regressões separadas por mês sobre uma série com
tendência e sazonalidade visualmente óbvias — constitui um alvo fácil demais
("chutar cachorro morto"): a violação seria perceptível por inspeção
qualificada, sem necessidade do aparato formal completo (Mayo, Spanos,
Krause, da Costa) para ser identificada. A crítica é procedente e deve ser
reconhecida explicitamente no relatório, na seção de limitações — não como
falha da tese, mas como limitação da força demonstrativa de um único caso
ilustrativo escolhido por sua clareza pedagógica, não por sua severidade como
teste do framework.

---

## 8. Conclusões e Limitações (→ Cap. Conclusões)

**O que foi estabelecido**: (a) o modelo de Santos et al. (2023) é
estatisticamente inadequado (independência e homocedasticidade rejeitadas,
robustamente) e axiomaticamente desonesto (afirmação de sucesso via $R^2$ sem
auditoria correspondente, configurando BENT no sentido de Mayo); (b)
honestidade axiomática e adequação estatística são critérios logicamente
independentes, não sinônimos; (c) o Chen FTS, operando sob axiomas distintos,
ilustra honestidade sem que a pergunta de adequação probabilística sequer se
aplique a ele.

**Limitações, nomeadas com precisão, não escondidas**:
1. O caso empírico único (Santos et al.) não constitui, sozinho, uma
   demonstração severa da necessidade do aparato filosófico completo — a
   violação era detectável por inspeção qualificada, sem exigir toda a
   maquinária formal (limitação apontada em banca/apresentação).
2. Honestidade axiomática, como definida aqui, permanece em nível conceitual
   — não é um predicado formal no sentido de Suppes (Cap. 3, *Models and
   Methods*), e essa formalização é trabalho futuro, não desta IC.
3. Não existe, na literatura consultada, um critério de adequação análogo ao
   de Spanos para modelos de Fuzzy Time Series na arquitetura de Chen/Song-
   Chissom — apenas para modelos fuzzy *rule-based* (regressão fuzzy, ~2010),
   estrutura diferente da usada aqui. Construir esse critério é a pergunta
   de pesquisa que naturalmente sucede esta IC.

**Sugestões para trabalhos futuros** (mestrado ou continuidade da IC):
- Auditar casos adicionais, publicados em periódicos com revisão por pares,
  onde a violação de pressupostos não seja visualmente óbvia — fortalecendo
  o argumento por severidade (um caso) ou por prevalência (vários casos
  documentados sistematicamente).
- Investigar o critério de invariância de Suppes como candidato a
  formalização geral de honestidade axiomática, cross-Σ.
- Formalizar um critério de adequação específico para FTS na arquitetura de
  Chen, respondendo à pergunta: existe, para relações lógicas fuzzy, algo
  estruturalmente equivalente a um teste de Ljung-Box?

---

## 9. Checklist de requisitos do template — o que falta

O `relatorio_pic.tex` pede, no mínimo: 3 livros de embasamento teórico, 3
artigos (≤5 anos), teses/dissertações, 2 livros de metodologia científica.

- ✅ **Livros teóricos** (≥3, atendido): da Costa (1994), Krause & Arenhart
  (2017), Mayo (2018, livro), Nielsen (2020, livro).
- ⚠ **Artigos ≤5 anos (janela 2021–2026)**: dos artigos verificados nesta
  pesquisa, **apenas Krause (2022)** cai dentro dessa janela. Granger-Newbold
  (1974), Mayo & Spanos (2004) e Spanos (2007) são fundamentais para o
  argumento mas não contam para este requisito. **Você provavelmente precisa
  localizar 2 artigos adicionais, recentes, na sua área**, para cumprir a
  exigência do template — sugestão: buscar trabalhos recentes (2021+) em
  misspecification testing aplicado ou Fuzzy Time Series, já que são os dois
  eixos centrais da pesquisa.
- ⚠ **Teses/dissertações**: nenhuma foi usada nesta pesquisa até aqui.
  Precisa localizar ao menos uma, tema compatível (séries temporais,
  filosofia da estatística, ou lógica fuzzy).
- ⚠ **2 livros de metodologia científica**: não discutidos nesta pesquisa —
  são livros-padrão de metodologia de pesquisa acadêmica (ex.: Gil, *Como
  Elaborar Projetos de Pesquisa*; Marconi \& Lakatos [Eva Maria, metodóloga
  brasileira — não confundir com Imre Lakatos, o filósofo da ciência citado
  no referencial teórico], *Fundamentos de Metodologia Científica*), a
  escolher conforme orientação da Profa. Denise Durante.

---

## 10. Notas finais para quem for escrever

- O registro do relatório PIC é mais formal/institucional que o das slides
  de congresso — a definição de honestidade axiomática pode (e deve)
  aparecer com mais desenvolvimento textual aqui do que caberia num slide.
- Mantenha a distinção entre o que está **estabelecido** (Seção 8, primeiro
  bloco) e o que é **limitação reconhecida** (Seção 8, segundo bloco) — essa
  separação explícita é, ela mesma, um exemplo do critério de honestidade
  axiomática sendo aplicado ao próprio relatório.
- Os números da Seção 6 estão prontos para uso direto no capítulo de
  Resultados; os marcados com ⚠ precisam da sua contribuição — não foram
  fornecidos nesta pesquisa e não devem ser preenchidos com valores
  inventados.

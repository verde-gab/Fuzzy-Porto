# Síntese do Projeto — Honestidade Axiomática (pós-revisão Claude Chat)

> Este documento consolida o estado do projeto após uma sessão de revisão no Claude chat que leu todo o material de referência, os quatro notebooks (ETL, EDA, Forecasting, M-S_Testing), o PDF do congresso, e os dois documentos gerados no Opus 4.6/Antigravity (`project_review.md`, `spurious_regression_examples.md`). Escrito para ser lido por outra instância (Opus/Antigravity) sem precisar da conversa original.

---

## 1. Tese central (atualizada)

**Honestidade axiomática**: um modelo é axiomaticamente honesto quando (a) nenhuma inferência que dele se extrai excede o que suas premissas formais autorizam, testadas contra os dados, e (b) a **estrutura formal** do modelo — autoregressiva, funcional-estática, etc. — corresponde à estrutura real do fenômeno modelado.

A cláusula (b) é uma extensão que não estava explícita antes desta rodada de revisão. Nem Spanos nem Mayo tratam disso — os dois discutem premissas *estatísticas* dentro de uma forma já escolhida (iid, homocedasticidade). A cláusula (b) generaliza o critério para além disso: aplicar um método autoregressivo (FTS) a um fenômeno sem estrutura temporal, ou vice-versa, é uma forma de desonestidade categorial, distinta da desonestidade estatística clássica. Isso resolve, por construção, o risco identificado abaixo (§4.1) de forçar Chen FTS num domínio que não é o dele.

---

## 2. Decisões fechadas — não reabrir sem motivo novo

| Decisão | Status |
|---|---|
| Descartar Porto de Santos / Santos et al. (2023) como estudo de caso central | **Fechado**. Confirmado independentemente pela revisão do Opus e por esta revisão. Mantido no texto no máximo como nota de rodapé sobre a motivação original do projeto. |
| Kepler vs. Ptolomeu (Spanos 2007) como âncora canônica | **Fechado**. Caso já publicado, com misspecification tests completos — não precisa ser reproduzido, só citado. |
| Não usar FTS (Chen) sobre dados sem estrutura autoregressiva | **Fechado**. Ver §1, cláusula (b). |
| FIS (não FTS) para o caso Kazi et al. (poluição do ar, Belgrado) | **Fechado**, condicionado a definir o critério de adequação interna do FIS antes de rodar o pipeline (§4.2, em aberto). |
| Parar de acumular casos empíricos novos além do necessário para preencher a matriz lógica (§3) | **Fechado como princípio**, com uma exceção justificada: um segundo caso genuinamente autoregressivo com FTS, para não perder de vez o "F" de Fuzzy *Time Series* do projeto original de IC (ver §5). |

---

## 3. A matriz lógica que o capítulo empírico precisa preencher

O objetivo não é maximizar número de casos — é preencher, com o mínimo de casos possível, as quatro células abaixo:

| | Clássico/estocástico | Fuzzy |
|---|---|---|
| **Desonesto** | Ptolomeu (Spanos) · HW multiplicativo | Kazi et al. (se FIS replicar a mesma cegueira temporal — resultado aceitável, ver §6) |
| **Honesto** | Kepler (Spanos) · HW aditivo | PWFTS(diff12) · FIS bem especificado (se incorporar sazonalidade) |

Hoje faltam apenas: (1) fechar o caso FIS/Kazi de fato, (2) decidir se um segundo caso FTS-próprio entra ou não (ver §5).

---

## 4. Lacunas teóricas em aberto — ordem de prioridade

### 4.1 [RESOLVIDO NESTA REVISÃO] Objeção de vacuidade
Definição antiga de honestidade ("nenhuma inferência excede a premissa") torna qualquer modelo de baixo compromisso (como FTS puro, que só dá previsão pontual) honesto por padrão — não por mérito, por ausência de afirmação. Um revisor com formação estatística vai sentir isso mesmo sem formalizar.

**Correção**: exigir que cada paradigma tenha seu próprio **teste de adequação interna positivo**, não apenas ausência de violação:
- OLS/regressão clássica → Gauss-Markov (BLUE), testado via Breusch-Pagan, Durbin-Watson/Ljung-Box, normalidade
- PWFTS (cadeia de Markov sobre FLRGs) → ergodicidade via **Perron-Frobenius** (irredutibilidade/primitividade da matriz de transição garantindo distribuição estacionária única)
- FIS → **em aberto, ver 4.2**

Isso também dá função real ao Ljung-Box aplicado a resíduos de FTS no notebook: não testa honestidade (FTS nunca reivindicou iid), testa **eficiência informacional residual** — quanto de estrutura temporal ficou sem ser explorada. Distinção que precisa estar explícita no texto.

### 4.2 [EM ABERTO] Critério de adequação interna para FIS
FIS não afirma resíduos iid, então Ljung-Box não serve como teste de honestidade para ele, pelo mesmo argumento do FTS. Mas, ao contrário do PWFTS, ainda não há um candidato definido para o teste de adequação interna do FIS. Candidatos a avaliar:
- Completude da base de regras (nenhuma região do espaço de entrada sem regra aplicável) — ligado à noção de completude que Hájek (1998) já formaliza para lógica fuzzy em geral
- Consistência (ausência de regras contraditórias para a mesma região)
- Fidelidade das funções de pertinência às categorias linguísticas que alegam representar (ex: as faixas oficiais de CETESB/EPA sérvia)

**Isso precisa ser decidido antes de construir o pipeline do FIS**, não depois — senão o experimento vira "comparar dois gráficos e ver qual parece melhor", exatamente o erro que o artigo critica.

### 4.3 [DO OPUS, AINDA NÃO FEITO] Declarar posição filosófica explícita
Você cita estruturalistas (Suppes/Krause), empiristas construtivos (van Fraassen) e error-statisticians (Mayo) sem declarar onde se posiciona entre eles. Síntese proposta (Opus + esta revisão): honestidade axiomática = decisão dentro de uma estrutura metamatemática (Krause/Suppes) + submetida a teste severo (Mayo/Spanos) + com pluralismo lógico (da Costa) garantindo que a escolha do formalismo é livre, mas seu respeito não é opcional depois de feita.

### 4.4 [CORREÇÃO à revisão do Opus] Não usar quatro âncoras filosóficas em paralelo para "honestidade"
O `project_review.md` (Gap 4) lista van Fraassen (empirical adequacy), Suppes (hierarquia de modelos), da Costa & French (partial truth) e Mayo (severidade) como candidatos, sem escolher. Isso dilui o conceito ao invés de fundamentá-lo — é o mesmo erro estrutural de acumular exemplos empíricos demais, só que na camada teórica.

**Resolução recomendada**: âncora primária = severidade (Mayo) + constrangimento metamatemático (Krause). Van Fraassen entra como **posicionamento meta-teórico** (onde a tese fica entre realismo e anti-realismo — achado relevante: uma citação de van Fraassen 1980 já está, sem uso, dentro do próprio slide do Krause no PDF do congresso). Da Costa & French fica de fora do corpo principal ou vira nota de rodapé.

### 4.5 [DO OPUS] Desambiguar "modelo"
Três sentidos usados sem separação: lógico/metamatemático (estrutura satisfazendo axiomas — Tarski/Suppes/van Fraassen), estatístico (família de distribuições parametrizada — Mayo/Spanos), aplicado (algoritmo específico — Chen FTS, OLS). Resolver isso é barato (meia página) e fecha uma vulnerabilidade real a acusação de equivocação.

### 4.6 [CONFLITO A RESOLVER] Kuhn
O checklist do Opus (item 10) recomenda desenvolver a conexão com Kuhn. Isso contradiz uma decisão editorial sua anterior, registrada, de cortar Kuhn do argumento — o Opus não tinha acesso a essa decisão. Escolher conscientemente: manter o corte (recomendado — da Costa já cobre o pluralismo necessário) ou reabrir.

---

## 5. A questão do segundo caso FTS — ainda não resolvida

Argumento a favor de manter uma instanciação própria de FTS (não só FIS): o projeto de IC original submetido ao programa PIC era especificamente sobre Fuzzy Time Series. Se o artigo final não tiver nenhum caso genuinamente autoregressivo com FTS, a crítica de "o projeto se distanciou da proposta original" volta, sem defesa.

Candidatos levantados nesta revisão (nenhum verificado a fundo — checar antes de adotar):

- **Mojar et al. (2023)**, "Senior High School Student Enrollment Forecasting Model: An Application of Time Series Analysis" — regressão linear sobre matrícula (SJBHSI, Filipinas), $N=8$ semestres (2018–2022). Volta ao domínio original de Chen/Song-Chissom. **Limitação séria**: $N$ pequeno demais para um Ljung-Box informativo — o defeito do paper pode ter que ser argumentado pela ausência de qualquer teste, não por reprodução de um teste fraco.
- **Modelos de matrícula da Oklahoma State University (1962–2004)** — ARIMA vs. regressão linear com termo defasado, $N$ muito melhor. Não verificado se a regressão ali é ingênua o suficiente para servir de alvo, ou já rigorosa demais.
- Vale reler os próprios Song & Chissom (1993) e Chen (1996), já nos seus uploads — há indício (não confirmado) de que o paper original de Song & Chissom já compara FTS contra regressão linear como benchmark interno. Se for o caso, isso não é um "caso novo", é material primário que você já tem, subaproveitado.

**Nenhum destes tem código aberto confirmado.** Se não achar, o plano B já estabelecido continua valendo: construir a regressão você mesmo sobre o dado bruto, citando o paper como evidência de que a prática (regressão ingênua sobre matrícula, sem checar autocorrelação) é documentada, não inventada.

---

## 6. Resultado experimental esperado que NÃO deve ser tratado como falha

Se o FIS sobre Kazi et al., configurado sem entrada sazonal (replicando a mesma cegueira temporal da regressão original), também falhar Ljung-Box nos resíduos — isso não invalida o experimento. É, na verdade, o resultado mais forte possível para a tese: mostra que um modelo fuzzy aplicado sem cuidado é tão desonesto quanto um clássico aplicado sem cuidado, fechando a objeção de vacuidade (§4.1) pelo lado empírico — "fuzzy" não é honesto por definição, honestidade depende de como o modelo é configurado, não do paradigma que usa. Entrar no experimento sem a premissa de que o FIS "precisa ganhar".

---

## 7. Resultados empíricos já obtidos (notebooks) — referência técnica

> Material do pipeline Porto de Santos. **Não faz mais parte do capítulo central do artigo** (§2), mas os números seguem válidos como demonstração técnica pessoal e como precedente metodológico para os pipelines novos (Kazi/FIS, segundo caso FTS).

| Modelo                                         | Teste                              | Resultado                                                  | Veredito                                                                                                                                                      |
| ---------------------------------------------- | ---------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OLS (12 regressões mensais)                    | Breusch-Pagan                      | $p = 1{,}3\times10^{-5}$                                   | Heterocedasticidade — rejeita                                                                                                                                 |
| OLS                                            | Durbin-Watson                      | $DW = 0{,}91$                                              | Autocorrelação forte                                                                                                                                          |
| OLS                                            | Ljung-Box                          | $p \approx 0$                                              | Rejeita — resíduos não são ruído branco                                                                                                                       |
| OLS                                            | Shapiro-Wilk                       | $p = 0{,}051$                                              | Não rejeita (fronteira)                                                                                                                                       |
| OLS                                            | Jarque-Bera                        | $p = 0{,}0285$                                             | Rejeita — **contradiz Shapiro-Wilk**, não afirmar categoricamente "resíduos não normais" no texto sem qualificar essa divergência                             |
| Holt-Winters aditivo                           | Ljung-Box (lags 1/6/12/24)         | $p = 0{,}82 / 0{,}08 / 0{,}34 / 0{,}27$                    | Passa limpo — honesto                                                                                                                                         |
| Holt-Winters multiplicativo                    | Ljung-Box (lags 1/6/12/24)         | $p \approx 0$ em todos                                     | Falha catastroficamente — **apesar de "parecer" melhor visualmente**; par de controle intra-paradigma clássico, mostra que o critério não é fuzzy-vs-clássico |
| Chen FTS sobre $\Delta y$ (diff(1))            | Ljung-Box (lag 1 / 6 / 11-13 / 24) | $p=0{,}36$ (ok) / $p=0{,}0001$ / $p\approx0$ / $p\approx0$ | Falha exatamente nos lags sazonais — diff(1) não remove sazonalidade; Chen/Song-Chissom original nunca tratou dado sazonal (matrícula é anual)                |
| PWFTS sobre $\Delta_{12}y$ (diff(12), ordem 3) | Ljung-Box (lags 1/6/12/13/24)      | $p = 0{,}61/0{,}75/0{,}28/0{,}28/0{,}33$                   | Passa limpo em todos os lags — o caso fuzzy mais forte do pipeline inteiro                                                                                    |

---

## 8. Catálogo de exemplos de regressão espúria — reconciliado

Do `spurious_regression_examples.md` (Opus), com uso recomendado:

**Usar no corpo do texto:**
1. Spanos (2007) — Kepler vs. Ptolomeu. Kepler $R^2=0{,}999$ (passa todos os 5 testes M-S, $p>0{,}10$); Ptolomeu $R^2=0{,}992$ (falha todos, $p=0{,}00000$). Caso central.
2. Mayo & Spanos (2004) — "Grandmother's Shoes": população dos EUA vs. pares de sapato da avó de Spanos, $R^2=0{,}995$, $t=79{,}5$; respecificação revela $F(3,26)=0{,}302$, $p=0{,}823$ — poder preditivo nulo. Ilustração breve, tom deliberadamente absurdo, contraste de registro com Kepler-Ptolomeu.

**Citar en passant, não desenvolver:**
3. Granger & Newbold (1974) — já na sua bibliografia.
4. Yule (1926) — primeira documentação histórica de correlação espúria.

**Não usar no corpo — risco de inflar o capítulo sem ganho lógico** (violaria o princípio do §2): Hendry (1980), Plosser & Schwert (1978), Phillips (1986), Beenstock et al., Meese & Rogoff (1983), Grant & Lebo (2016). Ficam disponíveis só se um arguidor pedir mais exemplos.

---

## 9. Material novo desta revisão — casos internacionais para FIS

- **Kazi, Z.; Filip, S.; Kazi, L. (2023)**. "Predicting PM2.5, PM10, SO2, NO2, NO and CO Air Pollutant Values with Linear Regression in R Language." *Applied Sciences*, 13(6), 3617. DOI: 10.3390/app13063617. Código: `github.com/AirPolWRL/APPWRL`. Dado: Agência de Proteção Ambiental da Sérvia, Belgrado, desde 2008. Regressão bivariada poluente-vs-poluente, sem teste de má especificação, amostra restrita a inverno (justificada citando que dado sazonal ajusta melhor que dado anual — *tell* direto de curve fitting). Categorização institucional em 5 níveis linguísticos.
- **Olvera-García, M.A.; Carbajal-Hernández, J.J.; Sánchez-Fernández, L.P.; Hernández-Bautista, I. (2016)**. "Air quality assessment using a weighted Fuzzy Inference System." *Ecological Informatics*, 33, 57–74. Precedente direto de FIS no mesmo domínio (Cidade do México), 5 estágios linguísticos. Citar como precedente metodológico, não como alvo de crítica.

---

## 10. Checklist de próximos passos

| # | Item | Bloqueia o quê |
|---|---|---|
| 1 | Definir critério de adequação interna do FIS (§4.2) | Todo o pipeline Kazi/FIS |
| 2 | Verificar Mojar et al. (2023) e o caso OSU — checar rigor real, disponibilidade de dado | Decisão sobre o 2º caso FTS |
| 3 | Reler Song & Chissom (1993) — confirmar se já comparam FTS vs. LR internamente | Pode eliminar a necessidade do item 2 |
| 4 | Declarar posição filosófica explícita (§4.3) | Estrutura da Parte II do artigo |
| 5 | Escrever a definição fechada de honestidade axiomática, com âncora única (§4.4) | Parte III inteira — é o parágrafo mais importante do artigo |
| 6 | Desambiguar "modelo" nos três sentidos (§4.5) | Precede a definição do item 5 |
| 7 | Decidir Kuhn — manter cortado ou reabrir (§4.6) | Escopo da Parte II |
| 8 | Construir pipeline Kazi + FIS (após item 1) | Capítulo empírico |
| 9 | Se decidido manter 2º caso FTS: construir pipeline sobre o paper escolhido | Capítulo empírico |

**Ordem sugerida**: 6 → 5 → 4 → 7 (fecha a Parte II/III inteira, sem depender de mais nenhuma busca) → 1 → 3 → 2 → 8 → 9.

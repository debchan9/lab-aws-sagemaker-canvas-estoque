![Amazon SageMaker Logo](https://github.com/debchan9/lab-aws-sagemaker-canvas-estoque/blob/main/imagens/sagemaker-logo.png)

![AWS Logo](https://github.com/debchan9/lab-aws-sagemaker-canvas-estoque/blob/main/imagens/aws-logo.png)

![Badge Nexa ML GenAI](https://github.com/debchan9/lab-aws-sagemaker-canvas-estoque/blob/main/imagens/nexa-badge.png)
# Previsão de estoque inteligente com Amazon SageMaker Canvas

Este repositório documenta meu projeto de previsão de estoque utilizando o Amazon SageMaker Canvas (no‑code). Aqui você encontra o passo a passo, decisões de modelagem, métricas, e como reproduzir os resultados com seu próprio dataset.

---

## Visão geral do projeto

- **Objetivo:** Construir um modelo de ML para prever níveis de estoque e apoiar decisões de reposição.
- **Abordagem:** Fluxo no‑code com o SageMaker Canvas, do upload dos dados à geração de previsões.
- **Resultado:** Relatório de desempenho, explicabilidade das features e previsões exportadas para análise de negócio.

> Este README é a minha versão do desafio DIO “Previsão de Estoque Inteligente na AWS com SageMaker Canvas”.

---

## Pré-requisitos

- **Conta AWS:** Acesso ativo com permissão ao Amazon SageMaker.
- **Dados:** Arquivo CSV ou Parquet com histórico de vendas/estoque.
- **Acesso ao Canvas:** SageMaker Studio habilitado na região suportada.

---

## Estrutura do repositório

- **datasets/** Exemplos de datasets usados ou de referência.
- **notebooks/** Explorações paralelas (opcional, apenas para análise).
- **exports/** Resultados de previsões e relatórios salvos.
- **img/** Capturas de tela do Canvas (pipeline, métricas, explicabilidade).
- **README.md** Este documento com todo o processo.

---

## Dataset e definição do problema

- **Variável-alvo:** Nível de estoque futuro, reposição necessária ou demanda prevista.
- **Principais campos do dataset:**
  - **Data:** granularidade diária/semanal/mensal
  - **SKU/Produto:** identificador único
  - **Estoque atual:** quantidade disponível
  - **Vendas históricas:** unidades vendidas por período
  - **Lead time:** tempo médio de reposição
  - **Preço/promoções:** se aplicável
  - **Sazonalidade/eventos:** feriados, campanhas, clima (se disponível)
- **Janela temporal:** Definição do horizonte de previsão (ex.: 2–4 semanas).
- **Tratamento de dados:**
  - **Limpeza:** remoção de duplicatas e padronização de datas.
  - **Imputação:** preenchimento de faltantes com regras simples.
  - **Enriquecimento:** eventos sazonais e variáveis de negócio, quando possível.

---

## Passo a passo no SageMaker Canvas

### 1. Selecionar e importar o dataset
- **Escolha do dataset:** Defini um arquivo com histórico de vendas e estoque por SKU.
- **Upload:** Fiz o upload no Canvas via integração com S3 ou upload direto.
- **Verificação:** Conferi esquemas (tipos de dados, datas, categóricas e numéricas).

### 2. Construir e treinar o modelo
- **Configuração da tarefa:** Previsão (regressão ou série temporal, conforme dados).
- **Variáveis:**
  - **Entrada:** SKU, data, vendas passadas, estoque atual, lead time, preço, promoções.
  - **Saída:** estoque futuro ou demanda prevista no horizonte definido.
- **Treinamento:** Executei o treinamento automático e acompanhei o progresso.
- **Versões:** Registrei iterações com diferentes janelas de lookback e features.

### 3. Analisar desempenho e explicabilidade
- **Métricas analisadas:** MAE, RMSE, MAPE e R² (conforme disponível no Canvas).
- **Validação:** Separação temporal para evitar vazamento de informação.
- **Importância de features:** Avaliei quais variáveis mais influenciaram a previsão.
- **Ajustes:** Remoção/adicionamento de features, ajuste de horizonte e re‑treino.

### 4. Gerar previsões e exportar
- **Previsões em lote:** Executei para um intervalo futuro por SKU.
- **Exportação:** Baixei CSV com previsões, intervalos de confiança (quando disponíveis) e sinalizações.
- **Análise de negócio:** Confrontei o output com capacidades de reposição e metas.

---

## Decisões e aprendizados

- **Granularidade temporal:** Optei por granularidade semanal para reduzir ruído e capturar sazonalidade.
- **Lead time:** Incluir lead time melhorou a precisão em SKUs com reposição variável.
- **Sazonalidade:** Eventos (como feriados) explicaram picos e ajudaram a reduzir erro percentual.
- **Generalização por SKU:** Em SKUs de baixa movimentação, agregação por categoria foi mais estável.
- **Trade‑offs:** Modelos mais complexos podem melhorar métricas, mas exigem dados consistentes e atualizados.

---

## Resultados e métricas

- **Desempenho médio (exemplo):**
  - **MAPE:** 12–18% em SKUs de alta rotatividade
  - **RMSE:** Redução significativa após incluir promoções e sazonalidade
- **Explicabilidade:**
  - **Top features:** vendas recentes, estoque atual, lead time, sazonalidade, preço
- **Limitações:**
  - Volatilidade extrema e rupturas não previstas (quebras de fornecimento) afetam o erro.

> Observação: Os valores exatos estão nos relatórios exportados em exports/.

---

## Como reproduzir

1. **Preparar dados**
   - **Formato:** CSV com colunas padronizadas (ex.: date, sku, sales, stock, lead_time, price).
   - **Qualidade:** Verifique faltantes e consistência temporal.

2. **Configurar AWS**
   - **SageMaker Studio:** Crie/abra o domínio e habilite o Canvas.
   - **Permissões:** Garanta acesso a S3 para leitura/escrita dos dados.

3. **Canvas**
   - **Upload:** Importe seu dataset.
   - **Defina alvo:** estoque futuro ou demanda.
   - **Treine:** Execute e aguarde conclusão.
   - **Avalie:** Revise métricas e importância das features.
   - **Preveja:** Exporte previsões em lote e valide com dados reais.

4. **Validação externa**
   - **Backtesting:** Compare previsões com períodos históricos.
   - **KPIs de negócio:** Estoque de segurança, nível de serviço, cobertura de estoque.

---

## Boas práticas e próximos passos

- **Atualização contínua:** Re‑treinar com dados mais recentes para capturar mudanças de padrão.
- **Monitoramento:** Acompanhar erro ao longo do tempo e drift de dados.
- **Integração:** Conectar previsões ao ERP/WMS para automação de reposição.
- **Feature store:** Centralizar variáveis derivadas (sazonalidade, calendários, eventos).
- **Expansão:** Testar segmentação por família de produtos e diferentes horizontes.

---


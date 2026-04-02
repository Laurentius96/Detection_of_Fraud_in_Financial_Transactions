# 📋 Documentação do Projeto
## IEEE Fraud Detection — Kaggle Competition

> **Início:** 26/03/2026
> **Última atualização:** 02/04/2026
> **Status:** 🟡 Em andamento — Passo 6 concluído

---

## 🎯 Objetivo Geral do Projeto

Construir um modelo de Machine Learning capaz de **detectar transações financeiras fraudulentas**, utilizando o dataset da competição IEEE-CIS Fraud Detection do Kaggle.

O projeto é desenvolvido de forma **guiada e didática**, com foco em:
- Aprender o fluxo completo de um projeto de Data Science de mercado
- Construir um portfólio com qualidade profissional
- Dominar boas práticas em cada etapa do pipeline

---

## 📁 Dados Utilizados

| Arquivo | Linhas | Colunas | Descrição |
|---|---|---|---|
| `train_transaction.csv` | 590.540 | 394 | Dados das transações financeiras |
| `train_identity.csv` | 144.233 | 41 | Dados de identidade/dispositivo |
| `test_transaction.csv` | 506.691 | 393 | Transações para predição |
| `test_identity.csv` | 119.930 | 41 | Identidade para predição |

**Target:** `isFraud` (0 = legítima, 1 = fraude)
**Desbalanceamento:** 96,5% legítimas / 3,5% fraudes

---

## 🗺️ Pipeline Completo do Projeto

### ✅ FASE 1 — Entendimento e Preparação dos Dados

#### ✅ Passo 1 — Setup do Ambiente
**Data:** 26/03/2026
**Objetivo:** Configurar o ambiente de desenvolvimento e importar as bibliotecas necessárias.
**O que foi feito:**
- Importação das bibliotecas: `pandas`, `numpy`, `matplotlib`, `seaborn`, `sklearn`
- Configuração de opções de display do pandas
- Definição dos caminhos dos arquivos

**Conceitos aprendidos:**
- Organização de imports por categoria (stdlib → third-party → local)
- Boas práticas de configuração de ambiente

---

#### ✅ Passo 2 — Carregamento dos Dados
**Data:** 26/03/2026
**Objetivo:** Carregar os 4 arquivos CSV de forma eficiente e segura.
**O que foi feito:**
- Carregamento de `train_transaction`, `train_identity`, `test_transaction`, `test_identity`
- Verificação de shape e primeiras linhas de cada DataFrame
- Uso de `pd.read_csv()` com boas práticas

**Conceitos aprendidos:**
- Diferença entre dados de treino e teste
- Estrutura de datasets de competição Kaggle

---

#### ✅ Passo 3 — Entendimento do Target (isFraud)
**Data:** 26/03/2026
**Objetivo:** Analisar a variável alvo e compreender o desbalanceamento de classes.
**O que foi feito:**
- Contagem e percentual de fraudes vs legítimas
- Visualização da distribuição do target
- Discussão sobre impacto do desbalanceamento na acurácia

**Conceitos aprendidos:**
- Desbalanceamento de classes e seus efeitos
- Por que acurácia é uma métrica enganosa em fraude
- Diferença entre Modelo Bom e Modelo Ruim com acurácia similar
- Métricas mais adequadas: Precision, Recall, F1, AUC-ROC

**Números-chave:**
```
Legítimas  →  569.877  (96,50%)
Fraudes    →   20.663  ( 3,50%)
Total      →  590.540
```

---

#### ✅ Passo 4 — Análise Inicial das Colunas
**Data:** 28/03/2026
**Objetivo:** Entender a estrutura e os tipos de dados de cada DataFrame.
**O que foi feito:**
- Análise de `dtypes` de cada coluna
- Identificação dos grupos de colunas: TransactionDT, TransactionAmt, card1–6, addr, dist, C, D, M, V, id_, Device
- Mapeamento do significado de cada grupo

**Conceitos aprendidos:**
- Grupos de features em datasets de fraude bancária
- Diferença entre features de transação e features de identidade
- Importância de entender o domínio do negócio antes de modelar

**Grupos de colunas identificados:**
| Grupo | Qtd | Descrição |
|---|---|---|
| TransactionDT | 1 | Timestamp relativo da transação |
| TransactionAmt | 1 | Valor da transação |
| card1–6 | 6 | Dados do cartão |
| addr1–2 | 2 | Endereços |
| dist1–2 | 2 | Distâncias entre endereços |
| C1–C14 | 14 | Contagens (ex: nº de endereços associados) |
| D1–D15 | 15 | Timedeltas (tempo entre eventos) |
| M1–M9 | 9 | Match flags (ex: nome bate com banco?) |
| V1–V339 | 339 | Features Vesta (engineered) |
| id_01–38 | 38 | Dados de identidade |
| DeviceType | 1 | Tipo de dispositivo |
| DeviceInfo | 1 | Modelo do dispositivo |

---

#### ✅ Passo 5 — Análise de Valores Nulos
**Data:** 02/04/2026
**Objetivo:** Mapear onde estão os dados ausentes e definir estratégia de tratamento.
**O que foi feito:**
- Função `analisar_nulos()`: visão geral de nulos por DataFrame
- Função `top_nulos()`: ranking das colunas com mais nulos
- Identificação de grupos de colunas com nulos idênticos
- Definição da regra de decisão por faixa de % de nulos

**Conceitos aprendidos:**
- Nulo pode ser feature (ausência como informação)
- Colunas com mesmo nº de nulos = mesma origem/bloco
- Criar flag `col_isnull` antes de imputar
- Por que usar `-999` em vez de `0` ou média em modelos de árvore
- Regra de decisão por faixa de % de nulos

**Resultados:**
| DataFrame | % Nulos Geral | Pior Coluna | % Nulos |
|---|---|---|---|
| train_transaction | ~48% | dist2 | 93,6% |
| train_identity | ~60% | id_24 | 96,7% |

**Regra de decisão definida:**
```
< 5%   → Imputar direto (média/mediana/moda)
5–30%  → Analisar correlação com target antes de decidir
30–80% → Criar flag is_null + imputar com -999
> 80%  → Candidata a dropar (avaliar correlação com target)
```

**Bug encontrado e corrigido:**
- `.query('nulos' > 0)` → TypeError (string vs int)
- Correção: `.query('nulos > 0')` (expressão completa entre aspas)

---

#### ✅ Passo 6 — Otimização de Memória
**Data:** 02/04/2026
**Objetivo:** Reduzir o consumo de memória RAM dos DataFrames sem perda de informação.
**O que foi feito:**
- Função `memoria_atual()`: diagnóstico de memória antes da otimização
- Função `otimizar_memoria()`: downcast de int/float + conversão object→category
- Função `converter_str_category()`: correção para tipo `str` do pandas 4

**Conceitos aprendidos:**
- Pandas carrega int64/float64 por padrão (conservador)
- `downcast='integer'` testa int8 → int16 → int32 automaticamente
- `category` guarda dicionário + índices inteiros (eficiente para baixa cardinalidade)
- `object` vs `str` no pandas 4 (breaking change)
- `category` não vale a pena para alta cardinalidade (> 50% únicos)

**Resultados:**
| DataFrame | Antes | Depois | Redução |
|---|---|---|---|
| train_transaction | 2.062 MB | 1.203 MB | 41,7% |
| train_identity | 143 MB | ~120 MB | ~16% (após correção str) |

**Warning identificado:**
- Pandas 4: `str` dtype ≠ `object` dtype → `select_dtypes(include='object')` não captura `str`
- Solução: verificar `str(df[col].dtype) == 'string'` explicitamente

---

### 🔲 FASE 2 — Integração e Feature Engineering

#### 🔲 Passo 7 — Merge dos DataFrames
**Status:** Próximo passo
**Objetivo:** Unir `train_transaction` + `train_identity` em um único DataFrame para modelagem.
**O que será feito:**
- Merge via `TransactionID` (chave comum)
- Verificar linhas que não fazem match (transações sem identidade)
- Entender o tipo de join correto (left join)
- Verificar shape após merge

---

#### 🔲 Passo 8 — Feature Engineering
**Status:** Pendente
**Objetivo:** Criar novas features a partir das existentes para melhorar o modelo.

---

#### 🔲 Passo 9 — Tratamento de Nulos (Imputação)
**Status:** Pendente
**Objetivo:** Aplicar a estratégia definida no Passo 5 para tratar os valores ausentes.

---

### 🔲 FASE 3 — Modelagem

#### 🔲 Passo 10 — Baseline Model
**Status:** Pendente
**Objetivo:** Criar um modelo simples como referência de performance mínima.

#### 🔲 Passo 11 — Modelo Principal (LightGBM / XGBoost)
**Status:** Pendente

#### 🔲 Passo 12 — Validação e Métricas
**Status:** Pendente
**Foco:** AUC-ROC, Precision, Recall, F1 — não acurácia!

#### 🔲 Passo 13 — Tuning de Hiperparâmetros
**Status:** Pendente

---

### 🔲 FASE 4 — Entrega

#### 🔲 Passo 14 — Geração do Arquivo de Submissão
**Status:** Pendente
**Objetivo:** Gerar `submission.csv` no formato exigido pelo Kaggle.

#### 🔲 Passo 15 — Submissão e Análise do Score
**Status:** Pendente

---

## 🐛 Bugs Encontrados e Corrigidos

| Data | Passo | Bug | Causa | Solução |
|---|---|---|---|---|
| 02/04/2026 | 5 | `.query('nulos' > 0)` → TypeError | String comparada com int fora do query | `.query('nulos > 0')` |
| 02/04/2026 | 6 | Colunas `str` não convertidas para `category` | Pandas 4: `str` ≠ `object` | Verificar `str(dtype) == 'string'` explicitamente |

---

## 📚 Conceitos-Chave Aprendidos

| Conceito | Passo | Resumo |
|---|---|---|
| Desbalanceamento de classes | 3 | 96,5% legítimas → acurácia enganosa |
| Acurácia vs AUC-ROC | 3 | Acurácia ignora minoria; AUC-ROC não |
| Nulo como feature | 5 | Ausência pode ser sinal de fraude |
| Blocos de nulos idênticos | 5 | Mesma origem → tratar como bloco |
| Flag is_null | 5 | Criar antes de imputar para preservar info |
| -999 em árvores | 5 | Árvores aprendem ausência; 0 pode ser valor real |
| Downcast de tipos | 6 | int64→int8/16/32; float64→float32 |
| object vs category | 6 | Category: dicionário + índices inteiros |
| str vs object pandas 4 | 6 | Breaking change: tratar separadamente |
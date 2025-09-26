# ☕️ Análise de Consumo de Café com Storytelling 🎬

Projeto de **exploração e visualização** do consumo de café a partir de um dataset transacional. Todas as análises consideram **contagem de registros** (linhas) como *proxy* de consumo.

> 💡 O storytelling (passo a passo, hipóteses e decisões) está dentro do notebook **`notebooks/fase2.ipynb`** em células Markdown.

---

## 🎯 Objetivo

Entender como o consumo de café varia por **hora do dia**, **dia da semana**, **mês do ano** e **estações do ano**, e testar hipóteses sobre **turnos** (manhã/tarde/noite) e **sazonalidade**.

---

## 🧰 Stack

* 🐍 **Python**: Pandas, Matplotlib, **python-dateutil** (datas), **calendar** (stdlib)
* 📓 **Jupyter Notebook**: `notebooks/fase2.ipynb` (com storytelling)
* 🗃️ **Dataset tratado**: `data/Coffe_sales_tratado3.csv`

---

## 🗺️ Como acompanhar o storytelling

1. 🎬 **Contexto & objetivo** → o que queremos responder.
2. 🧹 **Dados & preparação** → limpeza e colunas derivadas.
3. 🔎 **Explorações iniciais** → hora, dia da semana, mês × ano.
4. 🧪 **Hipóteses** → turnos (Top‑N), estações (meteorológica × astronômica), controle por ano.
5. ✅ **Conclusões & limitações** → o que aprendemos e cuidados ao interpretar.

> ✨ Dica: as saídas textuais usam `to_string(name=False, dtype=False)` e os gráficos usam `plt.subplots(...); df.plot(ax=ax)` para evitar *figuras vazias*.

---

## 🧾 Dados (principais colunas)

* 📆 `date` (YYYY-MM-DD)
* ⏰ `hour_of_day` (0–23)
* 🗓️ `weekday` (Monday..Sunday)
* 📅 `month_name` (January..December)
* 🌇 `time_of_day` (Morning, Afternoon, Evening, Night)
* 🧉 `coffee_name` (categorias de cafés)
* 🛠️ Engenheiradas: `_year`, `_month`, `_day`, `season` (meteorológica), `season_astro` (astronômica)

> 📏 **Métrica**: contagem de linhas (não receita/volume físico).

---

## 🧼 Pré-processamento

* ✍️ Normalização de categóricas (`weekday`, `time_of_day`).
* 🔁 Conversão de `date` → `datetime` (`YYYY-MM-DD`).
* 🧱 Criação de colunas auxiliares `_year`, `_month`, `_day`.
* ❄️ **Season (meteorológica)** por meses:

  * Winter: Dez–Fev
  * Spring: Mar–Mai
  * Summer: Jun–Ago
  * Autumn: Set–Nov
* 🌞 **Season_astro (astronômica, sem fuso)** por datas aproximadas:

  * Winter: [21/12, 20/03)
  * Spring: [20/03, 21/06)
  * Summer: [21/06, 22/09)
  * Autumn: [22/09, 21/12)
---

## 📊 Análises principais

1. ⏱️ **Consumo por hora do dia** → Agrupamento por `hour_of_day` (0–23), tabela e gráfico. *Ticks* 0..23.
2. 🗓️ **Consumo por dia da semana** → `weekday` normalizado (Monday→Sunday); Tabela limpa e gráfico de linha. Opção de de fixar eixo Y `ax.set_ylim(0, 580)` para manter rótulos visíveis.
3. 📅 **Consumo por mês × ano** → `date` → `_year` + `month_name` categórico. Pivot `month_name × _year` e gráfico de **barras agrupadas**. ℹ️ Observação relevante do dado: **Março aparece em 2024 e 2025**; **não há Jan/Fev 2024**.
4. 👨‍🍳 **Turno × tipo de café (Top‑N)** → time_of_day normalizado. Seleção do **Top‑N cafés** (N=5/7) pelo total e pivot time_of_day × coffee_name apenas para esse conjunto. Gráfico de **barras agrupadas** (legenda posicionada fora quando necessário).
5. 🍂 **Estações do ano (duas abordagens)** →
   * **Meteorológica (por meses)**: simples, mas **sensível ao Março duplicado** (distorce comparações inverno/outono).
   * **Atronômica (por datas)**: cortes por dia (20/03, 21/06, 22/09, 21/12). Corrige a alocação de **Dezembro** (21/12→Winter) e **Março** (20/03→Spring).
     Além disso, **separação por ano** para controlar o impacto da duplicidade de Março.

> Os *prints* no notebook usam `to_string(name=False, dtype=False)` para tabelas limpas e `fig, ax = plt.subplots(...); df.plot(ax=ax)` para evitar `<Figure ... with 0 Axes>`. Legendas longas vão para fora do gráfico (`bbox_to_anchor`).

---

## ❓ Perguntas & Hipóteses (storytelling)

* **Pergunta Chave:**
  👉 *“Como os padrões de consumo de café variam ao longo do dia, da semana e dos meses? Existem horários de pico previsíveis que podem orientar estratégias operacionais?”*

* **H1 — Top 5 por turno**
  👉 *“Entre manhã, tarde e noite, quais são os **5 tipos de café mais vendidos** em cada turno e **como muda o ranking** entre os turnos?”*

* **H2 — Estações (mapeamento por meses)**
  👉 *“Usando a classificação de estações por **faixas de meses** (Dez–Fev, Mar–Mai, Jun–Ago, Set–Nov), o **inverno** tem consumo total maior do que as demais estações?”*

* **H3 — Estações (cortes por data)**
  👉 *“Aplicando os **cortes de datas** (~20/03, ~21/06, ~22/09, ~21/12), o **inverno** passa a liderar o consumo quando comparado ao outono?”*

* **H4 — Estações por ano (tratando Março duplicado)**
  👉 *“Separando por ano (2024 e 2025) para tratar a duplicidade de março, e definindo inverno como **21/12 → 20/03**, o **inverno ainda é a estação de maior consumo** em cada ano? Qual é o total de registros do inverno em cada ano e qual a diferença entre eles — em número de registros?*

---

## 🧠 Decisões práticas (refletidas no notebook)

* 🧾 Impressões limpas: `Series.to_string(name=False, dtype=False)`.
* 🖼️ Sem figuras vazias: `plt.subplots` + `ax=...` nos `plot`s do Pandas.
* 🏷️ Legenda fora quando necessário: `bbox_to_anchor=(1.02,1)` + `tight_layout(rect=[0,0,0.82,1])`.
* 📈 Eixo Y fixo quando útil: `ax.set_ylim(0, 580)`.
* 🧱 Categorias ordenadas para `weekday`, `month_name`, `season`.

---

## ⚠️ Limitações do dataset

* 🔁 **Março duplicado** (2024 e 2025) distorce estações sem corte por data.
* 🕳️ **Sem Jan/Fev 2024** → afeta comparação mensal por ano.
* 🧮 Métrica é **contagem de registros**, não R$ nem volume físico.

---

## 🗂️ Estrutura do repositório

```
├── data/
│   ├── Coffe_sales_tratado3.csv   
├── notebooks/
│   └── fase2.ipynb
└── README.md
```

---

## 📢 Sobre

Este é um **projeto acadêmico** da disciplina de **Data Science**.

* **Versão 1:** foco em **tratamento do dataset** e entendimento da **lógica dos dados** (limpeza, padronização e primeiros diagnósticos).
* **Versão 2:** foco em **análise exploratória**, **visualização**, **storytelling** e **levantamento/validação de hipóteses** (o conteúdo deste repositório).

> 🎓 Objetivo: praticar o ciclo analítico completo — **preparação → exploração → hipótese → visualização → comunicação**.

---

## 🎯 Aprendizados

* ✅ Reconhecimento e padronização de **tipos de dados** no Pandas (ex.: `date` → `datetime`, uso de **categóricas ordenadas** para `weekday`, `month_name`, `season`).
* ✅ Escolha de **visualizações adequadas** ao objetivo (linha vs. barras; **barras agrupadas** para mês×ano; barras simples para hora/dia/estações).
* ✅ Aplicação de **filtros/segmentações** para clareza: **Top-5 por turno** (filtrar cafés mais consumidos), **separação por ano no gráfico mensal** (tratar março duplicado) e **cortes por data para estações astronômicas** (21/12–20/03; 20/03–21/06; 21/06–22/09; 22/09–21/12).
* ✅ **Formatação e design** que melhoram leitura (legenda externa, limites de eixo como `ax.set_ylim(0, 580)`, rótulos nos pontos e **tabelas limpas** com `to_string(name=False, dtype=False)`).
* ✅ Entendimento do **viés e peculiaridades do dataset** (Março duplicado; ausência de Jan/Fev-2024) e seu impacto na interpretação.

---














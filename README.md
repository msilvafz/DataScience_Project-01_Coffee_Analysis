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
* 🧱 Criação de `_year`, `_month`, `_day`.
* ❄️ **Season (meteorológica)** por meses:

  * Winter: Dez–Fev | Spring: Mar–Mai | Summer: Jun–Ago | Autumn: Set–Nov
* 🌞 **Season_astro (astronômica, sem fuso)** por datas aproximadas:

  * Winter: [21/12, 20/03) • Spring: [20/03, 21/06) • Summer: [21/06, 22/09) • Autumn: [22/09, 21/12)

---

## 📊 Análises principais

1. ⏱️ **Consumo por hora do dia** → grupo por `hour_of_day` (0–23), tabela e gráfico.
2. 🗓️ **Consumo por dia da semana** → ordem Monday→Sunday; tabela limpa e gráfico (linha ou barras). Opção de `ax.set_ylim(0, 580)`.
3. 📅 **Consumo por mês × ano** → pivot `month_name × _year` e barras agrupadas. ℹ️ Março aparece em **2024** e **2025**; **não há Jan/Fev 2024**.
4. 👨‍🍳 **Turno × tipo de café (Top‑N)** → Top‑5/Top‑7 cafés; barras agrupadas (legenda fora).
5. 🍂 **Estações do ano** → meteorológica (mês) × astronômica (datas). Separação **por ano** para lidar com Março duplicado.

---

## ❓ Hipóteses & perguntas (storytelling)

* **H1 — Top 5 por turno**
  👉 *“Entre manhã, tarde e noite, quais são os 5 tipos de café mais vendidos em cada turno e como muda o ranking entre os turnos?”*

* **H2 — Estações (mapeamento por meses)**
  👉 *“Usando faixas de meses (Dez–Fev, Mar–Mai, Jun–Ago, Set–Nov), o inverno tem consumo total maior que as demais estações?”*

* **H3 — Estações (cortes por data)**
  👉 *“Aplicando cortes ~20/03, ~21/06, ~22/09, ~21/12, o inverno passa a liderar o consumo quando comparado ao outono?”*

* **H4 — Estações por ano (tratando Março duplicado)**
  👉 *“Separando 2024 e 2025, e definindo inverno como 21/12→20/03, o inverno ainda é a estação de maior consumo em cada ano? Qual o total do inverno por ano e a diferença (registros e %)?”*

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

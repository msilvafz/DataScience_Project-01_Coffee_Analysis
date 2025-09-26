# Análise de Consumo de Café — v2 (com Storytelling no Notebook)

Projeto de exploração e visualização do consumo de café a partir de um dataset transacional. Todas as análises consideram **contagem de registros** (linhas) como *proxy* de consumo. Esta versão do README está alinhada ao **notebook `fase2.ipynb` atualizado**, que inclui o **storytelling completo** em Markdown (passo a passo do raciocínio, hipóteses e decisões).

---

## 🎯 Objetivo

Responder como o consumo de café varia por **hora do dia, dia da semana, mês do ano** e **estações do ano**, e validar hipóteses sobre **turnos** (manhã/tarde/noite) e **sazonalidade**.

---

## 🔧 Stack

* Python: **Pandas**, **Matplotlib**, **python-dateutil** (parse de datas), **calendar** (stdlib)
* Jupyter Notebook (`notebooks/fase2.ipynb`) — storytelling em Markdown
* Dataset tratado (`data/Coffe_sales_tratado3.csv` / `data/Coffe_sales_tratado3.xls`)

---

## 📂 Estrutura do repositório (sugerida)

```
├── data/
│   ├── Coffe_sales_tratado3.csv
│   └── Coffe_sales_tratado3.xls
├── notebooks/
│   └── fase2.ipynb
└── README.md
```

> O storytelling (passo a passo) está no **`notebooks/fase2.ipynb`**.

---

## 📄 Dados (principais colunas)

* `date` (YYYY-MM-DD)
* `hour_of_day` (0–23)
* `weekday` (Monday..Sunday)
* `month_name` (January..December)
* `time_of_day` (Morning, Afternoon, Evening, Night)
* `coffee_name` (categorias de cafés)
* Engenheiradas no notebook: `_year`, `_month`, `_day`, `season` (meteorológica), `season_astro` (astronômica aproximada)

> Métrica usada: **contagem de linhas** (não receita nem volume físico).

---

## 🧹 Pré-processamento (resumo)

* Normalização de categóricas (`weekday`, `time_of_day`).
* `date` → `datetime` (formato `YYYY-MM-DD`).
* Colunas auxiliares: `_year`, `_month`, `_day`.
* `season` (meteorológica, por meses):

  * Winter: Dez–Fev | Spring: Mar–Mai | Summer: Jun–Ago | Autumn: Set–Nov
* `season_astro` (astronômica, **sem fuso**, por datas aproximadas):

  * Winter: [21/12, 20/03)
  * Spring: [20/03, 21/06)
  * Summer: [21/06, 22/09)
  * Autumn: [22/09, 21/12)

---

## 📊 Análises principais

1. **Consumo por hora do dia**
   Agrupamento por `hour_of_day` (0–23) + tabela e gráfico. *Ticks* 0..23.

2. **Consumo por dia da semana**
   `weekday` normalizado (Monday→Sunday) + série/tabela e gráfico (linha ou barras). Possibilidade de fixar eixo Y (ex.: 0–580) para manter rótulos visíveis.

3. **Consumo por mês do ano com separação por ano**
   `date` → `_year` + `month_name` categórico. Pivot `month_name × _year` e gráfico de **barras agrupadas**. Observação relevante do dado: **Março aparece em 2024 e 2025**; **não há Jan/Fev de 2024**.

4. **Turno × tipo de café (Top‑N)**
   `time_of_day` normalizado. Seleção do **Top‑N cafés** (N=5/7) pelo total e pivot `time_of_day × coffee_name` apenas para esse conjunto. Gráfico de **barras agrupadas** (legenda posicionada fora quando necessário). Alternativas: **heatmap** ou **barras 100% empilhadas** quando há muitas categorias.

5. **Estações do ano (duas abordagens)**

   * **Meteorológica (por meses)**: simples, mas **sensível ao Março duplicado** (distorce comparações inverno/outono).
   * **Atronômica (por datas)**: cortes por dia (20/03, 21/06, 22/09, 21/12). Corrige a alocação de **Dezembro** (21/12→Winter) e **Março** (20/03→Spring).
     Além disso, **separação por ano** para controlar o impacto da duplicidade de Março.

> Os *prints* no notebook usam `to_string(name=False, dtype=False)` para tabelas limpas e `fig, ax = plt.subplots(...); df.plot(ax=ax)` para evitar `<Figure ... with 0 Axes>`. Legendas longas vão para fora do gráfico (`bbox_to_anchor`).

---

## ❓ Hipóteses e perguntas (alinhadas ao storytelling)

**H1 — Top 5 por turno**
*Pergunta:* “Entre manhã, tarde e noite, quais são os **5 tipos de café mais vendidos** em cada turno e **como muda o ranking** entre os turnos?”

**H2 — Estações por meses (mapeamento simples)**
*Pergunta:* “Usando a classificação de estações por **faixas de meses** (Dez–Fev, Mar–Mai, Jun–Ago, Set–Nov), o **inverno** tem consumo total maior do que as demais estações?”

**H3 — Estações por datas específicas (cortes astronômicos)**
*Pergunta:* “Aplicando os **cortes de datas** (~20/03, ~21/06, ~22/09, ~21/12), o **inverno** passa a liderar o consumo quando comparado ao outono?”

**H4 — Estações por ano (tratando Março duplicado)**
*Pergunta:* “Separando por ano (2024 e 2025) para tratar a duplicidade de março, e definindo inverno como **21/12 a 20/03**, o **inverno ainda é a estação de maior consumo** em cada ano? Qual é o total de registros do inverno em cada ano e qual a diferença entre eles — em número de registros e em porcentagem?”

---

## 🧠 Principais decisões práticas (que aparecem no notebook)

* **Saídas de texto limpas:** `Series.to_string(name=False, dtype=False)`.
* **Nada de figuras vazias:** usar `plt.subplots` + `ax=...` nos `plot`s do Pandas.
* **Legenda fora do gráfico** quando há muitas categorias: `bbox_to_anchor=(1.02,1)` + `tight_layout(rect=[0,0,0.82,1])`.
* **Limite fixo do eixo Y** quando necessário (ex.: `ax.set_ylim(0, 580)`).
* **Ordenação categórica** consistente para `weekday`, `month_name` e `season`.

---

## ⚠️ Limitações do dataset

* **Março duplicado** (em 2024 e 2025) afeta comparações por estação se **não** houver corte por data.
* **Ausência de Jan/Fev/2024** altera a leitura de agregações mensais por ano.
* Métrica = **contagem de registros** (não receita/volume físico).

---

## 🚀 Como executar

1. Ambiente e libs

```bash
python -m venv .venv && source .venv/bin/activate   # (Windows: .venv\Scripts\activate)
pip install -U pip pandas matplotlib python-dateutil
```

2. Estrutura

```
data/
  Coffe_sales_tratado3.csv
  Coffe_sales_tratado3.xls
notebooks/
  fase2.ipynb
```

3. Rode o notebook

* Abra `notebooks/fase2.ipynb` no Jupyter (Lab/Notebook) e execute as células.
* Ajuste caminhos se necessário.

---

## 📌 Resultados em 1 frase (resumo)

* **Mês/ano:** Março aparece em 2024 e 2025 → gráfico mensal separado por ano.
* **Estações:** Com cortes **astronômicos** por data e separação por **ano**, o **inverno** aparece como a estação de maior consumo, corrigindo o viés de março duplicado e de dezembro inteiro no outono.

---

## 📜 Licença

Projeto educacional/demonstrativo. Ajuste a licença (ex.: MIT) conforme necessário.

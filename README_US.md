---

# ☕️ Coffee Consumption Analysis with Storytelling 🎬

Project focused on **exploration and visualization** of coffee consumption using a transactional dataset. All analyses consider **record counts** (rows) as a *proxy* for consumption.

> 💡 The storytelling (step-by-step, hypotheses, and decisions) is documented in the notebook **`notebooks/fase2.ipynb`** in Markdown cells.

---

## 📢 About

This is an **academic project** from a **Data Science** course.

* **Version 1:** focused on **dataset preparation** and understanding the **data logic** (cleaning, standardization, and first diagnostics).
* **Version 2:** focused on **exploratory analysis**, **visualization**, **storytelling**, and **hypothesis testing/validation** (the content of this repository).

> 🎓 Goal: practice the full analytical cycle — **preparation → exploration → hypothesis → visualization → communication**.

---

## 🎯 Objective

Understand how coffee consumption varies by **hour of the day**, **day of the week**, **month of the year**, and **seasons**, and test hypotheses about **time shifts** (morning/afternoon/evening) and **seasonality**.

---

## 🧰 Stack

* 🐍 **Python**: Pandas, Matplotlib, **python-dateutil**, **calendar** (stdlib)
* 📓 **Jupyter Notebook**: `notebooks/fase2.ipynb` (with storytelling)
* 🗃️ **Clean dataset**: `data/Coffe_sales_tratado3.csv`

---

## 🧾 Data (main columns)

* 📆 `date` (YYYY-MM-DD)
* ⏰ `hour_of_day` (0–23)
* 🗓️ `weekday` (Monday..Sunday)
* 📅 `month_name` (January..December)
* 🌇 `time_of_day` (Morning, Afternoon, Evening, Night)
* 🧉 `coffee_name` (coffee categories)
* 🛠️ Engineered: `_year`, `_month`, `_day`, `season` (meteorological), `season_astro` (astronomical)

> 📏 **Metric:** row count (not revenue or physical volume).

---

## 🧼 Preprocessing

* ✍️ Normalization of categorical variables (`weekday`, `time_of_day`).
* 🔁 Conversion of `date` → `datetime`.
* 🧱 Creation of auxiliary columns (`_year`, `_month`, `_day`).
* ❄️ **Season (meteorological)** by months:

  * Winter: Dec–Feb
  * Spring: Mar–May
  * Summer: Jun–Aug
  * Autumn: Sep–Nov
* 🌞 **Season_astro (astronomical, approx. dates)**:

  * Winter: [21/12, 20/03)
  * Spring: [20/03, 21/06)
  * Summer: [21/06, 22/09)
  * Autumn: [22/09, 21/12)

---

## 📊 Main Analyses

1. ⏱️ **Consumption by hour of the day** → grouped 0–23, table and bar chart.
2. 🗓️ **By weekday** → normalized (Monday→Sunday), line chart with optional fixed Y-axis (`ax.set_ylim(0, 580)`).
3. 📅 **By month × year** → grouped bars; note that March appears in both 2024 and 2025; Jan/Feb 2024 are missing.
4. 👨‍🍳 **Shift × coffee type (Top-N)** → selection of Top-5/7 most sold coffees; grouped bar chart.
5. 🍂 **Seasons (two approaches)** →

   * **Meteorological (by months):** affected by duplicated March.
   * **Astronomical (by dates):** daily cuts fix allocation of March and December; year separation included.

---

## ❓ Key Questions & Hypotheses

* **Main Question:**
  👉 *“How do coffee consumption patterns vary throughout the day, week, and months? Are there predictable peak times that can guide operations?”*

* **H1 — Top 5 by shift**
  👉 *“Across morning, afternoon, and evening, which are the **5 most sold coffee types** and how does the ranking change?”*

* **H2 — Seasons (month-based classification)**
  👉 *“Using the month-based classification (Dec–Feb, Mar–May, Jun–Aug, Sep–Nov), does **winter** show the highest consumption?”*

* **H3 — Seasons (astronomical dates)**
  👉 *“Applying date cuts (~20/03, ~21/06, ~22/09, ~21/12), does **winter** surpass autumn in total consumption?”*

* **H4 — Seasons by year (handling March duplication)**
  👉 *“Separating by year (2024 & 2025) to handle March duplication, and defining winter as **21/12 → 20/03**, does **winter still lead** in each year? What is the difference in total counts?”*

---

## 🧠 Practical Decisions

* 🧾 Clean table outputs: `to_string(name=False, dtype=False)`
* 🖼️ No empty figures: `plt.subplots` + `ax=...`
* 🏷️ Legends placed outside when necessary (`bbox_to_anchor`)
* 📈 Fixed Y-axis when useful (`ax.set_ylim(0, 580)`)
* 🧱 Ordered categories for `weekday`, `month_name`, `season`

---

## ⚠️ Dataset Limitations

* 🔁 **Duplicated March** (2024 & 2025) affects seasonal comparisons
* 🕳️ **Missing Jan/Feb 2024** impacts month-year aggregation
* 🧮 Metric = row count (not revenue or physical volume)

---

## 🎯 Learnings

* ✅ Recognition and standardization of **data types** in Pandas (`date` → `datetime`, ordered categoricals for `weekday`, `month_name`, `season`).
* ✅ Selection of **appropriate visualizations** (line vs. bars; **grouped bars** for month×year; simple bars for hour/day/seasons).
* ✅ Application of **filters/segmentations** for clarity: **Top-5 by shift**, **year separation in month plots** (handling duplicated March), and **astronomical cuts for seasons** (21/12–20/03; 20/03–21/06; 21/06–22/09; 22/09–21/12).
* ✅ **Formatting and design** for readability (external legends, axis limits like `ax.set_ylim(0, 580)`, data labels, and **clean tables**).
* ✅ Understanding **dataset biases and peculiarities** (duplicated March; missing Jan/Feb-2024) and their analytical impact.

---

## 🗂️ Repository Structure

```
├── data/
│   ├── Coffe_sales_tratado3.csv   
├── notebooks/
│   └── fase2.ipynb
└── README.md
```

---
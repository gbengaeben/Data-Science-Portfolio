# ☕ Thermal Dynamics & Coffee Cooling Analysis

An Exploratory Data Analysis (EDA) project examining how different container designs (insulated with lid, insulated without lid, and ceramic mug) impact liquid thermal retention over a 50-minute observation window.

---

## 📊 Project Overview
When enjoying hot coffee, container thermodynamics play a critical role in temperature retention. This project leverages **Python, Pandas, Seaborn, and Matplotlib** to analyze temperature decline, evaluate heat loss velocity, and visualize Newton's Law of Cooling in action.

---

## 🛠️ Tech Stack & Tools
* **Language:** Python
* **Data Manipulation:** Pandas (`.diff()` feature engineering, indexing)
* **Visualization:** Seaborn (`sns.lineplot`, `sns.regplot`, `sns.heatmap`) & Matplotlib
* **Environment:** Jupyter Notebook / Google Colab

---

## 📈 Key Findings & Insights

1. **Evaporative Cooling Impact:**  
   The insulated cup *with a lid* lost **3.0°C less heat** than the lidless container in the first 10 minutes, proving that blocking steam escape (evaporation) is essential for initial heat retention.

2. **Material Conduction:**  
   The **Ceramic Mug** suffered the greatest heat loss, dropping **23.1°C** overall (from 83.2°C to 60.1°C), illustrating continuous thermal transfer through uninsulated ceramic walls.

3. **The Optimal Drinking Window ("Orange Zone"):**  
   Heatmap analysis identified the **55°C–65°C range** as the ideal thermal zone for safe, flavor-rich consumption. Ceramic drops into this window rapidly, while insulated containers hold higher temperatures significantly longer.

4. **Newton's Law of Cooling:**  
   Scatter regression models confirm that heat decay slows down non-linearly as liquid temperature approaches ambient room temperature.

---

## 📁 Repository Structure
```text
├── hot_coffee.csv              # Raw thermal decay dataset
├── hot_coffee_analysis.ipynb   # Complete Python EDA notebook
└── README.md                   # Project documentation & insights

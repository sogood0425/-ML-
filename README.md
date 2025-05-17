# 🧪 Diabetes Prediction Using Machine Learning

## 📌 Motivation  
近年來，隨著飲食習慣和生活方式的改變，糖尿病的發生率在全球呈現持續上升的趨勢。  
透過分析糖尿病預測的資料集，希望能深入了解糖尿病的相關資訊，並探討導致糖尿病的主要風險因子。

## 🎯 Objective  
本專案目的是利用病人的健康數據（如血糖、血壓、BMI 等指標）來預測其是否可能罹患糖尿病。  
希望透過建立機器學習模型，提供病患健康風險評估的依據，達到預警效果，幫助醫療人員提早介入或進行治療建議。

📂 **資料來源**: [Kaggle - Pima Indians Diabetes Database](https://www.kaggle.com/datasets/kumargh/pimaindiansdiabetescsv)

---

## 📊 Dataset Summary
- **Features**:  
  `'Pregnancies', 'Glucose', 'BloodPressure', 'SkinThickness', 'Insulin', 'BMI', 'DiabetesPedigreeFunction', 'Age'`
- **Target**:  
  `'Class'` （0 = Non-diabetic, 1 = Diabetic）

---

## 🧼 Step 1: Feature Cleaning

### 🔹 處理不合理的 0 值  
某些生理指標如 `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI` 出現 0 是不合理的，視為遺漏值，替換為 NaN。

### 🔹 缺失值填補  
根據 `Class` (是否患有糖尿病)，分別計算各欄位平均值進行填補，保留分群特性。

---

## 🔁 Step 2: Feature Transformation

### 🔹 離群值處理（IQR）  
使用 IQR（四分位距）檢測離群值，避免極端值影響模型訓練。

### 🔹 特徵縮放  
- `Glucose`：使用 **MinMaxScaler**（因無明顯離群值）  
- 其他欄位（如 `Insulin`, `BloodPressure`, `SkinThickness`, `DiabetesPedigreeFunction`）：使用 **RobustScaler**

### 🔹 分箱處理  
- **BMI 分為四類**：
  - `0`: Underweight  
  - `1`: Healthy  
  - `2`: Overweight  
  - `3`: Obese

- **年齡分箱**：
  - 區間：[10-20), [20-30), ..., 80+
  - Label：1–8

> 分箱的目的是降低連續變數對模型的干擾，提升模型對類別邏輯的學習能力。

---

## 📌 Step 3: Feature Selection

### 🔹 以相關係數（Correlation）作為特徵選擇依據  
我們計算了每個特徵與 `Class` 的相關係數：

- `Glucose`：0.49  
- `Insulin`：0.41  
- `SkinThickness`：0.31  
- 其餘欄位相關性較低

但整體來看，並無出現極端顯著或明顯不相關的特徵，因此：

> **選擇保留所有特徵，視為潛在重要變數，以完整納入模型訓練。**

---

## 🤖 模型訓練與評估

### 🔹 Logistic Regression  
- **Accuracy**: 83.08%  
- **Confusion Matrix**:  
[[119 17]
[ 17 48]]

- **特點**：簡單、可解釋性強，作為 baseline 模型效果良好。

---

### 🔹 XGBoost Classifier  
- **Accuracy**: 87.56%  
- **Confusion Matrix**:  
[[126 10]
[ 15 50]]

- **特點**：能捕捉複雜特徵交互作用，整體預測效果優於邏輯回歸。

---

## 📦 資料匯出 (for Tableau)

將 **最後 100 筆資料**輸出為 CSV，供後續視覺化使用。  
`inference_data.to_csv('pima-indians-diabetes_tableau.csv')`

---

## ✅ 結論 Summary

| 步驟               | 說明 |
|--------------------|------|
| **Feature Cleaning**      | 處理遺漏值與不合理的 0 值，提升資料可信度 |
| **Feature Transformation** | 特徵正規化與分箱，降低模型對極端值的敏感度 |
| **Feature Selection**     | 保留與糖尿病最相關的特徵以提升模型效能 |
| **Modeling**              | 建立兩種模型進行比較，XGBoost 顯示出更好的預測效果 |


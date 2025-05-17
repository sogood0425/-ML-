🧪 Diabetes Prediction Using Machine Learning
📌 動機 Motivation
近年來，隨著飲食習慣和生活方式的改變，糖尿病的發生率在全球呈現持續上升的趨勢。透過分析糖尿病預測的資料集，希望能深入了解糖尿病的相關資訊，並探討導致糖尿病的主要風險因子。

🎯 目的 Objective
本專案目的是利用病人的健康數據（如血糖、血壓、BMI 等指標）來預測其是否可能罹患糖尿病。希望透過建立機器學習模型，提供病患健康風險評估的依據，達到預警效果，幫助醫療人員提早介入或進行治療建議。

資料來源: Kaggle - Pima Indians Diabetes Database[https://www.kaggle.com/datasets/kumargh/pimaindiansdiabetescsv]

📊 Dataset Summary
Features:
'Pregnancies', 'Glucose', 'BloodPressure', 'SkinThickness', 'Insulin', 'BMI', 'DiabetesPedigreeFunction', 'Age'

Target:
'Class' (0 = Non-diabetic, 1 = Diabetic)

🧼 Step 1: Feature Cleaning
🔹 處理不合理的 0 值
某些生理指標如 Glucose、BloodPressure、SkinThickness、Insulin 和 BMI 出現 0 是不合理的，代表遺漏值，因此替換為 NaN。

🔹 缺失值填補
根據病患是否患有糖尿病 (Class 0 或 1)，分別計算各欄位的平均值來填補缺失。

🔁 Step 2: Feature Transformation
🔹 IQR 方法檢測離群值
使用 IQR（四分位距）方法檢測離群值，避免極端值影響模型訓練。

🔹 特徵縮放
Glucose：使用 MinMaxScaler（無明顯離群值）。

其他有離群值欄位使用 RobustScaler，如 Insulin、BloodPressure。

🔹 分箱處理
BMI 分為四類：

0: Underweight

1: Healthy

2: Overweight

3: Obese

年齡分箱：

分為區間 [10-20), [20-30), ..., 80+，對應 label 1–8

分箱的目的在於降低連續數值的敏感度，強化模型對於分段特徵的理解能力。

📌 Step 3: Feature Selection
🔹 以相關係數（Correlation）作為特徵選擇依據
我們計算了每個特徵與目標變數 Class（是否患有糖尿病）之間的相關係數：

然而，從結果來看，各欄位與 Class 的相關性並未呈現極端明顯的高低差異。因此：

雖然進行了相關係數分析，但在此階段並未剔除任一特徵，而是選擇將所有特徵視為潛在重要特徵，完整納入模型訓練，以保留更多資訊。

🤖 模型訓練與評估
🔹 Logistic Regression
Accuracy: 83.08%

混淆矩陣：

[[119  17]
 [ 17  48]]
 
優點在於簡單、可解釋性強，作為 baseline 模型效果良好。

🔹 XGBoost Classifier
Accuracy: 87.56%

混淆矩陣：

[[126  10]
 [ 15  50]]
 
更強大的分類器，能捕捉更複雜的特徵交互作用，整體表現優於邏輯回歸。

📦 資料匯出 (for Tableau)
將最後 100 筆資料導出供後續視覺化使用：


✅ 結論 Summary
步驟	說明
Feature Cleaning	處理遺漏值與不合理的 0 值，提升資料可信度
Feature Transformation	特徵正規化與分箱，降低模型對極端值的敏感度
Feature Selection	保留與糖尿病最相關的特徵以提升模型效能
Modeling	建立兩種模型進行比較，XGBoost 顯示出更好的預測效果

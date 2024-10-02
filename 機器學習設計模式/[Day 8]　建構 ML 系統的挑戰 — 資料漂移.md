# 摘要
　　當資料隨時間變動，可能導致原有的模型無法得到預期結果。資料漂移、概念漂移和資料庫漂移是資料變動的三種形式，分別影響模型的輸入特徵分布、輸出標籤和資料結構。為了應對這些變動，可以採用「橋接」策略，將舊資料調整為新格式，確保模型在過渡期間繼續運行並兼容新資料。
# 資料漂移
## 定義
　　在學習機器學習的初期，我曾學到這樣的概念：「可以將機器學習模型視為一個一對一的函數，測試樣本是輸入 x，經過 f(x) 這個函數後，會得到一個固定的輸出 y。」也就是說，在模型沒有吃到新的訓練資料的狀況下，整體流程應該要是靜態的，相同的輸入會得到相同的結果（類似的概念請參考 [[[Day 7]　建構 ML 系統的挑戰 — 再現性]]）。然而，資料是會發生變動的，如果函數相同，輸入改變，這個函數可能就無法得出我們需要的結果。
　　資料漂移指的就是資料分布隨時間改變的現象，例如類別分布與過去不同，導致模型輸出的結果無法符合當前的需求。
## 相似的概念
- 資料漂移 (data drift)：指的是**輸入特徵的統計分布**變化。例如，數值特徵的平均值或標準差發生變化，或是類別特徵的分布比例改變。這種變化會影響模型的輸出結果，因為模型是基於過去的數據分布進行訓練的。
- 概念漂移 (concept drift)：當**輸出標籤或數值的統計性質**變化時，模型對應的預測目標也會改變。這可能是模型需要重新學習的情況，因為資料與過去不同。
- 資料庫漂移 (database drift, schema drift)：**資料的結構**發生改變，例如特徵欄位的數量、類型或名稱發生變化。這種情況需要模型的輸入格式重新調整。
## 解決方案：Bridged Schema
　　嘗試將訓練資料從舊有的、原始的資料格式調整成一個更符合現實需求的新格式。由於新資料的累積需要時間，我們不可能在這段期間完全停滯模型的調整，因此可以採取「橋接 (bridge)」的策略：調整舊資料的格式、單位或計算方式，來過渡模型更新期。這樣不僅能夠確保模型在過渡期間繼續運行，也能讓新的模型順利兼容並利用舊資料，具體的實施方法和問題將在明天進一步說明。
# 參考資料
- Machine Learning Design Patterns: Solutions to Common Challenges in Data Preparation, Model Building, and MLOps CH1, CH6 Design Pattern:  Bridged Schema
- [[Day 04] 部署模型的挑戰 — 資料也懂超級變變變!?](https://ithelp.ithome.com.tw/m/articles/10267729)
- [Data Drift Vs Concept Drift in Machine learning](https://medium.com/@pooja93palod/data-drift-vs-concept-drift-in-machine-learning-9b49e763f584)
- [Detecting schema and data Drift within automation - How Azure can help?](https://www.linkedin.com/pulse/detecting-schema-data-drift-within-automation-how-azure-jayanty/)
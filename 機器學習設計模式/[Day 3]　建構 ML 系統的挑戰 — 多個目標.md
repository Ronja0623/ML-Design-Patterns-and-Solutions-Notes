# 摘要
　　在建構 ML 系統時，定義目標是整個專案的核心，不過問題往往相當複雜。為了將目標有效地融入系統的設計中，今天我們將介紹常見的五大類方法，包含調整損失函數、調整決策閾值、樣本重新採樣、自訂評估指標、自訂模型結構。
# 多個目標
　　我們在[昨天](https://ithelp.ithome.com.tw/articles/10351319)討論到整個 MLOps 的核心是定義問題，也提到在定義問題時，需求面可能包含不只一方的要求，也就是說，不同方對於「成功的模型」存在不統一的期待。我們也需要考慮資料面的問題，畢竟如果沒有良好的資料，就不用考慮進一步實作的問題了。關鍵特徵是怎麼分布的呢？我們是否需要處理資料集不平衡的問題？
　　我們需要以 ML 的角度嘗試解讀所有的需求，然後將確立的目標轉換到模型中，常見的做法包含：
## 調整損失函數 (Loss Function)
  - 加權損失函數 (Weighted Loss Function)：對某些類別給予更高的權重，使模型更關注部分類別。
	  - 常用於處理不平衡資料，例如罕見事件預測。
	  - 範例：
		  - [Use weighted loss function to solve imbalanced data classification problems](https://medium.com/@zergtant/use-weighted-loss-function-to-solve-imbalanced-data-classification-problems-749237f38b75)：裡面有使用 pytorch 的程式範例
		  - [DNN AND CNN WITH WEIGHTED AND MULTI-TASK LOSS FUNCTIONS FOR AUDIO EVENT DETECTION](https://arxiv.org/pdf/1708.03211)：論文中使用了兩類樣本，其中一類樣本量遠比另一類的樣本量多（不平衡資料），所以它對量少的那類樣本加重懲罰。
  - 代價敏感學習 (Cost-Sensitive Learning)：調整不同預測錯誤的代價，賦予不同錯誤類型不同成本，讓模型在訓練時學習到不同錯誤的嚴重性。
	  - 範例：
		  - [Cost-Sensitive Learning: Beyond the Accuracy in Imbalanced Classification](https://www.blog.trainindata.com/cost-sensitive-learning-for-imbalanced-data/)：裡面有程式範例
## 調整決策閾值 (Decision Threshold)
- 方法一：做一系列的實驗，以得到不同決策數據對某些指標（例如精確度、準確度、召回率等）的影響。在得到實驗數據之後，我們可以採用有助於得到目標的演算法取得最佳決策閾值。 -> 將決策閾值作為超參數
	- 範例：
		- [ROC 曲線上的最佳閾值：Youden Index 與圖解法介紹](https://haosquare.com/roc-curve-best-cutoff/)：這篇文章從統計的角度介紹了數個計算最佳閾值的演算法，不過需要注意演算法是否符合需求，因為不是每一個演算法計算出的最佳閾值訓練出的模型都會符合實際需求。
- 方法二：採用合適的演算法，使模型取得符合要求的決策閾值。 -> 決策閾值會在訓練中自動調整
##  樣本重新採樣 (Resampling)
- 過採樣（Oversampling）：增加樣本量較少的類別的樣本數量。
	- 複製樣本量較少的類別的樣本
	- SMOTE (Synthetic Minority Over-sampling Technique)：合成少數樣本
- 欠採樣（Undersampling）：減少多數類別的樣本數量，以平衡數據集中的類別分佈。詳細原理可以參考[這篇文章](https://medium.com/%E6%95%B8%E5%AD%B8-%E4%BA%BA%E5%B7%A5%E6%99%BA%E6%85%A7%E8%88%87%E8%9F%92%E8%9B%87/smote-enn-%E8%A7%A3%E6%B1%BA%E6%95%B8%E6%93%9A%E4%B8%8D%E5%B9%B3%E8%A1%A1%E5%BB%BA%E6%A8%A1%E7%9A%84%E6%8E%A1%E6%A8%A3%E6%96%B9%E6%B3%95-cdb6324b711e)。
## 自訂評估指標 (Custom Evaluation Metrics)
　　根據預測目標的具體需求，尋找最適合或乾脆自己定義評估指標。舉例來說，如果我們的目標是偵測一個群體是否罹患某致死機率極高的疾病，我們的目標就是竭盡全力降低偽陰性的數量，也因此我們的重點評估指標應該會是召回率，而非精確度。
　　常見的指標意義可以進一步參考[這篇文章](https://hackmd.io/@CynthiaChuang/Common-Evaluation-MetricAccuracy-Precision-Recall-F1-ROCAUC-and-PRAUC)。
## 自訂模型結構 (Custom Model Struture)
- 注意力機制 (Attention Mechanism)：用於讓模型專注於某部分特徵，常用於自然語言處理 (NLP)。
- 集成學習 (Ensemble Learning)：類似於分治法（Divide and Conquer）的概念，但重點不在於每個模型應達成不同的目標，而是在於透過結合多個不同模型的預測結果來提升整體的準確性和穩定性。這些模型不需要是完美的，但透過集成，它們的綜合表現通常會優於單一模型。
# 延伸閱讀：不平衡的資料集
　　以上介紹的部分方法常用來處理資料不平衡的問題，下面推薦的這篇論文就介紹了前述提到的部分方法可以如何用來處理不平衡的資料集。
　　論文：[A Systematic Review on Imbalanced Learning Methods in Intelligent Fault Diagnosis](https://ieeexplore.ieee.org/document/10049129/)
　　這是一篇 Review 類型的論文。它設定的情境是當我們在做異常檢測的時候，通常大多數的資料都會是正常的，可以想像，在工廠裡，機器故障的時間不會和正常運作的時間一樣多，這就是為什麼我們取得的資料集會有嚴重不平衡的問題。
　　在它框定的情境下，它分類並介紹了常見的解決方法，如果有進一步研究的需求，可以透過這篇論文快速檢索符合需求的其他論文。
# 參考資料
- [Use weighted loss function to solve imbalanced data classification problems](https://medium.com/@zergtant/use-weighted-loss-function-to-solve-imbalanced-data-classification-problems-749237f38b75)
- [DNN AND CNN WITH WEIGHTED AND MULTI-TASK LOSS FUNCTIONS FOR AUDIO EVENT DETECTION](https://arxiv.org/pdf/1708.03211)
- [Cost-Sensitive Learning: Beyond the Accuracy in Imbalanced Classification](https://www.blog.trainindata.com/cost-sensitive-learning-for-imbalanced-data/)
- [ROC 曲線上的最佳閾值：Youden Index 與圖解法介紹](https://haosquare.com/roc-curve-best-cutoff/)
- [SMOTE + ENN : 解決數據不平衡建模的採樣方法](https://medium.com/%E6%95%B8%E5%AD%B8-%E4%BA%BA%E5%B7%A5%E6%99%BA%E6%85%A7%E8%88%87%E8%9F%92%E8%9B%87/smote-enn-%E8%A7%A3%E6%B1%BA%E6%95%B8%E6%93%9A%E4%B8%8D%E5%B9%B3%E8%A1%A1%E5%BB%BA%E6%A8%A1%E7%9A%84%E6%8E%A1%E6%A8%A3%E6%96%B9%E6%B3%95-cdb6324b711e)
- [常見評價指標：Accuracy、Precision、Recall、F1、ROC-AUC 與 PR-AUC](https://hackmd.io/@CynthiaChuang/Common-Evaluation-MetricAccuracy-Precision-Recall-F1-ROCAUC-and-PRAUC)
- [A Systematic Review on Imbalanced Learning Methods in Intelligent Fault Diagnosis](https://ieeexplore.ieee.org/document/10049129/)
- Machine Learning Design Patterns: Solutions to Common Challenges in Data Preparation, Model Building, and MLOps CH1, CH8
# 摘要
# 轉換
　　將原始輸入轉為模型可接受的資料表示型態，屬於特徵工程的一部份。關於資料表示與特徵工程的問題我們曾在 [[[Day 11]　問題類型：資料表示 (Data Presentation)]] 中討論。
## 解決方案
　　利用使用的框架中的 transform 定義轉換所需的邏輯和產物，例如 BigQuery ML 中的 TRANSFORM ，使其能自動執行這些轉換。
## 替代方案
　　假如採用的框架沒有內建支援的 transform 工具，那就要仔細設計模型的結構，讓它能夠輕鬆重現在訓練期間執行地轉換。有一個方法是儲存轉換後的特徵，這個方法我們也曾在 [[[Day 13]　設計模式：嵌入 (Embedding)]] 的再現性提到。或者也可以嘗試將轉換存入模型圖。
### 將轉換存入模型圖：以 Tensorflow 和 Keras 為例
1. 為每個輸入建立一個 Input 層
2. 建立用於轉換特徵的字典，如果需要的話
3. 1 建立 Keras Lambda 層或 Preprocessing 層來處理資料的對應
3. 2 建立 Keras Lambda 層來計算值，例如 Euclidean 距離
4. 將用於轉換的層集中並接入 DenseFeature 層
5. 再進入神經網路之前，可以維持轉換後的原樣、編碼、分組等
6. 建立剩餘的涉及訓練的模型

　　總結來說，這種做法的層數與目標：
1. Inputs Layer
2. Transform Layer
3. 結合被轉換過的特徵的 DenseFeature Layer
4. 之後就是正常的模型架構
#### 代價
　　如果轉換程序相當複雜，那麼每次訓練都要對原始資料進行轉換便會大幅增加計算成本。
# 補充
## 訓練／伺服傾斜 (Training-Serving Skew)
## 模型圖
## 特徵攔
　　書中提到 Tensorflow 支援特徵欄的概念，這裡的特徵欄是什麼？
## Serving Function
## Batch Serving
# 參考資料
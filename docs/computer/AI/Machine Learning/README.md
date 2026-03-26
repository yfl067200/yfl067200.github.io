機械學習 (Machine Learning，ML) 是從雜換無章的資料中找出有用的資訊。換句話說，機械學習就是試著找出數學公式，用來對資料進行分類，或是依照分類預測出新資訊的值 (迴歸)。

分類可分為二元分類 (Binary Classification) 與多元分類 (Multiple Classification) 兩大類

# 術語

以下為機械學習常用的術語。

| 術語                         | 解釋                                                                                                                                                      |
| :------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 模型 (model)                 | 將特徵作為輸入的演算法                                                                                                                                             |
| 特徵 (feature)               | Raw Data 中某一項資訊的數字化資料<p>如果該資訊非數值，該數值可能會 index 等                                                                                                         |
| 特徵工程 (feature engineering) | 將 Raw Data 中挑選出重點資訊<p>將該資訊轉換成適合演算法理解的值                                                                                                                  |
| 數量 (Scalar)                | 單一的數值化資訊                                                                                                                                                |
| 向量 (Vector)                | 一個==有序的數值化==資訊列表<p>機械學習中，大量使用向量作為輸入。因此在特徵工程一章，會進一步說明如何建立好的向量<p>通常向量會被視覺化為幾何空間中的一個點。大多數情況下，會從原點 (0,[...,]0) 劃一條線或是箭頭到該點<p>向量所表現的空間被稱為向量空間 (vector space) |
| 矩陣 (Matrix)                |                                                                                                                                                         |

# 相關重點

- [特徵工程](docs/computer/AI/Feature%20Engineering/README.md)
- [訓練](./Training/README)
    - [監督式學習](./Training/Supervised%20Learning)
        - [K 近鄰演算法](./Training/algorithms/K-Nearest%20Neighbors)
        - [線性模型](./Training/algorithms/Linear%20Models)
        - [樸素貝葉斯分類法](./Training/algorithms/Naive%20Bayes%20Classifiers)
        - [決策樹](./Training/algorithms/Decision%20Trees)
        - [決策樹集成](Ensembles%20of%20Decision%20Trees.md)
        - [核化支援向量機](./Training/algorithms/Kernelized%20Support%20Vector%20Machines)
        - [神經網路](./Training/algorithms/Neural%20Networks)
    - [非監督式學習](./Training/Unsupervised%20Learning)
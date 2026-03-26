機械學習 (Machine Learning，ML) 是從雜換無章的資料中找出有用的資訊。換句話說，機械學習就是試著找出數學公式，用來對資料進行分類，或是依照分類預測出新資訊的值 (迴歸)。

> ![NOTE]
> 分類可分為二元分類 (Binary Classification) 與多元分類 (Multiple Classification) 兩大類，取決於最後分類的數量

在機械學習中，我們不在提供複雜的規則讓機器執行。我們提供一個模型 (model) - 讓機器可以對輸入的資料進行運算的演算法，並提供機器得到錯誤結果之後，可以微調演算法的方法。我們期待透過這個方式，假以時日機器將能夠正確的解答問題。

從數學的角度來說模型就是一個多元方程式 $h(x, \theta)$；其中 $x$ 為陣列 (vector) 型態的輸入，而 $\theta$ 為另一個陣列型態的參數 (即為演算法可調整的值)，且 $\theta$ 的數量為 $x$ 的數量 + 1。模型的結果將會是 1 或是 -1 (二元分類)，公式如下：

$h(x, \theta) = \left\{ \begin{array}{rcl} -1 & \mbox{~if x~} \cdot \left[ \begin{array}{c} \theta_1 \\ \vdots \\ \theta_n \end{array} \right] + \theta_0 \lt 0 \\ 1 & \mbox{~if x~} \cdot \left[ \begin{array}{c} \theta_1 \\ \vdots \\ \theta_n \end{array} \right] + \theta_0 \ge 0 \end{array}\right.$


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

- [特徵工程](docs/Computer/AI/Feature%20Engineering/README.md)
- [訓練](docs/Computer/AI/Machine%20Learning/Training/README.md)
    - [監督式學習](Supervised%20Learning.md)
        - [K 近鄰演算法](K-Nearest%20Neighbors.md)
        - [線性模型](Linear%20Models.md)
        - [樸素貝葉斯分類法](Naive%20Bayes%20Classifiers.md)
        - [決策樹](Decision%20Trees.md)
        - [決策樹集成](Ensembles%20of%20Decision%20Trees.md)
        - [核化支援向量機](Kernelized%20Support%20Vector%20Machines.md)
        - [神經網路](Neural%20Networks.md)
    - [非監督式學習](Unsupervised%20Learning.md)
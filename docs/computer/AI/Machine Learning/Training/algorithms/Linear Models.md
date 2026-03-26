線性模型 (Linear Models) 在實務上也廣泛被使用。透過訓練資料，建立線性函式用以進行分類與迴歸。
分類用的線性函式公式如下。其中的 $w[n]$ 可以視為特徵 $f[n]$ 的係數 (coefficients)，而 $f(x)$ 為各特徵值與截距 (intercept) $\beta$ 的合。這個線性函式即為決策邊界

$f(x) = (w[0] * f[0]) + (w[1] * f[1]) + ... + (w[n] * f[n]) + \beta$

線性模型基本上就是一個 N 元方程式 (對應 N 個特徵)，我們只是想辦法找出一個能滿足多數資料的方程式。如果將方程式轉換成圖形，可以得到一個將 N 維度分為兩部分的 (N - 1) 維度的超平面；以 2 維平面來說，我們只需要 1 個特徵形成的直線 (2 維的超平面)，就可將 2 維平面一分為二。以 3 維空間來說，2 個特徵形成的平面 (3 維的超平面) 亦可將 3 維空間分為兩部分。

> [!Note]
> 超平面 (Hyperplane) 是一個 N - 1 維度的子空間，將 N-維空間 (N-dimensional space) 切割成兩個部分。

> [!IMPORTANT]
> 與 [K 近鄰演算法](./K-Nearest%20Neighbors)不同，線性模型在特徵數量少的情況下，準確率低很多。這是因為特徵數越小，線性模型的公式越接近一條直線，容易成為欠似合的狀況。相反的，當特徵數越多，線性模型的結果會越接近訓練資料，反倒有機會成為過似合的狀況。

建立線性函式的演算法有很多種，主要的差異有二：

1. 從訓練資料中衡量如何調整係數 (coefficients) 與截距 (intercept) 
2. 調整的方式為何


# 分類

將新資料的特徵帶入公式，結果==小於 0== 為類別 A，==大於等於 0== 為類別 B。
## 多元分類 (Multiple Classification)

從相關的說明中，可以輕易地看出線性模型的基本原則就是將資料集合一分為二，非常適合用於二元分類 (Binary Classification)。當用於多元分類時，線性模型反倒捉襟見肘。

將二元分類擴展到多元分類常見的手法就是採用 One-vs-Rest 的機制。在線性模型中所謂的 One-vs-Rest 的機制，就是針對每一個類別建立一個線性模型函式；將新資料逐一帶入各線性模型中，最後結果最高的就是新資料所屬的類別。以下圖為例，我們可以建立 3 組線性模型，對應 3 個類別

![[Screenshot 2026-03-12 at 5.51.49 PM.png]]
# 迴歸


# 參考資料

目前在 [scikit-learn](https://scikit-learn.org/stable/) (使用 Python 的機械學習架構) 提供兩種分類演算法：

1. 邏輯迴歸 (logistic Regression)，實作於 sklearn.linear_model 中的 LogisticRegression 類別

2. 線性支援向量機 (linear support vector machines)，實作於 sklearn.svm 中的 LinearSVC 類別


目前在 [scikit-learn](https://scikit-learn.org/stable/) 提供三種迴歸演算法：

1. 普通最小平方法 (ordinary least squares, OLS)，實作於 sklearn.linear_model 中的 LinearRegression 類別

2. 嶺回歸 (Ridge regression)，實作於 sklearn.linear_model 中的 Ridge 類別

3. 套索算法 (Lasso)，全名為最小絕對值收斂和選擇算子 (least absolute shrinkage and selection operator)，實作於 sklearn.linear_model 中的 Lasso 類別
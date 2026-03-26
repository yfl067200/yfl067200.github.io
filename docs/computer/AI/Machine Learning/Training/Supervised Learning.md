監督式學習 (Supervised Learning) 是其中一種最常見且最成功的訓練方式。藉由給予樣本與結果，我們預期演算法將能從新的資料中進行分類與辨識，最後得到正確的預測結果。

# 主要問題

監督式學習要解決的兩大問題為分類與迴歸。

## 分類 (classification)
分類的主要目標是將輸入的資料，依照演算法將其分類至某(一)類資訊。而用來識別資料屬於哪一類的決策依據，被稱為==決策邊界 (Decision Boundary)==。讓演算法比對輸入的資料，依照建立的機率列表，將該資料打上已知的 label 或是 tag。

分類可分為二元分類 (Binary classification) 或是多元分類 (Multiclass classification) 兩大類。二元分類會建立類似二元樹的機制，在每個節點進行資料的分類。雖然每一層都只會分類為正類別 (positive class) 與負類別 (negative class) 兩種，但是分類結果將導致下一層的問題不同，進行更近一步的分類。

> [!NOTE]
> 二元分類中的分類結果只有正類別 (possitive class) 與負類別 (negative class) 兩種，完全取決於分類的問題為何，而非任何正面或是負面意義。舉個例字，當問題是 "是否為圓形"，正類別為 "圓形" 負類別為 "非圓形"。

與二元分類不同的是，多元分類的結果為多種已定義好的類別，這導致演算法更加的複雜。而越是複雜的演算法越有機會導致非正規化。

## 迴歸 (regression)
迴歸比較像是分類之後的下一步，基於同類別的已知資訊，預測新資料的值。譬如說已知當地一公頃土地玉米的產能，用來預測鄰近地區 1.5 公頃土地玉米的產能。

# 演算法

以下為監督式訓練常用的演算法

- [K 近鄰演算法](./algorithms/K-Nearest%20Neighbors)
    - 當特徵數量增加，預測所需的時間會越長
    - 訓練資料屬於稀疏資料集 (sparse datasets) 時，效果都不好
    - 基本上沒人使用
- [線性模型](./algorithms/Linear%20Models)
    - 準確率高，但是訓練時間長
- [樸素貝葉斯分類法](./algorithms/Naive%20Bayes%20Classifiers)
- [決策樹](./algorithms/Decision%20Trees)
    - 準確率高，但是容易導致過似合
        - 可以透過預剪枝跟後剪枝的機制降低過似合的狀況
    - 有明確方法可以改善決策樹
- [決策樹集成](Ensembles%20of%20Decision%20Trees.md)
- [核化支援向量機](./algorithms/Kernelized%20Support%20Vector%20Machines)
- [神經網路](./algorithms/Neural%20Networks)


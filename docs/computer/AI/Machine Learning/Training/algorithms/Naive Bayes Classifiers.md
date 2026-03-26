樸素貝葉斯分類法 (Naive Bayes Classifiers) 是分類法的一個家族，與[線性模型](Linear%20Models.md)非常類似，但是訓練所需的時間更短；其代價是預測結果會比線性模型稍差。之所以樸素貝葉斯分類法之所以如此有效率，



# 參考資料

目前在 [scikit-learn](https://scikit-learn.org/stable/) (使用 Python 的機械學習架構) 提供三種演算法：

1.  (GaussianNB)，實作於 sklearn.linear_model 中的 LogisticRegression 類別
    - 適用於連續資料

2.  (BernoulliNB)，實作於 sklearn.svm 中的 LinearSVC 類別
    - 適用於二進位

3. (MultinomialNB)，
    - 適用於計數



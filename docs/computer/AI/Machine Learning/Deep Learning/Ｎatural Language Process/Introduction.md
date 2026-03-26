# 上古年代

在 1970 之前，NLP 的發展主要在於文字的預處理。所謂的預處理，就是突出資料的重點，方便後後續流程更加容易處理。而預處理的重點集中於剔除無意義的資料 (Data Cleaning) 以及一般化單詞 (Data Normalization)，包涵：

- Data Cleaning
	- 除去贅詞與標點
	- 移除重複的文字
- Data Normalization
	- 小寫化 (Lower casting)
	- 將數字轉換成文字
	- 詞幹提取 (stemming)
	- 詞型還原 (Lemmatization)

> [!Note]
> Stemming 是透過規則，將字的前後綴詞等移除，進而將單詞統一化；但是統一化的單詞不一定跟字典中的單詞一致。簡單的規則類似移除 "ed" 字尾，因此像是 "jumped" 會變成 "jump"；但是 "ran" 或是 "swam" 並不會變成 "run" 或是 "swim"。
> 
> 而 Lemmatization 則是藉由分析語意，透過字典之類的對照表，將單詞轉換成現在式。因此 "ran" 或是 "swam" 會變成 "run" 或是 "swim"。

透過 data cleaning，突顯重點；經由 data normalization，可以減少分析時使用的詞語資料庫大小。整個文字預處理的流程大致如下圖
![[Screenshot 2025-01-30 at 10.33.31 AM.png]]

# 中古時代

除了機械學習 (ML，Machine Learning) 之外，另一個將 NLP 視為核心的技術為 (statistical language modeling)，這項技術透過分析現有的資訊，進而預測接下來可能會接收的資料。該項技術用於語音處理、翻譯、與產生文章等。

另一項技術為深度學習 (DL，Deep Learning)，該項技術藉由人工智慧網路來處理大量資料。用於理解文字、總結重點、

# 挑戰

NLP 的主要挑戰在於理解文字的重點。人類使用的語言要如何讓機械理解；或是讓機器能藉由文字間的關聯等級，進而找出文章中的重點
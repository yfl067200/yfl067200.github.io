
A.I. 的數學背景可以粗分為以下幾個領域：

- 數字系統 (Number System)
- 三角函數 (Trigonometric functions)
- 線性代數 (Linear Algebra)
- 總和、乘績、與對數 (Sum, product, and logarithms)


為了更加方便進行說明，先在此解釋在各自領域常用的符號

# 數字系統 (Number System)

A.I. 常用的是實數 (real number) 與複數 (complex number)

## 實數 (real number)

實數其實就是代表實數線 (real number line) 上的任一點，-5、0、或是 4.6 等都是實數。而實數線是一條從 0 開始，往左右延伸的一條直線；往左是負無窮大，往右是正無窮大；這也反映了實數是無窮盡的。通常用使用字符 ℝ 表示實數的集合

![[Screenshot 2025-05-30 at 2.22.47 PM.png]]

為了計算方便，我們通常會用整數 (integer) 來取代實數；雖然整數是實數的子集合，但是整數依舊是無窮盡的。通常使用字符 ℤ 表示整數。在自然界中，數字都是正整數；因此正整數的集合又被稱為自然數，使用字符 ℤ+ 或是 ℕ 表示自然數。

實數可以分為兩大類。第一類被稱為有理數，其字符為 ℚ (rational number)。任一可以使用==兩個整數除式==表示的實數值，即爲有理數；舉個例子，0.375 = 3 / 8 就是有理數。而不屬於有理數的實數，則歸類為無理數

整個實數家族的關係如右：ℕ ∈ ℤ ∈ ℚ ∈ ℝ，且 ℝ = ℚ + 無理數

## 複數 (complex number)

複數的代表是字符為 ℂ。複數是一個不存在於實數線上的數，所以複數不存在於實數的集合。為了解釋複數，必須使用二維空間來說明，因此要使用==兩個實數==來表示複數的值 (z)；第一個實數被稱為實部 (real part)，第二個實數被稱為虛部 (imaginary part)；其中實部多用 x 表示，而虛部多用 y 表示，而複數的表達數為 `z = x + iy`。表達式中的變數 `i`，其值為 -1 的根號，意即 `i * i = -1`。

此為使用**阿爾岡平面**表示的複數值。複數 z = x + iy = (|z|.cos θ) + i.(|z|.sin θ)
![[Screenshot 2025-05-30 at 3.51.17 PM.png]]


# 三角函數 (Trigonometric functions)

三角函數將==直角三角形==相關聯。三角函數在研究三角形和圓形等幾何形狀的性質時有著重要的作用，亦是研究振動、波、天體運動和各種[週期性現象](https://zh.wikipedia.org/wiki/%E5%91%A8%E6%9C%9F%E5%87%BD%E6%95%B0 "週期函數")的基礎數學工具。與直角三角形中直角對應的邊被稱為斜邊 (h)；斜邊與其他邊相連處被稱為內角，與某一內角 ($\theta$) 相連的邊被稱為鄰邊 (b)，另一邊被稱為對邊 (a)。

![[Screenshot 2025-06-04 at 12.09.58 PM.png]]

常見的三角函數包括：

- 正弦 ($sin$)：透過斜邊 (h) 與內角，求出對邊 (a)。***a = $sin\theta$ *h**
- 餘弦 ($cos$)：透過斜邊 (h) 與內角，求出鄰邊 (b)。**b = $cos\theta$ * h**
- 正切 ($tan$)：透過鄰邊 (b)與內角，求出對邊 (a)。**a = $tan\theta$ * b**
- 餘切 ($cot$)：透過對邊 (a) 與內角，求出鄰邊 (b)。**b = $cot\theta$ * a**
- 正割 ($sec$)：透過鄰邊 (b) 與內角，求出斜邊 (h)。**h = $sec\theta$ * b**
- 餘割 ($csc$)：透過對邊 (a) 與內角，求出斜邊 (h)。**h = $csc\theta$ * a**

從上述函式的定義，可以推導出個函式的關聯性：

$sin\theta$ 為 $csc\theta$ ：a = $sin\theta$ * h = h / $sec\theta$ --> **$sin\theta * csc\theta$ = 1**
$tan\theta$ 與 $cot\theta$： a = $tan\theta$ * b = b / $bot\theta$ --> **$tab\theta$ * $cot\theta$ = 1**
$sec\theta$ 與 $cos\theta$：b = $cos\theta$ * h = h / $sec\theta$ --> **$sec\theta$ * $cos\theta$ = 1**


 
 ## 三角恆等式

依據畢達哥拉斯原理，直角三角形兩邊平方和等同於斜邊平方：

a$^{2}$ + b$^{2}$ = h$^{2}$ => (h * sin$\theta$)$^{2}$ + (h * cos$\theta$)$^2$ = h$^{2}$ --> **sin$\theta^2$ + cos$\theta^2$ = 1**

1 + ($\cos\theta$ / $\sin\theta$)$^{2}$ = (1 / $\sin\theta$)$^{2}$ ==> 1 = ((1 - $\cos\theta$)/$\sin\theta$)$^{2}$

1 + (b/a)$^2$ = (h/a)$^2$ => 1 = ( (csc$\theta$ * a)/a )$^2$ - ((cot$\theta$ * a) / a)$^2$ --> **1 = scs$\theta^2$ - cot$\theta^2$**

(a/b)$^2$ + 1 = (h/b)$^2$ => 1 = ((sec$@t$ * b)/b)$^2$ - ((tan$\theta$ * b)/b)$^2$ --> **1 = sec$\theta^2$ - tan$\theta^2$**



# 線性代數 (Linear Algebra)

主要的內容為向量 (vector) 與矩陣 (matrix)，本主題是 A.I. 相關技術的關鍵原理。

## 向量 (vector)

在線性代數中，純量 (scalar) 用來儲存單一資料；而向量就是用來一組純量的集合。向量是多維的，==維度的大小等同於向量中儲存的純量數目==。用來表示向量的方法有兩種，一種是水平式，稱為行向量 (raw vector)；另一種則是重直式，稱為列向量 (column vector)。

![[docs/Mathematics123/figures/Screenshot 2025-05-30 at 4.20.04 PM.png]]![[Screenshot 2025-05-30 at 4.20.22 PM.png]]

行向量與列向量，可以透過轉換公式轉為另一個表示法，轉換公式以 ⊤ 表示
![[Screenshot 2025-05-30 at 4.20.41 PM.png]]  ![[Screenshot 2025-05-30 at 4.21.07 PM.png]]  

## 矩陣 (matrix)

# 總和、乘績、與對數 (Sum, product, and logarithms)

總和的字符為 𝚺，其定義為 ![[Screenshot 2025-05-30 at 4.34.18 PM.png]]

乘積的字符為 Π，其定義為 ![[Screenshot 2025-05-30 at 4.39.11 PM.png]]

對數為幕運算的逆運算。

幕運算的定義為 ![[Screenshot 2025-05-30 at 4.52.06 PM.png]]
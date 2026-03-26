在線性代數中，使用向量作為一筆資料的集合。簡單的說，就是將相關的資料放進同一個集合。向量的參數量稱為規模 (Magnitude)，即集合中的總資料數。向量的表示法分為列向量與行向量兩種，沒有特別註明，將以列向量方式表示。以下為範例：

> 某一集合 $s = \{ 1, 3, 5 \}$
> 列向量 $\vec{s} = \begin{pmatrix} 1 \\ 3 \\ 5 \end{pmatrix}$
> 行向量 $\vec{s} = \begin{pmatrix} 1 & 3 & 5 \end{pmatrix}$
> 列向量等同於行向量 $\vec{s} = \begin{pmatrix} 1 \\ 3 \\ 5 \end{pmatrix} = \begin{pmatrix} 1 & 3 & 5 \end{pmatrix}$

舉個例子來說，如果想要探討身高與體重的關係，則可以建立一個向量代表一個人的身高與體種：$\vec{v} = \begin{pmatrix} v_{1} \\ v_{2} \end{pmatrix}$ ，其中 $v_{{1}}$ 代表身高，而 $v_{2}$ 代表體重；且  $v_{{1}}$ 與 $v_{2}$ 的值==必須==為同一個人的身高與體重。 倘若有多筆資料，可以使用多個向量，或是改用矩陣 (matrix)。不論是採用哪種方式，參數的定義必須一致，不可某些 $v_{1}$ 代表體重，必須都是表示身高。

向量的符號通常是一個上方帶有箭頭的小寫字母，該向量的起點為 0；但是亦可使用兩個大寫字母表示從起點 (A) 到終點 (B)，如果 $\vec{AB}$ 等同於 $\vec{a}$，表示 A 的值為 0。
![[Screenshot 2025-07-01 at 5.08.34 PM.png]]

> [NOTE]
> 定義向量的兩個要素
> - 方向 (Direction) - 標示起點與終點的座標
> 	- 使用 $\vec{v}$ 表示向量 $\vec{v}$ 的座標
> 	- 使用 $\| \vec{v} \|$ 表示向量 $\vec{v}$ 的長度，亦即向量的強度
 > - 規模 (Magnitudes) - 亦稱維度 (dimentions)，向量包含的資料數量

# 基本運算
為了方便說明，我們這邊有兩個向量 $\vec{a}$ 與 $\vec{b}$，其值為：$A = \{1, 3\}$ 與 $B = \{2, 2\}$
## 向量的加法與減法
一般來說，使用==向量加法==時，兩個向量都使用==相同的表示法==；多半都是使用列向量。兩向量的加總會產生出距離起點的另一個座標。

$\vec{a} + \vec{b} = \begin{pmatrix} 1 \\ 3 \end{pmatrix} + \begin{pmatrix} 2 \\ 2 \end{pmatrix}  = \begin{pmatrix} 1 + 2 \\ 3 + 2 \end{pmatrix} = \begin{pmatrix} 3 \\ 5 \end{pmatrix}$ 

基本上，向量沒有減法。但是可以將某個向量的數值改為負數，再進行加法

$\vec{a} - \vec{b} = \vec{a} + \vec{(-b)} = \begin{pmatrix} 1 \\ 3 \end{pmatrix} + \begin{pmatrix} -2 \\ -2 \end{pmatrix}= \begin{pmatrix} 1 + (-2) \\ 3 + (-2) \end{pmatrix} = \begin{pmatrix} -1 \\ 1 \end{pmatrix}$ 

用圖來呈現上述的加減法。紫色的點分別代表兩向量 $\vec{a}$ 與 $\vec{b}$ 加法與減法的值；我們可以發現向量加法的值不會因為起點而改變：$\vec{a} + \vec{b} = \vec{b} + \vec{a} = (3, 5)$。但是減法，會因為起點不同，而有不同的結果：$\vec{a} - \vec{b} = (-1, 1)$，而 $\vec{b} - \vec{a} = (1, -1)$。
![[Screenshot 2025-06-30 at 5.55.42 PM.png]]

舉另一個例子，在一個朝北前進，時速為 60 公里的火車上，向東方丟出一顆石頭；假設石頭的初速為 20 公里，通過向量加法，可以得到石頭的落點 $B'$，亦求出石頭的速度 (velocity)：$\|\vec{b'} \| = \sqrt{ 60^2 + 20^2 }$
![[Screenshot 2025-07-02 at 9.38.03 AM.png]]

### 進階議題
將向量轉換成二維座標圖，可以更簡單的理解向量的加減法。以下圖為例：
![[Screenshot 2025-07-02 at 10.06.56 AM.png]]

- 從向量的加法，我們知道 $\vec{a} + \vec{b} = \vec{c}$，因此可以導出 $\vec{a} = \vec{c} - \vec{b}$ 與 $\vec{b} = \vec{c} - \vec{a}$ 兩個等式。
- 令 $\vec{a}$ 與 $\vec{c}$ 之間的夾角為 $\alpha$ 度，$\vec{b}$ 與 $\vec{c}$ 之間的夾角為 $\beta$ 度，且 $A'$ 與 $B'$ 為直角。可以透過餘弦公式，得出 $\vec{a}$ 或是 $\vec{b}$ 投射在 $\vec{c}$ 上的長度：
	-  $\| \vec{a'} \|  = \| \vec{a} \|  \cos \alpha$，
	-  $\| \vec{b'} \| = \| \vec{b} \| \cos \beta$

## 乘法

進行兩==向量相乘==時，會==採用不同的表示法==。被乘數向量多使用列向量，而乘數向量使用行向量。譬如：
令兩向量 $\vec{a} = \{ a_{1}, a_{2}, \dots, a_{n}\}$ 與 $\vec{b} = \{ b_{1}, b_{2}, \dots, b_{n}\}$，

兩向量的內積 (點積) 為：$\vec{a} \ . \vec{b} = \begin{pmatrix} a_{1} \\ a_{2}  \\  \dots \\ a_{n} \end{pmatrix} \ . \begin{pmatrix} b_{1} & b_{2} & \dots & b_{n} \end{pmatrix} = a_{1} * b_{1} + a_{2} * b_{2} + \dots + a_{n} * b_{n} = \sum_{i=i}^n (a_{i} * b_{i})$

兩向量的外積 (叉積) 為：$\vec{a} \times \vec{b} = \begin{pmatrix} a_{1} \\ a_{2}  \\  \dots \\ a_{n} \end{pmatrix} . \begin{pmatrix} b_{1} & b_{2} & \dots & b_{n} \end{pmatrix} = \lvert a_{1} * b_{1} \rvert + \lvert a_{2} * b_{2} \rvert + \dots + \vert a_{n} * b_{n} \rvert  = \sum_{i=i}^n \vert a_{i}*b_{i} \rvert$

### 純量乘法

將向量 ($\vec{v}$) 乘上任一==純量 (scalar) r==，其公式為 $r * \vec{v}$ ，且 $r \in \mathbb{R}$ 。主要用於計算該純量對向量的方向與強度影響程度。如果純量 $r > 1$，則向量會朝同方向加強；如果純法 $0 < r < 1$，向量會朝同方向，但是強度會減弱。如果純量 $r$ 為負數，則向量的強度調整如同正數，只是方向會朝反方向走。

以下為範例：
> 2 * $\begin{pmatrix} 3 \\ 5 \end{pmatrix} = \begin{pmatrix} 2*3 \\ 2*5 \end{pmatrix} = \begin{pmatrix} 6 \\ 10 \end{pmatrix}$ 
> 2 * $\begin{pmatrix} 3 & 7 \\ 5 & 9 \end{pmatrix} = \begin{pmatrix} 2*3 & 2*7 \\ 2*5 & 2*9 \end{pmatrix} = \begin{pmatrix} 6 & 14 \\ 10 & 18 \end{pmatrix}$ 
> 0.5 * $\begin{pmatrix} 3 \\ 5 \end{pmatrix} = \begin{pmatrix} 0.5 * 3 \\ 0.5 * 5 \end{pmatrix} = \begin{pmatrix} 1.5 \\ 2.5 \end{pmatrix}$
> -0.5 * $\begin{pmatrix} 3 \\ 5 \end{pmatrix} = \begin{pmatrix} -0.5 * 3 \\ -0.5 * 5 \end{pmatrix} = \begin{pmatrix} -1.5 \\ -2.5 \end{pmatrix}$
![[Screenshot 2025-07-03 at 11.50.47 AM.png]]

### 內積 (Inner Product)

又稱==點積 (Dot Product)==。兩個相等的==向量相乘==，結果為一==純量 (scalar)==。點積的目的在於求出兩向量的相似性。使用內積的先決條件為：兩向量的維度 (dimention) 必須一樣

![[Screenshot 2025-07-02 at 10.56.00 AM.png]]
圖中有三個向量 $\vec{a}$、$\vec{b}$、與 $\vec{d}$，以及衍生出的 $\vec{c} = \vec{a} + \vec{b}$ 與 $\vec{e} = \vec{b} + \vec{d}$。這 5 個向量中，哪一個向量與 $\vec{z}$ 的相似度最高？從圖中可以透過各向量投影到 $\vec{z}$ 的長度，輕易的得到結論。但是如果只有數據，該如何比較？

如果知道各向量與 $\vec{z}$ 的夾角大小，我們可以透過餘弦算出各向量在 $\vec{z}$ 上的投影長度。我們以 $\vec{a}$ 為例：假設 $\vec{a}$ 與 $\vec{z}$ 之間的夾角角度為 $\theta$，$\| \vec{a'} \| = \| \vec{a} \| \cos \theta$。如此一來，就可以計算出各向量在 $\vec{z}$ 上的投影長度，就知道哪個向量與 $\vec{z}$ 最相似。但是，如果不知道夾角角度，該怎麼比較？

用上圖 $\vec{a}$ 與 $\vec{z}$ 而言，我們可以組成一個大三角形，頂點為 $(0, A, Z)$，其三邊長為 $\| \vec{a} \|$、$\| \vec{z} \|$、與 $\| \vec{ZA} \|$；透過餘弦定理，得到等式 $(\| \vec{ZA} \|)^2 = (\| \vec{z} \|)^2 + (\| \vec{a} \|)^2 - 2(\| \vec{z} \| \| \vec{a} \| \cos \theta)$。從向量的加減法，我們知道 $\vec{ZA} = \vec{z} - \vec{a}$。上面等式將成為 $(\vec{z} - \vec{a})^2 = \vec{z}^2 - 2(\vec{z} \ . \vec{a}) + \vec{a}^2 = (\lvert \lvert \vec{z} \rvert \rvert)^2 + (\lvert \lvert \vec{a} \rvert \rvert)^2 - 2(\lvert \lvert \vec{z} \rvert \rvert \lvert \lvert \vec{a} \rvert \rvert \cos \theta)$，進而推導出等式： $\vec{z} \ . \vec{a} = \| \vec{z} \| (\| \vec{a} \| \cos \theta) = \| \vec{z} \| \| \vec{a'} \|$ 

> 向量內積的公式，也可以計算出兩向量的夾角：$\cos \theta = \frac{\vec{z} \ . \vec{a}}{\| \vec{z} \| \| \vec{z} \|}$

我們知道 $\| \vec{a'} \|$ 為 $\vec{a}$ 在 $\vec{z}$ 上投影的長度，我們也知道 $\| \vec{z} \|$ 為一常數，即 $\vec{z}$ 的長度。因此我們可以藉由向量點積，求出各向量投影的長度 * $\| \vec{z} \|$；如此一來，即可透過比對各向量與 $\vec{z}$ 之間的點積，比對出哪個向量與 $\vec{z}$  最相似。

向量點積公式：令兩向量 $\vec{a} = \{ a_{1}, a_{2}, \dots, a_{n}\}$ 與 $\vec{b} = \{ b_{1}, b_{2}, \dots, b_{n}\}$，兩向量的點積為 
$\vec{a} \ . \vec{b} = \begin{pmatrix} a_{1} \\ a_{2}  \\  \dots \\ a_{n} \end{pmatrix} \ . \begin{pmatrix} b_{1} & b_{2} & \dots & b_{n} \end{pmatrix} =  a_{1} * b_{1} + a_{2} * b_{2} + \dots + a_{n} * b_{n} = \sum_{i=i}^n a_{i}*b_{i}$

### 向量間的相似性

從==向量內積公式==，我們可以計算兩向量間的夾角角度 $\theta$。從三角函式中可知，$\theta$ 角度的範圍從 0 到 180，故 $\cos \theta$ 值為 -1 ~ 1 之間：

- 1 - 表示兩向量的方向一致
- 0 - 表示兩量量垂直
- -1 - 表示兩向量方向相反

舉個例子。假設某個使用者給電影 Ａ 的評價為 $\vec{A} = \begin{pmatrix} 1 \\ 3 \end{pmatrix}$，已知該使用者使用相同的方式評價另外兩部電影 $\vec{B} = \begin{pmatrix} 4 \\ 2 \end{pmatrix}$ 與 $\vec{C} = \begin{pmatrix} 2 \\ 3 \end{pmatrix}$，電影 B 與 C 在使用者的評價中，最接近電影 A 的是？

從向量內積公式，我們可以求出以下值：

$\vec{A} \ . \vec{B} = \begin{pmatrix} 1 \\ 3 \end{pmatrix} \ . \begin{pmatrix} 4 & 2 \end{pmatrix} = 1 * 3 + 3 * 2 = 9$
$\vec{A} \ . \vec{C} = \begin{pmatrix} 1 \\ 3 \end{pmatrix} \ . \begin{pmatrix} 2 & 3 \end{pmatrix} = 1 * 2 + 3 * 3 = 11$
 
 如此一來，即可求出向量 A 與另外兩個向量間的 $\cos \theta$ 值：
 $\cos (\theta)_{AB} = \frac{\vec{A} \ . \vec{B}} { \sqrt{ 1^2 + 3^2 } \ * \sqrt{ 4^2 + 2^2 }} = \frac{9} {\sqrt{ 200 }} \approx 0.707$
 
 $\cos (\theta)_{AC} = \frac{\vec{A} \ . \vec{C}} { \sqrt{ 1^2 + 3^2 } \ * \sqrt{ 2^2 + 3^2 }} = \frac{11} {\sqrt{ 130 }} \approx 0.967$

比對 $\cos (\theta)_{AB}$ 與 $\cos (\theta)_{AC}$，可以得到 $\cos (\theta)_{AB}$ 比較接近 1。故在使用者的看法中，電影 C 的評價比較接近電影 A。

### 外積 (External Product)

又稱為==叉積 (Cross Product)==。兩個相等的==向量相乘==，結果為一==向量==。

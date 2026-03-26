現代機率論的根可以回朔自 16 世紀，由意大利學者吉羅拉莫·卡丹諾 ([Gerolamo Cardano](https://en.wikipedia.org/wiki/Gerolamo_Cardano "Gerolamo Cardano")) 所提出的機率遊戲 (Game of Chance)。在 17 世紀因法國貴族沈迷於賭博，卻不知在賭局被中止的情況下，該如何分配賭金；因此湊合當時兩位頂尖數學家皮耶·德·費馬 ([Pierre de Fermat](https://en.wikipedia.org/wiki/Pierre_de_Fermat "Pierre de Fermat")) 與布萊士·帕斯卡 ([Blaise Pascal](https://en.wikipedia.org/wiki/Blaise_Pascal))，兩者 透過 17 封書信進行點數分配問題 ([problem of points](https://en.wikipedia.org/wiki/Problem_of_points "Problem of points")) 的討論；最後產生出機率論 (Probability Theory) 期望值 (Expected value) 的概念。

> [!NOTE]
> 17 世紀的賭局是誰先贏 N 場，賭金就歸誰。當未分出勝負之前中斷賭局，賭金該如何分配就是所謂的點數問題。
> 
> 費馬的解法是先找出還需要幾場才能確定賭局結束：假設 A 還差 $r$ 場就能贏 10 場，而 B 還差 $s$ 場；在這種情況下，賭局最多還需要 $r+s-1$ 場才會結束，而共有 $2^{(r+s-1)}$ 種不同的結果；因為賭局提早結束的情況下，繼續額外的遊戲並不會影響勝負結果：假設在第 $r$ 場 A 已經贏了，就算剩下的場都是 B 贏了，B 也只贏了 $(10-s)+(r+s-1)-r = 9$ 場。因此，可以藉由列出所有的結果，可以得到各玩家獲勝的百分比，即獲勝的機率。
> 
> 巴斯卡基於費馬的解法，提出改善：假設在只差一場就可以分出勝負的情況，我門可以想像出兩種結果：某人 (A) 贏 10 場獲得所有獎金，或是另一人 (B) 贏獎金必須採用另一種分配方式。對於 A 來說，將這兩種結果的平均就是多玩一場並結束賭局應得到獎金。藉由這個方式，我們可以倒推至目前的狀態下，各玩家應分得的賭金。這就是==期望值 (expected value)== 的概念。
> 
> 巴斯卡從這個方案中，進一步創作出現代被稱為巴斯卡三角形 ([Pascal's triangle](https://en.wikipedia.org/wiki/Pascal%27s_triangle)) 的計算，巴斯卡最終提出的解決方案如下：在接下來第 k 局，左側為玩家 A 應獲得的賭金，右側為玩家 B 應獲得的賭金：
> $\sum\limits^{s-1}_{k=0}\begin{pmatrix}r+s-1\\k\end{pmatrix}: \sum\limits^{r+s-1}_{k=s}\begin{pmatrix}r+s-1\\k\end{pmatrix}$

最初機率只用於計算獨立事件 (discrete events)，即不受其他事件影響的事件 A 發生的機率 $P(A)$；主要的演算法來自於組合數學 ([combinatorics](https://en.wikipedia.org/wiki/Combinatorics))。最終因為數學分析學 ([Mathematical Analysis](https://en.wikipedia.org/wiki/Mathematical_analysis)) 的關係，也納入連續事件的機率 ($P(A|B)$ 當事件 B 發生之後，事件 A 發生的機率)；進而形成了近代機率學，於 1933 年由安德烈·柯爾莫哥洛夫 ([Andrey Nikolaevich Kolmogorov](https://en.wikipedia.org/wiki/Andrey_Nikolaevich_Kolmogorov "Andrey Nikolaevich Kolmogorov")) 奠基。藉由結合理查·馮·米澤斯 ([Richard von Mises](https://en.wikipedia.org/wiki/Richard_von_Mises)) 的樣本空間 (sample space) 概念搭配測量理論 ([measure theory](https://en.wikipedia.org/wiki/Measure_theory))，提出他所建立的機率公設 ([axiom system for probability](https://en.wikipedia.org/wiki/Kolmogorov_axioms "Kolmogorov axioms")) 理論


# 常見符號

| 符號                           | 說明                                                                         |
| :--------------------------- | :------------------------------------------------------------------------- |
| $P(A)$                       | 事件 A 發生的機率                                                                 |
| $P(A\|B)$                    | 當事件 B 發生之後，事件 A 的發生機率<br>$P(A\|B) = {P(A \cap B) \over P(B)}$              |
| $P(A \cap B)$ 或是 $P(AB)$<br> | 事件 A 與 B 的交集，即兩件事同時發生的機率<br>$P(A \cap B) = P(A) * P(B)$                    |
| $P(A \cup B)$                | 事件 A 與 B 的聯集，即事件 A 或是 B 發生的機率<br>$P(A \cup B) = P(A) + P(B) - P(A \cap B)$ |
| $P(A^')$                     | 事件 A 不發生的機率<br>$1 - P(A)$                                                  |
| $\Omega$                     | 樣本空間，即所有可能發生結果的集合                                                          |
| $\emptyset$                  | 空集合，不可能發生的事件                                                               |

# 子議題


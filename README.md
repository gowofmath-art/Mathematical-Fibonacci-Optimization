# Mathematical-Fibonacci-Optimization
Exploring Algorithmic Complexity: Optimizing Fibonacci Sequence to O(log n) via Matrix Diagonalization and Fast Doubling

# 📐 Mathematical Fibonacci Optimization: From $O(2^n)$ to $O(\log n)$

> **"Theory is essential. True optimization happens when mathematical truths are translated into low-level engineering mastery."**

本專案從**第一性原理 (First Principles)** 出發，以嚴謹的數學語言剖析演算法效能。專案以**費氏數列 (Fibonacci Sequence)** 為核心研究對象，探討如何透過線性代數中的**矩陣對角化 (Diagonalization)** 與**快速冪 (Binary Exponentiation)** 技術，將計算的時間複雜度從指數級的 $\mathcal{O}(2^n)$ 降至對數級的 $\mathcal{O}(\log n)$。

---

## 📊 五大演算法實作與複雜度分析 (Algorithmic Approaches)

專案內含完整 Python 實作（詳見 `fibonacci_optimization.ipynb`），涵蓋以下五種思維層次：

| 方法 (Approach) | 時間複雜度 (Time) | 空間複雜度 (Space) | 實務評估 (Practical Review) |
| :--- | :---: | :---: | :--- |
| **1. 迭代法 (Iteration)** | $\mathcal{O}(n)$ | $\mathcal{O}(1)$ | 基礎解法，隨 $n$ 線性增長。 |
| **2. 遞迴法 (Recursion)** | $\mathcal{O}(\phi^n)$ | $\mathcal{O}(n)$ | 具備數學美感，但存在大量重複計算，效能極差。 |
| **3. Binet 公式解 (Analytic Solution)** | $\mathcal{O}(1)$ | $\mathcal{O}(1)$ | 理論上最快，但因浮點數精度限制，在 $n \ge 72$ 時會產生捨入誤差。 |
| **4. 矩陣快速冪 (Matrix Exponentiation)** | $\mathcal{O}(\log n)$ | $\mathcal{O}(1)$ | **兼具精確度與速度的實務解法**。 |
| **5. 快倍公式法 (Fast Doubling)** | $\mathcal{O}(\log n)$ | $\mathcal{O}(\log n)$ | **全場最速**，純整數運算，免去矩陣乘法的呼叫成本。 |

---

## 🧠 核心數學推導：為什麼是 $\mathcal{O}(\log n)$？

費氏數列的遞迴定義為 $F_{n+2} = F_{n+1} + F_{n}$。我們將其轉化為線性變換矩陣形式：

$$\begin{pmatrix} F_{n+1} \\ F_{n} \end{pmatrix} = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix} \begin{pmatrix} F_{n} \\ F_{n-1} \end{pmatrix}$$

透過數學歸納法，問題的核心轉化為求轉換矩陣 $M$ 的 $n$ 次方：

$$\begin{pmatrix} F_{n+1} \\ F_{n} \end{pmatrix} = M^n \begin{pmatrix} F_{1} \\ F_{0} \end{pmatrix} = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}^n \begin{pmatrix} 1 \\ 0 \end{pmatrix}$$

### ⚙️ 轉換矩陣之對角化 (Diagonalization)
為了求得 $M^n$ 的一般項，我們求解特徵方程式：

$$\det(M - \lambda I) = \det\begin{pmatrix} 1-\lambda & 1 \\ 1 & -\lambda \end{pmatrix} = \lambda^2 - \lambda - 1 = 0$$

解得特徵值為黃金比例：
$$\lambda_1 = \phi = \frac{1+\sqrt{5}}{2}, \quad \lambda_2 = \psi = \frac{1-\sqrt{5}}{2}$$

建構特徵向量矩陣 $P$ 將其對角化為 $M = PDP^{-1}$，進而求得 $M^n = PD^nP^{-1}$。最終完美導出 Binet 公式：

$$F_n = \frac{1}{\sqrt{5}} \left[ \left(\frac{1+\sqrt{5}}{2}\right)^n - \left(\frac{1-\sqrt{5}}{2}\right)^n \right]$$

### ⚡ 快速冪的工程實踐
在實務工程中，利用**二進位拆解**（如當 $n$ 為偶數時 $A^n = A^{n/2} \cdot A^{n/2}$），我們不需要做 $n$ 次乘法，只需 $\log_2 n$ 次矩陣乘法，成功達到 $\mathcal{O}(\log n)$。

---

## 🚀 效能實測效能 (Performance Benchmark)

當測試巨大的第 20,000 項費氏數列時（結果高達數千位數），各演算法之運算耗時對比：

```text
1. 迭代法 (O(n))          -> 運算時間: 0.007752 秒
2. 矩陣快速冪 (NumPy)      -> 運算時間: 0.001215 秒
3. 矩陣快速冪 (純手寫)     -> 運算時間: 0.001076 秒
4. Fast Doubling (最速)   -> 運算時間: 0.000422 秒
*(註：遞迴法 F(50) 即需花費 3689 秒，在此不列入大數測試)*

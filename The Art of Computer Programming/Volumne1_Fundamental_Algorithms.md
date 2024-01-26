# The Art of Computer Programming——Volumne 1 Fundamental Algorithms Third Edition
## Chapter1 Basic Concepts 
> **Euclid's Algorithms**
> Given two positive integers $m$ and $n$, find their *greatest common divisor* that is , the largest positive integer that evenly divides both $m$ and $n$.
> **E1.**\[Find remainder.\] Divide $m$ by $n$ and let $r$ be the remainder.
> **E2**. \[Is it zero?\] If $r=0$, the algorithm terminates; $n$ is the answer.
> **E3**.\[Reduce\] Set $m\leftarrow n, n\leftarrow r$ and go back to step **E1**

![image-20240121230329716](C:\Users\Chenjiefeng\AppData\Roaming\Typora\typora-user-images\image-20240121230329716.png)

### Mathematical Induction 
  Mathematical induction is a method to proof the law of the mathematic. For example, to prove the behind law:
$$
n \ times \ n+3 \ is \ an even \ number
$$

> Algorithms I (Construct a proof). Given a positive integer $n$, this algorithm will output a proof that $P(n)$ is ture.
> I1.\[Prove P(1).\] Set $k\leftarrow 1$, and accroding to (a), output a proof of $P(1)$
> I2. \[k=n?\] IF $k=n$, terminate the algorithm; the required proof has been output.
> I3. \[Prove P(k+1)\] Accroding to (b), output that "If all of $P(1),\cdots ,P()k$ are ture, then $P(k+1)$ is ture." Also output a proof "We are already proved $P(1),\cdoots , P(k)$; hence $P(k+1)$ is ture"
> I4. \[Increase k\] Increase $k$ by $1$ and go to step $I2$. 


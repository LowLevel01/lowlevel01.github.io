## Fast Multiplication: Karatsuba's Algorithm
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
  Since this is my first blog ever, I chose to write about a simple and interesting subject which is the multiplication of integers. Is there a faster way to multiply two integers than the classical method?<br />

  The answer is actually yes and the way is <strong>Karatsuba's algorithm</strong>.<br />

  Computer scientists consider the complexity of multiplication to be <strong>O(1)</strong> which is just a simplification for small numbers.<br />
  <h3>I. The classical method of multiplication:</h3>
  <br>
  The usual way of multiplication has a complexity of <strong>O(n²)</strong> but the algorithm we're gonna learn about has a complexity of 
 
$$
  \begin{align}
  O(n^{log_2(3)}) = O(n^{1.58})
  \end{align}
$$

 <br />

Let's consider two <strong>n</strong>-bit integers <strong>x</strong> and <strong>y</strong> written in base b, and consider for a while that <strong>n</strong> is even.<br>
Let's consider two entities <strong>H</strong> and <strong>L</strong> such that <strong>H</strong> represents the higher bit being the bigger part (the left one) and <strong>L</strong> the lower bit (the right one)
<br>
Therefore we can write <strong>x</strong> and <strong>y</strong> in the following way:<br>
  
  
  $$
  \begin{align}
        x = x_H * b^{\frac{n}{2}} + x_L \\
         y = y_H * b^{\frac{n}{2}} + y_L
  \end{align}
$$

<br> By multiplying the two integers we have:<br>

$$
  \begin{align}   
         x * y &= (x_H * b^{\frac{n}{2}} + x_L) * (y_H * b^{\frac{n}{2}} + y_L) \\
         &= x_H * y_H*b^n + (x_H * y_L + x_L * y_H) * b^{\frac{n}{2}} + y_H * y_L
  \end{align}
$$ 

<br>
<em>The expression above has four multiplications; It has a complexity of <strong>O(n²)</strong></em><br>
<br>
<h3>II. The faster algorithm: Karatsuba's:</h3> <br>
Now let's get down to the real business. Let's considers three so-called subproblems:

$$
\begin{equation}
  \begin{aligned}   
         a &= x_H * y_H \\
         d &= x_L * y_L \\
         e &= (x_H * x_L + y_H * y_L) - a - d
  \end{aligned}
\end{equation}
$$ 

<br> Which gives us : <br>

$$
  \begin{align}
        x * y = a * b^n + e * b^{\frac{n}{2}} + d
  \end{align}
$$


<br> This expression only requires three multiplications. We can deduce the following recurrence relation: <br>

$$
  \begin{align}
        3*T(\frac{n}{2}) + O(n) = O(n^{log(3)})
  \end{align}
$$

<br> Which allows the algorithm to be applied recursively AKA divide and conquer. <br><br>
<em>In the approach above we assumed for the sake of simplicity that <strong>n</strong> is even. We can make use of the floor and ceil functions to circumvent that obstacle</em>
<br>
<h3>III. Python implementation:</h3>

```python
from math import ceil, floor

def karatsuba(x, y):
  if x < 10 and y < 10:               #base case
    return x*y     
    
  n = max(len(str(x)), len(str(y)))
  m = ceil(n/2)
  
  x_H  = floor(x / 10**m)
  x_L = x % (10**m)

  y_H = floor(y / 10**m)
  y_L = y % (10**m)
  
  a = karatsuba(x_H,y_H)               #recursive steps from the recurrence relation
  d = karatsuba(x_L,y_L)
  e = karatsuba(x_H + x_L, y_H + y_L) - a - d

  return int(a*(10**(m*2)) + e*(10**m) + d)

```

<br>
<h3>IV. Complexity of Karatsuba's algorithm: </h3>

Let's suppose for a while that the number of multiplications performed by the algorithm for an integer <strong>n</strong> is <strong>M(n)</strong>.<br>
The algorithm multiplies two <strong>n</strong>-bit numbers. If we assume, for example, that <strong>n</strong> is a power of 2, then the algorithm recurses three times on n/2-bit numbers.
This leads to the recurrence relation
<br>


$$
  \begin{align}   
         M(n) = 3 * M(\frac{n}{2})
  \end{align}
$$ 


<br> Also, there are <strong>O(n)</strong> additions and substractions. Therefore, the final recurrence relation for the whole Karatsuba algorithm is:
<br>

$$
  \begin{align}   
         T(n) = 3 * T(\frac{n}{2}) + O(n)
  \end{align}
$$ 

<br> We can use the <strong><a href="https://brilliant.org/wiki/master-theorem/">master theorem</a></strong> to deduce that the running time of Karatsuba's algorithm is 

$$
  \begin{align}   
         Θ(n^{log_2(3)} ) ≈ Θ(n^{1.585} )
  \end{align}
$$ 

<br>
<h4>Finally, </h4> there are other algorithms such as the Schönhage–Strassen algorithm and the Toom–Cook algorithm which are both faster than the Karatsuba algorithm.
<br>
<br>
Happy reading !
<br>
<br>
<br>
<br>
<h5>References:</h5>
[1] : <a href = "https://brilliant.org/wiki/karatsuba-algorithm/">Karatsuba Algorithm</a>
<br>
<br>
<br>


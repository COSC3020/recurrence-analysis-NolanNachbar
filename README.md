# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.


First analysing the function
```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);\\ This will take T(n/3) time complexity
        var count = 0;
        mystery(n / 3); \\ This will take T(n/3) time complexity
        for(var i = 0; i < n*n; i++) { \\ This loop will take n^2 time complexity
            for(var j = 0; j < n; j++) {\\ This loop will take n time complexity
                for(var k = 0; k < n*n; k++) { \\ This loop will take n^2 time complexity
                    count = count + 1; 
                }
            }
        }
        mystery(n / 3); \\ This will take T(n/3) time complexity
    }
}
```

Combining all those we get:

$$
T(n) = 
\begin{cases} 
1 & \text{if } n \leq 1 \\
3 T(n/3) + n^5 & \text{otherwise}
\end{cases}
$$

Now substituting,

$$
T(n) = 3 T(n/3) + n^5 
$$

$$
= 3 (3T(n/9) + (\frac{n}{3})^5) + n^5
$$

$$
= 9T(n/9) + \frac{n^5}{3^5} + n^5
$$

$$
= 9 (3T(n/27) + (\frac{n}{9})^5) + \frac{n^5}{3^5} + n^5
$$

$$
= 27T(n/27) + \frac{n^5}{3^5} + \frac{n^5}{3^4} + n^5
$$

$$
\vdots
$$

$$
= 3^i T(n/3^i) + \frac{n^5}{3^{5(i - 1)}} + \frac{n^5}{3^{5(i - 2)}} + \dots + n^5
$$

For $i = \log_3 n$,

$$
T(n) = 3^{log_3 n} T(n/3^{log_3 n}) + \frac{n^5}{3^{5(log_3 n - 1)}} + \frac{n^5}{3^{5(log_3 n - 2)}} + \dots + n^5
$$

$$
= 3^{log_3 n} T(1) + \frac{n^5}{3^{5(log_3 n - 1)}} + \frac{n^5}{3^{5(log_3 n - 2)}} + \dots + n^5
$$

$$
= n + \frac{n^5}{3^{5(log_3 n - 1)}} + \frac{n^5}{3^{5(log_3 n - 2)}} + \dots + n^5
$$

Note that:

$$
\frac{n^5}{3^{5(log_3 n - k)}}, k = 1,2,3,4,...,i
$$

Are all constants. 

So, $n^5$ is the leading term, because $n^5 >> n$. Hence, $T(n) \in O(n^5)$


I certify that I have listed all sources used to complete this exercise, including the use of any Large Language Models. All of the work is my own, except where stated otherwise. I am aware that plagiarism carries severe penalties and that if plagiarism is suspected, charges may be filed against me without prior notice.

I followed the recursion analysis example on slide 39 of the lecture01-sorting slides. I looked at this to see if my answer was right, this one never got approved but we got to this same answer.
https://github.com/COSC3020/recurrence-analysis-peppersmoke42/blob/0e2d47b67afc735781476439bd308553ae9a1236/README.md

I also plotted some of the things from the equation on Desmos:
[Plots on Desmos](https://www.desmos.com/calculator/0mzx6tvzja)

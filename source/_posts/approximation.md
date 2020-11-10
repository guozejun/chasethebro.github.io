---
title: Approximation
abstract: In computer science, a polynomial-time approximation scheme (PTAS) is a type of approximation algorithm for optimization problems (most often, NP-hard optimization problems).
date: 2019-05-15 20:13:03
tags: Algorithm
---
## Polynomial-time approximation scheme

In computer science, a polynomial-time approximation scheme (PTAS) is a type of approximation algorithm for optimization problems (most often, NP-hard optimization problems).

A PTAS is an algorithm which takes an instance of an optimization problem and a parament $\epsilon > 0$ and, **in polynomial time**, produces a solution that is within a factor $1 + \epsilon$ of being optimal (or $1 - \epsilon$ for maximization problems). Or, we can say for any fixed $\epsilon > 0$, the scheme runs in **polynomial time** in the size $n$ of its input instance.

The running time of a PTAS is required to be polynomial in n for every fixed $\epsilon$ but can be different for different $\epsilon$. For example consider a minimization problem, if $\epsilon$ is 0.5, then the solution provided by the PTAS algorithm is **1.5-approximate**. The running time of PTAS must be polynomial in terms of n. But it can be exponential in terms of $\epsilon$.

## Fully polynomial-time approximation scheme

In PTAS algorithms, the exponent of the polynomial can increase dramatically as ε reduces, for example if the runtime is $O(n^{(1/\epsilon)!})$ which is a problem. So we need a stricter scheme.

Fully polynomial-time approximation scheme(FPTAS) requires the algorithm to be **polynomial time** in both the problem size $n$ and $1/\epsilon$. All problems in FPTAS are fixed-parameter tractable. Both the knapsack problem and bin pack problem admit an FPTAS.

## Bin packing problem

Given n items of different weights and bins each of capacity c, assign each item to a bin such that number of total used bins is minimized. It may be assumed that all items have weights smaller than bin capacity.

### Lower bound

We can always find a lower bound on minimum number of bins required. The lower bound can be given as

$$
Min\ no.\ of\ bins  >=  Ceil ((Total\ Weight) / (Bin\ Capacity))
$$

### Online algorithms

#### Next fit

When processing next item, check if it fits in the same bin as the last item. Use a new bin only if it does not.

Next Fit is 2-approximate, i.e., the number of bins used by this algorithm is bounded by twice of optimal. Consider any two adjacent bins. The sum of items in these two bins must be bigger than the comtent; otherwise, NextFit would have put all the items of second bin into the first. The same holds for all other bins. Thus, at most half the space is wasted, and so Next Fit uses at most 2M bins if M is optimal.

```cpp
#include <iostream>
using namespace std; 
  
// Returns number of bins required using next fit  
// online algorithm 
int nextFit(int weight[], int n, int c) 
{ 
   // Initialize result (Count of bins) and remaining 
   // capacity in current bin. 
   int res = 0, bin_rem = c; 
  
   // Place items one by one 
   for (int i=0; i<n; i++) 
   { 
       // If this item can't fit in current bin 
       if (weight[i] > bin_rem) 
       { 
          res++;  // Use a new bin 
          bin_rem = c - weight[i]; 
       } 
       else
         bin_rem -= weight[i]; 
   } 
   return res; 
} 
  
// Driver program 
int main() 
{ 
    int weight[] = {2, 5, 4, 7, 1, 3, 8}; 
    int c = 10; 
    int n = sizeof(weight) / sizeof(weight[0]); 
    cout << "Number of bins required in Next Fit : "
         << nextFit(weight, n, c); 
    return 0; 
}
```

#### First fit

When processing the next item, see if it fits in the same bin as the last item. Start a new bin only if it does not.

If M is the optimal number of bins, then First Fit never uses more than 1.7M bins. So First Fit is better than Next Fit in terms of upper bound on number of bins.

```cpp
#include <iostream>
using namespace std; 
  
// Returns number of bins required using first fit  
// online algorithm 
int firstFit(int weight[], int n, int c) 
{ 
    // Initialize result (Count of bins) 
    int res = 0; 
  
    // Create an array to store remaining space in bins 
    // there can be at most n bins 
    int bin_rem[n]; 
  
    // Place items one by one 
    for (int i=0; i<n; i++) 
    { 
        // Find the first bin that can accommodate 
        // weight[i] 
        int j; 
        for (j=0; j<res; j++) 
        { 
            if (bin_rem[j] >= weight[i]) 
            { 
                bin_rem[j] = bin_rem[j] - weight[i]; 
                break; 
            } 
        } 
  
        // If no bin could accommodate weight[i] 
        if (j==res) 
        { 
            bin_rem[res] = c - weight[i]; 
            res++; 
        } 
    } 
    return res; 
} 
  
// Driver program 
int main() 
{ 
    int weight[] = {2, 5, 4, 7, 1, 3, 8}; 
    int c = 10; 
    int n = sizeof(weight) / sizeof(weight[0]); 
    cout << "Number of bins required in First Fit : "
         << firstFit(weight, n, c); 
    return 0; 
}
```

## 0-1 knapsack problem

We know that 0-1 knapsack is NP Complete. There is a DP based pseudo polynomial solution for this. But if input values are high, then the solution becomes infeasible and there is a need of approximate solution. One approximate solution is to use Greedy Approach (compute value per kg for all items and put the highest value per kg first if it is smaller than W), but Greedy approach is not PTAS, so we don’t have control over accuracy. Below is a FPTAS solution for 0-1 Knapsack problem:

### Input:

- **W** (Capacity of Knapsack)
- **val[0..n-1]** (Values of Items)
- **wt[0..n-1]** (Weights of Items)

### Solution:

- Find the maximum valued item, i.e., find maximum value in val[]. Let this maximum value be maxVal.
- Compute adjustment factor k for all values
```
k  = (maxVal * ε) / n
```
- Adjust all values, i.e., create a new array val'[] that values divided by k. Do following for every value val[i].
```
val'[i] = floor(val[i] / k)
```
- Run DP based solution for reduced values, i,e, val'[0..n-1] and all other parameter same.

The above solution works in polynomial time in terms of both n and ε. The solution provided by this FPTAS is $(1 - \epsilon)-approximate$. The idea is to rounds off some of the least significant digits of values then they will be bounded by a polynomial and $1/\epsilon$.
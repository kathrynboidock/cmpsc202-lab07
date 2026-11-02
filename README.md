# Lab 07: The Integer-Valued Knapsack Problem

**Name:** Kathryn Boidock  

## Introduction

In this lab, you will explore different variations of the Knapsack problem and design a dynamic programming solution for one of them. You may work in small groups or individually, but everyone should write up their own answers to the questions below. 

Suppose we're going hiking. We have items $1, \ldots, n$ that we could pack in multiples with corresponding values $v_1, \ldots, v_n$ and weights $w_1, \ldots, w_n$. For example, we might have a water bottle (weight 1, value 2) and a sandwich (weight 2, value 5). Consider the following three problem variations:

1. Maximize value subject to capacity $W$, where you can pack at most one of each item.
2. Maximize value subject to capacity $W$, where you can pack a positive integer value of each item.
3. Minimize weight subject to minimum value $V$, where you can pack a positive integer value of each item.

---

## Part 1: Dynamic Programming Fundamentals

**Question:** Which of the three problem variations can be solved using the dynamic programming approaches we've studied in lecture? Explain your reasoning.

*Your Answer:* All three problem variations can be solved using dynamic programming approaches. For each variation we can break the problem down into smaller subproblems and build up a solution using previously computed results.

## Part 2: The Naive Recursive Algorithm

Below is a naive recursive algorithm intended to solve one of the variations listed above.

```text
Function MaxValue(W):
    if W == 0:
        return 0
        
    max_val = 0
    for i from 1 to n:
        if w_i <= W:
            current_val = v_i + MaxValue(W - w_i)
            if current_val > max_val:
                max_val = current_val
                
    return max_val
```

**Question:** Which of the three problem variations (1, 2, or 3) does this algorithm solve? How do you know? Explain your reasoning.

*Your Answer:* Problem variantion 2 is what is solved by this algorithm. The algorithm recursively considers each item that fits and adds its value to the optimal value of the remaining capacity. Since it does not keep track of which items have already been selected, the same item can be chosen multiple times

## Part 3: Recursion Tree

Consider the following simple example:

* **Knapsack Capacity:** $W = 3$
* **Item 1:** Weight $w_1 = 1$, Value $v_1 = 2$
* **Item 2:** Weight $w_2 = 2$, Value $v_2 = 5$
* **Item 3:** Weight $w_3 = 1$, Value $v_3 = 3$

**Question:** Draw the full recursion tree for `MaxValue(3)` using the example above. Label the remaining capacity at each node and indicate which item is chosen along each branch.

*Your Answer (take a picture and upload it):*
[Recursion Tree](recursion.jpg)

## Part 4: Recurrence Relation

**Question:** Derive a mathematical recurrence relation for $OPT(W)$, the maximum value you can pack into a knapsack of capacity $W$, based on the naive recursive algorithm. Be sure to state your base case(s) and explain how you arrived at your formula.

*Your Answer:*
$$
OPT(W) = 
\begin{cases} 
0 & \text{if } W = 0 \\
max(v_i + OPT(W - w_i)) & \text{if an item fits } \\
0 & \text{if no item fits}
\end{cases}
$$

## Part 5: Bottom-Up Dynamic Programming

Instead of using the slow recursive approach, we can build a table bottom-up. 

**Question:** Use your recurrence relation from Part 4 to fill in the following table for the example provided in Part 3 ($w_1=1, v_1=2$ and $w_2=2, v_2=5$). At each step, record the optimal value and the specific item chosen to achieve that value. 

| Capacity ($W$) | Optimal Value ($OPT$) | Item Chosen |
|:---:|:---|:---|
| **0** | 0 | None |
| **1** | 2 | Item 1 |
| **2** | 5 | Item 2 |
| **3** | 7 | Item 1 or Item 2 |
| **4** | 10 | Item 2 |
| **5** | 12 | Item 1 or Item 2 |
| **6** | 15 | Item 2 |

*Explain how you calculated the row for $W=6$*
I used the recurrence relation to compare the values of choosing Item 1 and Item 2. Choosing Item 1 gives 2 + OPT(5) = 14, while choosing Item 2 gives 5 + OPT(4) = 15. Since 15 is greater than 14, the optimal value for capacity 6 is 15, and Item 2 is superior.

## Part 6: Traceback

**Question:** Using the "Item Chosen" column from the table you completed in Part 5, trace back to find the exact quantities of each item to pack that maximize the value. What is the final optimal packing configuration for $W=6$ and the maximum value?

*Your Answer:*
Starting at W = 6, the table shows that Item 2 is chosen. Since Item 2 has a weight of 2, the remaining capacity becomes 4. At W = 4, Item 2 is selected again, leaving a capacity of 2. Finally, Item 2 is chosen once more, bringing the remaining capacity to 0.

The optimal packing configuration is 0 of Item 1 and 3 of Item 2. This gives a total weight of 6 and a maximum value of 15.

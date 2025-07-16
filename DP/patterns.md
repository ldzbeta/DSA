# **Patterns of Questions**
## 1) Normal Relaxation
- Just use relaxation to and genearalise for i
- where recursion from n-1 to 0
- ⚠️ Any Time tabulation goes opposite of recursion , starts from base case of recursion

## 2) Start or End is not a precise location  
&ensp; **Example:** [Triangle](https://leetcode.com/problems/triangle/description/)  
&emsp;- If we start from `n-1` or the last row, we have to call a recursive function on each element of the last row.  
&emsp;- At the start, we have only one element and can make a tree of paths.  
&emsp;- End whenever reaching the last column.  
&emsp;- Recursion: `0` to `n-1`, so tabulation: `n-1` to `0`.  
&emsp;- Even though the recursion follows a top-to-bottom approach, there is no change.  
&emsp;- Compute top rows from bottom rows.


    

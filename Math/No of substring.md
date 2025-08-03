# Substrings in a String of Length n

## Formula
The total number of substrings formed by n letters is given by:
`(n(n + 1)) / 2`

## Explanation

### Substrings of Length 1
- These are single characters.
- In a string of length n, there are n such substrings (one starting at each position).

Example for "abc" (n = 3):  
"a", "b", "c" → 3 substrings.

---

### Substrings of Length 2
- These are sequences of two consecutive characters.
- For each possible starting position, there's a substring of length 2 until we don't have enough characters left.

Number of such substrings:  
n - 1

Example for "abc":  
"ab", "bc" → 2 substrings (3 - 1 = 2).

---

### Substrings of Length 3
- These are sequences of three consecutive characters.

Number of such substrings:  
n - 2

Example for "abc":  
"abc" → 1 substring (3 - 2 = 1).

---

### Generalization
For substrings of length k:
Number = n - k + 1

### Total Substrings
The total number of substrings is the sum of substrings of all possible lengths (from 1 to n):
Sum from k=1 to n of (n - k + 1)
This simplifies to the formula:
(n(n + 1)) / 2

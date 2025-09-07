# 🔑 Note: Palindrome Checking with Bitmask Trick

## Idea
A sequence can be rearranged into a palindrome if:

- **Even length** → all elements occur an even number of times.  
- **Odd length** → at most one element occurs an odd number of times.

## Bitmask Trick
Use an integer `mask` where each bit represents whether a number’s count is odd (`1`) or even (`0`).

For each number `num`, flip its bit:
```c
mask ^= (1 << (num - 1));
```

After processing all numbers:

- `mask == 0` → all even counts → palindrome possible.  
- `mask` has exactly one bit set → one odd count allowed → palindrome possible.  
- Otherwise → not possible.

## Bit Test
```c
(mask & (mask - 1)) == 0
```
checks if `mask` has at most one bit set.

- **Special case:** `mask == 0` → `mask & (mask - 1)` also `0` (valid).

## C Function Example
```c
#include <stdbool.h>

bool canFormPalindrome(int nums[], int n) {
    int mask = 0;
    for (int i = 0; i < n; i++) {
        int bit = 1 << (nums[i] - 1);  
        mask ^= bit; // flip bit for each occurrence
    }
    // palindrome possible if at most one bit set
    return (mask & (mask - 1)) == 0;
}
```

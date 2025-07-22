## Function to calculate binomial coefficient C(n, k)
```cpp
unsigned long long binomialCoefficient(int n, int k) {
    if (k > n - k) k = n - k; // Take advantage of symmetry
    unsigned long long res = 1;
    
    for (int i = 0; i < k; ++i) {
        res *= (n - i);
        res /= (i + 1);
    }
    
    return res;
}
```

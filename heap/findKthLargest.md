## Optimal Approach (Using Min-Heap)
```cpp
int findKthLargest(std::vector<int>& nums, int k) {
    std::priority_queue<int, std::vector<int>, std::greater<int>> min_heap;
    
    for (int num : nums) {
        min_heap.push(num);
        if (min_heap.size() > k) {
            min_heap.pop();
        }
    }
    return min_heap.top();
}
```
Min-heap stores K largest elemnts and remove smaller ones

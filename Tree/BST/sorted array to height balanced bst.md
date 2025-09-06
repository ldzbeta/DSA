# Constructing a Height-Balanced BST from a Sorted Array

A sorted array can be converted into a height-balanced binary search tree (BST) by repeatedly choosing the middle element as the root of the (sub)tree. This guarantees that the left and right subtrees of every node are as balanced as possible.

- A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

## Key idea
- **Balanced root selection**: Always pick the middle element of the current array (or subarray) as the root.  
- **Divide and conquer**: Recursively build left and right subtrees from the left and right halves of the array.
- **Base case**: An empty subarray corresponds to a null subtree.

## Algorithm (step-by-step)
1. **If the array is empty, return `null`.**  
2. **Find the middle index** of the current subarray: `mid = (start + end) / 2` (use integer division).  
3. **Create a new `TreeNode`** with value `nums[mid]`.  
4. **Recursively construct the left subtree** from `nums[start:mid]`.  
5. **Recursively construct the right subtree** from `nums[mid+1:end]`.  
6. **Assign the left and right subtrees** to the current node.  
7. **Return the root node.**

## Complexity
- **Time complexity**: O(n) — each element is visited once to create a node.  
- **Space complexity**: O(log n) for recursion stack in the best case (height of balanced BST); O(n) worst-case stack if unbalanced indices or recursion overhead considered.

## Example (conceptual)
Given `nums = [-10, -3, 0, 5, 9]`:
- Choose `0` (middle) as root.
- Left subtree from `[-10, -3]` → root `-10` or `-3` depending on mid rule.
- Right subtree from `[5, 9]` → root `5` or `9` depending on mid rule.
Result is a height-balanced BST.

## Notes and variants
- For even-length subarrays there are two middle choices. Either left-middle or right-middle produces a valid height-balanced BST; choose consistently (e.g., `mid = (start + end) // 2` or `mid = (start + end + 1) // 2`).
- This method produces a BST with minimal possible height for the given sorted input.
- Implementation detail: prefer passing `start` and `end` indices to avoid creating new subarrays and reduce memory usage.

**Summary**: Pick the middle element as root and recursively repeat on subarrays to obtain a height-balanced BST in linear time.

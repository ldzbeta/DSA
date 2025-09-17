<img width="1193" height="687" alt="image" src="https://github.com/user-attachments/assets/691808d9-680e-4b70-9e4f-8b61a512bd9f" />


```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    // base case
    if (root == NULL || root == p || root == q) {
        return root;
    }

    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);

    // result
    if (left == NULL) {
        return right;
    } else if (right == NULL) {
        return left;
    } else { // both left and right are not null, we found our result
        return root;
    }
}
```
- **Complexity**:
  - Time: \(O(n)\), where \(n\) is the number of nodes (each node visited once).
  - Space: \(O(h)\) due to recursion stack, where \(h\) is the tree height.

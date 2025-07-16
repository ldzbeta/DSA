`node`: The current node being checked.
`minimum`: The minimum allowable value for the current node.
`maximum`: The maximum allowable value for the current node.

```cpp
 boolean isValid(TreeNode node, long min, long max) {
        if (node == null) return true;
        if (node.val <= min || node.val >= max) return false;
        return isValid(node.left, min, node.val) && isValid(node.right, node.val, max);
}
```

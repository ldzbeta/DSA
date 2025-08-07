```cpp
    vector<TreeNode*> generate(int left, int right) {
        vector<TreeNode*> trees;
        if (left > right) {
            trees.push_back(nullptr);
            return trees;
        }

        for (int val = left; val <= right; val++) {
            // Generate all possible left subtrees
            vector<TreeNode*> leftTrees = generate(left, val - 1);
            // Generate all possible right subtrees
            vector<TreeNode*> rightTrees = generate(val + 1, right);

            // Combine them in all possible ways
            for (TreeNode* leftTree : leftTrees) {
                for (TreeNode* rightTree : rightTrees) {
                    TreeNode* root = new TreeNode(val, leftTree, rightTree);
                    trees.push_back(root);
                }
            }
        }
        return trees;
    }

    vector<TreeNode*> generateTrees(int n) {
        return generate(1, n);
    }
```

# Level Order Traversal

## Using BFS (Breadth-First Search)
```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int levelSize = q.size();
        vector<int> currentLevel;

        for (int i = 0; i < levelSize; ++i) {
            TreeNode* node = q.front();
            q.pop();
            currentLevel.push_back(node->val);

            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }

        result.push_back(currentLevel);
    }

    return result;
}
```

## Using DFS (Depth-First Search)
```cpp
int getHeight(Node* root) {
    if (!root) return 0;
    int lh = getHeight(root->left);
    int rh = getHeight(root->right);
    return (lh > rh ? lh : rh) + 1;
}

void storeLevel(Node* root, int level, int* arr, int* index) {
    if (!root) return;
    if (level == 1) {
        arr[(*index)++] = root->data;
    } else {
        storeLevel(root->left, level - 1, arr, index);
        storeLevel(root->right, level - 1, arr, index);
    }
}

void levelByLevelStore(Node* root) {
    if (!root) {
        printf("Tree is empty.\n");
        return;
    }

    int height = getHeight(root);
    int levels[MAX_LEVELS][MAX_NODES_PER_LEVEL] = {0};
    int levelCount[MAX_LEVELS] = {0};

    for (int i = 1; i <= height; i++) {
        int idx = 0;
        storeLevel(root, i, levels[i - 1], &idx);
        levelCount[i - 1] = idx;
    }

    // Print levels
    for (int i = 0; i < height; i++) {
        printf("Level %d: ", i);
        for (int j = 0; j < levelCount[i]; j++) {
            printf("%d ", levels[i][j]);
        }
        printf("\n");
    }
}
```

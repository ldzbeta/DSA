```cpp
void reverseDfs(TreeNode* left,TreeNode* right,int level){
        if(!left || !right)return;

        // if(level%2){
            int temp=left->val;
            left->val=right->val;
            right->val=temp;
        // }
        reverseDfs(left->left,right->right,level+1);
        reverseDfs(left->right,right->left,level+1);
}
```

```c
void left_view(node root, int level)
{
    if(root==NULL)
        return;
    if(level==i)
    {
        printf("%d ",root->data);
        i++;
    }
        
    left_view(root->left,level+1);
    left_view(root->right,level+1);

}

void right_view(node root, int level)
{
    if(root==NULL)
        return;
    if(level==i)
    {
        printf("%d ",root->data);
        i++;
    }
        
    right_view(root->right,level+1);
    right_view(root->left,level+1);

}
```

#### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

示例：

```
输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```



### 思路

同步地遍历两棵树上的节点，直接在 t1 上修改。
如果把 mergeTrees 函数作为递归函数，参数 t1 和 t2 是指：当前遍历的节点（子树）

递归。总是关注当前节点

>t1t1 为 null 、t2t2 不是 null，t1t1 换成 t2t2 。（return t2）
>t2t2 为 null、t1t1 不是 null，t1t1 依然 t1t1 。（return t1）
>t1t1 和 t2t2 都为 null，t1t1 依然 t1t1。（return t1）
>t1t1、t2t2 都存在，将 t2t2 的值加给 t1t1 。（t1.val += t2.val）

『子树的合并』交给递归去做，它会对每一个节点做同样的事情。

```js
t1.left = mergeTrees(t1.left, t2.left);
t1.right = mergeTrees(t1.right, t2.right);
```

### code

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} t1
 * @param {TreeNode} t2
 * @return {TreeNode}
 */
var mergeTrees = function(t1, t2) {
    if(!t1)return t2
    if(!t2)return t1
    t1.val +=t2.val
    t1.left = mergeTrees(t1.left,t2.left)
    t1.right = mergeTrees(t1.right,t2.right)
    return t1
};
```


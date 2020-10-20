# 二叉树

二叉树本身作为一种典型的数据结构，常作为解题构件出现。对于二叉树本身来说，最常见的考点是遍历二叉树。遍历二叉树有两种常见遍历方式，递归遍历和非递归遍历。递归遍历相较非递归遍历实现更为简单，但面试时通常会考察非递归遍历的实现🙃。

### 二叉树遍历

- 前序遍历 root -> left -> right
- 中序遍历 left -> root -> right
- 后续遍历 left -> right -> root

#### 递归模版

```Python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if root is None:
            return []
        left_order = self.preorderTraversal(root.left)
        right_order = self.preorderTraversal(root.right)
        return [root.val] + left_order + right_order

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        if root is None:
            return []
        left_order = self.preorderTraversal(root.left)
        right_order = self.preorderTraversal(root.right)
        return left_order + [root.val] + right_order
      
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if root is None:
            return []
        left_order = self.preorderTraversal(root.left)
        right_order = self.preorderTraversal(root.right)
        return left_order + right_order + [root.val]

```

注意：

- Python类中递归调用函数需要使用self
- is None 相较 == None 更高效

#### 非递归模版

**前序非递归**

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        stack, res, cur = [], [], root
        while len(stack) > 0 or cur is not None:
            if cur != None:
                res.append(cur.val)
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                cur = cur.right
        return res
```

说明：

- 栈用来辅助存储已经遍历过的节点，这样在子节点为空节点时可以快速找到父节点。

**中序非递归**

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        stack, res, cur = [], [], root
        while len(stack) > 0 or cur is not None:
            if cur is not None:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                res.append(cur.val)
                cur = cur.right
        return res
```

**后续非递归**

常规思路

核心在于确保遍历完父亲节点的右子树后再弹出父亲节点。

```Python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        stack, res, cur, last_visit = [], [], root, None
        while len(stack) > 0 or cur is not None:
            if cur is not None:
                stack.append(cur)
                cur = cur.left
            else:
                peek = stack[-1] 
                # 判断父亲节点是否存在未遍历的右子节点
                if peek.right is not None and last_visit != peek.right: # 如果右子节点不为空且未被访问
                    cur = peek.right # 遍历右子树
                else:
                    last_visit = stack.pop() # 将右子节点标记为已遍历
                    res.append(last_visit.val) # 输出结果

        return res
```



==推荐思路==

仔细观察后续遍历（left -> right -> root）与前序遍历(root -> left -> right) 的区别，可以发现如果将前序遍历的 left -> right 改为 right -> left 那么会得到与后续遍历恰好相反的遍历顺序。

```Python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        stack, res, cur = [], [], root
        while len(stack) > 0 or cur is not None:
            if cur is not None:
                stack.append(cur)
                res.append(cur.val)
                cur = cur.right
            else:
                cur = stack.pop()
                cur = cur.left
        return res[::-1]
```

#### 层次遍历

二叉树的层次遍历可使用BFS算法来解决
---
layout: post
title: Everything about Binary Trees
gh-repo: sherr3h/sherr3h.github.io
gh-badge: [star, fork, follow]
tags: [DataStructure, Python]
comments: true
---
I'm trying to sum up all sorts of stuffs that you can do with a binary tree.

- Easy tasks: Insert/print by ascending order, get size/sum, get height, inverse a binary tree
- Medium tasks: Vertical traversal (DFS), path sum (DFS) 

### Initialization

```python
class Node:
    def __init__(self,data):
        self.right=self.left=None
        self.data = data

```

### Insert elements by ascending order
```python
class Solution:
    def insert(self,root,data):
        if root is None:
            return Node(data)
        else:
            if data<=root.data:
                cur=self.insert(root.left,data)
                root.left=cur
            else:
                cur=self.insert(root.right,data)
                root.right=cur
        return root

```

### String method to print the tree from left to right 
```python
# in class Solution:
    def _str_(self, root):
        if root is None:
            return '';
        else:
            left_str = self._str_(root._left)
            right_str = self._str_(root._right)
            ret = str(root._value)
            if left_str:
                ret = left_str + ' ' + ret
            if right_str:
                ret = ret + ' ' + right_str
            return ret
```

### Get size of tree
```python
# in class Solution:
    def size(self, root):
        if root is None:
            return 0
        else:
            left_size = self.size(root._left)
            right_size = self.size(root._right)
            return left_size + 1 + right_size
```

### Get sum of all tree elements
```python
# in class Solution:
    def sum(self, root):
        if root is None:
            return 0
        else:
            return (self.sum(root._left)
                    + cur_node._value
                    + self.sum(root._right))
```

### Print a tree
```python
# in class Solution:
    def print_pretty(self, root):
        self._print_pretty_help(root, 0)

    def _print_pretty_help(self, cur_node, l):
        if cur_node is not None:
            self._print_pretty_help(cur_node._right, l + 1)
            print('\t\t'*l + str(cur_node._value))
            self._print_pretty_help(cur_node._left, l + 1)
```

### Get height of tree
```python
# in class Solution:
    def getHeight(self,root):
        if root:
            height_right = 0
            height_left = 0
            height_right = self.getHeight(root.right) + 1
            height_left = self.getHeight(root.left) + 1
            return max(height_right,height_left)
        # else:
        return -1
```

### Inverse binary tree
```python
# in class Solution:
     def invertTree(self, root: Node) -> Node:
        if root is None:
            return None
        else:
            root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
```

### Vertical Order Traversal of a Binary Tree
(https://leetcode.com/explore/challenge/card/august-leetcoding-challenge/549/week-1-august-1st-august-7th/3415/)

```python
# in class Solution:
def verticalTraversal(self, root: Node) -> List[List[int]]:
        from collections import defaultdict
        dic = defaultdict(list)
        self.min_x = 10000
        self.max_x = -10000
        def dfs(root, level_x, level_y):
            self.min_x = min(level_x, self.min_x)
            self.max_x = max(level_x, self.max_x)
            dic[level_x].append((level_y, root.val))
            if root.left:
                dfs(root.left, level_x-1, level_y + 1)
            if root.right:
                dfs(root.right, level_x+1, level_y + 1)
                
        dfs(root, 0, 0)
        
        output = []
        for i in range(self.min_x, self.max_x +1):
            output += [[j for i, j in sorted(dic[i])]]
        return output




```


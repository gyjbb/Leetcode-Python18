# Leetcode-Python18

## 530. Minimum Absolute Difference in BST， 501. Find Mode in Binary Search Tree

June 09, 2023  4h

Congratulations!\
This is the twelfth day for leetcode python study. Today we will learn more about the Binary Tree!\
The challenges today are about ~~need to delete later~~.


## 530. Minimum Absolute Difference in BST
[leetcode](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)\
[Reading link](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.md)\
[video](https://www.bilibili.com/video/BV1DD4y11779/?spm_id_from=pageDriver&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
二叉搜索树的最小绝对差，二叉树遍历上**双指针**操作，优先掌握递归。\
此处定义一个全局变量去记录节点间最小差值，函数就没有返回值了。return; 表示什么也不return。\
pre如何紧跟cur成为当前遍历节点的前一个节点呢？pre = cur\
前两种方法都用了递归，第三种方法用了迭代+stack来实现的！
```python
# ways 1: recursion, 利用中序递增，结合数组
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.vec = []

    def traversal(self, root):
        if root is None:
            return          #什么也不return
        self.traversal(root.left)
        self.vec.append(root.val)   #讲二叉搜索树转换为有序数组
        self.traversal(root.right)

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.vec = []   #初始化数组
        self.traversal(root)
        if len(self.vec) < 2:
            return 0
        result = float('inf')   #要返回最小值，就定义一个超大数
        for i in range(1, len(self.vec)):
            # 统计有序数组的最小差值
            result = min(result, self.vec[i] - self.vec[i-1])
        return result
```
```python
# ways 2: recursion, 利用中序递增，找到该树最小值
class Solution:
    def __init__(self):
        self.result = float('inf')
        self.pre = None
    
    def traversal(self, cur):
        if cur is None:
            return
        self.traversal(cur.left)    #left
        if self.pre is not None:    #middle
            self.result = min(self.result, cur.val - self.pre.val)
        self.pre = cur  #记录前一个
        self.traversal(cur.right)    #right

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.traversal(root)
        return self.result
```
```python
# ways 3: 迭代法
class Solution:
    def getMinimumDifference(self, root):
        stack = []
        cur = root
        pre = None
        result = float('inf')

        while cur is not None or len(stack) > 0:
            if cur is not None:
                stack.append(cur)  # 将访问的节点放进栈
                cur = cur.left  # 左
            else:
                cur = stack.pop()
                if pre is not None:  # 中
                    result = min(result, cur.val - pre.val)
                pre = cur
                cur = cur.right  # 右

        return result
```


## 501. Find Mode in Binary Search Tree
[leetcode](https://leetcode.com/problems/find-mode-in-binary-search-tree/)\
二叉搜索树中的众数， one code skill here!\
对于搜索数，还是使用中序遍历，中是处理逻辑，计算节点值的count。\
pre = cur,把cur的值赋给pre，在下一层循环cur自动更新到下一个节点时，pre帮助记录了上一个节点，也跟着移动了。\
max_count及时更新，result结果集清空，装入新的数值。
```python
# ways 1: recursion, 利用中序递增
class Solution:
    def __init__(self):
        self.maxCount = 0   # 最大频率
        self.count = 0      # 统计频率
        self.pre = None
        self.result  = []

    def searchBST(self, cur):
        if cur is None:
            return      #什么也不return
        
        self.searchBST(cur.left)    #left
        #middle
        if self.pre is None:   #第一个节点
            self.count = 1
        elif self.pre.val == cur.val:   #与前一个节点数值相同
            self.count += 1
        else:   #与前一个节点数值不同
            self.count = 1
        self.pre = cur   #更新上一个节点

        if self.count == self.maxCount:   #如果与最大值频率相同，放进result
            self.result.append(cur.val)
        if self.count > self.maxCount:    # 如果计数大于最大值频率
            self.maxCount = self.count    # 更新最大频率
            self.result = [cur.val]  # 很关键的一步，不要忘记清空result，之前result里的元素都失效了

        self.searchBST(cur.right)    #right
        return

    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.count = 0
        self.maxCount = 0
        self.pre = None     #记录前一个节点
        self.result = []

        self.searchBST(root)
        return self.result
```
```python
# ways 2: use dictionary
from collections import defaultdict
class Solution:
    def searchBST(self, cur, freq_map):
        if cur is None:
            return
        freq_map[cur.val] += 1
        self.searchBST(cur.left, freq_map)
        self.searchBST(cur.right, freq_map)

    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        freq_map = defaultdict(int)  # key:元素，value:出现频率
        result = []
        if root is None:
            return result
        self.searchBST(root, freq_map)
        max_freq = max(freq_map.values())
        for key, freq in freq_map.items():
            if freq == max_freq:
                result.append(key)
        return result
```


## 236. 
二叉树的最近公共祖先, hard!


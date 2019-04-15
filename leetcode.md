
#### 1.两数之和
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
示例:
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]


```python
def twoSum(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: List[int]
    """
    hashmap = {}
    for index, num in enumerate(nums):
        another_num = target - num
        if another_num in hashmap:
            return [hashmap[another_num], index]
        hashmap[num] = index
    return None
```

#### 2.两数相加
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。  
示例：  
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)  
输出：7 -> 0 -> 8  
原因：342 + 465 = 807  


```python

#Definition for singly-linked list.构建单链表
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
    addone = 0
    su =0
    result = []
    while l1!=None or l2!=None:
        if l1!=None:
            su += l1.val
            l1=l1.next
        if l2!= None:
            su += l2.val
            l2 = l2.next
        result.append(su%10)
        su = int(su/ 10)
        #result = result.next
    if su == 1:
        result.append(1)
    return result
        
```

#### 3. 无重复字符的最长子串
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
示例 1:  
输入: "abcabcbb"  
输出: 3   
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。  
示例 2:  
输入: "bbbbb"  
输出: 1  
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。


```python
def lengthOfLongestSubstring(s):
    """
    :type s: str
    :rtype: int
    """
    l1 = list(s)
    result = 0
    j = 0
    a={}
    longestlen = 0
    for i in range(len(l1)):
        if l1[i] in a:        ##重复
            j = max(a[l1[i]],j) #对比当前与之前重复的
        result = max(result,i-j+1) #i-j+1:当前位置
        a[l1[i]] = i+1 # 例子{'a':1}
    return result
#test
a="abcabcbb"
print(lengthOfLongestSubstring(a))
```

    3


#### 4. 寻找两个有序数组的中位数 (困难)
题目：
给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。
请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。
你可以假设 nums1 和 nums2 不会同时为空。

示例 1:
nums1 = [1, 3]，nums2 = [2]，则中位数是 2.0，
示例 2:
nums1 = [1, 2]，nums2 = [3, 4]，则中位数是 (2 + 3)/2 = 2.5

思路:

这道题如果时间复杂度没有限定在$O(log(m+n))$,我们可以用$O(m+n)$的算法解决,用两个指针分别指向两个数组,比较指针下的元素大小,一共移动次数为(m+n + 1)/2​,便是中位数.

首先,我们理解什么中位数:指的是该数左右个数相等.

比如: odd : [1,| 2 |,3],2就是这个数组的中位数,左右两边都只要1位;

even: [1,| 2, 3 |,4],2,3就是这个数组的中位数,左右两边1位;

那么,现在我们有两个数组:

num1: [a1,a2,a3,...an]

nums2: [b1,b2,b3,...bn]

[nums1[:left1],nums2[:left2] | nums1[left1:], nums2[left2:]]

只要保证左右两边**个数**相同,中位数就在|这个边界旁边产生.

如何找边界值,我们可以用二分法,我们先确定num1取m1左半边,那么num2取m2 = (m+n+1)/2 - m1的左半边,找到合适的m1,就用二分法找,关于我的二分法看另一篇文章

当 [ [a1],[b1,b2,b3] | [a2,..an],[b4,...bn] ]

我们只需要比较 b3和a2的关系的大小,就可以知道这种分法是不是准确的!

例如:我们令:

nums1 = [-1,1,3]

nums2 =[2,4,6,8]

当m1 = 3,m2 = 1

median = (num1[m1] + num2[m2])/2

时间复杂度:$O(log(min(m,n)))$

对于代码中边界情况,大家需要自己琢磨.




```python
# 4.寻找两个有序数组的中位数

def findMedianSortedArrays(nums1,nums2):
    n1 = len(nums1)
    n2 = len(nums2)
    if n1 > n2:
        return self.findMedianSortedArrays(nums2,nums1)
    k = (n1 + n2 + 1)//2
    left = 0
    right = n1
    while left < right :
        m1 = left +(right - left)//2
        m2 = k - m1
        if nums1[m1] < nums2[m2-1]:
            left = m1 + 1
        else:
            right = m1
    m1 = left
    m2 = k - m1 
    c1 = max(nums1[m1-1] if m1 > 0 else float("-inf"), nums2[m2-1] if m2 > 0 else float("-inf") )
    if (n1 + n2) % 2 == 1:
        return c1
    c2 = min(nums1[m1] if m1 < n1 else float("inf"), nums2[m2] if m2 <n2 else float("inf"))
    return (c1 + c2) / 2
# test
a=[1,3,5,7,9]
b=[2,4,6,8,10]
mid = findMedianSortedArrays(a,b)
print(mid)
```

    5.5


####  5. 最长回文子串（中等）
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"


```python
#找到最长回文子串
def longestPalindrome(s):
        """
        :type s: str
        :rtype: str
        """
    
        
#test
s = "babad"
rs = longestPalindrome(s)
rs
```


```python

```

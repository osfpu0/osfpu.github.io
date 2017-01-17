---
layout: post
title: 蓄水池抽样算法（revervoir samping）
category: 技术
tags: 算法
keywords: 抽样
---

## 1 简介


```
问题：在长度为N的元素集合中抽取k个元素，要求每个元素被抽取的概率完全相等，N无法确定
```

**蓄水池抽样算法**便是解决该问题的一个优秀算法，其思路是维护一个长度为k的“蓄水池”，初始化为集合的前k个元素，然后从k＋1开始遍历集合后面每一个元素，以`某个概率`(算法核心)替换蓄水池里的元素，遍历完之后，“蓄水池”内的元素便是所需的k个随机元素。


## 2 实现

#### 2.1 抽样大小为1的简单情况

让我们看一个LeetCode上一个运用该算法的题：

有一个长度不确定的链表，我们要随机的取出其中一个节点，要求任意节点被取出的概率相等，以`蓄水池抽样算法`的思想C++实现如下：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
private:
    ListNode* revervoir;
    ListNode* head;
public:
    /** @param head The linked list's head. Note that the head is guanranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        revervoir = head;
        this->head = head;
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        ListNode *p = head->next;
        int i = 2;
        while (p) {
            if (rand() % i == 0) revervoir = p;
            p = p->next;
            i++;
        }
        int result = revervoir->val;
        revervoir = this->head;
        return result;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```
代码很简单，相当于维护一个长度为1的“蓄水池”，把第一个节点放进去，然后遍历其余节点，遍历第二个节点时，以1/2概率替换“蓄水池”里的节点，遍历第三个节点时以1/3概率替换“蓄水池”里的节点，以此类推。
	
简单的验证一下其正确性：

* 当只有一个节点的时候，其取出概率为1
* 当有两个节点时，第二个节点有1/2概率替换第一个节点，每个节点被取出的概率为1/2
* 当有三个节点时，很明显，取出第三个节点的概率为1/3，那么前两个节点被取出的概率为2/3 ＊ 1/2 ＝ 1/3
* 很容易用数学归纳法证明n个节点时，每个节点被取出的概率为1/n

#### 2.2 抽样大小为k的普通情况
伪代码如下，配合注释很容易理解，在遍历到第i个元素时，以k/i的概率替换蓄水池中的元素。其正确性的证明见下一小节。

```c
/*
 * S是元素集合，抽取结果存到R中， n > k
 */
ReservoirSample(S[1..n], R[1..k])
  // 初始化
  for i = 1 to k
      R[i] := S[i]

  // 以逐渐递减测概率替换R中的元素
  for i = k + 1 to n
    j := random(1, i)   // 替换概率为 k／i
    if j <= k
        R[j] := S[i]
```


## 3 证明
同大小为1的“蓄水池”一样，对于大小为k的“蓄水池”，我们可以使用数学归纳法来证明上述算法的正确性，即证明对于所有的i(k + 1到n），元素集合中每个元素最终存在于“蓄水池”中的概率为k / i：

* 当 i = k 时， 每个元素被选中的概率为 k / k = 1 = `k / i`
* 假设 i = n - 1 时，每个元素被选中的概率为 k / (n - 1) = `k / i`
* 则 i = n 时，最后一个元素被选中的概率为 k / n = `k / i`，前 n － 1 个元素被选中的情况是在i = n - 1时被选中，在i ＝ 1时没有被替换（被替换的概率为1 / n），则前 n － 1 个元素被选中的概率为 (k / (n - 1)) * ((n - 1) / n) = k / n = `k / i`

## 4 应用



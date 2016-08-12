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

首先，先看一个LeetCode上一个运用该算法的题：

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

## 2 实现

```c
/*
 * S是元素集合，抽取结果存到R中
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

## 4 应用



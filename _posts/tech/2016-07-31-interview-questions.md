---
layout: post
title: 常见面试题
category: 技术
tags: 算法
keywords: 面试题，算法
---

## 手写快排
```c++
void quick_sort(int nums[], int begin, int end) {
	if (begin >= end) {
		return;
	}
	int i = begin, j = end;
	int key = nums[begin];
	while (i < j) {
		while (i < j && nums[j] >= key) j--;
		nums[j] = nums[i];
		while (i < j && nums[i] <= key) i++;
		nums[i] = nums[j];
	}
	nums[i] = key;
	quick_sort(nums, begin, i - 1);
	quick_sort(nums, i + 1, end);
}
```

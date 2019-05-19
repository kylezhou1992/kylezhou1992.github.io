---
layout:     post
title:      3sum--LeeCode
subtitle:   CodeTraining 3sum--LeeCode
date:       2019-05-19
author:     MY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - LeeCode
    - C/C++
---
# 3sum--LeeCode
Descripe:
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:
The solution set must not contain duplicate triplets.

Example:
```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

##First Solution
首先使用快速排序，通过三个for循环遍历数组，找到三个数之和为0的数据，并且返回。对函数的参数进行了修改，因为没有理解源函数retrunColumnSizes这个双重指针的含义
```C
int** threeSum(int* nums, int numsSize, int* returnSize);
void quickSort(int* nums, int numsSize, int lo, int hi);
void exec(int* nums, int i, int j);
int main()
{
	int **result = NULL;
	int i = 0;
	int nums[6] = {-1, 0, 1, 2, -1, -4};
	int size = 0;
	result = threeSum(nums, 6, &size);
	for (i = 0; i < size; i++)
	{
		printf("%d, %d, %d\n", result[i][0], result[i][1], result[i][2]);
	}
	free(*result);
	return 0;
}

/**
* Return an array of arrays of size *returnSize.
* The sizes of the arrays are returned as *returnColumnSizes array.
* Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
*/
int** threeSum(int* nums, int numsSize, int* returnSize) {
	int i, j, k, p;
	int t = 0;
	int **result = NULL;
	int sum = -1;
	result = (int **)malloc(sizeof(int *) * numsSize);
	quickSort(nums, numsSize, 0, numsSize - 1);

	for (i = 0; i < numsSize; i++)
	{
		for (j = i + 1; j < numsSize; j++)
		{
			for (k = j + 1; k < numsSize; k++)
			{
				sum = nums[i] + nums[j] + nums[k];
				if (sum == 0)
				{
					/*if the digits that their sum is zero is repeatable, they should be skipped.*/
					if (t != 0 && nums[i] == result[t - 1][0] && nums[j] == result[t - 1][1])
						continue;
					result[t] = (int *)malloc(sizeof(int) * 3);
					result[t][0] = nums[i];
					result[t][1] = nums[j];
					result[t][2] = nums[k];
					t++;
				}
			}
		}
	}
	*returnSize = t;
	return result;
}

/*quick sort*/
void quickSort(int* nums, int numsSize, int lo, int hi)
{
	int i = lo;
	int j = hi;
	int temp = nums[i];

	if (lo >= hi)
		return;

	while (true)
	{
		while (nums[++i] < temp)
		{
			if (i == hi) break;
		}

		while (nums[--j] > temp)
		{
			if (j == lo) break;
		}
		if (i >= j) break;
		exec(nums, i, j);
	}
	exec(nums, lo, j);
	quickSort(nums, numsSize, lo, j - 1);
	quickSort(nums, numsSize, j + 1, hi);

}

void exec(int* nums, int i, int j)
{
	int temp = nums[i];
	nums[i] = nums[j];
	nums[j] = temp;
}
```
time complexity
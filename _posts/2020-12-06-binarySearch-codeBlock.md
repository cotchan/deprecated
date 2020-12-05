---
title: 이분탐색(Binary Search) 손코딩 코드# TITLE
author: Cotchan # AUTHOR
date: 2020-12-06 00:47:21 +0800 # YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [Algorithm, CodeBlock] # [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [algorithm] # [TAG]     # TAG names should always be lowercase
---

## Code

이분탐색 손코딩 코드입니다.    

+ ver.1

```c++
bool binarySearch(int targetNumber)
{
	int st, mid, en;
	st = 0;
	en = arr.size();

	while (st < en)
	{
		mid = (st + en) / 2;

		if (arr[mid] == targetNumber) 
		{
			return true;
		}
		else if (arr[mid] < targetNumber)
		{
 			st = mid + 1;
 		}
		else
 		{
			en = mid;
		}
	}

	return false;
}
```

---

+ ver.2

```c++
bool binarySearch(int targetNumber)
{
	int st, mid, en;
	st = 0;
	en = arr.size();

	while (st <= en)
	{
		mid = (st + en) / 2;

		if (arr[mid] == targetNumber) 
		{
			return true;
		}
		else if (arr[mid] < targetNumber)
		{
			st = mid + 1;
		}
		else
		{
			en = mid - 1;
		}
	}

	return false;
}
```

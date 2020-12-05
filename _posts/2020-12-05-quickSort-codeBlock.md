---
title: 퀵소트 손코딩 코드 # TITLE
author: cotchan # AUTHOR
date: 2020-12-05 16:37:21 +0800 # YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [Algorithm, CodeBlock] # [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [algorithm] # [TAG]     # TAG names should always be lowercase
---

## Code

퀵소트 손코딩 샘플 코드입니다.      
저도 이전에 다른 분의 포스팅을 참고하여 작성하였던 내용인데 출처가 기억나지 않아 생략하였습니다.    
혹시 출처를 알려주신다면 반영해놓도록 하겠습니다.        

```c++
void quickSort(int *arr, int st, int en)
{
	if (en - st <= 1)
		return;

	int pivot = arr[st];
	int l = st + 1;
	int r = en - 1;

	while (true)
	{
		while ((l <= r) && (arr[l] <= pivot)) l++;
		while ((l <= r) && (arr[r] >= pivot)) r--;

		if (l > r)
			break;

		swap(arr[l],arr[r]);
	}

	swap(arr[st], arr[r]);

	quickSort(arr, st, r);
	quickSort(arr, r+1,en);
}
```

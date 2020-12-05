---
title: 머지소트 손코딩 코드 # TITLE
author: cotchan # AUTHOR
date: 2020-12-05 16:37:21 +0800 # YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [Algorithm, CodeBlock] # [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [algorithm] # [TAG]     # TAG names should always be lowercase
---

## Code

머지소트 손코딩 샘플 코드입니다.      
저도 이전에 다른 분의 포스팅을 참고하여 작성하였던 내용인데 출처가 기억나지 않아 생략하였습니다.    
혹시 출처를 알려주신다면 반영해놓도록 하겠습니다.        

```c++
int tmp[N];	//merge 과정에서 데이터를 임시로 담을 변수. 주어진 배열의 크기만큼 메모리를 선언해줍니다.


void merge(int *arr, int st, int en)
{
	int idx1, idx2, idx3, mid;
	
	mid = (st+en)/2;
	idx1 = st;
	idx2 = mid + 1;
	idx3 = st;

	while (idx1 <= idx2 && idx1 <= mid && idx2 <= en)
	{
		if (arr[idx1] < arr[idx2])
		{
			tmp[idx3++] = arr[idx1++];
		}
		else
		{
			tmp[idx3++] = arr[idx2++];
		}
	}

	for (int rest = idx1; rest <= mid; ++rest)
		tmp[idx3++] = arr[rest];

	for (int rest = idx2; rest <= en; ++rest)
		tmp[idx3++] = arr[rest];

	for (int i = st; i <= en; ++i)
		arr[i] = tmp[i];
}

void mergeSort(int *arr, int st, int en)
{
	if (st >= en)
	{
		return;
	}
	else if (st + 1 == en)
	{
		if (arr[st] > arr[st+1])
		{		
			swap(arr[st],arr[st+1]);
		}

		return;
	}
	else
	{
		int mid = (st+en)/2;
		mergeSort(arr, st, mid);
		mergeSort(arr, mid+1, en);
		merge(arr, st, en);
		return;
	}
}

//호출 방법: mergeSort(arr, 0, arr.length() -1);
```

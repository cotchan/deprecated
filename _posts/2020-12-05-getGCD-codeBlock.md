---
title: 최대공약수(GCD) 알고리즘 코드 # TITLE
author: cotchan # AUTHOR
date: # 2020-12-05 18:15:21 +0800
categories: [Algorithm, CodeBlock] # [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [algorithm] # [TAG]     # TAG names should always be lowercase
---

## 최대공약수(GCD) - for문

추후 업데이트 예정입니다.

## 최대공약수(GCD) - 재귀

```c++
int getGCD(int a, int b)
{
	if (b == 0)
	{
		return a;
	}
	else
	{
		return getGCD(b, a % b);
	}
}
```

---

+ 출처
    + [[유클리드 알고리즘] GCD 최대공약수 (반복문, 재귀)](https://blockdmask.tistory.com/53)

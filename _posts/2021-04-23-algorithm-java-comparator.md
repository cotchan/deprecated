---
title: Java) Comparator 사용 방법
author: cotchan
date: 2021-04-23 08:36:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. Comparator 사용 예시

+ **String을 리스트로 받고, `길이가 짧은 순서대로 정렬`하고 싶은 경우**

+ param1, param2를 비교 시 
  + 순서가 그대로 유지되길 원한다면 
      + **`return값이 0이나 음수`이면 자리바꿈을 하지 않습니다.**
  + 순서가 바뀌길 원한다면 
      + **`return값이 양수`이면 자리바꿈을 수행합니다.**

```java
int phoneNumberSize = Integer.parseInt(br.readLine());
			ArrayList<String> numberList = new ArrayList<>();
			
			for (int loop = 0; loop < phoneNumberSize; ++loop)
			{
				String phoneNumber = br.readLine();
				numberList.add(phoneNumber);
			}
			
			Collections.sort(numberList, (a,b) -> {
				if (a.length() < b.length()) return -1;
				else if (a.length() == b.length()) return 0;
				else return 1;				
			});
```


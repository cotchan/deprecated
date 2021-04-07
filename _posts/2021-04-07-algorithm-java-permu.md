---
title: Java) 순열/조합 만드는 방법
author: cotchan
date: 2021-04-07 08:20:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. 순열, 조합 코드 Summary 

+ **`순열`**

```java
//Header
private static void permutation(int pickCnt);

//호출 시
permutation(0);
```

---

+ **`조합`**

```java
//Header
private static void combination(int idx, int pickCnt);

//호출 시
combination(0,0);
```

---

## 2. 순열 코드

+ **`Main Logic`**

```java
private static void permutation(int pickCnt);


public class Main {

	public static int N, C;
	public static int origin[], result[];
	public static boolean isSelected[];
  
	public static List<int[]> results;
	
	public static void main(String arg[]) {
				
		origin = new int[N];
		isSelected = new boolean[N];
		result = new int[C];

		permutation(0);
	}
	
	private static void permutation(int pickCnt) {
	
		if(pickCnt == C)
		{
			int[] candiate = result.clone();
			results.add(candiate);
			return;
		}
    
		// 해당 자리에 뽑을 가능한 모든 수에 대해 시도
		for(int i = 0; i < N; i++)
		{
			if(isSelected[i]) continue;

			result[pickCnt] = origin[i];
			
			//set isSelected true
			permutation(pickCnt + 1); // 다음 자리의 순열 뽑기
			//set isSelected false
		}
	}    
}  
```

---

+ **`Full Code`**

```java
public class Main {

  	//N개의 숫자에서 C개의 숫자로 순열을 만드는 경우
	public static int N, C;
  
  	//origin: 주어진 N개의 숫자를 담는 배열
  	//result: 하나의 순열이 담기는 배열
	public static int origin[], result[];
  
  	//isSelected: 숫자 사용 여부 체크
	public static boolean isSelected[];
  
  	//results: result list
	public static List<int[]> results = new LinkedList<>();
	
	public static void main(String arg[]) throws IOException {
				
		origin = new int[N];
		isSelected = new boolean[N];
		result = new int[C];

		permutation(0);
	}
	
	private static void permutation(int pickCnt) {
	
		if(pickCnt == C)
		{
			int[] candiate = result.clone();
			results.add(candiate);
			return;
		}
		// 해당 자리에 뽑을 가능한 모든 수에 대해 시도
		for(int i = 0; i < N; i++)
		{
			if(isSelected[i]) continue;
			result[pickCnt] = origin[i];
			isSelected[i] = true;
			permutation(pickCnt + 1); // 다음 자리의 순열 뽑기
			isSelected[i] = false;
		}
	}    
}  
```

---

## 2-2. 순열 예시 문제

+ [[BOJ15649번: N과M(1)]](https://www.acmicpc.net/problem/15649)

+ **정답 코드**

```java
import java.io.*;
import java.util.*;

public class BOJ15649 {

  	//N개의 숫자에서 C개의 숫자로 순열을 만드는 경우
	public static int N, C;
  
  	//origin: 주어진 N개의 숫자를 담는 배열
  	//result: 하나의 순열이 담기는 배열
	public static int origin[], result[];
  
  	//isSelected: 숫자 사용 여부 체크
	public static boolean isSelected[];
  
  	//results: result list
	public static List<int[]> results = new LinkedList<>();
	
	public static void main(String arg[]) throws IOException{
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		
		String input[] = br.readLine().split(" ");
		N = Integer.parseInt(input[0]);
		C = Integer.parseInt(input[1]);
		
		origin = new int[N];
		isSelected = new boolean[N];
		result = new int[C];
		
		for (int i = 0; i < N; ++i)
			origin[i] = i+1;
		
		permutation(0);
	}
	
	private static void permutation(int idx){
		if(idx == C){
			int[] candiate = result.clone();
			results.add(candiate);
			return;
		}
		// 해당 자리에 뽑을 가능한 모든 수에 대해 시도
		for(int i = 0; i < N; i++){
			if(isSelected[i]) continue;
			result[idx] = origin[i];
			isSelected[i] = true;
			permutation(idx + 1); // 다음 자리의 순열 뽑기
			isSelected[i] = false;
		}
	}    
}
```

---

## 3. 조합 코드


+ **`Main Logic`**

```java
public static void combination(int idx, int pickCnt);

	public static void combination(int idx, int pickCnt) {
				
		if (pickCnt == C)
		{
			int[] candidate = picked.clone();
			results.add(candidate);
			return;
		}
		
		if (idx == N)
		{
			return;
		}
		
		if (!isSelected[idx])
		{
			isSelected[idx] = true;
			picked[pickCnt] = origin[idx];
			//하나를 더 뽑고 다음 depth로 이동
			combination(idx + 1, pickCnt + 1);
			isSelected[idx] = false;
		}
		
		//하나를 더 뽑지 않고 다음 depth로 이동
		combination(idx + 1, pickCnt);
	}
	
	public static void main(String[] args) throws IOException {
			
		origin = new int[N];
		isSelected = new boolean[N];
		picked = new int[C];
		
		combination(0,0);
	}
```

---

+ **`Full Code`**

```java
```

---

## 3-2. 조합 예시 문제

+ [[BOJ15649번: N과M(2)]](https://www.acmicpc.net/problem/15650)

+ **정답 코드**

```java
package BOJ;

import java.io.*;
import java.util.*;

public class BOJ15650 {

	public static int N,C;
	public static int[] origin;
	public static boolean[] isSelected;
	public static int[] picked;
	public static List<int[]> results = new LinkedList<>();
	
	public static void combination(int idx, int pickCnt) {
				
		if (pickCnt == C)
		{
			int[] candidate = picked.clone();
			results.add(candidate);
			return;
		}
		
		if (idx == N)
		{
			return;
		}
		
		if (!isSelected[idx])
		{
			isSelected[idx] = true;
			picked[pickCnt] = origin[idx];
			combination(idx + 1, pickCnt + 1);
			isSelected[idx] = false;
		}
		
		combination(idx + 1, pickCnt);
	}
	
	public static void main(String[] args) throws IOException {
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		
		String[] info = br.readLine().split(" ");
		N = Integer.parseInt(info[0]);
		C = Integer.parseInt(info[1]);
		
		origin = new int[N];
		isSelected = new boolean[N];
		picked = new int[C];
		
		for (int i = 0; i < N; ++i)
			origin[i] = i + 1;
		
		combination(0,0);
		
		for (int[] result : results)
		{
			StringBuilder sb = new StringBuilder();
			
			for (int number : result)
			{
				sb.append(number);
				sb.append(" ");
			}
			
			bw.append(sb.toString());
			bw.newLine();
		}
		
		bw.flush();
		bw.close();
		br.close();
	}
}
```

---

+ 출처

---
title: Java) 순열 / 조합 만드는 방법
author: cotchan
date: 2021-01-14 08:45:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
계속 업데이트할 예정입니다.

---

## 1. 순열

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
	
	private static void permutation(int pickCnt){
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
	
	public static void main(String arg[]) throws IOException{
				
		origin = new int[N];
		isSelected = new boolean[N];
		result = new int[C];

		permutation(0);
	}
	
	private static void permutation(int pickCnt){
		if(pickCnt == C){
			int[] candiate = result.clone();
			results.add(candiate);
			return;
		}
		// 해당 자리에 뽑을 가능한 모든 수에 대해 시도
		for(int i = 0; i < N; i++){
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

## 1-2. 순열 예시 문제

---

## 2. 조합

---

## 2-2. 조합 예시 문제




---

+ 출처
  + [정렬 HOW TO](https://docs.python.org/ko/3/howto/sorting.html)

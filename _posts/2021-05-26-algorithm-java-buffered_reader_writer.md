---
title: Java) 입출력 방법(BufferedReader, BufferedWriter)
author: cotchan
date: 2021-05-26 08:50:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. BufferedReader

+ **파일이나 콘솔로부터 데이터를 읽을 때는 `BufferedReader`를 사용합니다.**
+ 보통 `readLine()`을 사용하여 한 줄씩 읽습니다.
+ 추가로 공백(스페이스 바)을 구분해야 한다면 `split()`을 사용합니다.

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

String[] info = br.readLine().split(" ");
int start = Integer.parseInt(br.readLine());
```

---

## 2. BufferedWriter

+ **결과값을 콘솔로 찍을 때는 `BufferedWriter`를 사용합니다.**
+ 크게 아래 3개의 메소드를 사용합니다.
  + **`.append(CharSequence csq)`**
  + + **`.newLine()`**
  + **`.flush()`**

```java
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

int[] result = djikstra(start);
for (int dist : result) {
    StringBuilder sb = new StringBuilder();
			
    if (dist == INF) {
      sb.append("INF");
    } else {
      sb.append(dist);
    }

    bw.append(sb.toString()); //1. 데이터 넣기
    bw.newLine(); //2. 개행 문자 넣기
}

bw.flush();   //3. 실제 출력 
```

---


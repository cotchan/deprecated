---
title: db) ON절 의미와 WHERE절과의 차이
author: cotchan 
date: 2021-05-09 20:47:21 +0800 
categories: [Database]
tags: [database]
---

+ 아래 출처의 내용들을 바탕으로 작성한 내용입니다.    
+ **개인 공부 목적으로 작성한 글입니다.**

---

## 1. ON 의미

```
SELECT * 
  FROM 테이블1 T1 
  LEFT JOIN 테이블2 T2 ON (T2.테이블을 연결할 컬럼 = T1.테이블을 연결할 컬럼 );
```

+ **`조인문을 사용할 때 ON절을 이용해서 해당 조건으로 테이블을 조인하게 됩니다.`**
+ **즉, `서로 다른 두 테이블을 조인으로 연결할 컬럼들을 설정합니다.`**
  + 파라미터 바인딩 가능합니다.
  + AND 조건으로 연결할 컬럼을 여러 개 나열도 가능합니다.

---

## 2. ON절 실행 순서

+ ON JOIN 절이 추가된 SQL문의 순서는 FROM 절 다음으로 진행합니다. 
+ **`즉 ON절은 WHERE절 보다 순서상으로 더 빠릅니다.`**
+ **FROM -> ON -> JOIN -> WHERE -> ...**

---

## 3. ON과 WHERE의 차이

+ **ON절과 WHERE절의 차이는 `OUTER JOIN에서 발생합니다.`**

+ test data

```sql
SELECT *
FROM dept d LEFT OUTER JOIN emp e
ON d.deptno = e.deptno;
```

![스크린샷 2021-05-09 오후 8 46 11](https://user-images.githubusercontent.com/75410527/117571077-f64c1200-b107-11eb-8775-3da3312b681c.png)

---

+ case1. WHERE절에 조건문이 있는 경우
    + dept 테이블에 DEPTNO 40인 값이 존재하지만 where 절을 만족하지 못하므로 결과에서 삭제해버립니다. 

```sql
SELECT d.deptno, sum(e.sal)
FROM dept d LEFT OUTER JOIN emp e
ON d.deptno = e.deptno
WHERE e.sal > 2000
GROUP BY d.deptno
ORDER BY d.deptno;
```

![스크린샷 2021-05-09 오후 8 46 34](https://user-images.githubusercontent.com/75410527/117571083-fd732000-b107-11eb-9119-5488b0dfa064.png)

---

+ case2. ON절에 조건문이 있는 경우
    + dept 테이블에 DEPTNO 40인 값이 e.sal > 2000 조건을 만족하지 못하지만 WHERE절 없는 OUTER JOIN의 이므로 결과에 나옵니다.

```sql
SELECT d.deptno, sum(e.sal)
FROM dept d LEFT OUTER JOIN emp e
ON d.deptno = e.deptno AND e.sal > 2000
GROUP BY d.deptno
ORDER BY d.deptno;
```

![스크린샷 2021-05-09 오후 8 46 43](https://user-images.githubusercontent.com/75410527/117571088-019f3d80-b108-11eb-880a-83d79a0f3653.png)

---

+ **OUTER JOIN을 사용하는 의도는 `특정 테이블의 모든 값이 포함된 결과를 보기 위함입니다.`**
+ **where절을 사용하면 `where 절에 맞지 않는 해당 행은 그냥 지워버립니다.`**
+ **ON절의 경우 `JOIN할 때 해당 조건을 처리하고` `outer 조인이기 때문에 dept table에 존재하고 emp table에는 존재하지 않는 결과도 함께 출력합니다.`**

---
+ 출처
    + [[Oracle] ON절과 WHERE절 조건 차이, JOIN 대상 차이](https://myjamong.tistory.com/229)
    + [INNER JOIN, LEFT JOIN(OUTER JOIN) 비교 이너조인, 아우터조인](https://aljjabaegi.tistory.com/13)

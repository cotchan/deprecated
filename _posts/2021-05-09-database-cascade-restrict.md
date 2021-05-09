---
title: db) 외래키 RESTRICT, CASCADE 옵션
author: cotchan 
date: 2021-05-09 18:40:21 +0800 
categories: [Database]
tags: [database]
---

+ 아래 출처의 내용들을 바탕으로 작성한 내용입니다.    
+ 개인 공부 목적으로 작성한 포스팅입니다.

---

## 1. 예시 테이블

```sql
CREATE TABLE comments
(
    seq       bigint       NOT NULL AUTO_INCREMENT,              -- 댓글 기본키(PK)
    user_seq  bigint       NOT NULL,                             -- 댓글 작성자 PK
    post_seq  bigint       NOT NULL,                             -- 댓글 대상 post PK
    contents  varchar(500) NOT NULL,                             -- 댓글 내용
    create_at datetime     NOT NULL DEFAULT CURRENT_TIMESTAMP(), -- 작성일자
    PRIMARY KEY (seq),
    CONSTRAINT fk_comment_to_user FOREIGN KEY (user_seq) REFERENCES users (seq) ON DELETE RESTRICT ON UPDATE RESTRICT,
    CONSTRAINT fk_comment_to_post FOREIGN KEY (post_seq) REFERENCES posts (seq) ON DELETE CASCADE ON UPDATE CASCADE
);
``` 

---

## 2. INTRO

+ 이해하기 쉽게 쓰기 위해 아래 내용을 전제합니다.
+ **전제: `comments 테이블은 users 테이블과 posts 테이블을 참조`하고 있습니다.**
  + 그러므로 
    + **users 테이블과 posts 테이블은 `참조의 대상이 되는 개체`입니다.**
    + **comments 테이블은 `참조하고 있는 개체`입니다.**

+ **외래키를 지정할 때는 몇 가지 옵션이 있습니다.**

---

## 2-1. 변경 제약

+ **`ON UPDATE {RESTRICT | CASCADE | NO ACTION | SET NULL}`**

---

## 2-2. 삭제 제약

+ **`ON DELETE {RESTRICT | CASCADE | NO ACTION | SET NULL}`**

---

## 3. RESTRICT 옵션

+ users 개체를 변경/삭제할 때 users 개체를 참조하고 있는 comments 개체가 존재하면 **`users 개체에 대한 명령(변경/삭제)이 취소됩니다.`**
+ A 개체를 변경/삭제할 때 다른 개체가 변경/삭제할 A 개체를 참조하고 있을 경우 A 개체의 변경/삭제가 취소됩니다.

---

## 4. CASCADE 옵션

+ users 개체를 변경/삭제할 때 users 개체를 참조하고 있는 comments 개체가 존재하면 **`users 개체를 참조하고 있는 모든 comments 개체들이 변경/삭제됩니다.`**
+ A 개체를 변경/삭제할 때 다른 개체가 변경/삭제할 A 개체를 참조하고 있을 경우 A개체를 참조하고 있는 모든 다른 개체도 함께 변경/삭제됩니다.

---

## 5. SET NULL 옵션

+ A 개체를 변경/삭제할 때 다른 개체가 변경/삭제할 A 개체를 참조하고 있을 경우 다른 개체에서 A 개체를 참조하고 있는 값이 NULL로 세팅됩니다.

---

## 6. NO ACTION

+ MySQL에서는 RESTRICT과 동일합니다.

---
+ 출처
    + [MYSQL 외래키(Foreign key) 지정(RESTRICT, CASCADE, NO ACTION, SET NULL)](https://h5bak.tistory.com/125)
    + [Mysql 외래키(Foreign key) 정리](https://thereclub.tistory.com/30)
    + [Mysql 외래키 간단히 정리](https://sshkim.tistory.com/129)

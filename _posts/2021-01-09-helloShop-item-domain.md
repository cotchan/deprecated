---
title: HelloShop) 상품 도메인 개발(Entity, Repo, Service, Test)
author: cotchan
date: 2021-01-09 14:10:21 +0800
categories: [Project, HelloShop]
tags: # [TAG]     # TAG names should always be lowercase
---

## 0. 구현 기능
  + 상품 등록
  + 상품 목록 조회
  + 상품 수정


---

## 1. 상품 Entity 개발 (비즈니스 로직 추가)

+ 객체지향적 관점에서 **`데이터를 가지고 있는 쪽에` `비즈니스 메서드가 있는 것`이 가장 바람직합니다.**

+ **비즈니스 로직 분석**
  + `addStock()` 메서드는 파라미터로 넘어온 수만큼 재고를 늘립니다. 이 메서드는 재고가 증가하거나 상품 주문을 취소해서 재고를 다시 늘려야 할 때 사용합니다.
  + `removeStock()` 메서드는 파라미터로 넘어온 수만큼 재고를 줄입니다. 만약 재고가 부족하면 예외가 발생합니다. 주로 상품을 주문할 때 사용합니다.

```java
package jpabook.jpashop.domain.item;

import jpabook.jpashop.domain.Category;
import jpabook.jpashop.exception.NotEnoughStockException;
import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Getter @Setter

//상속 전략을 적어줘야 합니다.
//우리의 경우 SingleTable 전략
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)

//Book일 때, Album일 때, Movie일 때 어떻게 할거야를 구분해주는 것
@DiscriminatorColumn(name = "dtype")
public abstract class Item {

    @Id
    @GeneratedValue
    @Column(name = "item_id")
    private Long id;

    private String name;
    private int price;
    private int stockQuantity;

    @ManyToMany(mappedBy = "items")
    private List<Category> categories = new ArrayList<>();

    //== 비즈니스 로직 추가 ==//
    //재고를 늘리고 줄이는 것
    //변경해야 될 일이 있으면, setter를 사용하는 것이 아니라,
    //addStock, removeStock과 같이 action 비즈니스 메서드를 통해 변경을 해야 합니다.
    /**
     * stock 증가
     */
    public void addStock(int quantity) {
        this.stockQuantity += quantity;
    }

    /**
     * stock 감소
     */
    public void removeStock(int quantity) {
        int restStock = this.stockQuantity -= quantity;
        if (restStock < 0) {
            throw new NotEnoughStockException("need more stock");
        }

        this.stockQuantity = restStock;
    }
}
```

+ 보통 개발을 하는 경우 
  1. `ItemService`에서 `stockQuantity`를 가져와서 값을 만들기 
  2. 그 다음에 setItem.`setStock()`하는식으로 개발을 합니다.

+ 재고를 넣고 빼는 로직은 `stockQuantity` 값을 통해 일어납니다.
+ 이 값을 가지고 있는 것이 `Item Entity` 이기에 Item Entity에 비즈니스 로직이 있는 게 관리하기가 쉽습니다.

---

## 1-2. 예외 추가

+ **`NotEnoughStockException` 예외 추가**

```java
package jpabook.jpashop.exception;

public class NotEnoughStockException extends RuntimeException {

    public NotEnoughStockException() {
        super();
    }

    public NotEnoughStockException(String message) {
        super(message);
    }

    public NotEnoughStockException(String message, Throwable cause) {
        super(message, cause);
    }

    public NotEnoughStockException(Throwable cause) {
        super(cause);
    }
}
```

---

## 1-3. Album Entity

```java
package jpabook.jpashop.domain.item;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.DiscriminatorValue;
import javax.persistence.Entity;

@Entity
//이 값으로 구분한다는 뜻
@DiscriminatorValue("A")
@Getter @Setter
public class Album extends Item {

    private String artist;
    private String etc;
}
```

---

## 1-4. Book Entity

```java
package jpabook.jpashop.domain.item;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.DiscriminatorValue;
import javax.persistence.Entity;

@Entity
//이 값으로 구분한다는 뜻
@DiscriminatorValue("B")
@Getter @Setter
public class Book extends Item {

    private String author;
    private String isbn;
}
```

---

## 1-5. Movie Entity 

```java
package jpabook.jpashop.domain.item;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.DiscriminatorValue;
import javax.persistence.Entity;

@Entity
//이 값으로 구분한다는 뜻
@DiscriminatorValue("M")
@Getter @Setter
public class Movie extends Item {

    private String director;
    private String actor;
}
```

---

## 2. 상품 리포지토리 개발


```java
package jpabook.jpashop.repository;

import jpabook.jpashop.domain.item.Item;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import java.util.List;

@Repository
@RequiredArgsConstructor
public class ItemRepository {

    private final EntityManager em;

    //상품 저장
    public void save(Item item) {

        //item은 JPA에 저장하기 전까지는 id가 없습니다.
        //id가 없다는 것은, '새로 생성한 객체'라는 뜻입니다.
        if (item.getId() == null)
        {
            em.persist(item);
        }
        //id값이 있다는 것은, DB에 한 번 등록이 되었던 것을 '가져온 것'.
        //그래서 여기서의 save는 update의 의미로 이해하면 됩니다.
        else
        {
            //update
            em.merge(item);
        }
    }

    public Item findOne(Long id) {
        return em.find(Item.class, id);
    }

    //'여러 개' 찾는 것은 JPQL을 작성해야 합니다.
    public List<Item> findAll() {
        return em.createQuery("select i from Item i", Item.class)
                .getResultList();
    }
}
```

+ **기능 설명**
  + `save()` 
    + `id`가 없으면 신규 상품으로 보고 `persist()` 실행
    + `id`가 있으면 이미 데이터베이스에 저장된 엔티티를 수정한다고 보고 `merge()` 실행


-- 


## 3. 상품 서비스 개발

+ **상품 서비스는 상품 리포지토리에 단순히 위임만 하는 클래스**

```java
package jpabook.jpashop.service;

import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.repository.ItemRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class ItemService {

    private final ItemRepository itemRepository;

    @Transactional
    public void saveItem(Item item) {
        itemRepository.save(item);
    }

    public List<Item> findItems() {
        return itemRepository.findAll();
    }

    public Item findOne(Long id) {
        return itemRepository.findOne(id);
    }
}
```

---

## 4. 상품 기능 테스트

+ **기능 설명**
  + 상품 저장
  + 상품 조회(단일 상품)
  + 상품 조회(상품 List)

```java
package jpabook.jpashop.service;

import jpabook.jpashop.domain.item.Album;
import jpabook.jpashop.domain.item.Book;
import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.domain.item.Movie;
import jpabook.jpashop.repository.ItemRepository;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

import static org.junit.Assert.*;

@RunWith(SpringRunner.class)
@Transactional
@SpringBootTest
public class ItemServiceTest {

    //상품_저장
    //상품_조회_단일
    //상품_조회_List

    @Autowired
    ItemService itemService;

    @Autowired
    ItemRepository itemRepository;

    @Test
    public void 상품_저장() {

        //given
        Item item = new Book();
        itemService.saveItem(item);

        //when
        //then
        assertEquals(item, itemService.findOne(item.getId()));
    }

    @Test
    public void 상품_조회_단일() {

        //given
        Item item = new Album();
        itemService.saveItem(item);

        //when
        //then
        assertEquals(item, itemService.findOne(item.getId()));
    }

    //상품 조회 (List)
    @Test
    public void 상품_조회_List() {

        //given
        Item album = new Album();
        itemService.saveItem(album);

        Item book = new Book();
        itemService.saveItem(book);

        Item movie = new Movie();
        itemService.saveItem(movie);

        //when
        //then
        assertEquals(album, itemService.findOne(album.getId()));
        assertEquals(book, itemService.findOne(book.getId()));
        assertEquals(movie, itemService.findOne(movie.getId()));

        List<Item> items = itemService.findItems();
        assertEquals(items.size(), 3);
    }
}
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)



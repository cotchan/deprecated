---
title: Spring-Boot) 변경 감지와 병합(merge)
author: cotchan 
date: 2021-01-11 21:26:21 +0800 
categories: [Spring-Boot, Spring-Boot_JPA]
tags: [spring-boot] 
---


## 1. 준영속 엔티티

+ **영속성 컨텍스트가 더는 관리하지 않는 엔티티를 말합니다.**
+ **그러므로 값을 바꿔도 DB에 UPDATE가 일어나지 않습니다.**

+ 여기에서는 `itemService.saveItem(book)`에서 수정을 시도하는 `Book` 객체를 의미합니다.
+ **`Book`객체는 이미 DB에 한번 저장되어서 `식별자가 존재합니다.` **
+ 이렇게 임의로 만들어낸 엔티티도 기존 식별자를 가지고 있으면 `준영속 엔티티`로 볼 수 있습니다.
  + 새로운 Book이 아니고, 아이디가 이미 셋팅이 되어 있습니다. 즉, 뭔가 JPA에 한 번 들어갔다 나온 애.
  + 이런 객체를 (식별자가 정확하게 데이터베이스에 있으면) 준영속 상태의 객체라고 합니다.

```java
@GetMapping("items/{itemId}/edit")
public String updateItemForm(@PathVariable("itemId") Long itemId, Model model) {

    Book item = (Book) itemService.findOne(itemId);

    //update를 하는데 BookForm을 보낼 것임.
    BookForm form = new BookForm();
    form.setId(item.getId());
    form.setName(item.getName());
    form.setPrice(item.getPrice());
    form.setStockQuantity(item.getStockQuantity());
    form.setAuthor(item.getAuthor());
    form.setIsbn(item.getIsbn());

    model.addAttribute("form", form);
    return "items/updateItemForm";
}
```

---

## 1-1. 영속 상태의 엔티티는

+ 엔티티가 영속상태로 관리되면 `값만 바꾸면` JPA가 트랜잭션 커밋시점에 `변경된 내용을 알아서 DB에 반영`해줍니다.
+ **JPA가 관리하는 영속 상태 엔티티는 변경감지(`Dirty Checking`)가 일어납니다.**
  + 값을 가져와서 값을 바꿔놓으면 JPA가 이거 바뀌었네하고 트랜잭션 커밋시점에 DB update문을 날리고 변경사항을 반영시켜줍니다.
  + 즉, flush할 때 dirty checking이 일어납니다.


---


## 2. 준영속 엔티티를 수정하는 2가지 방법

1. **변경 감지 기능 사용(`Dirty Checking`)**
2. 병합(merge) 사용

---

## 2-1. 변경 감지 기능

+ **`영속성 컨텍스트에서` 엔티티를 다시 조회한 후에 데이터를 수정하는 방법**
+ **동작 원리**
  1. 트랜잭션 안에서 엔티티를 다시 조회, 변경할 값 선택
  2. 트랜잭션 커밋 시점에 변경 감지(`Dirty Checking`)가 동작해서 데이터베이스에 UPDATE SQL 실행

```java
//Dirty Checking Sample1
@Transactional
void update(Item itemParam) { //itemParam: 파리미터로 넘어온 준영속 상태의 엔티티
    Item findItem = em.find(Item.class, itemParam.getId()); //같은 엔티티를 조회한 다.
    findItem.setPrice(itemParam.getPrice()); //데이터를 수정한다. 
}
```

---

```java
//Dirty Checking Sample2
//ItemService
@Transactional
public void updateItem(Long itemId, Book param) {
    Item findItem = itemRepository.findOne(itemId);

    findItem.setPrice(param.getPrice());
    findItem.setName(param.getName());
    findItem.setStockQuantity(param.getStockQuantity());
}
```

---

## 2-2. 병합 사용

+ 병합은 준영속 상태의 엔티티를 영속 상태로 변경할 때 사용하는 기능입니다.

![Desktop View](/assets/img/post/spring-boot/2021-01-12-springboot-jpa-merge.png)

+ **병합 동작 방식**
  1. `merge()`를 실행합니다.
  2. 파라미터로 넘어온 준영속 엔티티의 식별자 값으로 1차 캐시에서 엔티티를 조회합니다.
    + 만약 1차 캐시에 엔티티가 없으면 데이터베이스에서 엔티티를 조회하고, 1차 캐시에 저장합니다.
  3. 조회한 영속 엔티티(`mergeMember`)에 `member` 엔티티의 값을 채워넣습니다.
    + member 엔티티의 `모든 값`을 mergeMember에 밀어넣습니다.
    + 이때 mergeMember의 "회원1"이라는 이름이 "회원명변경"으로 바뀝니다.
  4. `영속 상태`인 mergeMember를 반환합니다.

+ **병합 동작 방식을 간단히 정리**
  1. 준영속 엔티티의 식별자 값으로 영속 엔티티를 조회합니다.
  2. 영속 엔티티의 값을 준영속 엔티티의 값으로 모두 교체합니다.(merge)
  3. 트랜잭션 커밋 시점에 변경 감지 기능이 동작해서 데이터베이스에 UPDATE SQL이 실행됩니다.


```java
@Transactional
void update(Item itemParam) { //itemParam: 파리미터로 넘어온 준영속 상태의 엔티티 
    Item mergeItem = em.merge(item);
}
```

+ **주의사항**
  1. 변경 감지 기능을 사용하면 원하는 속성만 선택해서 변경할 수 있습니다.
  2. **그러나 병합을 사용하면 모든 속성이 변경됩니다.**
    + 병합시 값이 없으면 `null`로 업데이트 할 윙험도 있습니다. (병합은 모든 필드를 교체)

---

## 3. 상품 리포지토리의 저장 메서드 분석

+ `ItemRepository`
  + `save()` 메서드는 식별자 값이 없으면(`null`) 새로운 엔티티로 판단해서 영속화(`persist`)하고 식별자가 있으면 병합(merge)합니다.
  + 지금처럼 준영속 상태인 상품 엔티티를 수정할 때는 `id`값이 있으므로 병합을 수행합니다.

+ 상품 리포지토리에선 `save()` 메서드 하나로 저장과 수정(병합)을 한 번에 처리합니다. 
+ 이렇게 함으로써 이 메서드를 사용하는 클라이언트는 저장과 수정을 구분하지 않아도 되므로 클라이언트의 로직이 단순해집니다.

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
}
```

---

+ **주의 사항**
  + `save()` 메서드는 식별자를 자동 생성해야 정상 동작합니다. 
  + 여기서 사용한 `Item` 엔티티의 식별자는 자동으로 생성되도록 `@GeneratedValue`를 선언했습니다.
    ```java
    public abstract class Item {

      @Id
      @GeneratedValue
      @Column(name = "item_id")
      private Long id;
    //...
    }
    ```
  + 따라서 식별자 없이 `save()` 메서드를 호출하면 `persist()`가 호출되면서 식별자가 값이 자동으로 할당됩니다.
  + 반면에 식별자를 직접 할당하도록 `@Id`만 선언했다면, 이 경우 식별자를 직접 할당하지 않고, `save()` 메서드를 호출하면 식별자가 없는 상태로 `persist()`를 호출합니다. 그러면 식별자가 없다는 예외가 발생합니다.

---


## 4. 결론(바람직한 엔티티 수정 방법)

## 4-1. 변경 감지만 사용하기 

+ **엔티티를 변경할 때는 항상 `변경 감지`를 사용하세요**

---

## 4-2. 컨트롤러에서 엔티티 생성 X

+ **컨트롤러에서 어설프게 엔티티를 생성하지 마세요**
  + 아래와 같은 방식은 지양해야 합니다.  
    ```java
    @PostMapping("items/{itemId}/edit")
    public String updateItem(@ModelAttribute("form") BookForm form) {

        Book book = new Book();

        book.setId(form.getId());
        book.setName(form.getName());
        book.setPrice(form.getPrice());
        book.setStockQuantity(form.getStockQuantity());
        book.setAuthor(form.getAuthor());
        book.setIsbn(form.getIsbn());

        itemService.saveItem(book);

        return "redirect:/items";
    }
    ```
  + **권장하는 방식**
    ```java
    @Controller
    @RequiredArgsConstructor
    public class ItemController {
        
        private final ItemService itemService;
        /**
        *상품 수정,권장 코드
        */
        @PostMapping(value = "/items/{itemId}/edit")
        public String updateItem(@ModelAttribute("form") BookForm form) {
            itemService.updateItem(form.getId(), form.getName(), form.getPrice());
            return "redirect:/items";
        }
    }
    ```

---

## 4-3. 서비스 계층에 식별자(id)와 데이터를 명확히 전달하기

+ **트랜잭션이 있는 서비스 계층에 식별자(`id`)와 변경할 데이터를 명확하게 전달하세요.(파라미터 or DTO)**

  ```java
  @Controller
  @RequiredArgsConstructor
  public class ItemController {
      private final ItemService itemService;
      /**
      *상품 수정,권장 코드
      */
      @PostMapping(value = "/items/{itemId}/edit")
      public String updateItem(@ModelAttribute("form") BookForm form) {
          itemService.updateItem(form.getId(), form.getName(), form.getPrice());
          return "redirect:/items";
      }
  }
  ```

---

## 4-4. 트랜잭션이 있는 서비스 계층에서 직접 조회, 변경하기 

+ **트랜잭션이 있는 서비스 계층에서 영속 상태의 엔티티를 조회하고, 엔티티의 데이터를 직접 변경하세요.**
  + **`트랜잭션 안에서` `엔티티를 조회해야` 영속 상태로 조회가 됩니다.**
  + 그리고 거기서 값을 변경해야 Dirty Checking(변경 감지)가 일어납니다.

  ```java
  public class ItemService {
      
      private final ItemRepository itemRepository;
      
      /**
      * 영속성 컨텍스트가 자동 변경
      */
      @Transactional
      public void updateItem(Long id, String name, int price) {
          Item item = itemRepository.findOne(id); 
          item.setName(name); 
          item.setPrice(price);
      }     
  }
  ```

+ **트랜잭션 커밋 시점에 변경 감지가 실행됩니다.**


---

## 4-5. setter는 닫고 엔티티에 생성메서드 따로 만들기

+ **`setter없이` 엔티티에서 바로 추적할 수 있는 메소드를 만드세요.**
  + 지금은 예시를 위해 필드값을 setter로 셋팅했지만, 실제 개발을 할 때는 **setter는 닫아놓고 별도로 엔티티에 생성 메서드를 만들어서** 비즈니스 로직을 추적하기 쉽게해야합니다.

---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)

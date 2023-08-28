---
layout: single
title:  "프로젝트 환경설정"
categories: miniProject1
tag: [spring boot, jpa, java, H2 db]
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 프로젝트 생성

## start.spring.io

https://start.spring.io 를 가서 아래 사진과 같이 세팅을 완료하고 &#39;GENERATE&#39; 버튼을 누른다.   
(저는 다운된 build.gradle 파일을 Intellij에서 열었습니다.)   
![스크린샷 2023-08-19 235131](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/3a3728b6-7ddc-45d1-a8e9-d28ca04eecc4)   

## lombok
귀여운 코끼리 모양의 build.gradle 파일에 들어가보면 lombok이 들어와있는걸 확인할 수 있는데 lombok 덕분에 Hello의 Getter Setter를 쉽게 만들 수 있다.   
![스크린샷 2023-08-20 hello](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/1dd1e876-0b81-469a-94eb-def8ea677f09)

```java
package jpabook.jpashop;

import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class Hello {
    private String data;
}
```

## 실행

JpashopApplication에 들어가 다음 코드로 실행하면 실행화면의 커다란 spring 글씨 위에 data = hello 라는 문구가 잘 뜸을 볼 수 있다. 
(Getter, Setter가 잘 작동함을 확인)   
![스크린샷 2023-08-20 jpashopApplicationRUN](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/f796556b-611d-4770-a09b-f8776af52931)   

# View 환경 설정

## hello.html

resources에 templates에 hello.html 파일을 만듦.   
![스크린샷 2023-08-20 hello html](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/236d8c73-c80f-4299-af5d-972e3d78eeec)   

```java
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p> //build.gradle에 devtools 넣음으로써 빌드에 recompile만 누르면 화면창 바뀜.
</body>
</html>
```

## HelloController

HelloController를 하나 만든다.   
![스크린샷 2023-08-20 helloController](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/f37334ef-f7aa-42fd-8bdb-08fcb651fd23)

```java
package jpabook.jpashop;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {
    @GetMapping("hello")//hello라는 url로 오면 이 컨트롤러가 호출되겠다는 의미
    public String hello(Model model) { //Model은 스프링이 지원하는 기능으로써, key와 value로 이루어져있는 HashMap이다.
        // Controller가 model에 데이터를 실어 veiw에 넘길 수 있음.
        model.addAttribute("data","hello!!!");
        return "hello.html"; //return은 화면 이름이다.
    }
}
```

## devtools

귀여운 코끼리 모양의 build.gradle 파일로 들어가 아래 사진과 같이   
implementation &#39;org.springframework.boot:spring-boot-devtools&#39; 를 넣어준다.   
![스크린샷 2023-08-20 170801](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/d784ada3-4d5c-4f5c-81f8-451cc565e7b7)   
   
devtools가 reloading을 만들어줌.   
   
빌드에 다시 컴파일을 누르고 http://localhost:8080/hello 를 들어가면 잘 컴파일 된다.   
![스크린샷 2023-08-20 compileEZ](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/a138a7d7-d608-4a80-9dcf-dbf3be503b89)   

# H2 데이터베이스 설치

## 생성
https://h2database.com 에 들어가 나의 spring과 맞는 버전을 다운받는다. 다운받은 후 아래 사진의 오른쪽하단 위치에서 노란색 사파이어를 클릭해 실행한다.   
![스크린샷 2023-08-23 here](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/3aa78ec6-13da-44ef-9f1a-3ced160c58ac)   

실행하면 이렇게 뜨는데   
![스크린샷 2023-08-23 h2before](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/54ab84f2-8355-4d67-ad18-e6221d306ce1)   

여기서 key 값을 유지시키고 앞에 localhost:8082/?key= 를 붙여준다.   
![스크린샷 2023-08-23 h2after](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/3685df64-d46e-4f9d-a915-a8f4480b86b7)   

그리고 실행하면 아래 사진과 같이 jpashop.mv.db가 잘 생성됨을 확인할 수 있다.   
![스크린샷 2023-08-23 jpashop mv db](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/4c04668e-6e21-46bc-9a62-539f046a40ab)   

## 접근
생성 이후론 tcpmode로 접근하면 된다.   
![스크린샷 2023-08-23 tcpmode](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/319757f9-6458-4bda-85c5-a5f3786e016b)

즉, 처음에 db파일 생성할 땐 파일오더로 접근하고 이후로는 tcp 네트워크모드로 접근하면 된다.   

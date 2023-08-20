---
layout: single
title:  "프로젝트 환경설정"
categories: pracProject1
tag: [spring boot, jpa, java, H2 db]
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 프로젝트 생성

## start.spring.io

https://start.spring.io/ 를 가서 아래 사진과 같이 세팅을 완료하고 &#39;GENERATE&#39; 버튼을 누른다.   
(저는 IntelliJ에서 다운된 build.gradle 파일을 열었습니다.)   
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


## devtools

귀여운 코끼리 모양의 build.gradle 파일로 들어가 아래 사진과 같이   
implementation &#39;org.springframework.boot:spring-boot-devtools&#39; 를 넣어준다.   
![스크린샷 2023-08-20 170801](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/d784ada3-4d5c-4f5c-81f8-451cc565e7b7)

# View 환경 설정




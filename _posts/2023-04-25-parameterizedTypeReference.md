---
title: "[Spring] ParameterizedTypeReference-작성중"
excerpt: "ParameterizedTypeReference를 알아보자"

categories:
  - Blogs
tags:
  - [Spring]

permalink: /blogs/parameterizedTypeReference

toc: true
toc_sticky: true

date: 2023-04-25
---

#### 필요했던 케이스
- '티켓'이라는 별도의 서비스와 통신을 해서 값을 받아와야 하는 상황
- return 타입은 Generic 으로 지정해주는 게 컨벤션
- 여기서는 RestTemplate 통신 중
#### 그럼 ParameterizedTypeReference란?
- 공식 문서 : [ParameterizedTypeReference에 대한 spring doc](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/ParameterizedTypeReference.html)
- 즉, 런타임 시 사라지는 타입 정보를 잡아서(capture) 런타임에도 유지(retain)하기 위한 추상 클래스. 
##### +) 새롭게 알게 된 점
- Generics Type Erasure : 제네릭 타입은 컴파일 타임에만 검사하고, 런타임에는 타입 정보를 소거한다.
<br>

#### 최종 구현
```
Type type = new ParameterizedTypeReference<TicketResponse<List<TicketsResponse>>>() {  
}.getType();  

TicketResponse<List<TicketsResponse>> ticketResponses = send(  
uri, type, HttpMethod.GET, null);
```
- '티켓' 서버에서 지정된 타입으로 get 해 오는 것을 확인했다. 



---
더 알아보고 추가해야 할 부분
- super type tokens : [Neal Gafter's blog](https://gafter.blogspot.com/2006/12/super-type-tokens.html)
- 상속을 통해 제네릭 정보를 넘기는 부분에 대한 추가 설명
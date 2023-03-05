---
title: Querydsl 에서 동적 sorting을 위해 OrderSpecifier 클래스 구현
---
createDate :  2023-03-02


### Querydsl 에서 동적 sorting을 위해 OrderSpecifier 클래스 구현

- #### 필요했던 케이스
	- paging 조회 시, 클라이언트가 설정한 정렬 조건 대로 내려줘야 했다.
	- 정렬 조건은 가입일 (id가 순서를 보장하지 않는 케이스), 이름 (국영숫), 나이 등의 오름 || 내림차순
	
- #### OrderSpecifier란?
	- 공식 문서 : http://querydsl.com/static/querydsl/4.0.7/apidocs/com/querydsl/core/types/OrderSpecifier.html
	- Querydsl 에서 동적 sorting을 위해 필요한 클래스

- #### 활용 예제
	``` java
	private OrderSpecifier getOrderCondition(AccountsSortType accountsSortType,  
      boolean ascending, QAccounts accounts) {  
	   switch (accountsSortType) {  
	      case JOIN_DATE:  
	         return ascending ? accounts.joinDate.asc(): accounts.joinDate.desc();  
	      case NAME:  
	         return ascending ? accounts.name.asc() : accounts.name.desc();  
	      case LAST_LOGIN:  
	         return ascending ? accounts.lastLoginDate.asc()  
	               : accounts.lastLoginDate.desc();  
	      default:  
	         return null;  
	   }  
}
```
	- 케이스별로 sorting을 진행할 수 있게 구현함


---
* 태그 : #JPA #Querydsl
* 관련 내용
	[[]]

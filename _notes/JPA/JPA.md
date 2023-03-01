---
title: JPA
---
### 2023.02

#### 1.
양방향 매핑이 돼 있는 WorkPlaces - WhiteIp (1 : 다)
연관관계 주인은 WhiteIp였고, WorkPlaces는 lazyLoading, cascade가 걸린 상황.

최초 개발했을 때
```java
private void modifyWhiteIps(boolean useWhiteIp, List<WhiteIpModifyRequest> modifyRequests,  
    WorkPlaces workPlace) {  
    Set<WhiteIp> origin = workPlace.getWhiteIps();  
    // 사용하던 유저가 사용 안 함으로 변경할 경우, 기존 데이터 삭제  
    if (useWhiteIp == false) {  
        whiteIpRepository.deleteAllInBatch(origin);  // whiteIp에서 삭제해주기만 해도 삭제 됐었음
     
    } else {  
        for (WhiteIpModifyRequest request : modifyRequests) {  
            origin.forEach(ip -> {  
                if (ip.getIpAddress().equals(request.getIpAddress())) {  
					ip.modifyWhiteIp(request.getIpAddress(), request.getContents());  
                }  
            });  
        }  
    }  
}
```

하지만 이유를 모르겠지만.. WorkPlaces쪽에서도 삭제해주지 않으면 return 시 WP 데이터에서는 WhiteIp값이 남아있었다.

연관관계 주인은 WhiteIp였고 WorkPlaces는 주인이 아닌데 왜 조회할 때 최신화된 값이 안나오는지 모르겠다.

#### 2. 이미 fetchJoin 해서 가져온 엔티티는, 다른 트랜젝션에서 변경사항이 있을 때 업데이트 된 값을 보여주는가?

예를 들어, Company 와 Member의 관계가 1 대 다, 양방향 관계라고 하자.
Company를 쿼리해오면서 Member를 fetchJoin 해온 뒤, 내가 다른 로직들을 수행하던 중 다른 트랜젝션에서 누군가가 member A의 name을 update 헸다고 가정하자.

```java
Company company = companyRepository.findByCompanyIdWithMember(companyId);

// 다른 로직들 시행하던 중 다른 트랜젝션에서 update 됨

Set<Member> members = company.getMembers();

```
이때, members 중에서 member A를 꺼내 name을 확인하면 변경된 Name이 나올까 아니면 company를 쿼리해올 때의 Name, 즉 변경 전 Name이 나올까?
정답은 "변경 전 name이 나온다" 였다.

내가 확신을 못했던 이유는 결국 영속성에 대한 이해가 부족했던 것 같다.
(추가 정리를 하겠다.)

여기서 덧붙여서 궁금한 점!
다른 트랜젝션에서 발생한 일도 현재의 영속성에 영향을 줄 수 있는 방법도 있을 것 같은데... 없으려나?
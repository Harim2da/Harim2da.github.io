
2023.02
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
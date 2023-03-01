---
title: JAVA
---

### 객체의 실제 내용을 비교할 때
객체끼리 비교하고자 할 때, A.equals(A)를 진행했는데도 false를 return 받을 수 있다.
객체 내부의 값은 동일하지만, 실제 객체는 다른 곳에 위치하므로 그러하다.
이럴 때는 equals() 메서드를 @Override 해서 사용해야 한다.
```java

public class AResponse {
	private String name;
	A(String name) {
		this.name = name;
	}

	@Override
	public boolean equals(Object obj) {
		if(obj instanceOf AResponse) {
			if(this.name == ((A)obj).name) return true;			
		}
		return false;
	}

	@Override
	public int hashCode() {
		return name.hashCode();
	}
}


```
hashCode를 오버라이딩한 곳은, 'name'의 문자열이 동일하면 동일한 hashCode가 return 된다.
Hash형태의 자료 구조에서는 동등 비교를 위해서 hashCode() 결과값으로 비교하므로, equals와 함께 overriding 해야한다.


### ConcurrentmodificationException
* 보통 Map 등의 값을 루프 돌면서 삭제할 때 발생함
* 내가 발생한 이슈는 삭제가 아니라 add를 하게되면서 발생했음
일대다 양방향 연관 관계일 때, add하면서 일인 곳에 add를 해주다보니 엔티티 하나를 add 하니 총 개수가 안 맞아서 발생
https://hbase.tistory.com/322


---
![[Pasted image 20230214143851.png]]

메시지 로그 조회해오는 데에만 95초 (약 6만5천건)
엑셀 파싱하는데에 소요 시간 : [2023-02-14 15:04:13.137] [http-nio-8080-exec-9] INFO i.c.g.a.a.m.MassMessageFacade@findMessageHistoryDetailsExcel(135) - 엑셀 파싱 작업 소요시간 : 154.885초

----
1차 시도 - 병렬 스트림에서 no 넣는 부분 제거
메세지 로그 조회 소요시간 : 100.337초

2차 시도 - 엑셀 만들 때 stream이 아닌 for문으로 add
메세지 로그 조회 소요시간 : 90.893초
엑셀 만드는 시간 : 엑셀 파싱 작업 소요시간 : 152.127초

3차 시도 - 엑셀 만드는 방식을 String으로 map에 넣어서 유틸 사용 -> 하나하나 데이터 찍기
![[Pasted image 20230214161537.png]]


[2023-02-14 16:51:22.951] [http-nio-8080-exec-1] INFO i.c.g.a.a.m.MassMessageFacade@findMessageHistoryDetailsExcel(130) - 메세지 로그 조회 소요시간 : 102.542초
[2023-02-14 16:51:36.764] [http-nio-8080-exec-1] INFO i.c.g.a.a.m.MassMessageFacade@findMessageHistoryDetailsExcel(136) - 엑셀 파싱 작업 소요시간 : 8.181초

--------
[2023-02-14 17:17:12.666] [http-nio-28180-exec-1] INFO i.c.g.m.d.m.MessageHistoryService@findMessageHistoryLogsAll(203) - 쿼리 소요 시간 : 1.327초
응답 파싱 소요 시간 : 97.54초

---------------
4차 시도 - 메시지 로그 응답 파싱 마다 쿼리문 치던 걸 -> List로 가져와서 stream으로 동일한 것 찾도록 수정
[2023-02-14 17:46:46.431] [http-nio-8080-exec-1] INFO i.c.g.a.a.m.MassMessageFacade@findMessageHistoryDetailsExcel(130) - 메세지 로그 조회 소요시간 : 14.637초
[2023-02-14 17:46:54.259] [http-nio-8080-exec-1] INFO i.c.g.a.a.m.MassMessageFacade@findMessageHistoryDetailsExcel(136) - 엑셀 파싱 작업 소요시간 : 7.824초

![[Pasted image 20230214175303.png]]
![[Pasted image 20230214175328.png]]

메세지 로그 조회 소요시간 : 2.366초
메세지 로그 조회 소요시간 : 3.374초
3.088초
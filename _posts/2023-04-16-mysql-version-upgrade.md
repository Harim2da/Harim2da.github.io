---
title: "[프로젝트] Mysql 5.7 -> 8.0 Version Upgrade"
excerpt: "AWS RDS Mysql 8.0 버전 업그레이드"

categories:
  - Projects
tags:
  - [AWS, MySQL]

permalink: /projects/mysql-version-upgrade

toc: true
toc_sticky: true

date: 2023-04-16
---

## 작업 배경

- AWS RDS 엔진버전이 2.10.3 지원 중단
- 2.11 버전으로 자동 업그레이드 가능하나, 운영 서버의 다운타임 우려
- 다운타임은 불가피 하므로, 이 기회에 <b>MySQL 8.x 버전으로 업그레이드 진행 결정</b>


## 작업대상

: 클라우드게이트 RDS, 셀러게이트 RDS 전체

* <img src="../assets/posts_img/region-cluster.png" alt="rds test env">

-> 지원중단 관련 [공고](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraMySQLReleaseNotes/AuroraMySQL.Updates.2103.html)

  

## 사전 준비

### 1) 업그레이드 테스트
- develop DB에서 선 테스트 진행 -> 실 운영 DB의 스냅샷으로 테스트 진행
1. 기존 DB 스냅샷을 뜬다
2. 그 스냅샷으로 test용 DB를 띄운다
3. test용 DB를 버전 업그레이드 해본다
4. 이슈가 있다면 모두 수정 및 이슈 해결 한다
5. 모든 이슈가 해결되면 본 DB를 업그레이드 진행한다
<br>
  
#### 실서버 DB 스냅샷 테스트
* 작업 방식은 크게 두 가지
* 블루/그린 배포 방식 : down time을 최소화 하면서 RDS를 교체하는 방식
* 인플레이스 방식 : 기존 RDS를 바로 버전 업 하는 방식. 작업하는 동안 서비스 운영 불가
- AWS 에서는 블루그린 배포 방식을 권장하기에 두 방식 모두 테스트 해보기로 함

<br>
##### 실서버 DB 테스트 작업 계획
- 운영 DB 스냅샷을 두 개 복원한 뒤, 하나는 블루그린 테스트용 다른 하나는 인플레이스용으로 세팅
<img src="assets/posts_img/testrds.png" alt="rds test env">

- 각 버전의 공통 체크 리스트
	1) 파라미터 그룹 값들 확인하기
	2) api 연결 체크
	3) 버전 업 총 소요 시간 체크

- 블루/그린 버전일 때 추가 체크
	- 블루 -> 그린으로 전환될 때 엔드포인트도 변경되는지 확인

  

##### 테스트 결과

- 블루그린 배포 방식
	- 총 6시간 소요
	- 기존 SQL mode 설정값 때문에 버전업이 원활하지 않은 이슈가 있었으나 해결
	- 엔드포인트 변함 없음
- 인플레이스 배포 방식
	- 파라미터 그룹 값 : 기존과 동일하게 세팅 및 확인 완료
	- api 연결 체크 : 로컬에서 직접 접근으로 확인 완료
	- 버전 업 총 소요 시간 : 21분 소요 (16:57 ~ 17:18)

  
- 테스트 중 만난 이슈
	- 파라미터 그룹에서 charset 설정을 안 해준 때에 데이터가 ?로 저장
	- 파라미터그룹이 default 일 때 저장된 값

- ![[Pasted image 20230317084926.png]]
	-> 파라미터 그룹 내, charset 설정을 모두 진행 후 테스트했더니 정상 노출 돼었다.

  
  
  

## 업그레이드 진행 (3/26(일) 오전 1:00)

  

순서

1) 기존 서버들 binlog_format mixed로 변경, 재부팅
2) 블루그린 생성
3) 그린(버전업)을 prod로 스위칭
4) 기존 버전 삭제



##### 관련 내용
- [MySQL 5.7 -> 8.0의 차이점은?](../blogs/diff-mysql5-ver-8)
- 다중 AZ 구성 : [다중AZ란?](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
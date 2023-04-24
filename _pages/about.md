---
title: "커뮤니케이션이 뛰어난 개발자 이하림입니다."
permalink: /about/
layout: single
comments: false
---

## Introduce

함께 일하면 편하다, 커뮤니케이션이 잘 된다는 평을 들으며 2년 째 백엔드 개발팀에서 일하고 있습니다.
스타트업에서 웹 서비스를 개발, 배포, 운영하고 AWS 관리도 함께 하는 중입니다.
개발을 공부하고 업무를 해 볼수록, 상황에 맞게 필요한 기술을 적용하고 개선해 나가는 점이 즐거워 재밌게 일하고 있습니다.
<br>

## Skills
* Back-End 
	* Java, Spring Boot, Spring Batch, Spring JPA, Gradle
* DataBase 
	* MySQL, Redis, QueryDSL
* DevOps 
	* <b>AWS</b> : CodeDeploy, CodePipeline, EC2, S3, CloudWatch, SystemManager, RDS-Aurora, IAM, VPC
	* Jenkins
* ETC
	* Github, JIRA, Notion, Postman, IntelliJ
<br>

## Contact
<!-- - blog (study log) : https://velog.io/@dev-hr2 -->
- email : helloharim09@gmail.com
<br>

## Work
2021.12.27 ~ 현재<br>
더화이트커뮤니케이션
- 챗봇, 웹챗, 콜, 메일 상담 솔루션 SaaS 서비스
<br>

### Project

**멀티브랜드 프로젝트**

* 기간 : 2022.08 ~ 현재
* 그룹사이거나 다중 브랜드를 소유한 고객사의 업무 편리성을 위한 솔루션 전체 구조 개선 프로젝트
* 주요 성과
	* 어드민 페이지 중 정산 페이지 전체 개선 
	* 로그인 로직 수정 및 개선
		* 로그인 로직을 역할과 책임에 맞게 개선
	* event 처리로 service와 service의 호출 제거
	* 신규 계정 초대 메일 발송부터 초대 대상의 가입까지 관련된 전체 API 개발
	* 메시지 발송 내역 조회 및 엑셀 다운로드 성능 개선 (메시지 발송 건 6만 5천여 건 기준)
		* DML이 거의 없는 데이터를 건마다 쿼리하던 방식을 `findAll()` 로 가져와 stream 돌리는 것으로 개선
		* 그 결과 발송 내역 조회 `4900% 개선 (100초 -> 2초)`
		* 해당 건 엑셀 다운로드 `2100% 개선 (155초 -> 7초)`
		* 개선에 대한 [세부 설명](https://harim2da.github.io/projects/message)
	* 기존 API 의 파라미터 및 매핑 등 구조 개선 진행

<br> 

**커스텀 프로젝트**
* 기간 : 2022.06 ~ 2022.07
* 주요 성과 : N억 매출 달성에 기여
* 작업 내용 : 특정 클라이언트용 챗봇 로직 개발 및 수정
	* 엑셀 파일을 읽어 자주 묻는 질문, 검색 및 응답 데이터 적재 로직 작성
	* 적재 데이터 DB 스키마 설계
	* 검색 로직 개발

<br>

**AWS 관리**
* 기간 : 2022.02 ~ 현재
* AWS 관리 전반 담당
* 주요 성과
	* AWS RDS Mysql 5.7 -> 8.0 버전 업 작업 단독 진행 [자세히 보기](https://harim2da.github.io/projects/mysql-version-upgrade)
	* 대형 클라이언트의 요구사항 충족을 위한 CloudWatch 모니터링 구조 변경, 경보 알람 트리거를 통해 자동 scale in, out 연결
	* 배포 실패에 관한 트러블 슈팅 진행, 해결 방법 문서화
	* 외주사의 작업을 위해 특정 S3 폴더만 접근 가능하도록 IAM 권한 설정 변경
	* S3 폴더 관리
	* Code Deploy 관리
	* 신규 입사자 및 타 부서 권한 요청 시 IAM 권한 부여

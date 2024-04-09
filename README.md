# BookTalkBang
[서비스 링크](https://woogin.shop/booktalk)

## 프로젝트 개요
### 프로젝트 소개
> 책과 관련해 중고거래가 메인인 사이트로 서로 책에 대해 추천하거나 이야기를 나눌 수 있는 서비스(팀 프로젝트)

### 주요 기능
* 중고 거래 상품 등록
* 외부 상품과의 가격 비교 기능
* 거래를 위한 채팅기능
* 거래 후 판매자 평점 & 상품 리뷰기능
* 채팅과 리뷰를 위한 알림기능
* 회원 신고기능
* 백오피스 관리자 기능

### 개인 역할

* 부팀장
  * 스프링 시큐리티를 사용한 회원가입 로그인 구현
  * Oauth 2.0을 통해 카카오 소셜 로그인 구현
  * JPA ORM을 통해 유저 프로필 관리
  * 회원가입 로그인 프로필 CRUD 테스트코드 작성
  * JWT를 통한 인증방식
  * Redis로 RefreshToken을 관리하여 JWT보완
  * 게시판 및 댓글 도메인 프론트엔드

### 팀 프로젝트 기간 및 소속

내일배움캠프 부트캠프 8조 소속 (4인)
<br>
2024.01~2024.02 (5주)

### 팀 프로젝트 Repo/Notion URL
[팀 프로젝트 Repo](https://github.com/junyeong237/booktalk)
<br>
[팀 프로젝트 Notion](https://www.notion.so/teamsparta/Book-3e585aeb94e0478cbf8285718775849b)

## 사용 기술 스택(개인)
<details>
  <summary>BackEnd</summary>
 
* Java 17
* Spring boot 3.2.1
* Spring security 6.2.1
* JWT 0.11.5
* gradle 8.5
* spring data jpa 3.2.1
* spring data redis 3.2.1
* Oauth 2.0
* junit5 5.10.1
  
</details>

<details>
  <summary>FrontEnd</summary>
 
* HTML
* CSS
* JavaScript(jQuery)
* AJAX
* bootstrap 5.3.0
* thymeleaf 3.1.2
  
</details>

<details>
  <summary>DB</summary>
 
* MySQL 8.2.0
* Redis
  
</details>

## 프로젝트 진행 단계

* 1주차 : 백엔드 구현
* 2주차 : 프론트엔드 구현
* 3주차 : 오류 수정
* 4주차 : 유저 배포 및 피드백 반영
* 5주차 : 피드백 반영

## 프로젝트 세부과정

### 서비스 아키텍쳐
![Untitled](https://github.com/junyeong237/booktalk/assets/145661542/6e0342c6-add0-4271-b553-eb9d82604c73)

### ERD
![B08](https://github.com/junyeong237/booktalk/assets/145661542/53997058-e978-4099-acf4-def3a1ca24b4)

### 기술적 의사결정

<details>
  <summary>프런트 배포 방식 - 타임리프 & Ajaxs</summary>
 
* 타임리프를 사용하여 서버에서 동적으로 HTML을 생성하고 비동기적으로 데이터를 처리할 수 있는 ajax를 사용
* 따라서 모노리틱 아키텍쳐기반으로 프런트와 백엔드를 같이배포하는 방식선택
  
</details>

<details>
  <summary>유저 인증방식</summary>
 
* jwt
  * 사용자의 로그인 상태정보를 클라이언트에 저장하기 때문에 서버의 부담감소
  * 서버 간 토큰을 공유하거나 검증할 필요가 없기 때문에 확장성 보장
* Refresh Token
  * AccessToken의 사용주기를 짧게 하여 보안을 강화함과 동시에 사용자에게 잦은 로그인 경험을 주지 않기 위해 사용
  * 인메모리DB인 레디스를 활용해 적은 메모리사용과 빠른 작성으로 Refresh Token을 저장
  * 레디스의 TTL을 통해 RefreshToken 사용주기 관리가 용이
  * key value 형태로 RefreshToken과 user_id를 저장
* authentification filter vs userService에 직접 구현
  * 실제적으로 다른 api에서 요구되는 인가 상태는 authoriaztion filter를 통해 모듈화된 상태
  * 소셜로그인, 차단유저와 탈퇴유저의 로그인 제한 등 인증매커니즘의 유연성 필요로 직접 구현 결정
    
</details>

## 프로젝트 회고
* 잘한 점 : 새로운 기능 구현, 팀원과 원할한 의사소통
* 한계점 : 서버 종료 시 인메모리DB인 Redis 데이터 초기화 발생 -> 기존 유저가 지닌 토큰과 redis의 초기화된 토큰 정보가 일치하지 않아 접근이 제한되는 상황에 대처를 못함
* 개선사항 : Redis의 RDB/AOF를 통해 영속성을 설정하여 데이터 백업으로 해결 예상됨



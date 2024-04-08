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

내일배움캠프 부트캠프 8조 소속
<br>
2024.01~2024.02 (5주)

### 팀 프로젝트 Repo/Notion URL
[팀 프로젝트 Repo](https://github.com/junyeong237/booktalk)
<br>
[팀 프로젝트 Notion](https://www.notion.so/teamsparta/Book-3e585aeb94e0478cbf8285718775849b)

## 사용 기술 스택
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
1주차 : 백엔드 구현
2주차 : 프론트엔드 구현
3주차 : 오류 수정
4주차 : 유저 배포 및 피드백 반영
5주차 : 피드백 반영

## 프로젝트 세부 과정

## 프로젝트 회고
잘한 점 : 
<br>
한계점 :
<br>
개선사항 :
<br>

## 기술적 의사결정

<details>
  <summary>Repsitory의존 + default Method 사용</summary>
 
* 다른 서비스 도메인에서 다른 서비스를 호출하는 방식보단 레포지토리에 의존하는 방식 선택
* 이 경우 중복 메서드를 매번 작성해줘야하지만 이를 방지하기 위해 레포지토리단에서 디폴트 메서드를 사용해 중복성 제거
  
</details>

<details>
  <summary>WebSocket&Stomp 사용</summary>
 
* 채팅시스템을 위해선 클라이언트와 서버간의 양방향 통신을 제공받아야함
* 따라서 Http 통신과 다르게 연결을 맺고 바로 끊어버리는게 아닌 계속 유지할 수 있는WebSocket선택
* STOMP는 웹소켓과 함께 사용되는 메시징 프로토콜로 WebSocket만 사용했을때 일일히 handler를 수동작성해줘야했던것과 달리 STOMP를 웹소켓과 같이 사용하여 비교적 쉬운 초기설정과 관리가 가능한 pub/sub구조의 체계적인 응답형식을 가지고있다.
  
</details>

<details>
  <summary>Redis Cache 효율성 문제</summary>
 
* 같은 쿼리문이 쓸데없는 반복되는것을 피하기 위해 캐싱 사용
* 하지만 redis에 데이터가 쌓이는것 자체가 메모리 낭비로 이어질 수 있는 가능성이 있음
* 따라서 적절한 trade-off를 고려하여 레디스 캐싱의 시간을 줄이고 쿼리문 최적화가 필수적
  
</details>

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

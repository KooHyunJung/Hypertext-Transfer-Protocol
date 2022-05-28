## 05 HTTP method 활용 ##
- [01 클라이언트에서 서버로 데이터 전송](#1)
- [02 HTTP method 속성](#2)

---

<a name="1"></a>
## 01 클라이언트에서 서버로 데이터 전송 ##
- 클라이언트에서 서버로 어떻게 데이터를 전송할까?
- HTTP API 설계 예시

<details>
  <summary>
    <h3> 클라이언트에서 서버로 데이터 전송 </h3>
  </summary>

1) 쿼리 파라미터를 통한 데이터 전송
    - URI 끝에 쿼리 파라미터를 넣어서 전송하는 방법 
    - GET
    - 주로 정렬 필터(검색어)
2) HTTP [메시지 바디]를 통한 데이터 전송
    - POST, PUT, PATCH
    - 언제? -> 회원 가입, 상품 주문, 리소스 등록, 리소스 변경 등등..

```
<데이터 전송 4가지>
  1) 정적 데이터 조회
    -> 이미지, 정적 테스트 문서
  2) 동적 데이터 조회
    -> 주로 검색, 게시판 목록에서 정렬 필터(검색어)
  3) HTML Form을 통한 데이터 전송
    -> 회원 가입, 상품 주문, 데이터 변경
    -> form 태그에는 method [GET, POST]만 사용 가능.
  4) HTML API를 통한 데이터 전송
    -> 회원 가입, 상품 주문, 데이터 변경
    -> 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)
    -> 모든 method 사용 가능
```

</details>

<a name="2"></a>
## 02 HTTP method 속성 ##

<h3> HTML API 설계 예시 </h3>

```
- HTTP API [컬렉션]
  - POST 기반 등록
  - 예) 회원 관리 API 제공
- HTTP API [스토어]
  - PUT 기반 등록
  - 예) 정적 컨텐츠 관리, 원격 파일 관리
- HTML FORM 사용
  - 웹 페이지 회원 관리
  - GET, POST만 지원
```

<details>
  <summary>
    <h3> 1) HTTP API [컬렉션] POST 기반 </h3>
  </summary>

> 회원 관리 시스템 API를 만든다고 생각해보자.

- 회원 목록 /members      **GET**
- 회원 등록 /members      **POST**
- 회원 조회 /members/{id} **GET**
- 회원 수정 /members/{id} **PATCH, PUT, POST**
- 회원 삭제 /members/{id} **DELETE**

</details>

---
### 관련 내용 참조 ###
- https://www.inflearn.com/course/http-웹-네트워크/lecture/61369?volume=1.00&tab=note
- 
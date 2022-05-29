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
    - 메서드-> GET
    - 언제? -> 주로 정렬 필터(검색어)
2) HTTP [메시지 바디]를 통한 데이터 전송
    - 메서드-> POST, PUT, PATCH
    - 언제? -> 회원 가입, 상품 주문, 리소스 등록, 리소스 변경 등등..

```
<데이터 전송 4가지>
  1) 정적 데이터 조회
    -> [GET] 이미지, 정적 테스트 문서
    -> 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능
  2) 동적 데이터 조회
    -> 주로 검색, 게시판 목록에서 정렬 필터(검색어)
  3) HTML Form을 통한 데이터 전송
    -> 회원 가입, 상품 주문, 데이터 변경
    -> ‼️ form 태그에는 method [GET, POST]만 사용 가능.
  4) HTML API를 통한 데이터 전송
    -> HTML Form 외의 모든 상황
    -> 모든 method 사용 가능
    -> 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)
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

> 
POST 기반으로 API를 설계한다면 위와 같이 설계하면 된다. 
수정은 개념적으로 PATCH를 주로 쓴다. 종종 PUT을 쓰기도 하지만,
PATCH는 부분 수정이 가능하지만 PUT은 전체를 덮어 씌우기 때문에 이를 주의하기!!
PATCH, PUT 모두 사용 애매하다면 [POST]를 사용하면 된다.
>

- ```[POST] 리소스 등록```
  1) 클라이언트에서 [POST] 메소드를 사용해 서버에 정보 전달
  2) 서버에서는 데이터를 DB에 저장하고
  3) 서버가 [새로은 리소스 식별자 생성]하여 클라이언트에 다시 전달해 준다.(서버가 새로 등록된 리소스 URI 생성)
  - [응답데이터] Location에 정보를 넘겨준다 -> Location: /members/100
  - 컬렉션(Collection)
    - 서버가 관리하는 리소스 디렉토리
    - 서버가 리소스의 URI를 생성하고 관리
    - 여기서 컬렉션은 ```/members```

</details>

<details>
  <summary>
    <h3> 1) HTTP API [스토어] POST 기반 </h3>
  </summary>

> 이 때는 [컬렉션] 개념과 다르게 PUT 사용, ```클라이언트에서 URI 생성```한다

- 클라이언트가 리소스 URI를 알고 있어야 한다
  - 파일 등록 /files/{filename} -> PUT
  - PUT /files/star.jpg
- 클라이언트가 직접 리소스의 URI를 지정한다
- 스토어(Store)
  - 클라이언트가 관리하는 리소스 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - 여기서 스토어는 /files
</details>

<details>
  <summary>
    <h3> 3) HTTP FORM 사용 </h3>
  </summary>

- HTTP FORM은 GET, POST만 지원
  - GET, POST만 사용하므로 제약이 있음
  - 따라서 ```컨트롤 URI```를 사용한다
    - 이런 제약을 해결하기 위해 동사로 된 리소스 경로 사용
    - POST /new, /edit, /delete 컨트롤 URL
    - HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)
- AJAX 같은 기술을 사용해서 해결 가능 -> 회원 API 참고
- 여기서 순수 HTTP, HTTP FORM만을 이야기 하겠음.

</details>


<details>
  <summary>
    <h3> 정리 </h3>
  </summary>

> 아래 내용은 "정답"은 아니고 좋은 "참고" 개념이다.

<참고하면 좋은 URI 설계 개념>

- ```문서```
  - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
  - 예) /members/100, files/star.jpg
- ```컬렉션```
  - 대부분 컬렉션 스타일을 사용
  - 서버가 관리하는 리소스 디렉터리
  - 서버가 리소스의 URI를 생성하고 관리
  - 예) /member
- ```스토어```
  - 클라이언트가 관리하는 지원 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - 예) files
- ```컨트롤러, 컨트롤 URI```
  - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행할 때 사용
  - 동사를 집접 사용
  - 예) /member/100/delete

</details>

---
### 관련 내용 참조 ###
- https://www.inflearn.com/course/http-웹-네트워크/lecture/61369?volume=1.00&tab=note
- https://restfulapi.net/resource-naming

## 04 HTTP method ##
- [01 HTTP API를 만들어보자](#1)
- [02 HTTP method - GET, POST](#2)
- [03 HTTP method - PUT, PATCH, DELETE](#3)
- [04 HTTP method 속성](#4)

---

<a name="1"></a>
## 01 HTTP API를 만들어보자 ##
### HTTP 메시지 구조 ###
<img width="763" alt="스크린샷 2022-05-21 오후 6 57 23" src="https://user-images.githubusercontent.com/96563289/169646377-2ea2f850-a78b-470d-b12f-08c400eb07c5.png">

### HTTP API ###
- 회원 정보 PAI URI 설계
- URI(Uniform Resource Identifier)

```
    - 회원 목록 조회 /read-member-list
    - 회원 조회 /read-member-by-id
    - 회원 등록 /create-member
    - 회원 수정 /update-member
    - 회원 삭제 /delete-member
```

> 이것이 과연 좋은 URI 설계일까?

- 무엇을 기준으로 설계해야하나? [리소스의 의미]를 담아야 한다.

- [리소스(Resource)의 의미]란?
    - 회원을 [등록하고 수정하고 조회하는 하라!] 이게 리소스가 아니다.
    - [회원] 개념 자체가 리소스이다.

```
    - 회원 목록 조회 /members
    - 회원 조회 /members/{id}
    - 회원 등록 /members/{id}
    - 회원 수정 /members/{id}
    - 회원 삭제 /members/{id}
    [참고: 계층 구조상 상위를 컬렉션으로 보고 복수단어 사용 권장(member-> members)]
```

> 그럼 같은 URI를 가지고 있는 걸 어떻게 구분할 수 있을까?

> URI는 리소스만 식별한다. [리소스]와 [행위]를 분리해야 한다!

- 리소스 : 회원
- 행위 : 조희, 등록, 삭제, 변경
    - 그렇다면 [행위(method)]는 어떻게 구분할까?


<a name="2"></a>
## 02 HTTP method - GET, POST ##

    < HTTP 메서드 종류 >
    ✅ 주요 메서드
    - GET : 리소스 조회
    - POST: 요청 데이터 처리, 주로 등록에 사용
    - PUT : 리소스를 대체, 리소스가 없으면 생성
    - PATCH: 리소스 부분 변경, 특정 필드를 변경할 때 사용
    - DELETE: 리소스 삭제
    ✅ 기타 메서드
    - HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄고 헤더만 반환
    - OPTIONS: 대상 리소스에 대한 통신 가능 옵션(method)을 설명. 주로 COPS에서 사용
    - CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정, 거이 사용하지 않음.
    - TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트 수행. 거이 사용하지 않음.

- **GET**
    - 리소스 전달
    - 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
    - **메시지 바디를 사용해서 데이터를 전달할 수 있지만**, 지원하지 않는 서버가 많아서 권장하지 않음
    
    클라이언트에서 [GET요청] -> 서버에서 [json형태로 데이터 생성] -> 서버에서 [응답]

- **POST**
    - 요청 데이터 처리
    - **메시지 body를 통해 서버로 요청 데이터 전달**
    - 서버는 요청 데이터를 처리
        - -> 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
    - 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용
    
    클라이언트에서 [POST요청] body를 통해 데이터 전달 -> 전달된 URI에 약속된 행위를 수행한다 -> 신규등록(DB 저장), 신규 리소스 생성 -> 서버에서 [응답]

- POST는 요청 데이터를 어떻게 처리한다는 뜻일까? 예시
    - 스펙 : POST 메서드는 대상 리소스가 리소스 고유한 의미 체계에 따라 요청에 포함된 포함 된 표현을 처리하도록 요청합니다.

```
    - HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
    - 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시
    - 서버가 아직 식별하지 않은 새 리소스 생성
    - 기존 자원에 데이터 추가
```

- -> **이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 한다. -> 정해진 것이 없음**

- **POST 정리**
    1) **새 리소스 생성(등록)**
        - 서버가 아직 식별하지 않은 새 리소스 생성
    2) **요청 데이터 처리**
        - 단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리하는 경우
            - 예) [결제완료 > 배달시작 > 배달완료] 이처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우
            - POST 결과로 리소스가 생성되지 않을 수도 있음
                -   POST /orbers/{orberld}/**start-delivery** (컨트롤 URI)
                    - 리소스만 가지고 URI를 전부 설계할 수는 없다
    3) **다른 메서드로 처리하기 애매한 경우**
        - 예) JSON으로 조회 데이터를 넘겨야 하는데, GET 메소드를 사용하기 어려운 경우
        - 애매하면 POST

> POST는 메시지를 내부에 담아 하는 행위 등 모든 것이 가능하다. 하지만 POST는 캐싱은 어렵다!


<a name="3"></a>
## 03 HTTP method - PUT, PATCH, DELETE ##
<details>
  <summary>
    <h3> PUT </h3>
  </summary>

- **리소스 대체** (컴퓨터 파일로 이해하면 쉽다)
    - 리소스가 있으면 완전히 대체(기존 내용/필드 삭제)
    - 리소스가 없으면 생성
    - 쉽게 이야기해서 덮어버림
- **‼️ 클라이언트가 리서스 식별**
    - 클라이언트가 리소스 위치를 알고 URI 지정(리소스를 100에 지정할 것이다)
        - 예) PUT members/100 HTTP/1.1
        - 이 부분이 POST와의 차이점이다
</details>

> 따라서 부분만 수정할 때는 PUT은 적합하지 않다. 수정 -> PATCH 사용

<details>
  <summary>
    <h3> PATCH </h3>
  </summary>

- 리소스 부분 변경
    - 필드 "이름", "나이" 부분 수정(update)하고 싶다면.
- 지금은 [PATCH] 대부분 지원하지만 안 될 경우 POST를 사용하면 된다.

<img width="1114" alt="스크린샷 2022-05-27 오후 7 06 07" src="https://user-images.githubusercontent.com/96563289/170679020-2e11ade9-7003-4bca-802c-7dfff1c69398.png">
</details>

<details>
  <summary>
    <h3> DELETE </h3>
  </summary>

- 리소스 제거

<img width="682" alt="스크린샷 2022-05-27 오후 7 10 27" src="https://user-images.githubusercontent.com/96563289/170679806-65d5efd0-6cef-473c-87c4-e3d8588b8f28.png">

<img width="621" alt="스크린샷 2022-05-27 오후 7 10 41" src="https://user-images.githubusercontent.com/96563289/170679863-5314525a-cbf7-4569-bf9d-c1085f4cbbe5.png">
</details>

<a name="4"></a>
## 04 HTTP method 속성 ##

```
- 안전(Safe Methods)
- 멱등(Idempotent Methods)
- 캐시가능(Cacheable Methods)
```
<img width="668" alt="스크린샷 2022-05-27 오후 7 20 17" src="https://user-images.githubusercontent.com/96563289/170681353-57277f63-db56-4395-80ab-8aaa776b5bfa.png">

### 안전(Safe) ###
- HTTP method에서 [안전]이란, 호출해도 리소스를 변경하지 않는다는 것이다.
- 따라서 GET, HEAD 변경이 없어 안전하다
    - Q : 그래도 계속 호출해서 로그 같은게 쌓여 장애가 발생하면?
    - A : 안전은 해당 리소스만 고려한다. 그런 부분까지 고려하지 않음.

### 멱등(Idempotent) ###
- f(f(x)) = f(x)
- 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같아야 멱등한 메서드이다.
- 멱등은 외부 요인으로 중간에 이소스가 변경되는 것 까지 고려하지 않는다 -> 이런 거는 서버에서 중간에 메서드가 바뀌었는지 확인해 줘야 한다.
- 멱등 메서드 
    - GET : 한 번 조회하든, 두 번 조회하든 같은 결과가 나온다
    - PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
    - DELETE: 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.
    - POST : ```멱등 아님!``` 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다.
- 어디에 사용할까?
    - 자동 복구 메커니즘
    - 서버가 TIMEOUT 등으로 정상 응답을 못 주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가? 판단 근거.

### 캐시가능(Cacheable) ###

```
<캐시>
    데이터나 값을 미리 복사해 놓은 임시 장소를 가르킨다. 리소스 파일들의 임시 저장소. 
    같은 웹 페이지에 접속할 때 사용자의 PC에서 로드하므로 서버를 거치지 않아도 된다
```

- 응답 결과 리소스를 캐시해서 사용해도 되는가?
- GET, HEAD, POST, PATCH 캐시 가능
- 실제로는 GET, HEAD 정도만 캐시로 가능(GET은 URL만 키로 잡고 캐시 가능. 실무에서는 거이 GET만 사용)
    - 왜냐면 POST, PATCH 본문 내용까지 캐시 키로 고려하는데 구현이 쉽지 않다.


---
### 관련 내용 참조 ###
- https://kotlinworld.com/114
- https://intrepidgeeks.com/tutorial/http-five-main-methods-getpostputpatchdelete
- https://developer.mozilla.org/ko/docs/Web/HTTP/Methods


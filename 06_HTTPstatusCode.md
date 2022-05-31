## 06 HTTP 상태 코드 ##
- [01 상태코드](#1)
- [02 상태 코드 상세](#2)
    - [1xx (Informational)](#2-1)
    - [2xx (Successful)](#2-2)
    - [3xx (Redirection)](#2-3)
    - [4xx (Client Error)](#2-4)
    - [5xx (Server Error)](#2-5)

---

<a name="1"></a>
## 01 상태 코드 ##

> 클라이언트가 요청을 보내면, 그 요청이 잘 처리가 됐는지/ 문제가 있는지를 응답해서 알려주는 기능

<details>
  <summary>
    <h3> 상태 코드 </h3>
  </summary>

- ``` 1xx (Informational) ```: 요청이 수신되어 처리 중(거이 사용되지 않음)
- ``` 2xx (Successful) ``` : 요청 정상 처리
- ``` 3xx (Redirection) ```: 요청을 완료하려면 추가 행동이 필요
- ``` 4xx (Client Error) ```: 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- ``` 5xx (Server Error) ```: 서버 오류, 서버가 정상 요청을 처리하지 못함

</details>

> 만약 모르는 상태 코드가 나타나면?
- 클라이언트가 인식할 수 없는 상태코드를 서버가 반환하면?
    - 클라이언트는 ```상위 상태코드로 해석```해서 처리
    - 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨

<a name="2"></a>
## 02 상태 코드 상세 ##

<a name="2-1"></a>
<details>
  <summary>
    <h3> 1xx (Informational) </h3>
  </summary>

- 요청이 서버에 수신되어 처리 중
- 이는 잘 사용되지 않는다

</details>

<a name="2-2"></a>
<details>
  <summary>
    <h3> 2xx (Successful) </h3>
  </summary>

- 성공
- 클라이언트에서 보낸 무언가가 잘 해결이 되었구나

```
- 200 OK
- 201 Creted
- 202 Accepted
- 204 No Content
```

- ```201 Creted```
    - 생성된 리소스는 응답의 Location 헤더 필드로 식별
    - ```Location: members/100```
- ```202 Accepted```
    - 요청이 접수되었으나 처리가 완료되지 않았음
    - 예) 요청 접수 후 서버에서는 1시간 있다가 배치를 하는 경우
- ```204 No Content```
    - 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
    - 예) 웹 문서 편집기에서 save 버튼
    - save 버튼의 결과로 아무 내용이 없어도 된다
    - 결과 내용이 없어도 204 메시지(2xx)만으로 성공을 인식할 수 있다

</details>

<a name="2-3"></a>
<details>
  <summary>
    <h3> 3xx (Redirection) </h3>
  </summary>

> 요청을 완료하기 위해 유저 에이전트의 추가 조치 필요

```
- 300 Multiple Choices
- 301 Moved Permanently
- 302 Found
- 303 See Other
- 304 Not Modifiled
- 307 Temporary Redirect
- 308 Permanent Redirect
```

>
**"리다이렉션"은 무엇인가?**
- 웹 브라우저에는 3xx 응답의 결과에 Location 헤더가 있으면, 
- Location 위치로 자동 이동(리다이렉트)

<리다이렉션 종류>
- **영구 리다이렉션**
    - 특정 리소스의 URI가 영구적으로 이동
    - /members -> /users
    - /event  -> /new-event
- **일시 리다이렉션**
    - 일시적인 변경
    - 주문 완료 후 주문 내역 화면으로 이동
    - PGR: Post/Redirect/Get
- **특수 리다이렉션**
    - 결과 대신 캐시를 사용
>

- ```영구 리다이렉션```
    - 301, 308
    - 리소스 URL를 사용하지 않는다. 검색엔진 등에서도 변경 인지
    - **301 Moved Permanently**
        - 리다이렉트시 요청 메서드가 GET으로 변하고, 분문이 제거 될 수 있음(MAY)
    - **308 Permanent Redirect**
        - 301과 기능은 같음
        - 리다이렉트시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트)
- ```일시 리다이렉션```
    - 302, 307, 303
    - 리소스 URI가 일시적으로 변경
    - 302 Found
        - 리다이렉트시 요청 메서드가 GET으로 변하고, 분문이 제거 될 수 있음(MAY)
    - 307 Temporary Redirect
        - 리다이렉트시 요청 메서드와 본문 유지(메서드를 변경하면 안된다. MUST NOT)
    - 303 See Other
        - 302와 기능은 같음
        - 리다이렉트시 요청 메서드가 GET으로 변경
- ```특수 리다이렉션``` 


</details>

<a name="2-4"></a>
<details>
  <summary>
    <h3> 4xx (Client Error) </h3>
  </summary>


</details>

<a name="2-5"></a>
<details>
  <summary>
    <h3> 5xx (Server Error) </h3>
  </summary>


</details>


<details>
  <summary>
    <h3> 정리 </h3>
  </summary>


</details>

---
### 관련 내용 참조 ###
- https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/lecture/61372?tab=note&volume=1.00
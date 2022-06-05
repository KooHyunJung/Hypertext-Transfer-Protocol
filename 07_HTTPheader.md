## 06 HTTP 상태 코드 ##
- [01 캐시 기본 동작](#1)
- [02 검증 헤더와 조건부 요청1](#2)
- [03 검증 헤더와 조건부 요청2](#3)
- [04 캐시와 조건부 요청 헤더](#4)
- [05 프록시 캐시](#5)
- [06 캐시 무효화](#6)

---

> HTTP 헤더는 굉장히 많다. 이를 자주 사용하는 [일반 헤더]와 [캐시 조건부 관련 헤더]로 나누어 설명한다.

<a name="1"></a>
## 01 캐시 기본 동작 ##

<details>
  <summary>
    <h3> "HTTP 헤더"란? </h3>
  </summary>

![스크린샷 2022-06-01 오후 2 25 35](https://user-images.githubusercontent.com/96563289/171333864-81847165-21a7-420e-bdf9-22872f503a16.png)

- HTTP 헤더 필드 형태
  - ```header-field = field-name : field-value```
- HTTP 용도
  - **HTTP 전송에 필요한 모든 부가 정보**
    - 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보...
  - 표준 헤더가 너무 많다.
  - 필요시 임의의 헤더 추가 가능
</details>


<a name="2"></a>
## 02 검증 헤더와 조건부 요청1 ##

<details>
  <summary>
    <h3> 02 검증 헤더와 조건부 요청1 </h3>
  </summary>

</details>

<a name="3"></a>
## 03 검증 헤더와 조건부 요청2 ##

<details>
  <summary>
    <h3> 검증 헤더와 조건부 요청2 </h3>
  </summary>
</details>

<a name="4"></a>
## 04 캐시와 조건부 요청 헤더 ##

<details>
  <summary>
    <h3> 캐시와 조건부 요청 헤더 </h3>
  </summary>
</details>

<a name="5"></a>
## 05 프록시 캐시 ##

<details>
  <summary>
    <h3> 프록시 캐시 </h3>
  </summary>
</details>

<a name="6"></a>
## 06 캐시 무효화 ##

<details>
  <summary>
    <h3> 캐시 무효 </h3>
  </summary>
</details>

---
### 관련 내용 참조 ###
- https://www.jaeme.dev/web-http6/
- https://wonit.tistory.com/308
- https://rangken.github.io/blog/2015/http-headers/
- https://gmlwjd9405.github.io/2019/01/28/http-header-types.html 

### 학습 메모 ###
- 생각보다 HTTP 헤더에 담는 데이터가 많다.
- 웹사이트를 만들면서 인증에 관련한 세션, 쿠키는 인숙하나 다른 개념은 반복 학습 필요.
- [07 HTTP 헤더] 내용은 참고 링크 보기.
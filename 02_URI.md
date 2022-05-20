## 02 URI와 웹 브라우저 요청 흐름 ##
- [01 URI](#1)
- [02 웹 브라우저 요청 흐름](#2)

---

> 웹 브라우저에 요청하면 HTTP 메시지가 만들어져서 어떤 방식으로 전달 될까? 

<a name="1"></a>
## 01 URI ##
### 통합 자원 식별자(Uniform Resource Identifier) ###
```
- Uniform : 리소스 식별하는 통일된 방식
- Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음. 거이 대부분이라 생각하면 됨)
- Identifier : 다른 항목과 구분하는데 필요한 정보
```
- 직역하면 리소스를 식별하는 통합된 방법이다. 네트워크 상 자원을 구분하는 식별자.
- 인터넷에 있는 자원을 나타내는 유일한 주소이다. URI의 존재는 인터넷에서 요구되는 기본조건으로서 인터넷 프로토콜에 항상 붙어 다닌다. URI의 하위개념으로 URL, URN 이 있다.
- https://www.ietf.org/rfc/rfc3986.txt 
    - "URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다"
        - 자원이 어디에 있는지 식별하는 방법
            - URL(Uniform Resource Locator) 
                ``` https://google.com ```
                - 리소스가 있는 [위치]를 지정
                - 리소스가 어디에 있고 어떻게 접근할 수 있는지 알려주는 역할 → http, ftp 등의 프로토콜 포함
                - 특정 시점의 위치를 알려주는 역할을 하므로 리소스가 옮겨지면 더는 사용할 수 없음
                    - 위치는 변할 수 있지만 이름은 변하지 않는다
            - URN(Uniform Resource Name)
                ``` urn:isbn:9780141036144 ```
                - 리소스에 [이름]을 부여
                - 독립적인 자원 지시자 → 리소스가 이동해도 항상 리소스를 가리킬 수 있는 유일한 이름
                - 리소스가 그 이름을 변하지 않게 유지하는 한, 여러 종류의 네트워크 접속 프로토콜로 접근해도 문제없음
                - 지속 통합 자원 지시자(Persistent Uniform Resource Locator, PURL)를 사용하면 URL로 URN의 기능을 제공 가능
                - 하지만 URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음

### URL - scheme ####

    scheme://[userinfo@]host[:port][/path][?query][#fragment]
    https://www.google.com/search?q=hello&hi=ko

- 주로 프로토콜 사용
- 프로토콜 : 어떤 방식으로 자원에 접근할 것인가. 약속 규칙
    - http, https, ftp 등
    - http 80 포트, https 443 포트 주로 사용. 포트는 생략 가능하다.
    - https는 http에 강력한 보안 추가 (http Secure)
    - https : http://wiki.hash.kr/index.php/HTTPS
    - path = "search?" : 리소스의 경로, 계층적 구조 ex) members/100
    - query = "?q=hello&oq=hello&hi=ko"
        - key = value 형태
        - ?로 시작, &로 추가 가능 -> ?keyA=valueA&keyB=valueB
        - query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터, 문자 형태
    - fragment
        - https://www.django-rest-framework.org/api-guide/generic-views/#mixins
        - #mixins
        - HTML 내부 북마크 등에 사용됨
        - 서버에 전송되는 내용 아님



<a name="2"></a>
## 02 웹 브라우저 요청 흐름 ##

    https://www.google.com/search?q=hello&hi=ko

- 검색창에 위와같이 입력하면
    - IP 주소와 포트 번호를 찾아낸다
        1) DNS 조회 -> IP: 200.200.200.2
        2) HTTPS 포트 443
    - 웹 브라우저가 **HTTP 요청 메시지 생성** (아래처럼 생성됨)
    ```
    GET /search?q=hello&hi=ko HTTP/1.1
    HOST: www.google.com
    (몇 가지 부가정보가 더 있지만.. 일단 간략하게!!)
    ```
    - 소켓 라이브러리를 통해 전달
        1) TCP/IP 연결(IP,PORT)
        2) 데이터 전달
    - TCP/IP 패킷 생성, **HTTP 메시지 포함**
    - 요청 패킷이 서버에 도착하면 TCP/IP 패킷을 까서? 버리고 **HTTP 메시지** 해석한다
    - 서버는 해석한 내용을 가지고 데이터를 찾아 응답
    - 서버에서 **HTTP 응답 메시지 생성** (아래처럼 생성됨)
    ```
    HTTP/1.1 200 OK
    Content-Type: text/html;charset=UTF-8
    Content-Length: 3423

    <html>
        <body>...<body>
    </html>
    ```
    - 응답 : TCP/IP 패킷 생성, **HTTP 메시지 포함**
    - 응답을 까서? 웹 브라우저 HTML 렌더링 -> 그러면 결과물을 우리가 보는 것이다!


---
### 관련 내용 참조 ###
https://www.ietf.org/rfc/rfc3986.txt

https://intrepidgeeks.com/tutorial/http-web-basics#15

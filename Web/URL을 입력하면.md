## 브라우저에 URL을 입력하면 어떤일이 일어날까?

### 1. 브라우저가 입력한 URL을 분석한다.
**https://brightestbulb.com/til/web/?type=post**
https 프로토콜로 brightestbulb.com 도메인에 접근한다. 이때, Path는 til/web이고 쿼리는 type=post이다.

### 2. URL을 분석한 후
1. URL 구조에 맞지 않으면 브라우저의 검색 엔진으로 입력어를 검색한다.
2. URL의 구조에 맞으면 HSTS (HTTP Strict Transport Security) 목록을 받아와 url 내의 주소와 목록의 주소가 일치하면 
https로, 그렇지 않으면 http로 첫 요청을 보낸다.

### 3. DNS(Domain Name Server)에 내가 접근하려는 Name 주소의 IP 주소를 요청한다.
1. Recursive Server의 cache를 먼저 확인한다. 있으면 그 IP 주소를 리턴한다.
2. 없을 경우에 DNS 시스템에 따라, Root server - TLD server - Authoriative server에 ip를 물어본다.

### 4. IP 주소를 받은 다음, ARP를 통해 실질적으로 내가 접근해야할곳의 MAC 주소를 추적한다.
1. MAC주소를 알아야하므로 OSI의 2계층도 연관되어 있다.
2. subnet인지 아닌지 판단하고, router 내에 존재할 경우(local Network), routing table을 추적하여 MAC 주소를 알아낸다.
3. local Network가 아닐 경우, gateway를 타고 밖으로 나가 상대방의 MAC 주소를 찾아낸다.
 이 과정에서, ARP를 broadcasting 한다.
4. 상대방의 IP주소와 MAC주소를 응답 받으면, 다시 DNS프로세스를 시작한다.
5. DNS에서 53번 포트를 통해 UDP로 실시하며 데이터 용량이 큰 경우, TCP로 진행한다.

### 5. 대상과 TCP 통신을 통해 Socket을 연다.

![](https://github.com/brightestbulb/TIL/blob/master/Web/img/osi7layer.png?raw=true)
1. OSI 7계층을 통해 클라이언트에서 서버까지 데이터 전달 및 세션 연결
2. 이때, OSI(4계층 - transport Layer) 에서 Session을 연결할 때 TCP 연결을 진행

### 6. HTTPS인 경우 TLS(Transport Layer Security) handshake가 추가된다.

### 7. HTTP 프로토콜로 요청한다.
1. Client가 서버에 HTTP 프로토콜을 요청한다.
      형식 =>   GET / HTTP/1.1 Host: google.com Connection: close [other headers]
      이때, [other headers]는 key : value 쌍을 말한다.
2. HTML에서 참조하는 모든 페이지(Image, css, favicon.ico 등)에 대해 이 과정을 반복한다.

### 8. HTTP의 응답
1. HTTPD(HTTP Demon : HTTP 요청 / 응답 처리 서버 => Apache, Nginx, IIS 등) 가 요청을 수신
2. 매개변수 구분 : HTTP Method(GET, POST, PUT, DELETE....) : URL을 직접 입력하는 경우는 GET + Domain ( tistory.com ) + 경로 ( /path )
3. tistory.com에 가상 호스트가 있는지 체크
4. 서버는 tistory.com이 GET 요청 수락 가능한지 체크
5. 요청에 따른 컨텐츠 불러옴
6. 해석 후 출력을 클라이언트로 스트리밍

### 9. 웹 브라우저의 그림 그리기
1. 구문 분석 (HTML, CSS, JS) + 렌더링 ( DOM Tree 구성 - 렌더 트리 구성 - 렌더트리 레이아웃 배치 - 렌더트리 그리기 )
2. HTML parsing, CSS parsing, Page Rendering, GPU Rendering 을 통해 그림을 그려냄



출처 : https://velog.io/@directorhwan59/%EC%9B%B9-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-URL%EC%9D%84-%EC%9E%85%EB%A0%A5%ED%95%98%EA%B3%A0-%EC%B2%AB-%ED%99%94%EB%A9%B4%EC%9D%B4-%EC%B6%9C%EB%A0%A5%EB%90%98%EA%B8%B0%EA%B9%8C%EC%A7%80

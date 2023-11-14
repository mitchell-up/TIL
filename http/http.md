# HTTP 통신에 대한 이해
HyperText Transfer Protocol의 줄임말이다. 문자 그대로 하이퍼텍스트를 전송하기 위한 통신규약으로 웹 브라우저와 웹 서버 간에 데이터를 주고 받는데 사용된다. HTML, CSS, JS, Image, Video 등 웹 브라우저에서 사용되는 리소스를 주고 받을 수 있다.

##### HTTP 요청 예시
```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.69 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8
```

##### HTTP 응답 예시
```
HTTP/1.1 200 OK
Date: Sun, 14 Nov 2023 12:00:00 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 2547

<!DOCTYPE html>
<html>
<head>
  <title>Example Page</title>
</head>
<body>
  <h1>Welcome to Example Page</h1>
  <p>This is a sample HTML page.</p>
</body>
</html>
```
<br/>

## OSI 7계층에서의 HTTP
![image](https://user-images.githubusercontent.com/59492312/160339237-6c677536-0e9d-43fc-862d-cca439bd9cae.png)
- HTTP는 OSI 7계층 중 Application 계층의 프로토콜이며, TCP를 기반으로 동작합니다.
- TCP는 연결지향 프로토콜로 웹 브라우저와 웹 서버의 연결에 관여합니다. 
- HTTP는 TCP의 연결 이후 데이터를 전송하고 전달받는 과정에 관여합니다.
<br/>

## HTTP 통신 흐름
1. 클라이언트가 HTTP 요청을 생성합니다.
2. 클라이언트의 HTTP 요청은 TCP/IP 스택으로 전달됩니다. TCP는 이 요청을 전송하기 위해 소켓을 생성하고, 목적지인 서버의 IP 주소와 포트 번호를 사용하여 연결을 설정합니다.
3. TCP는 세그먼트라는 작은 데이터 조각으로 HTTP 요청을 나누어서 서버로 전송합니다. 이 과정에서 TCP는 흐름 제어와 오류 복구를 담당합니다. 데이터가 분실되거나 손상되는 경우, TCP는 재전송을 요청하여 안정적인 전송을 보장합니다.
4. 서버는 TCP 연결을 수락하고 클라이언트로부터 온 요청을 받습니다.
5. 서버는 받은 HTTP 요청을 처리하고, 요청에 대한 적절한 HTTP 응답을 생성합니다.
6. 서버의 HTTP 응답은 TCP 소켓을 통해 클라이언트로 전송됩니다. 마찬가지로 TCP는 세그먼트로 데이터를 나누어 안정적으로 전송합니다.
7. 클라이언트는 TCP를 통해 받은 응답을 수신하고, 응답을 조립하여 원하는 데이터를 추출하거나 화면에 표시합니다.
8. 마지막으로, TCP는 연결을 종료하고 소켓을 닫습니다.
<br/>

## HTTP 특징
- Stateless
  - 상태가 없는 것을 의미한다. 즉 각 요청은 서로 독립적이며, 이전 요청에 대한 상태를 기억하지 않는다.
  - 따라서 확장성과 유연성을 높여준다.
- Connectionless
  - 연결을 유지하지 않고 즉시 끊음으로서 연결에 대한 리소스를 절약한다.
  - 많은 요청을 동시에 처리하는 상황에서 매우 중요하다.
- 불필요한 연결 제거
  - 웹사이트를 구성하기 위해서는 여러 요청을 서버에 보내야한다. 이때 매 요청마다 새로운 연결을 만들지 않고 이전 연결을 통해서 서버 응답을 받을 수 있다.
  - HTTP/1.1부터 'Keep-Alive' 매커니즘을 통해 단일 TCP 연결에서 여러 HTTP 요청을 처리할 수 있게 되었다.
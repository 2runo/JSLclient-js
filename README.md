# JSL Library for JavaScript
JSL은 [WebSocket](https://ko.wikipedia.org/wiki/%EC%9B%B9%EC%86%8C%EC%BC%93) 암호화 통신 프로토콜입니다.
SSL 작동 원리에 기반하여 RSA, AES 암호화 알고리즘을 통해 SSL 인증서 없이도 안전한 통신을 할 수 있도록 해줍니다.

현재 지원되는 서버 언어로는 [Python](python.org), 클라이언트 언어로는 [JavaScript](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8)가 있습니다.

해당 프로젝트는 JSL Client를 JavaScript에서 구현하도록 하는 라이브러리입니다.

JSL Server는 Python으로 구현할 수 있습니다. [여기](https://github.com/2runo/JSL-py)에서 JSL Server를 구축하는 방법을 알아보세요. 쉽습니다!

# JSL 작동 방식
JSL은 다음과 같은 순서로 작동합니다.
### 핸드셰이킹
1. client hello - 서버가 클라이언트에게 JSL 프로토콜 사용 여부를 묻습니다.
2. server hello - 클라이언트가 이에 응답합니다.
3. RSA public key 전달 - 서버가 클라이언트에게 RSA public key를 전달합니다.
4. AES key 생성 - 클라이언트에서 랜덤하게 AES key를 생성합니다.
5. AES key 전달 - 클라이언트가 서버에게 AES key를 RSA public key로 암호화하여 전달합니다. 이때 제 3자(해커)는 AES key를 알아낼 수 없습니다.
6. 핸드셰이킹 종료 - 이제 핸드셰이킹이 완료되었습니다!
### 통신
클라이언트가 서버에게 전달한 AES key를 통해 통신 메시지를 암호화하고 해독합니다.

통신 내용은 모두 암호화되므로 제 3자(해커)는 통신 내용을 확인할 수 없습니다!

# 사용법

우선 라이브러리를 가져옵니다:
```html
<script src="jsl.js"></script>
```

이제 아래의 코드로 JSL 라이브러리를 호출할 수 있습니다:

(이때, 연결하려는 WebSocket 서버는 JSL 프로토콜을 지원해야 합니다. [JSL 서버 구축하기](https://github.com/2runo/JSL-py))
```javascript
jsl = new JSLhandshaker("ws://example.com:1234");
```
`onopen`을 통해 JSL 핸드셰이킹이 완료되었을 때 실행할 코드 내용을 지정할 수 있습니다:
```javascript
jsl.onopen = function() {
  console.log("handshake succeeds!")
}
```
`onmsg`는 서버로부터 메시지가 도착했을 때 실행됩니다:
```javascript
jsl.onmsg = function(msg) {
  console.log(msg);
}
```
`send`는 서버에 메시지를 전송할 때 사용합니다 (물론 자동으로 암호화됩니다!):
```javascript
jsl.send("Hello JSL!");
```

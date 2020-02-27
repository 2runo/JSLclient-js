# JSL Library for JavaScript
JSL은 [WebSocket](https://ko.wikipedia.org/wiki/%EC%9B%B9%EC%86%8C%EC%BC%93) 암호화 통신 프로토콜입니다.
SSL 작동 원리에 기반하여 RSA, AES 암호화 알고리즘을 통해 SSL 인증서 없이도 안전한 통신을 할 수 있도록 해줍니다.

현재 지원되는 서버 언어로는 [Python](python.org), 클라이언트 언어로는 [JavaScript](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8)가 있습니다.

# 사용법

우선 라이브러리를 가져옵니다:
```html
<script src="jsl.js"></script>
```

이제 아래의 코드로 JSL 라이브러리를 호출할 수 있습니다:

(이때, 연결하려는 WebSocket 서버는 JSL 프로토콜을 지원해야 합니다. [JSL 서버 구축하기](github.com))
```javascript
jsl = new JSLhandshaker("ws://example.com");
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

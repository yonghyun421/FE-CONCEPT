# fetch & axios

## Axios

node.js와 브라우저를 위한 HTTP 통신 라이브러리, 비동기로 HTTP 통신을 가능하게 해주며 return 을 promise 객체로 해주기 때문에 response 데이터를 다루기도 쉽다.

## fetch

ES6부터 JS의 내장 라이브러리로 들어왔다. promise 기반으로 만들어졌기에 Axios와 마찬가지로 데이터를 다루는데 어렵지 않으며, 내장 라이브러리라는 장점으로 인해 상당히 편리하다.

### Axios의 장점

- response timeout 처리 방법이 있다.
- promise 기반으로 다루기가 쉽다.
- 크로스 브라우징에 신경을 많이 썼기에 브라우저 호환성이 뛰어나다.

### Axios의 단점

- 모듈 설치를 해줘야한다.

### Fetch의 장점

- 내장 라이브러리이기 때문에 별도의i import를 해줄 필요가 없다.
- promise 기반으로 다루기가 쉽다.
- 내장 라이브러리이기에 사용하는 프레임워크가 안정적이지 않을 때 사용하기 좋다.

### Fetch의 단점

- 브라우저 호환성이 상대적으로 떨어진다.
- 기능이 부족하다
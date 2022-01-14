# RxJS란?

Observable을 사용하여 비동기 및 이벤트 기반 프로그램을 작성하기 위한 라이브러리

## RxJS를 사용하는 이유

RxJS는 ReactiveX의 자바스크립트 버전의 라이브러리이다.
RxJS는 입력오류, 상태오류, 로직오류로 인해 발생하는 오류들을 효과적으로 처리하기 위한 최적의 라이브러리이다.
궁극적으로는 일관된 방식으로 안전하게 데이터 흐름의 처리를 도와주는 라이브러리이며 한마디로 정리하자면 범용데이터의 흐름을 제어하는 솔루션이다.
상태 전파를 하기 위해 리액티브 프로그래밍의 패러다임도 녹아있고, 로직오류를 방지하기 위한 함수형 퓨ㅡ로그래밍의 패러다임 기법도 사용하고 있다.

### 설치

```bash
npm install rxjs
npm install @reactivex/rxjs	// Typescript와 같이 사용하려면 설치
```

### 시작

#### Click 이벤트를 통해 console.log를 실행하는 코드

- JavaScript

```js
const eventHandler = (event) => {
  console.log(event.currentTarget);
};

document.addEventListener("click", eventHandler);
```

- RxJS

```js
import { fromEvent } from "rxjs";
const click$ = fromEvent(document, "click"); // observable
const observer = (event) => {
  console.log(event.currentTarget);
};
click$.subscribe(observer);
```

fromEvent를 통해 이벤트 Observable로 생성하고 observer가 Observable 객체를 구독한다.

Observable는 어떠한 행위를 하는 객체 또는 데이터로써 어떠한 동작을 하기 위해 데이터 또는 이벤트를 Observable로 생성하고, 이를 구독하여 어떠한 observer 행위를 하여 개발을 진행하는 방식이 RxJS의 개발 패턴이다.

=> 그렇기 때문에 RxJS 개발 패턴에서 가장 먼저 해야하는 것이 Observable을 생성하는 것이다.

## RxJS의 중요개념

### Observable

시간을 축으로 연속적인 데이터를 저장한 객체.
이러한 Observable 객체를 observer에 전달하여 처리하는 과정을 스트림이라고 한다.

### 오퍼레이터

filter와 같이 Observable 객체를 생성 또는 조작하는 함수를 오퍼레이터라고 한다. 이러한 오퍼레이터를 통해 Observable 객체를 생성하기도 하고, 각각의 Observable 객체를 서로 연결 또는 분리, 합치기도 한다. 오퍼레이터는 현재의 Observable의 인스턴스를 기반으로 항상 새로운 Observable 인스턴스를 반환한다. RxJS는 다양한 오퍼레이터들을 제공하고 있고, 상황에 맞는 오퍼레이터를 사용해야 한다.

과거에는 오퍼레이트 사용시 도트체이닝을 사용하여 개발하였으나, RxJS 6.0부터 지원 도트체이닝을 제공하지 않고, pipe 오퍼레이터만 제공한다.
도트체이닝 사용 시 Observable 객체가 모든 오퍼레이터를 가지고 있어야 하는데, 이는 불필요한 오퍼레이터도 모두 가지고 있어야 하기에 파일 사이즈가 증가한다.
pipe 오퍼레이터 사용시 webpack을 통해 트리쉐이킹(사용하지 않는 모듈을 번들링 할 때 제거)하여 사용된다.

### Observer

Observable에 전달된 데이터를 소비하는 주체이며, next, error, complete 함수를 가진 객체이다.

```js
const observer = {
  next: (x) => console.log(`Observer가 Observable로 받은 데이터: ${x}`),
  error: (err) =>
    console.log(`Observer가 Observable로 받은 에러 데이터: ${error}`),
  complete: () =>
    console.log(`Observer가 Observable로 부터 종료 되었다는 알림 메시지`),
};

click$.subscribe(observer);
```

### Subscription

Observable.prototype.subscribe의 반환값이다.
Subscription 객체는 자원의 해제를 담당하고 더이상 데이터를 전달받고 싶지 않은 경우 unsubscribe 메소드를 호출하여 자원을 해제해야한다.
유한한 데이터의 경우 Observable의 자원을 해지할 필요가 없다.
하지만 이벤트 핸들러 또는 interval을 통해 데이터를 계속 전달 받는 경우 자원해제가 필요하다.

```js
const subscription = currentTarget$.subscribe(observer);

subscription.unsubscribe();
```

## RxJS 개발방법

이벤트를 Observable로 변환하고 filter 등의 오퍼레이터를 이용하여 변경데이터를 처리할 Observer를 만들고 subscribe 메소드를 통해 Observable를 구독한다.

- 1. 데이터를 Observable로 변경
- 2. 오퍼레이터를 통해 변경 또는 추출 또는 여러 개의 Observable을 결합 또는 분리
- 3. 원하는 데이터를 처리할 Observer를 생성
- 4. Observable의 subscribe를 통해 Observer를 등록
- 5. Observable의 구독을 정지하고 자원을 해제

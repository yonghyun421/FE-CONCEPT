# Redux Observable란?

RxJS 기반의 Redux Middleware이다.
_Redux Middleware는 dispatch가 호출되었을 때, Action을 가로채, 작업을 처리하고 다음 Middleware에게 Action을 전달한다._
dispatch 가 호출 되었을때, 구독하고 있는 Epic 들이 원하는 Action 이 있다면, Action 을 Observable 로 작업을 처리하고, 다시 store.dispatch 로 Action 을 호출하는 방법으로 구성되어 있다.

```js
epic(action$, state$).subscribe(store.dispatch);
```

## redux-observable 구현 과정

1. set up
2. epic 생성
3. export epic : combineEpics 사용(epic이 두개 이상일때)
4. store 등록
   5-1. createEpicMiddelware() : epicMiddleware 생성
   5-2. store에 middleware 추가
   5-3. epicMiddleware.run()

## SET UP

```bash
npm install --save redux-observable@1.2.0
npm install rxjs
npm install rxjs-operators
```

## Epic 생성

### Epic이란?

Epic이란 reducer의 action 하나하나에 매칭되는 middleware이다.
epic내의 구현 흐름은 특정 action이 dispatch 되었을 경우 그것과 매칭되는 epic이 발동하고, epic 내의 순수함수들의 일련의 절차를 거치고 나서, 다시 특정 action을 반환한다. 따라서 epic은 action in, action out 형식이라고 생각할 수 있다.

### Epic 생성

epic(increment_plus3_epic)에 increment라는 action이 들어오면 incrementByValue()라는 액션의 parameter로 3을 넣어 action을 반환하는 과정으로 예를 들어보면

```js
import { Action, Observable } from "redux";
import {
  ActionObservable,
  StateObservable,
  ofType,
  combineEpics,
} from "redux-observable";
import { map, mergeMap } from "rxjs/operators";
import { incrementByValue, increment } from "./counter";

const increment_plus3_epic = (action$, state$) => {
  return action$.pipe(
    ofType(increment),
    map((result) => {
      return incrementByValue(3);
    })
  );
};

export const exampleEpic = combineEpics(increment_plus3_epic);
```

### epic export

위의 epic들을 store에 등록하기 위해 export한다. 이때 epic은 action 당 하나씩 만들어지기 때문에 하나 이상일 수 있다. 따라서 combineEpic을 통해 묶어서 export한다.

```js
export const exampleEpic = combineEpics(increment_plus3_epic);
```

## Store 등록

### createEpicMiddelWare()

만들어 놓은 epic을 store에 middleware로 등록하기 위해서, 여러 미들웨어들을 담을 대표가 필요하다. 이를 위해, createEpicMiddleware()를 이용한다.

```js
const epicMiddleWare = createEpicMiddleware();
```

### store 등록

앞에서 만든 대표 미들웨어를 store에 등록한다.

```js
export const store = configureStore({
  reducer: {
    counter: CounterReducer,
  },
  // 밑의 한 줄을 추가한다.
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(epicMiddleWare),
});
```

### epic 등록

편의상 대표 미들웨어라고 부른 것이 store에 등록되었기 때문에, 그 대표 미들웨어에 여러 epic들이 등록되며 자동으로 store에 등록된다. 미리 만들어 놓은 epic을 대표 미들웨어에 등록한다.

```js
epicMiddleware.run(exampleEpic);
```

### store의 전체 코드

```js
import { configureStore } from "@reduxjs/toolkit";
import { createEpicMiddleware } from "redux-observable";
import CounterReducer from " ../features/counter/counter";
import { exampleEpic } from "../features/counter/counterEpic";

const epicMiddleWare = createEpicMiddleware();

export const store = configureStore({
  reducer: {
    counter: CounterReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(epicMiddleWare),
});

epicMiddleWare.run(exampleEpic);
```

#### 결과

increment 액션을 보내면 epic이 받아서 incrementByValue(3)을 리턴하고 다시 increment와 incrementByValue(3)이 모두 reducer로 가서 각각 1, 3씩 state를 증가시킨다. 총 4씩 증가하는 것이다.
epic은 mniddleware로 주로 사용되기 때문에 네트워크 통신이나 비동기 처리에 사용하면 좋을 것이다.

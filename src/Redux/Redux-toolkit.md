# Redux의 문제점들

리덕스는 리액트의 상태 관리 라이브러리 중 대표로서 굳게 자리매김하고 있었지만, 사용하기 너무 복잡하고 배우기 어렵다는 의견이 많았다. (나또한 그렇게 느껴서 여러번 좌절할뻔했다...)
리덕스가 높은 러닝 커브와 복잡성을 가지는 원인들을 리덕스 측에서 문서로 공식적으로 발표한 것을 보면서 어떤 것들이 있는지 확인해보자.

### 1. store configuration 복잡성

우선, 리덕스는 스토어(store)를 설정하는게 너무 복잡하다. 리덕스는 앱 전체에 단 하나의 스토어를 가지며 이 스토어는 리액트 앱에서 사용할 상태(state)들을 저장하고 관리한다. 따라서 스토어는 상태 값들을 사용하고 관리하는 모든 API를 가지며 이것을 설정해주는 것은 복잡한 작업이었다.

### 2. redux를 사용하기 위해선 다른 라이브러리들의 추가설치가 너무 많이 필요했다.

redux를 제대로 사용하려면 다음과 같은 패키지들을 추가 설치해줘야 한다.

- react-redux : react와 바인딩
- immutable/immer : 불변성 유지
- redux-thunk/redux-saga/redux-pender : 비동기 통신
- redux-actions : 액션 생성
  심지어 리액트와 리덕스 특유의 높은 자유도로 인해 프로젝트마다, 혹은 개발자 취향마다 설치하는 패키지들이 다 제각각일 수 있다.

따라서 매번 프로젝트마다 다른 패키지를 학습해야 하는 상황이 발생하게 될 수 있는 것이다.

### 3. 너무 많은 양의 보일러플레이트를 파일마다 반복 작성해야 했다.

리덕스 액션과 리듀서들을 도메인마다 분리를 하고, 또 리액트 각 컴포넌트마다 바인딩을 하면, 엄청나게 많은 수의 파일에서 리덕스 코드를 사용하게 된다. 그런데 그 많은 파일에서 많은 양의 보일러 플레이트 코드를 반복해서 사용해줘야 해서 코드가 지저분해지고 관리포인트가 많다는 단점이 있다.

또한 추가적으로 설치하는 패키지에 따라 보일러플레이트 코드가 천차만별이어서 정형화된 보일러 플레이트를 바로 작성하기도 어렵다.

## Redux Toolkit을 통한 문제점 해결

리덕스 툴킷(Redux Toolkit)은 리덕스에서 공식적으로 내놓은 위에서 언급한 단점들을 제거한 패키지이다.
리덕스 공식 문서도 리덕스를 사용할때 이 리덕스 툴킷을 사용하는 것이 좋다고 적극 권장하고 있다.

### 1. store configuration

먼저 스토어 생성은 configureStore 함수를 실행하여 할 수 있다. 이 안에서 리듀서(reducer)들을 바인딩 해주면 된다.

```js
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({
  reducer: {},
});
```

그리고 리액트 앱에서는 이 store를 import해서 Provider로 제공해준다.

```js
ReactDOM.render(
  <Provider store={store}>
  <App />
  </Provider>
document.getElementById('root')
)
```

### 2. 너무 많은 패키지들 추가 설치

리덕스 툴킷은 딴 두 가지 패키지만 설치해서 사용할 수 있다.

```bash
yarn add @reduxjs/toolkit react-redux
```

리덕스 툴킷은 내부적으로 불변성을 지키기 위해 immer 라이브러리를 사용하고 비동기 통신을 위해 redux-thunk를 사용한다. 리덕스 툴킷은 이렇게 내부적으로 의존 라이브러리들을 사용해서 개발자는 툴킷만 설치하면 다른 패키지를 뭘 설치할지 고민하지 않아도 된다. 그러므로 리덕스 툴킷 사용시 추가 라이브러리를 설치하지 않고 공식문서의 가이드대로 두 가지만 설치를 해서 사용하는 것이 패키지 사용 통일성을 지킬 수 있어 좋다.

### 3. 너무 많은 양의 보일러 플레이트

리덕스 툴킷을 보면 보일러 플레이트 코드량을 최소화하도록 노력한 것을 알 수 있다. flux 패턴 중에 액션 생성 함수, 리듀서 코드는 다음과 같이 작성할 수 있다.

- **type.d.ts**

```ts
type ToDo = {
  id: number;
  content: string;
  checked: boolean;
};

type ToDoList = ToDo[] | [];

interface IToDoListState {
  toDoList: IToDoList;
}

interface IAction {
  type?: string;
}

interface IToggleToDoPayload {
  id: number;
  checked: boolean;
}

interface IToggleToDoAction extends IAction {
  payload: IToggleToDoPayload;
}

interface IDeleteToDoPayload {
  id: number;
}

interface IDeleteToDoAction extends IAction {
  payload: IDeleteToDoPayload;
}
```

- **src/store/toDoList/index.ts**

```ts
import { createAction, createReducer } from "@reduxjs/toolkit";

export const action = {
  toggleToDo: createAction<IToggleToDoPayload>("TOGGLE/TO_DO"),
  deleteToDo: createAction<IDeleteToDoPayload>("DELETE/TO_DO"),
};

const initialState: IToDoListState = {
  toDoList: [],
};

export const reducer = {
  toggleToDo: (state: IToDoListState, action: IToggleToDoAction) => {
    state.toDoList.find((todo: ToDo) => todo.id === action.payload.id).checked =
      action.payload.checked;
  },
  deleteToDo: (state: IToDoListState, action: IDeleteToDoAction) => {
    state.toDoList = state.toDoList.filter(
      (todo: ToDo) => todo.id !== action.payload.id
    );
  },
};

const toDoListReducer = createReducer(initialState, (builder) => {
  builder
    .addCase(action.toggleToDo, reducer.toggleToDo)
    .addCase(action.deleteToDo, reducer.deleteToDo);
});

export default toDoListReducer;
```

이처럼 리듀서 툴킷에서 제공하는 createAction, createReducer만 있으면 리덕스를 사용할 수 있다.
빌더 패턴을 사용하여 리듀서 함수들을 추가하기 때문에 createReducer 함수를 사용하는 코드가 매우 간결하다.

또한 리듀서는 내부적으로 immer 라이브러리를 사용하기 때문에 불변성에 대해서 신경쓰지 않고 state를 변경시켜주어도 내부적으로 불변성을 지켜준다.

만든 리듀서는 스토어에 다음과 같이 추가해준다.

- **src/store/index.ts**

```ts
import { configureStore } from "@reduxjs/toolkit";

import toDoListReducer from "src/store/toDoList";

export const store = configureStore({
  reducer: {
    toDoList: toDoListReducer,
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;

export type AppDispatch = typeof store.dispatch;
```

이제 보일러 플레이트가 얼마나 단순해졌는 확인해기 위해 List와 Item이라는 컴포넌트를 만들어서 확인해보자.

- **src/components/toDoList/List.tsx**

```ts
import { useSelector } from "react-redux";

import Item from "src/components/toDoList/item";
import { RootState } from "src/store";

function List() {
  const toDoList: ToDoList = useSelector(
    (state: RootState) => state.toDoList.toDoList
  );

  return (
    <div>
      {toDoList && toDoList.map((toDo) => <Item key={toDo.id} toDo={toDo} />)}
    </div>
  );
}

export default List;
```

store에 있는 state를 가져오기 위해서는 react-redux가 제공하는 useSelect를 사용한다. 이때 타입스크립트를 사용한다면 store에서 만든 RootState 타입을 사용하여야 한다.

- ** src/components/toDoList/Item.tsx**

```ts
import { useDispatch } from "react-redux";

import { action } from "src/store/toDoList";

interface IProps {
  toDo: ToDo;
}

function Item({ toDo }: IProps) {
  const { id, content, checked } = toDo;
  const dispatch = useDispatch();

  const onToggle = (event: any) => {
    dispatch(action.toggleToDo({ id, checked: event.target.checked as boolean }));
  };

  const onDeleteToDo = () => {
    dispatch(action.deleteToDo({ id }));
  };

  return (
    <div>
      <div>{id}</div>
      <p>{content}</p>
      <input type="checkbox" defaultChecked={checked} onChange={onToggle} />
      <button onClick={onDeleteToDo}>삭제</Button>
    </div>
  );
}

export default Item;
```

액션을 디스패치(dispatch)하기 위해서는 react-redux에서 제공하는 useDispatch를 사용한다.

컴포넌트 단에서도 useSelect와 useDispatch를 이용하면 보일러 플레이트가 많이 간결하다는 것을 확인할 수 있다.

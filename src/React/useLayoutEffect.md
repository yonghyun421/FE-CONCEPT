# useLayoutEffect

useLayoutEffect는 useEffect와 비슷한 역할을 실행해준다.
useEffect와 마찬가지로 componentDidMount 혹은 componentWillMount의 라이프 사이클 역할을 수행할 수 있다.

react 공식문서에 따르면

> Tip
> If you're migration code from a class component, note useLayoutEffect fires in the same phase as componentDidMount and componentDidUpdate. However, we recomment starting with useEffect first and only trying useLayoutEffetc if that causes a problem.

우선적으로 useEffect를 사용해보다가 만약 문제가 생길시 useLayoutEffect를 사용하라고 한다.

### useEffect

Component -> State Changes -> Component Renders -> Rendered Component is Printed on Screen -> useEffect runs

### useLayoutEffect

Component -> State Changes -> Component Renders -> useLayoutEffect runs -> Rendered Component is Printed on Screen

useEffect는 기본적으로 component가 화면에 렌더링이 이뤄진 후에 동작한다.
그렇지만 useLayoutEffect의 경우, 상태 변화후 component가 화면이 렌더링 되기 이전에 실행된다.

한가지 더 차이점은 비동기적으로 처리되는 useEffect와 달리 useLayoutEffect는 동기적으로 처리된다.

## useLayoutEffect를 언제 사용할까?

화면 blinking(깜빡깜빡 거리는 현상)을 접할 때 우리는 useLayoutEffect를 사용하기 좋다.
왜냐하면 화면이 렌더링 되기 이전에 동기적으로 내부 함수들을 실행시켜주기 때문에, rendering 이후에 비동기적으로 동작하며 blinking 현상을 야기하는 useEffect의 취약점을 보완해줄 수 있기 때문이다.

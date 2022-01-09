# 열거 타입(Enum)

열거 타입은 일정 수의 상수로 구성된 집합을 만든다. 상수로는 숫자나 문자열을 사용할 수 있다.
높은 가독성이 필여하고 일정 집합에만 값을 배정하고 싶으며, 데이터 저장을 효율적으로 하고 싶은 상황에선 열거 타입을 사용하는 것이 좋다.
타입스크립트는 enum 키워드로 일정 수의 상수로 구성된 집합을 만든다. 열거를 이용해 요일을 표시하는 타입을 선언해보면

```ts
enum Weekdays {
  Monday = 1,
  Tuesday = 2,
  Wednesday = 3,
  Thursday = 4,
  Friday = 5,
  Saturday = 6,
  Sunday = 7,
}
```

Weekdays 타입이 가질 수 있는 값은 수가 정해져 있다. 각 enum 멤버를 숫자값으로 초기화하고, 각 요일은 점 표기법으로 참조할 수 있다.

```ts
enum Weekdays {
  Monday = 1,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday,
}
```

다음과 같이 월요일을 1로 초기화하고 나머지 값을 따로 할당해주지 않으면 자동으로 1씩 증가된 값이 할당된다.

## 열거 타입을 사용해 리팩토링하기

```ts
function convertTemperature(temp: number, fromTo: string): number {
  return 'FtoC' === fromTo
  (temp - 32) * 5.0 / 9.0:	//	화씨에서 섭씨로 변환
  temp * 9.0 / 5.0 + 32;	//	섭씨에서 화씨로 변환
}
```

convertTemperature()는 화씨 온도를 섭씨 온도로 또는 그 반대로 변환하는 함수다. 이 함수를 열거 타입을 이용하여 리팩토링하면

```ts
enum Direction {
  FtoC,
  CtoF,
}
function convertTemperature(temp: number, fromTo: Direction): number {
  return Direction.FtoC === fromTo
    ? ((temp - 32) * 5.0) / 9.0
    : (temp * 9.0) / 5.0 + 32;
}
```

# 제네릭(Generic)

타입스크립트 제네릭을 사용하면 다양한 타입을 지원하는 함수를 작성할 수 있다. 즉 제네릭을 사용해 함수를 선언하면, 함수의 호출자가 나중에 구체적인 타입을 지정할 수 있다. 타입스크립트는 제네릭 함수, 클래스 또는 인터페이스를 작성할 수 있다. 제네릭 타입은 T in Array<T\>와 같이 임의의 문자로 표시한다. 배열 내 요소의 타입을 선언할 때 <\> 기호 안에 해당 타입을 작성한다.

```ts
let lotteryNumbers: Array<number>;
```

제네릭은 코드를 사용할 때 타입을 선언함으로써 여러 종류의 타입 값을 처리할 수 있게 해준다.

- 배열 요소의 타입을 정의한다

```ts
const someValues: number[];
```

- 제네릭 Array<> 키워드로 <> 기호 안에 타입 파라미터를 작성한다.

```ts
const someValues: Array<number>;
```

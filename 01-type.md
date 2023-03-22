# 변수

- 값의 선언(`var`)과 할당(`=`)

# 데이터 타입

### 1-1. primitive type

- `Number, String, Boolean, null, undefined, Symbol`
- stack memory
- 값의 주소를 직접 저장
- 같은 값은 오직 하나만 존재!(불변값)

### 1-2. reference type

- `Object - Array, Function, RegExp, Set/WeakSet, Map/WeakMap (...)`
- heap memory
- 값의 주소를 저장하되, 동적 할당을 위해 힙 메모리의 주소를 메모리에 저장하는 한 단계를 더 거치게 됨.

```js
var obj = {
  x: '매우 큰 용량을 차지하는 데이터',
  y: 3,
  arr: ['매우 큰 용량을 차지하는 데이터', 3],
};
```

### 변수 복사

- 값의 주소를 복사
- 기본형의 값을 바꿨을 때는 바로 바뀌는 반면, 참조형의 값을 바꿨을 때는 여전히 똑같은 객체를 바라보고 있음.
- 원본 객체의 값이 같이 바뀌는 것을 막기 위해 매번 새로운 객체를 만들어 `불변 객체`로 만들게 되는 것.

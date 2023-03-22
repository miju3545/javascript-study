# 1. 실행 컨텍스트

- 코드(함수)가 실행되는 환경(배경)
- 코드의 종류
  ☑️ 전역 공간 - 전역 컨텍스트
  ☑️ 함수 - 함수 컨텍스트
  ☑️ eval(제외)
  ☑️ module - 모듈 컨텍스트

💡 오직 함수에 의해서만 컨텍스트를 구분할 수 있음.
💡 if/for/switch/while는 `블록 스코프`로 별개의 독립된 공간으로서의 역할을 하지만 별개의 실행 컨텍스트를 가지고 있지는 않음.

## 2. call stack

- 현재 어떤 함수가 동작 중이고, 다음에 어떤 함수가 호출될 예정인지 등을 제어하는 자료 구조

```js
var a = 1;
function outer() {
  console.log(a); #1

  function inner() {
    console.log(a); #2
    var a = 3;
  }

  inner()

  console.log(a) #3
}

outer();
console.log(a) #4
```

## 3. 구조

```
ExecutionContext
├── inner
│   └── VaribableEnvironment: 식별자 정보 수집, 값의 변화가 실시간으로 반영X
│   └── LexicalEnvironment: 각 식별자의 값(데이터) 추적, 값의 변화가 실시간으로 반영
│        └── environmentRecord: 현재 실행 컨텍스트 내부의 식별자 정보(👉🏻 hoisting)
│        └── outerEnvironmentReference: 현재 실행 컨텍스트의 외부 환경을 참조하는 정보 (👉🏻 scope chain)
│   └── ThisBinding
├── outer
│   └── VaribableEnvironment
│   └── LexicalEnvironment 📌
│        └── environmentRecord
│        └── outerEnvironmentReference 📌
│   └── ThisBinding
└── global
    └── VaribableEnvironment
    └── LexicalEnvironment 📌
         └── environmentRecord
         └── outerEnvironmentReference
    └── ThisBinding
```

☑️ VaribableEnvironment, LexicalEnvironment: 현재 환경과 관련된 식별자(선언된 변수) 정보가 담김

### 3-1) Lexical Environment

- 어휘적/사전적 환경
- 실행 컨텍스트 A의 (내외부)환경 정보가 담겨 있는 사전
- 즉, 실행 컨텍스트를 구성하는 환경 정보들을 모아 사전처럼 구성한 객체.

```bash
내부식별자 a = undefined; # environmentRecord
내부식별자 b = 20;
외부 정보 D 참조 # outerEnvironmentReference
```

#### 3-1-1) environmentRecord

- context 생성 시 젤 먼저!
- 현재 컨텍스트의 식별자 정보를 모아서 environmentRecord에 담는 과정(`hoisting`)
- hoist: (식별자 정보를) 끌어올리다 (to the top).

- 호이스팅 전

```js
console.log(a());
console.log(b());
console.log(c());

function a() {
  return 'a';
}

var b = function bb() {
  return 'bb';
};

var c = function () {
  return 'c';
};
```

- 호이스팅 후

```js

function a() {
  return "a";
}

var b; # undefined
var c; # undefined

--------- environmentRecord 영역: 선언된 식별자 위로

console.log(a());
console.log(b()); // TypeError - not a function
console.log(c()); // TypeError - not a function

b = function bb() {
  return "bb"
}

c = function() {
  return "c"
}

```

#### 3-1-2) outerEnvironmentReference

- 실행 컨텍스트 외부 환경(Outer LexicalEnvironment)에 대한 참조
- 즉, 현재 문맥과 관련 있는 외부 식별자 정보
- `scope chain` 현상은 outerEnvironemtnReference 때문에 발생하는 현상
  - scope: 변수의 유효 범위로, 실행 컨텍스트의 내부에서만 존재하는 값에만 접근할 수 있고, outerEnvironmentReference를 통해서만 외부 값을 참조할 수 있음.
  - inner의 environmentRecord -> inner의 outerEnvironmentReference -> outer의 environmentRecord -> outer의 outerEnvironmentRecord -> global의 environmentRecord
  - 가장 가까운 자신으로부터 점점 멀리 있는 스코프를 찾아 나가는 것.

#### 실행 과정

```
Execution context
├── ✅ global context 생성
│   └── 변수 a 선언
│   └── 함수 outer 선언
│   └── 변수 a에 `1` 할당
│   └── 함수 outer 호출
│        └── ✅ outer context 생성
│           └── 함수 inner 선언
│           └── global context에서 a 탐색 후 `1` 출력
│           └── 함수 inner 호출
│                └── ✅ inner context 생성
│                     └── 변수 a 선언
│                     └── inner context에서 a 탐색 후 `undefined` 출력
│                     └── 변수 a에 `3` 할당
│                └── inner context 종료 후 call stack 제거
│           └── global context에서 a 탐색 후 `1` 출력
│        └── outer context 종료 후 call stack에서 제거
│   └──  global context에서 a 탐색 후 `1` 출력
└── global context 종료 후 call stack에서 제거
```

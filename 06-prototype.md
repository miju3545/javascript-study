# 프로토타입

☑️ prototype
☑️ [[Prototype]]
☑️ constructor

## 생성자 함수와 인스턴스 그리고 prototype

```
Constructor - prototype : Construtor.prototype
    ⬇ (new)
 instance - [[Prototype]]: instance[[Prototype]]

```

```bash
[1,2,3].constructor
[1,2,3].[[Prototype]].constructor
Array.prototype.constructor
Arrray

```

## primitive, reference와 메소드

- 원시 타입의 값은 메소드를 호출하는 순간에 임시로 해당 타입의 인스턴스를 만들어서 그 메소드를 실행하고 그 결과를 얻음과 동시에 인스턴스를 폐기

- 참조 타입의 값은 처음부터 인스턴스이기 때문에 메소드를 호출한 순간에 임시로 인스턴스를 생성하고 폐기하는 일은 없음!

- 생성자 함수의 prototype property의 메소드를 [[Prototype]]이라는 연결통로에 의해 접근

- null, undefined를 제외한 모든 데이터 타입은 생성자 함수가 존재하고, 각 생성자 함수의 프로토타입에는 데이터 타입에만 해당하는
  전용 메소드들이 정의되어 있다.

## 인스턴스와 Constructor.prototype

인스턴스에서 생성자 함수의 prototype 메소드에 접근하려면

1.  `instance.__proto__`
2.  `Object.getPrototypeOf(instance)`

```js
Constructor
instance.__proto__.constructor (__proto__ 는 생략 가능)
instance.constructor
Object.getPrototypeOf(instance).constructor
Constructor.prototype.constructor
```

```js
function Person(n, a) {
  this.name = n;
  this.age = a;
}

const roy = new Person('로이', 30);
const royClone1 = new roy.__proto__.constructor('로이_클론1', 10);
const royClone2 = new roy.constructor('로이_클론2', 20);
const royClone3 = new Object.getPrototypeOf(roy).constructor('로이_클론3', 30);
const royClone4 = new Person.prototype.constructor('로이_클론4', 13);
```

## 메소드 상속 및 동작 원리

```js
function Person(n, a) {
  this.name = n;
  this.age = a;
}

Person.prototype.setOlder = function() {
  this.age += 1;
}

Person.prototype.getAge = function() {
  return this.age;
}

Person.isPerson = function(obj) {
   return obj instanceof this
}

# 한번만 만든 메소드를 계속 참조할 수 있다!(메모리 용량 최적화)

```

## Prototype chaining

- 프로토타입은 모두 객체이므로 모든 데이터타입은 한결같이 이와 동일한 구조로 Object.prototype 까지 prototype chain으로 연결되어 있다.

```
Constructor - prototype : Construtor.prototype
    ⬇ (new)
 instance - [[Prototype]]: instance[[Prototype]]

Object.prototype
│   └── static members
│       └── 객체 전용 메소드X
│       └── 객체 생성자 함수 내에 직접 메소드를 정의하는 방법으로 특정 타입의 메소드 관리 static methods/properties
│   └── prototype
│       └── method1
│       └── method2
│       └── method3
│       └── (...)
│
└── instance
     └── [[Prototype]]
```

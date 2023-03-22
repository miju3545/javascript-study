# ThisBinding

- this: 함수 실행 주체
- 실행 컨텍스트가 생성될 때 this를 바인딩한다.
- 즉, 함수가 호출될 때 this가 결정된다!
- 동적으로 바인딩되기 때문에 호출하는 방식(런타임)에 따라 다르다
  ☑️ 전역 공간에서 `window(browser)/global(node)`
  ☑️ 함수 호출 시 `window/global`
  ☑️ 메서드 호출 시 "." 앞에, 메소드 호출 주체
  ☑️ callback 호출 시, 기본적으로는 함수 내부에서와 동일(window/global) but 제어권을 가진 함수가 콜백의 this를 지정해둔 경우도 있다(eventListener)
  ☑️ 생성자 함수 호출 시, 인스턴스 객체 자체가 this

- arrow function - 바로 위의 context에 있는 this를 그래도 가져다 쓴다.
- class/instance와 상관없이 객체와 관련된 동작이기만 하면 메소드로 인정.

```bash
var a = 10;
var obj = {
 a: 20,
 b: function() {
   console.log(this.a); # 20

   function c() {
     console.log(this.a) # 10
   }

   c()
 }
}

obj.b()
```

## this binding 우회법(암묵적인 this 바인딩)

```bash
var a = 10;
var obj = {
 a: 20,
 b: function() {
   var self = this;
   console.log(this.a); # 20

   function c() {
     console.log(self.a) # 20
   }

   c()
 }
}

obj.b()
```

## 명시적인 this 바인딩

- func.call(thisArg, arg1[, arg2[, ...]])
- func.apply(thisArg, [argsArray])
- func.bind(thisArg, arg1[, arg2[, ...]])

```bash
function a(x, y, z) {
  console.log(this, x, y, z);
}

var b = {
  bb: 'bb',
};

a.call(b, 1, 2, 3);
a.apply(b, [1, 2, 3]);

var c = a.bind(b);
c(1, 2, 3);

var d = a.bind(b, 1, 2);
d(3);
```

```bash
var callback = function () {
  console.dir(this);
};

var obj = {
  a: 1,
  b: function (cb) {
    cb() # global
    cb.call(this); # obj
  },
};

obj.b(callback);

setTimeout(callback, 1000); # global
setTimeout(callback.bind(obj), 1000); # obj

```

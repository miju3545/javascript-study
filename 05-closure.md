# 클로저

- 내부함수와 lexicalEnvironment의 조합
- A closure is the combination of a function bundled together with references to its surrounding state(the lexical environment).
- 즉, 실행 컨텍스트 A 내부에서 함수 B를 선언
- `B의 outerEnvironmentReference === A의 environmentRecord`
- 컨텍스트 A에서 선언한 변수를 내부함수 B에서 참조할 경우에 발생하는 특별한 현상
- 컨텍스트 A에서 선언한 변수 a를 참조하는 내부 함수 B를 A의 외부로 전달할 경우, A가 종료된 이후에도 a가 사라지지 않는 현상
- 지역 변수가 함수 종료 후에도 사라지지 않게 할 수 있다. 함수 종료 후에도 사라지지 않는 지역 변수를 만들 수 있다.

```bash

var outer = function() {
  var a = 1;
  var inner = function() {
    console.log(++a);
  }

  inner()
}

outer()

```

```bash

function user(_name) {
  var _logged = true;
  # 함수 종료 후에도 사라지지 안고 값을 유지하는 변수
  # 외부로부터 내부 변수 보호(갭슐화)

  return {
    get name() {return _name},
    set name(v) {_name = v},
    login() {_logged = true},
    logout() {_logged = false},
    get status() {
      return _logged ? "login" : "logout"
    }
  }
}

var xx = user("xx")
console.log(xx.name);
console.log(xx.login())
console.log(xx.status)
console.log(xx.logout());
console.log(xx.status)
```

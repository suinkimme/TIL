## 명시적 this binding

> ### call, apply, bind

### call 사용 방법

이미 할당되어있는 다른 객체의 함수/메서드를 호출하는 해당 객체에 재할당할 때 사용함

```javascript
var func = function (a, b, c) {
  console.log(this, a, b, c);
};

// no binding
func(1, 2, 3); // Window { ... } 1 2 3

// 명시적 binding
func.call({ x: 1}, 1, 2, 3); // { x: 1 } 1 2 3
```
```javascript
var obj = {
  a: 1,
  method: function (x, y) {
    console.log(this.a, x, y);
  }
};

// no binding
obj.method(2, 3); // 1 2 3

// 명시적 binding
obj.method.call({ a: 4 }, 5, 6); // 4 5 6
```

### apply 사용 방법

`call`과 동일한 기능을 한다. 하지만 재할당 할 때 함수의 인자를 배열로 작성해야 한다.

> `call`과 동일한데 왜 만들었을까? : 가변적인 개수의 인자를 함수에 전달할 때 편리하다.

```javascript
var func = function (a, b, c) {
  console.log(this, a, b, c);
};

// no binding
func(1, 2, 3); // Window { ... } 1 2 3

// 명시적 binding
func.apply({ x: 1 }, [1, 2, 3]); // { x: 1 } 1 2 3
```
```javascript
var obj = {
  a: 1,
  method: function (x, y) {
    console.log(this.a, x, y);
  }
};

// no binding
obj.method(2, 3); // 1 2 3

// 명시적 binding
obj.method.apply({ a: 4 }, [5, 6]); // 4 5 6
```
## 상황에 따라 달라지는 `this`

### 런타임

> 코드가 실행되는 환경

1. 노드
    - 전역 환경 : global 객체
2. 브라우저
    - 전역 환경 : window 객체

#### <노드 환경>

```javascript
console.log(this); // global 객체가 출력됨
console.log(global); // global 객체가 출력됨
console.log(this === global); // true
```
파일 실행으로는 확인할 수 없음, REPL(Read-Eval-Print Loop=대화형 쉘 환경)로 확인 바람

#### <브라우저>

```javascript
console.log(this); // window 객체가 출력됨
console.log(window); // window 객체가 출력됨
console.log(this === window); // true
```

---

### 메서드로서 호출할 때 그 메서드 내부에서의 `this`

#### 함수 vs 메서드

함수와 메서드, 상당히 비슷해보이지만 엄연한 차이가 있음, 기준은 `독립성`이고, 함수는 그 자체로 독립적인 기능을 수행함.

```javascript
함수명();
```

메서드는 자신을 호출한 대상 객체에 대한 동장을 수행함.

```javascript
객체.함수명();
```

- 함수 : `this` -> 전역 객체
- 메서드 : `this` -> 호출의 주체

---

### `this`의 할당

#### <CASE 1>

```javascript
var func = function (x) {
  console.log(this, x);
}
func(1); // Window { ... } 1
```
#### <CASE 2>

```javascript
var func = function (x) {
  console.log(this, x);
}

var obj = {
  method: func,
};
obj.method(2); // { method: [Function method] } 2
```

- 호출 주체를 명시할 수 있기 때문에 `this`는 해당 객체(obj)를 의미함
- 함수로서의 호출과 메서드로서의 호출 구분 기준 : `,`, `[]`

---

### 메서드 내부에서의 `this`

> 호출을 누가 했는지에 대한 정보가 담긴다.

```javascript
var obj = {
  methodA: function () { console.log(this); },
  inner: {
    methodB: function() { console.log(this); }
  }
};

obj.methodA(); // this === obj
obj['methodA'](); // this === obj

obj.inner.methodB(); // this === obj.inner
obj.inner['methodB'](); // this === obj.inner

/* 호출의 다른 경우의 수는 생략함 */
```

---

### 함수로서 호출할 때 그 함수 내부에서의 `this`

함수 내부에서의 `this`

- 어떤 함수를 함수로서 호출할 경우, this는 지정되지 않는다. (호출 주체 없음)
- 실행 컨텍스트를 활성화할 당시 this가 지정되지 않은 경우, `this`는 전역 객체를 의미함
- 함수를 함수로서 '독립적으로' 호출할 때는 `this는 항상 전역객체를 가리킨다`는 것을 명심할 것

메서드의 내부함수에서의 `this`

> 예외는 없다. 메서드의 내부라고 해도, 함수로서 호출된다면 `this`는 전역 객체를 의미한다.

```javascript
var obj1 = {
  outer: function () {
    console.log(this); // this === obj1
    var innerFunc = function () {
      console.log(this);
    };
    innerFunc(); // console.log => Window { ... }

    var obj2 = {
      innerMethod: innerFunc,
    };
    obj2.innerMethod(); // this === obj2
  },
};

obj1.outer();
```
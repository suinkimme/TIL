## 콜백함수 this 바인딩 예제로 찍어먹기

```javascript
var obj = {
  vals: [1, 2, 3],
  logValues: function (v, i) {
    console.log(this, v, i);
  },
};

// method로서 호출
obj.logValues(1, 2); // { vals: [...], logValues: [Function ...] } 1 2

// 함수로서 호출
[4, 5, 6].forEach(obj.logValues);
// Window { ... } 4 0
// Window { ... } 5 1
// Window { ... } 6 2
```

여기서 작성된 함수로서 호출을 풀어서 작성하면 아래와 같다.

```javascript
[4, 5, 6].forEach(function (v, i) {
  console.log(this, v, i);
});
```

`obj.logValues`로 메서드 형태처럼 보이나, 결국은 함수 자체를 넣은 것과 같다.<br>
메서드로서의 "호출"이라고 할 때는 "호출"을 해야하는 것이지, 메서드 형태지만 매개 변수로 넘겨 줬다고 해서 호출한 것과 같지 않다.

## 콜백 함수 내부의 this에 다른 값 바인딩하기

### 예제 1. 전통적인 방법

```javascript
var obj1 = {
  name: "obj1",
  func: function () {
    var self = this; // (1)
    return function () {
      console.log(self.name); // (2)
    };
  },
};

var callback = obj1.func();
setTimeout(callback, 1000);
```

함수를 반환 했기 때문에 `callback` 변수에 `obj.func`의 결과를 담았다고 할 수 있다. 위 코드를 풀어서 작성하면 아래와 같다.

```javascript
// ...

var callback = function () {
  console.log(self.name);
};
setTimeout(function () {
  console.log(self.name);
}, 1000);
```

`closure`라는 방식 덕분에 함수가 끝났음에도 불구하고, 참조하고 있는 함수 영역은 `(2)` 부분인데, 함수에서 사용하고 있는 `self`라는 변수는 바깥에 있으니, 바깥에서 참조하고 있는 `self`가 아직 살아있다.

### 예제 2. 전형적인 하드 코딩(!!!절대 피할 것!!!)

```javascript
var obj1 = {
  name: "obj1",
  func: function () {
    console.log(obj1.name);
  },
};

setTimeout(obj1.func, 1000);
```

결과만을 위한 코딩 이런 방법은 지양해야 한다. `this`의 장점은 전부 내다버린 방법이라 할 수 있다.

### 예제 3. call(or apply), 그 외

```javascript
var obj1 = {
  name: "obj1",
  func: function () {
    var self = this;
    return function () {
      console.log(self.name);
    };
  },
};

var obj2 = { name: "obj2" };
var callback = obj1.call(obj2);
setTimeout(callback, 2000); // 출력: obj2
```

obj2를 this 바인딩할 수 있게 call로 넘겨준 것

```javascript
var obj1 = {
  name: "obj1",
  func: function () {
    var self = this;
    return function () {
      console.log(self.name);
    };
  },
};

var obj3 = {
  name: "obj3",
  func: obj1.func,
};
```

뭐 이런 방법도 있긴 함

### 예제 4. bind (가장 효율적인 방법)

함수를 즉시 실행 함수가 아닌, `this`를 바인딩해서 새로운 함수를 return 한다.

> 함수를 만들어 놓을 때 유용하다.

```javascript
var obj1 = {
  name: "obj1",
  func: function () {
    console.log(this.name);
  },
};

var callback = obj1.func.bind(obj1);
setTimeout(callback, 2000); // 출력: obj1

var obj2 = { name: "obj2" };
setTimeout(obj1.func.bind(obj2), 1500); // 출력: obj2
```

`bind`해주지 않으면 전역 객체가 출력된다.(메서드 호출 처럼 보이지만 콜백 함수로 사용했기 때문에 아니다!! 헷갈리지마라!!!)<br>
`obj1`을 `bind` 해줌으로써 콜백 함수로 호출할 때도 `this = obj1`이 되어버린다!!
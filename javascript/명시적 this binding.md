## 명시적 this binding

> ### call, apply, bind

- call과 apply는 즉시 실행 함수다.

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

### call 응용하기

> 생성자 함수에 반복되는 코드를 줄여주는 예시임.

#### <적용 전>

```javascript
function Student(name, gender, school) {
  this.name = name;
  this.gender = gender;
  this.school = school;
}

function Employee(name, gender, company) {
  this.name = name;
  this.gender = gender;
  this.company = company;
}

var kd = new Student('길동', 'male', '서울대');
var ks = new Employee('길순', 'female', '삼성');
```

#### <적용 후>

```javascript
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
}

function Student(name, gender, school) {
  Person.call(this, name, gender);
  this.school = school;
}

function Employee(name, gender, company) {
  Person.call(this, name, gender);
  this.company = company;
}

var kd = new Student('길동', 'male', '서울대');
var ks = new Employee('길순', 'female', '삼성');
```

### bind 사용 방법

> call, apply와는 다르게 즉시 호출되지 않는다.

- 함수에 this를 '미리' 적용한다.
- 부분 적용 함수가 가능하다.
- `name` 프로퍼티로 추적하기가 매우 쉽다.

```javascript
var func = function (a, b, c, d) {
  console.log(this, a, b, c, d);
}

func(1, 2, 3, 4); // Window { ... } 1 2 3 4

// 함수에 this를 미리 적용
var bindFunc1 = func.bind({ x: 1 });
bindFunc1(5, 6, 7, 8); // { x: 1 } 5 6 7 8

// 부분 적용 함수 구현
var bindFunc2 = func.bind({ x: 1 }, 9, 10);
bindFunc2(11, 12); // { x: 1 } 9 10 11 12

// name 프로퍼티
console.log(func.name); // func
console.log(bindFunc1.name); // bound func
console.log(bindFunc2.name); // bound func
```

### 상위 컨텍스트의 this를 내부함수나 콜백함수로 전달하기

self 변수에 this를 넣어 우회할 수 있으나, call, apply, bind를 사용하면 깔끔하게 처리 가능하다.

#### <call 예제>

```javascript
var obj = {
  outer: function () {
    console.log(this);
    var innerFunc = function () {
      console.log(this);
    };

    // call을 이용해서 즉시 실행하며 this를 넘겨줌
    innerFunc.call(this);
  }
};

obj.outer();
```

#### <bind 예제>

call, apply 보다 많이 쓰인다.

```javascript
var obj = {
  outer: function () {
    console.log(this);
    var innerFunc = function () {
      console.log(this);
    }.bind(this);

    // bind를 사용하면 그냥 함수를 호출하듯 호출해도 된다.
    innerFunc();
  }
};

obj.outer();
```

#### 콜백 함수 this 입맛대로 변경하기

```javascript
var obj = {
  logThis: function () {
    console.log(this);
  },
  logThisLater1: function () {
    setTimeout(this.logThis, 500); // console 이 출력되지 않음.
  },
  logThisLater2: function () {
    setTimeout(this.logThis.bind(this), 1000); // console 이 출력됨
  },
};

obj.logThisLater1();
obj.logThisLater2();
```

#### this우회, call, apply, bind 보다 편리한 방법

화살표 함수는 실행 컨텍스트 생성 시, this를 바인딩하는 과정이 없다.

```javascript
var obj = {
  outer: function () {
    console.log(this); // { outer: [function outer ] }
    var innderFunc = () => {
      console.log(this);
    };
    innerFunc(); // { outer: [function outer ] }
  }
};

obj.outer();
```

> ## 레거시 코드도 있기 때문에 여러 기법들을 알고는 있자!
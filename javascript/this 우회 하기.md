## 내부 스코프에서 호출된 함수의 `this`를 우회하는 법

### 변수를 활용하는 방법

내부 스코프에 이미 존재하는 `this`를 별도의 변수에 할당하는 방법

```javascript
var obj1 = {
  outer: function () {
    console.log(this); // obj1

    // AS-IS
    var innerFunc1 = function () {
      console.log(this); // 전역 객체
    }
    innerFunc1();

    // TO-BE
    var self = this;
    var innerFunc2 = function () {
      console.log(self); // obj1
    }
    innerFunc2();
  }
};

obj1.outer();
```

### 화살표 함수(=`this`를 바인딩하지 않는 함수)

ES6에서 처음 도입된 화살표 함수는, 실행 컨텍스트를 생성할 때 `this` 바인딩 과정 자체가 없다.(따라서, `this`는 이전의 값의 상위값이 유지된다.=화살표 함수 안에서 `this`가 어디서 호출되었든 변경되지 않고 상위 스코프의 `this`를 따라간다.)

- ES6에서는 함수 내부에서 `this`가 전역 객체를 바라보는 문제 때문에 화살표 함수를 도입했다.

> 일반 함수와 화살표 함수의 가장 큰 차이점은 **this binding 여부**다.

```javascript
var obj = {
  outer: function () {
    console.log(this); // obj
    var innerFunc = () => {
      console.log(this); // obj
    };
    innerFunc();
  },
};

obj.outer();
```

### 콜백 함수 호출 시 그 함수 내부에서의 `this`

콜백 함수도 함수이기 때문에, `this`를 잃어 버린다. **단, 콜백 함수에 별도 `this`를 지정한 경우는 예외적으로 그 대상을 참조한다.**

```javascript
// 별도 지정 없음 : 전역 객체
setTimeout(function () { console.log(this); }, 300);

// 별도 지정 없음 : 전역 객체
[1, 2, 3, 4, 5].forEach(function (x) {
  console.log(this, x);
});

// addListener 안에서의 this는 항상 호출한 주체의 element를 return 하도록 설계 되었음.
// 따라서 this는 button을 의미함
document.body.innerHTML += '<button id="a">클릭</button>';
document.body.querySelector('#a').addEventListener('click', function (e) {
  console.log(this, e);
});
```

- setTimeout 함수, forEach 메서드는 콜백 함수를 호출 할 때 대상이 될 `this`를 지정하지 않으므로, `this`는 곧 window 객체가 된다.
- addEventListener 메서드는 콜백 함수 호출 시, 자신의 `this`를 상송하므로, `this`는 addEventLisener의 앞부분(button)

### 생성자 함수 내부에서의 `this`

> 생성자, 구체적인 인스턴스를 만들기 위한 일종의 틀

```javascript
var Cat = function (name, age) {
  this.bark = '야용';
  this.name = name;
  this.age = age;
};

var choco = new Cat('초코', 7); // this : choco
var nabi = new Cat('나비', 5); // this : nabi
```
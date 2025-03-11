> 특정 실행 컨텍스트가 생성되는 시점 = 콜 스택의 맨 위에 쌓이는 순간 = 실행할 코드에 실행 컨텍스트가 관여하게 되는 시점

## 실행 컨텍스트 객체의 실체(=담기는 정보)

> VE와 LE는 동일한 것이다. 변경 사항을 유지 하냐, 유지하지 않냐 차이다.

- *record = 식별자
- ThisBinding은 this가 function 안에서 어떤 기능을 할 지 결정해주는 역할

1. VariableEnvironment

    - 현재 컨텍스트 내의 식별자 정보(=record)를 갖고있다.
      - `var a = 3`
      - 위으 경우, `var a`를 의미
    - 외부 환경 정보(=outer)를 갖고 있다.
    - 선언 시점 LexicalEnvironment의 **snapshot** -> 생길 때 모습을 그대로 간직함

2. LexicalEnvironment

    - VariableEnvironment와 동일하지만, 변경사항을 실시간으로 반영한다.

3. ThisBinding

    - this 식별자가 바라봐야할 객체

## VariableEnvironment, LexicalEnvironment의 개요

### VE vs LE

이 두가지는 담기는 항목은 완벽하게 동일하다. 그러나 스냅샷 유지 여부는 다음과 같이 다르다.

- VE : 스냅샷을 유지한다.
- LE : 스냅샷을 유지하지 않는다. 즉, 실시간으로 변경사항을 계속해서 반영한다.

> ### 결국, 실행 컨텍스트를 생성할 때, VE에 정보를 먼저 담은 다음, 이를 그대로 복사해서 LE를 만들고 이후에는 주로 LE를 활용한다.

### 구성 요소(VE, LE는 서로 같다.)

1. VE, LE 모두 동일하며, `environmentRecord`와 `outerEnvironmentReference`로 구성
2. environmentRecord(=record)
    - 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다.
    - 함수에 지정된 매개변수 식별자, 함수 자체, var로 선언된 변수 식별자 등
3. outerEnvironmentReference(=outer)

## LexicalEnviroment - environmentRecord(=record)와 호이스팅

### 개요

1. 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장(수집) 된다.
2. 수집 대상 정보 : 함수에 지정된 매개변수 식별자, 함수 자체, var로 선언된 변수 식별자 등
3. 컨텍스트 내부를 처음부터 끝까지 **순서대로** 훑어가며 수집

> 순서대로 **수집**한다고 했지, 코드가 **실행**되지는 않는다.

### 호이스팅 (**식별자를 수집할 때 호이스팅 개념이 나온다.**) -> JS 엔진이 변수들을 수집하는 과정

1. 변수 정보 수집을 모두 마쳤더라도 아직 실행 컨텍스트가 관여할 코드는 실행 전의 상태다. (JS 엔진은 코드 실행 전 이미 모든 변수 정보를 알고 있는 것)
2. 변수 정보 수집 과정을 이해하기 쉽게 설명한 **'가상 개념'**

> 가상개념은 실제로 그렇지 않더라도 사람이 이해하기 쉬운 말로 풀어 표현했다는 것을 의미한다.

### 호이스팅 규칙

#### 1. 매개변수 및 변수는 선언부를 호이스팅 한다.

<적용 전>
```javascript
function a(x) {
    console.log(x);
    var x;
    console.log(x);
    var x = 2;
    console.log(x);
}

a(1);
```

<매개변수 적용>
```javascript
function a() {
    var x = 1;
    console.log(x);
    var x;
    console.log(x);
    var x = 2;
    console.log(x);
}

a(1);
```

<호이스팅 적용>
```javascript
function a() {
    var x;
    var x;
    var x;
    x = 1;
    console.log(x); // 1
    console.log(x); // 1
    x = 2;
    console.log(x); // 2
}

a(1);
```

#### 2. 함수 선언은 전체를 호이스팅한다.

<적용 전>
```javascript
function a() {
    console.log(b);
    var b = 'bbb';
    console.log(b);
    function b() { }
    console.log(b);
}

a();
```

<호이스팅 적용>
```javascript
function a() {
    var b;
    function b() { }
    console.log(b) // [function]
    b = 'bbb';
    console.log(b); // bbb
    console.log(b); // bbb
}

a();
```
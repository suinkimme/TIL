## LexicalEnvironment - 스코프, 스코프 체인, outerEnvironmentReference(=outer)

### 주요 용어

1. 스코프

    - 식별자에 대한 유효범위를 의미한다. (변수가 어디까지 영향을 끼칠 수 있는가)
    - 대부분 언어에 존재함, JS도 당연히 있음

2. 스코프 체인

    - outer는 현재 호출된 함수가 선언될 당시(이 말이 중요함)의 LexicalEnvironment를 참조한다. (그 당시 환경 정보를 저장한다.)
    - 예를 들어, `A함수 내부에 B함수 선언 -> B함수 내부에 C함수 선언(Linked List)`한 경우 어떻게 될까?
        - 결국 타고, 타고 올라가면 `전역 컨텍스트의 LexicalEnvironment를 참조`하게 된다.
    - 항상 outer는 오직 자신이 선언됨 시점의 LexicalEnvironment를 참조하고 있다. (가장 가까운 요소부터 차례대로 접근 가능 = 현재 실행 중인 함수나 블록에 없으면, 부모 스코프로 이동해서 찾는다는 의미)
    - 무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에게만 접근 가능

#### <적용 전>
```javascript
var a = 1;
var outer = function () {
    var inner = function() {
        console.log(a);
        var a = 3;
    }
    inner();
    console.log(a);
}
outer();
console.log(a);
```

#### <호이스팅 적용>
```javascript
// 부득이하게 call stack은 배열로 표기함.
// 설명: 마지막 index 위치의 컨텍스트가 실행할 코드에 영향을 미치고 있는 환경임

var a;
var outer;

a = 1;
outer = function () { /* 전역 컨텍스트의 환경을 참조함 */
    var inner;
    inner = function() { /* outer 함수 컨텍스트의 환경을 참조함 */
        var a;
        console.log(a); // undefined
        a = 3;
    }
    inner(); /* #2. callstack: [전역 컨텍스트, outer, inner] */
    console.log(a); // 1
}
outer(); /* #1. callstack: [전역 컨텍스트, outer] */
console.log(a); // 1
```
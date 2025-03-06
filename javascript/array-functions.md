각 함수의 콜백 함수 매개 변수 중 몇 가지는 생략되었다. 추후에 알아보도록 하겠다.

### 1. forEach
- 배열의 각 요소에 대해 한 번씩 제공된 콜백 함수를 실행하는 메서드
- 주어진 배열을 변경하지 않는다.
- 반환값이 없다. (`undefined` 반환)
```javascript
// 사용 예:
let numbers = [1, 2, 3, 4, 5];
numbers.forEach(function (item) {
  console.log(`numbers의 요소 => ${item}`)
});
```
```javascript
// 출력 예:
numbers의 요소 => 1
numbers의 요소 => 2
numbers의 요소 => 3
numbers의 요소 => 4
numbers의 요소 => 5
```
---
### 2. map
- 배열의 각 요소를 변환하여 새로운 배열을 반환하는 메서드
- 기존 배열은 변경하지 않는다.
- 반환값이 필수다. `return`을 생략하면 `undefined`가 반환된다.
```javascript
// 사용 예:
let numbers = [1, 2, 3, 4, 5];
let newNumbers = numbers.map(function (item) {
  return item * 2;
});

console.log(`newNumbers => ${newNumbers}`);
```
```javascript
// 출력 예:
newNumbers => [2, 4, 6, 8, 10]
```
---
### 3. filter
- 배열의 각 요소에 대해 주어진 조건을 검사해 `true`를 반환하는 요소만 모아 새로운 배열을 생성하는 메서드
- `return`을 생략할 경우 빈 배열 반환
- 전체 배열 순환으로 느릴 수 있음
```javascript
// 사용 예:
let numbers = [1, 2, 3, 4, 5];
let filteredNumbers = numbers.filter(function (item) {
  return item > 3;
});

console.log(`filteredNumbers => ${filteredNumbers}`);
```
```javascript
// 출력 예:
filteredNumbers => [4, 5]
```
---
### 4. find
- 배열에서 조건을 만족하는 첫 번째 요소를 반환하는 메서드
- 만족하는 조건이 없을 때 `undefined` 반환
- `return`을 생략할 경우 `undefined` 반환
- 첫 번째 요소 찾으면 순환 종료
```javascript
// 사용 예:
let numbers = [1, 2, 3, 4, 5];
let findNumber = numbers.find(function (item) {
  return item > 3;
});

console.log(`findNumber => ${findNumber}`);
```
```javascript
// 출력 예:
findNumber => 4
```
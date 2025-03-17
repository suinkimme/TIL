## `Python` 데코레이터(Decorator) 사용법

### 데코레이터란?

데코레이터(Decorator)는 함수나 메서드에 적용되어, 해당 함수나 메서드의 기능을 확장하거나 변경하는 역할을 한다. 기본적으로 함수를 인자로 받고, 또 다른 함수를 반환하는 **고차 함수(Hgiher-order function)다.**

### 작동 원리

#### <코드>

```python
def my_decorator(func):
  def wrapper():
    print('데코레이터가 추가한 내용')
    func()
    print('데코레이터가 추가한 내용')
  return wrapper

@my_decorator
def hello():
  print('Hello World!')

hello()
```

#### <결과>

```shell
데코레이터가 추가한 내용
Hello World!
데코레이터가 추가한 내용
```

### 다양한 예제

#### 타이머 데코레이터

```python
import time

def timer_decorator(func):
  def wrapper():
    start_time = time.time()
    result = func(*args, **kwargs)
    end_time = time.time()
    print(f'{func.__name__} 실행 시간: {end_time - start_time: .5f}초')
    return result
  return wrapper

@timer_decorator
def example_function():
  time.sleep(1) # 함수 스코프가 1초 뒤 종료됨

example_function() # 출력: example_function 실행 시간: 1.00293초
```

#### 로깅 데코레이터

_작동 원리_ 에서 제공한 예제와 동일하기 때문에 PASS

### 인자가 있는 데코레이터 예제

> 인자가 있는 데코레이터를 사용하면 더 유연하게 데코레이터를 사용할 수 있다.

#### 함수 호출 횟수 제한 데코레이터

```python
def limit_calls_decorator(max_calls):
  def decorator(func):
    calls = 0

    def wrapper(*args, **kwargs):
      nonlocal calls
      if calls < max_calls:
        calls += 1
        return func(*args, **kwargs)
      else:
        raise Exception('함수 호출 횟수 초과')
    
    return wrapper
  
  return decorator

@limit_calls_decorator(3)
def example_function():
  print('예제 함수 실행')

for i in range(5):
  try:
    example_function()
  except Exception as e:
    print(e)
```

#### 권한 확인 데코레이터

`user_perssmions` 리스트에 로그인한 사용자의 권한이 담겨 있다면, 검사 후 함수를 실행할 수 있음

```python
def permission_required_decorator(permission):
  def decorator(func):
    def wrapper(*args, **kwargs):
      user_permissions = ['read', 'write'] # 유저 권한 리스트
      if permission in user_permissions:
        return func(*args, **kwargs)
      else:
        raise Exception('권한이 없습니다.')
    return wrapper
  return decorator

@permission_required_decorator('read')
def read_function():
  print('읽기 함수 실행')

try:
  read_function()
except Exception as e:
  print(e)
```

## 데코레이터를 사용하는 이유

- 코드 재사용성 향상
    - 데코레이터 코드의 일부분을 여러 함수에서 공유할 수 있으며, 이로 인해 코드의 재사용성이 향상되며, 유지 보수도 용이해진다.
- 코드 가독성 향상
    - 기능을 확장하거나 변경하는 코드를 함수와 분리할 수 있다. 이로 인해 코드의 가독성이 향상되고, 이해하기 쉬워진다.
- 관심사 분리
    - 로깅이나 타이머와 같은 기능은 데코레이터를 사용하여 함수와 분리할 수 있다. 이로 인해 함수는 핵심 기능에만 집중할 수 있다.

## 데코레이터 주의 사항

데코레이터를 사용할 때 함수의 메타데이터가 변경될 수 있다. _원래 함수의 메타데이터를 보존할 수 있긴함_ <br>
또한, 데코레이터의 실행 순서에 주의해야 한다. 여러 데코레이터를 사용할 때 데코레이터가 적용되는 순서가 중요할 수 있다.

> 무분별한 사용은 코드의 복잡도를 높일 수 있으니 주의하자
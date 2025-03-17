## __name__이 무엇인가

> `__name__` 개념을 설명하기 앞서 파이썬 파일은 스크립트 파일 또는 파이썬 스크립트라고 불린다. 하지만 규모가 큰 프로젝트의 `.py` 파일들은 모듈(module) 또는 **패키지(package)**로 불리기도 한다.

`.py` 파일에는 `__name__`이라는 숨겨진 변수가 있다. 이 변수는 모듈의 이름을 갖고 있는 변수로, 현재 `.py` 파일의 이름을 갖고 있는 변수라는 의미다.

#### 예시

```python
# first.py

import second

my_name = __name__
print('first.py의 이름 : ', my_name) # 출력: first.py의 이름 : __main__
```

`first.py`에서 `second.py`를 import해 프로그램을 실행하면 다음과 같은 결과가 출력된다. 참고로, Python에서 import로 모듈을 가져오면 해당 스크립트 파일이 한 번 실행된다. 따라서 second 모듈을 가져오면 `second.py` 안의 코드가 실행된다는 것.

```shell
second.py의 이름 : second
first.py의 이름 : __main__
```

`first.py`를 기준으로 프로그램을 실행하면 `first.py`의 `__name__`은 `__main__`이고 `second.py`의 `__name__`은 `second`가 된다. 반대로, `second.py`를 기준으로 실행하게 되면 `second.py`의 `__name__`은 `__main__`이 된다.

## 결과

직접 실행된 파일의 `__name__`은 `__main__`이 되고 import되어 모듈로 사용된 파일의 `__name__`은 모듈 이름이 된다.

### 왜 사용하는걸까

Python은 최초로 시작한 스크립트 파일과 모듈의 차이가 없기 때문에 어떤 스크립트 파일이든 시작점도 될 수 있고, 모듈도 될 수 있다. 그래서 `__name__` 변수를 통해 현재 스크립트 파일이 시작점인지 모듈인지 판단한다.

#### 시작점이 맞는지 판단하는 코드

```python
if __name__ == '__main__':
    ...
```

### Python은 왜 시작점이 정해져 있지 않을까

Python은 처음 개발될 당시 Linux/Unix에서 사용하는 스크립트 언어 기반이었기 때문에 프로그램의 시작점이 따로 정해져 있지 않았다. _보통 Linux/Unix의 스크립트 파일은 한 개로 이루어진 경우가 많기 때문_
## MongoDB를 사용한 간단한 CRUD 구현하기

> Database는 크게 두 가지 종류로 나뉜다.

- RDBMS(SQL)
    - 행/열 생김새로 엑셀 데이터를 저장하는 것과 유사하다.
    - 정형화되어 있는 만큼 데이터가 일관적이고, 분석에 용이하다.
- NoSQL
    - 딕셔너리 형태로 데이터를 저장해두는 DB다.
    - 데이터 하나하나 마다 같은 필드 값들을 가질 필요가 없다.
    - 대신, 일관성이 부족할 수 있다.

### MongoDB

> MongoDB는 다양한 플랫폼에서 사용할 수 있는 NoSQL 타입의 데이터베이스 프로그램으로, JSON과 비슷한 형태로 자료를 정리한다.

- MongoDB의 자료는 딕셔너리인 도큐먼트가 모여 컬렉션, 컬렉션이 보여 DB가 되는 형태다.
    - *자료구조 컬레션과 개념과 다르니 유의하자

### pymongo로 mongoDB 조작하기

MS Excel을 파이썬으로 조작하려면 `openpyxl`이 필요한 것처럼, MongoDB를 조작하기 위해 특별한 라이브러리인 `pymongo`가 필요하다.

#### 1. 설치하기

> Studio 3T를 이용하면 MongoDB를 그래픽으로 볼 수 있게 도와준다. (MongoDB가 실행중인 URL 및 포트를 잘 입력하면 된다.)

```shell
pip install pymongo
```

#### 2. DB 연결하기

```python
from pymongo import MongoClient
client = MongoClient('localhost', 27017) # mongoDB의 기본 포트 = 27017
db = client.test # 'test' 라는 이름의 db를 만들게된다.
```

#### 3. 데이터 넣기

```python
db.users.insert_one({'name':'bobby','age':21}) # 'users'라는 collection에 {'name': 'bobby', 'age': 21}을 넣는다.
db.users.insert_one({'name':'kay','age':27})
db.users.insert_one({'name':'john','age':30})
```

#### 4. 모든 결과 값을 보기

```python
from pymongo import MongoClient
client = MongoClient('localhost', 27017) # mongoDB의 기본 포트 = 27017
db = client.test # 'test' 라는 이름의 db를 만들게된다.

# 데이터 모두 보기
all_users = list(db.users.find({})) # db.users.find는 Cursor 객체를 반환해 직접 리스트처럼 다룰 수 없음, list()를 사용하면, Cursor 객체를 리스트로 변환할 수 있음

# 특정 조건의 데이터 모두 보기
same_ages = list(db.users.find({'age': 21}))
```

#### 5. 특정 결과 값을 가져오기

```python
user = db.users.find_one({'name': 'bobby'}) # 조건에 부합하는 값 하나만 찾음
# 주의: 'bobby' 중 첫 번째 도큐먼트만 출력됨
# 조건: _id 필드의 오름차순(작은 값 -> 큰 값)으로 정렬된 상태에서 데이터를 반환함
# 여담: sort가 가능하긴함 예: db.user.find_one({'name': 'bobby'}, sort=[('age', 1)]) -> 나이가 가능 적은 bobby

# 특정 키 제외 가져오기
user = db.users.find_one({'name': 'bobby'}, {'_id': False}) # _id 키 제외 가져옴
```

#### 6. 수정하기

```python
# 생김새
db.people.update_many(찾을조건,{ '$set': 어떻게바꿀지 })

db.users.update_one({'name': 'bobby'}, {'$set': {'age': 19}}) # 'bobby'의 나이를 19로 변경함
```

#### 7. 삭제하기

```python
db.users.delete_one({'name': 'bobby'}) # 이름이 'bobby'인 도큐먼트를 삭제함
```

### pymongo 사용방법 (요약)

```python
# 저장
doc = {'name': 'bobby', 'age': 21}
db.users.insert_one(doc)

# 한 개 찾기
user = db.users.fidn_one({'name': 'bobby'})

# 여러 개 찾기
same_ages = list(db.users.find({'age': 21}, {'_id': False})) # 특정 키 값 제외도 추가됨

# 수정하기
db.users.update_one({'name': 'bobby'}, {'$set': {age: 19}})

# 삭제하기
db.users.delete_one({'name': 'bobby'})
```
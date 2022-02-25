Python
====

파이썬의 코드 스타일은 기본적으로 [PEP8](https://www.python.org/dev/peps/pep-0008/) 에서 제안하는 방식을 지향합니다.
여기에 추가로 다음 formatter를 적용합니다.
- isort
  + [공식 문서](https://pycqa.github.io/isort/)
  + [현재 설정](https://github.com/8percent/eight/blob/master/.isort.cfg)
- flake8
  + [공식 문서](https://flake8.pycqa.org/en/latest/)
  + [현재 설정](https://github.com/8percent/eight/blob/master/.flake8)


## Maximum Line Length
isort, flake8 설정을 따른다. 현재 120자.

## Line Break with Binary Operator
이항 연산자를 사용할 때 연산자 이전에 줄바꿈을 한다.

#### Do
```
total = (principal
         + interest
         - tax)
```

#### Don't
```
total = (principal +
         interest -
         tax)
```

## Imports
isort 설정을 따른다.

## Module Level Dunder
모듈 레벨의 던더(`__all__`, `__version__` 등)들은 모듈 Docstring 하단에 그리고 일반적인 모듈 임포트 위에 위치하도록 한다.

```
"""Module Docstring Sample

Blah Blah
"""

__all__ = ['A', 'B', 'C']

import sys
import os
```

## Class
클래스를 정의할 때는 최상위 클래스 object 상속을 명시하지 않고 생략한다.

#### Do
```
class Movie:
    pass
```

#### Don't
```
class Movie(object):
    pass
```


## Trailing Commas
Trailing Comma는 요소가 하나인 튜플을 표기하거나, 줄바꿈을 하는데 버전 관리 툴로 Diff를 보기 편한 경우 붙인다.

#### Do
```
tup = (a,)
lists = [
  'a',
  'b',
]
```

#### Don't
```
tup = (a, b,)
lists = ['a','b',]

tup = (
  'a',
  'b'
)
list = [
  'a',
  'b'
]
```

## Comments
[Google Python Style Guide](http://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)의 룰을 따른다.

## Multi-line Parameter
매개 변수 여러개가 있을 때 줄바꿈은 다음과 같은 방식으로 할 수 있다.

#### 매개변수가 2개인 경우
```
def greeting('안녕', 'hello'):
    pass

def greeting('안녕',
             'hello'):
    pass

def greeting(
        '안녕',
        'hello',
    ):
    pass
```  

#### 매개변수가 3개 이상인 경우
```
def greeting('안녕',
             'hello',
             'hola'):
    pass

def greeting(
        '안녕',
        'hello',
        'hola',
    ):
    pass
```

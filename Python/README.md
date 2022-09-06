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
함수 정의 시, 매개변수 리스트의 길이가 긴 경우 줄바꿈은 다음과 같은 방식으로 할 수 있다.
- [관련 논의](https://github.com/8percent/styleguide/discussions/39)

- PEP8 스타일
  - 첫 매개변수가, 매개변수 리스트를 여는 괄호와 같은 줄에 위치
  - 이후 라인에서는 들여쓰기를 첫 매개변수와 맞춤
  - 한 줄에 두 개 이상의 매개변수를 적지 않음
  - 매개변수 리스트를 닫는 괄호는 마지막 매개변수와 같은 줄에 위치(trailing comma 불필요)
- black 스타일
  - 첫 매개변수가, 매개변수 리스트를 여는 괄호의 다음 줄에 위치
  - 모든 매개변수는 1-level 들여쓰기 적용
  - 한 줄에 두 개 이상의 매개변수를 적지 않음
  - 매개변수 리스트를 닫는 괄호는 마지막 매개변수 다음 줄에 위치(trailing comma 필요)

### Do
```python
# PEP8 스타일
def function(argument1,
             argument2,
             argument3: bool,
             argument4: Optional[int] = None) -> None:
    ...


# black 스타일
def function(
    argument1,
    argument2,
    argument3: bool,
    argument4: Optional[int] = None,
) -> None:
    ...
```

### Don't do
```python
# 들여쓰기가 1-level이 아닌 경우
def function(
        argument1,
        argument2,
        argument3: bool,
        argument4: Optional[int] = None,
) -> None:
    ...


# 첫 매개변수 라인으로 들여쓰기 했지만, 닫는 괄호가 독립된 줄에 있는 경우
def function(argument1,
             argument2,
             argument3: bool,
             argument4: Optional[int] = None,
) -> None:
    ...
```

## f-string with equal sign
로깅 등의 목적으로 사용될 때, 필요한 경우 등호기호를 활용한다. 다만, 다음 경우에 대해서는 직접 포맷팅을 한다.
- 구분자로 등호가 아닌 기호를 사용해야하는 경우
- 평가되는 식과 다른 내용으로 header를 적어야 할 때

### Do
```python
logger.info(f'{name = }, {description = }')
```

### Don't do
```python
logger.info(f'name = {name!r}, description = {description!r}')
```

### Other case
```python
logger.info(f'name: {name!r}, description: {description!r}'). # 등호 대신 쌍점 사용이 필요한 경우
logger.info(f'{a} + {b} = {a + b}')  # 이름이 아닌 실제 값이 쓰여야 해서 {a + b = }로 대체할 수 없는 경우
logger.info(f'result = {a + b}')  # 식(a + b) 대신 별도의 이름이 필요한 경우
```

## String Quotes
docstring을 제외한 리터럴 문자열에 사용되는 따옴표에 대해,
관련 [PEP8 항목](https://peps.python.org/pep-0008/#string-quotes)에서는 따옴표에 대한 규칙이 없다.
[내부 논의](https://github.com/8percent/styleguide/discussions/33)를 통해 정한 다음 규칙을 따른다.
- 요약: `str.__repr__`을 따른다.
  - 항상 작은 따옴표를 우선 사용한다.
  - 작은 따옴표를 포함하는 문자열에 한해서 큰 따옴표를 사용한다.
  - 작은 따옴표와 큰 따옴표를 포함하는 문자열에서는 작은 따옴표를 사용한다.

### Do
```python
s1 = 'Hello'
s2 = "I'm a programmer."
```

### Don't
```python
s1 = "Hello"  # 작은 따옴표가 아닌 큰 따옴표를 사용한 경우
s2 = 'I\'m a programmer'  # 큰 따옴표 대신 escape sequence를 사용한 경우
```

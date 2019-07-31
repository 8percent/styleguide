Python
====

파이썬의 코드 스타일은 기본적으로 [PEP8](https://www.python.org/dev/peps/pep-0008/) 에서 제안하는 방식을 지향합니다.

## Maximum Line Length
라인별 최고 길이는 79자를 넘지 않도록 합니다.

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
모듈 임포트는 일반적으로 모듈 상단에 위치하며 한 줄에 한 개의 모듈을 임포트하도록 한다.

임포트의 순서는 다음과 같고, 각 그룹사이에는 빈 줄을 넣습니다.
1. 파이썬 표준 라이브러리
2. 써드파티 라이브러리
3. 애플리케이션 라이브러리

#### Do
```
import sys
import os

from dateutils import relativedelta

from .models import User
```

#### Don't
```
from .models import User
import sys, os
from dateutils import relativedelta
```

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

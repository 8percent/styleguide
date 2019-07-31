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

#### Do
```
import sys
import os
```

#### Don't
```
import sys, os
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

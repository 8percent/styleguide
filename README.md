Eight Style
===========



## Table of Contents

1. [PEP8](#pep8)
1. [Naming](#naming)
1. [Indenting](#indenting)
1. [Import](#import)
1. [Class](#class)
1. [Comment](#comment)
1. [Function](#function)
1. [URL](#url)
1. [Model](#model)
1. [Form](#form)
1. [Template](#template)
1. [View](#view)
1. [Exception](#exception)
1. [Log](#log)
1. [Setting](#setting)
1. [Test](#test)
1. [Parenthesis](#parenthesis)
1. [Reference](#reference)


## PEP8

[PEP 8](https://www.python.org/dev/peps/pep-0008)

## Naming

2.1 Variable
- `snake_case`로 변수를 정의합니다.
```python
snake_case_variable = '8percent'
```

- 모델 클래스를 변수로 정의하는 경우에는 대문자로 시작하는 이름을 허용합니다.
```python
User = get_user_model()
ContentType = apps.get_model('contenttypes', 'ContentType')
```

2.2 Method
- `snake_case`로 메서드의 이름을 정의합니다.
```python
def snake_case_name():
    ...
```

- 내부적으로 사용되는 메서드임을 표시하고 싶은경우 `_` (언더스코어) 를 메서드명 앞에 붙입니다.
```python
def _internal_use_only():
    ...
```

2.3 Class
- `PascalCase`로 클래스의 이름을 정의합니다.
```python
class SampleModel(models.Model):
    ...
```

2.4 Model
- 모델의 이름은 전체 App에서 유일하면 좋습니다. 하지만 의미를 명확하게 하는 것이 더 좋고 중복을 허용합니다.
```python
# 중복 허용
apps.transactions.models.Transaction
apps.accounting.models.Transaction

# import 할 때, alias를 적극 활용합니다.
from apps.accounting.models import Transaction as AccountingTransaction
```


## Indenting
3.1 4-Space를 들여쓰기의 기본 단위로 사용합니다.


## Import
4.1 한 줄에 하나의 모듈 혹은 메서드를 Import 합니다.
```python
import os
import sys

from django.conf import settings
```

4.2 python 표준 라이브러리, 3rd party 라이브러리, 애플리케이션 라이브러리 순으로 Import 합니다.
```python
import os
import sys

from django.conf import settings

from apps.users.models import User
```

## Class
5.1 클래스를 정의할 때, object의 상속을 명시하지 않는다.
```Python
# good
class SampleClass:

# bad
class SampleClass(object)
```

## Comment

## Function

## URL

## Model
9.1 내부 클래스의 순서는 다음과 같은 순서로 정의합니다.
  - `Fields`
  - `Manager`
  - `class Meta`
  - `def __str__()`
  - `def save()`
  - `def get_absolute_url()`
  - `Custom Methods`

9.2 Fields

9.3 Manager

9.4 Meta
- `class Meta`의 `verbose_name`은 최대한 유니크한 이름을 갖도록 합니다.
>  Admin에서 효과적으로 분류하기 위한 목적입니다.

9.5 save

9.6 Instance Method and Property
- 로직 내부에서 변경 작업이 없으면서, 매개변수가 없는 조회함수에 대해 다음 기준을 따른다.
```python
# method 사용
# get_foo 형태로 작성해서 로직내에서 계산이 진행됨을 명확하게 알린다.
def get_sum(self):
    return self.scores.aggregate(...)

# property 사용
@property
def is_male(self):
    return self.gender == 'M'

# cached_property 사용
# property 중에 cache가 되어도 문제가 없는 경우에 사용한다.
@cached_property
def some_calculation(self):
    return calculated_value
```




- [Manager]()
- [Property]()
- [Migration]()
## Form

## Template

## View

## Exception

## Log

## Settings

## Test

## Parenthesis

## Reference

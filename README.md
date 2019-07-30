Eight Style Guides
===========

## Purpose

8퍼센트를 만들어가는 사람들이 지향하는 코드 스타일을 정의한 문서입니다. 스타일 가이드를 활용하여 코드의 가독성을 높이고 다같이 일을 잘 할 수있도록 하는것이 목표입니다.

## Notice

이 스타일 가이드를 구성하는 항목들은 팀원들의 합의에 의해 지속적으로 개선되거나 변경될 수 있습니다.

## Table of Contents

1. [우리의 기준](#우리의-기준)
1. [주석](#주석)
1. [괄호](#괄호)
1. [줄 들여쓰기](#줄-들여쓰기)
1. [명명법](#명명법)
1. [Import](#import)
1. [클래스](#클래스)
1. [함수와 메서드](#함수와-메서드)
1. [예외 처리](#예외-처리)
1. [테스트](#테스트)
1. [로그](#로그)
1. [모델](#모델)
1. [Form](#form)
1. [View](#view)
1. [템플릿](#템플릿)
1. [설정하기](#설정하기)
1. [Reference](#reference)


## 우리의 기준

[PEP 8](https://www.python.org/dev/peps/pep-0008)


## 주석
- 참고: [구글 스타일](http://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)
- 문장인 경우: 마침표를 붙입니다.
- 문장이 아닌 경우: 마침표를 붙이지 않습니다.


## 괄호


## 줄 들여쓰기
- 4-Space를 들여쓰기의 기본 단위로 사용합니다.


## 명명법

2.1 변수 명명법
- `snake_case`로 변수를 정의합니다.
```python
snake_case_variable = '8percent'
```

- 모델 클래스를 변수로 정의하는 경우에는 대문자로 시작하는 이름을 허용합니다.
```python
User = get_user_model()
ContentType = apps.get_model('contenttypes', 'ContentType')
```

2.2 클래스 명명법
- `PascalCase`로 클래스의 이름을 정의합니다.
```python
class SampleModel(models.Model):
    ...
```

2.3 메서드 명명법
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

2.4 모델 명명법
- 모델의 이름은 전체 App에서 유일하면 좋습니다. 하지만 의미를 명확하게 하는 것이 더 좋습니다. 중복된 이름도 가능합니다.
```python
# 중복 허용
eight.models.EightModel
nine.models.EightModel

# import 할 때, alias를 적극 활용합니다.
from nine.model import EightModel as NineEightModel
```


## Import
4.1 한 줄에 하나의 모듈 혹은 메서드를 Import 합니다.
```python
# Good
import os
import sys

# Bad
import os, sys
```
- 각 그룹 사이에는 빈 줄을 추가합니다.
- 그룹 위계: Python 표준 library > 3rd party library > application library
- 참고: [Imports](https://www.python.org/dev/peps/pep-0008/#imports)

4.2 python 표준 라이브러리, 3rd party 라이브러리, 애플리케이션 라이브러리 순으로 Import 합니다.
```python
import os
import sys

from django.conf import settings

from apps.users.models import User
```


## 클래스
- 클래스를 정의할 때 object의 상속을 명시하지 않습니다.
* Python3 부터 가능합니다.
```Python
# Good
class EightClass:

# Bad
class EightClass(object)
```


## 함수와 메서드
7.1 메서드, 인스턴스 변수 명명법
참고: [메서드 이름과 인스턴스 변수](https://www.python.org/dev/peps/pep-0008/#method-names-and-instance-variables)
- 소문자를 사용합니다.
- 가독성을 위해 1개의 underscore로 단어를 구분합니다.
- 서브클래스와의 이름 충돌을 피하기 위해 2개의 underscore로 시작하는 이름을 사용합니다.
- 서브클래스가 없는 경우, 2개의 underscore를 사용하지 않습니다.

7.2 메서드와 프로퍼티
- 내부에서 변경(write) 작업이 없으면서 parameter가 없는 조회(read) 함수에 대해서는 다음 기준을 따릅니다.

7.2.1 메서드
- 내부에서 aggregation이 있을 때, get_oo처럼 작성해서 계산됨을 명시합니다.

7.2.2 프로퍼티
- 메서드에 해당하지 않는 경우, 프로퍼티로 구현합니다.

7.2.3 cached_property
- 프로퍼티 중 캐시되어도 문제가 없는 경우
- 이름 뒤에 "\_cached"를 추가합니다.


## 예외 처리


## 테스트


## 로그


## 모델
9.1 내부 클래스의 순서는 다음과 같은 순서로 정의합니다.
  - `Fields`
  - `Manager`
  - `class Meta`
  - `def __str__()`
  - `def save()`
  - `def get_absolute_url()`
  - `Custom Methods`

9.2 필드

9.3 모델 매니저

9.4 Meta
- `class Meta`의 `verbose_name`은 최대한 유니크한 이름을 갖도록 합니다.
>  Admin에서 효과적으로 분류하기 위한 목적입니다.

9.5 save()

9.6 인스턴스 메서드와 프로퍼티
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

## Form


## View


## 템플릿


## 설정하기


## Reference

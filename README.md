Eight Style
===========



## Table of Contents

* [PEP8]()
* [Naming]()
* [Indenting]()
* [Spacing]()
* [Import]()
* [Class]()
* [Comment]()
* [Function]()
* [URL]()
* [Permission]()
* [Model]()
  - [Manager]()
  - [Property]()
  - [Migration]()
* [Form]()
* [Template]()
* [View]()
* [Exception]()
* [Log]()
* [Setting]()
* [Test]()
* [Parenthesis]()
* [Reference]()


## PEP8

[PEP 8](https://www.python.org/dev/peps/pep-0008)

## Naming

2.1 Variable
- `snake_case`로 변수를 정의합니다.
- 모델 자체를 정의하는 경우에는 대문자를 혀용합니다.
```python
User = get_user_model()
ContentType = apps.get_model('contenttypes', 'ContentType')
```

2.2 Method
- 'snake_case'로 메서드의 이름을 정의합니다.
- 비공개 메서드인 경우 `_`를 메서드명 앞에 붙입니다.
- double underscore

2.3 Instance
- ?

2.4 Queryset
- ?

2.4 Class
- 대문자로 클래스를 정의합니다.
```python
class Sample(models.Model):
    ...
```

## Indenting
1.1 Indent는 4개의 Space로 지정합니다.

## Spacing

## Import
5.1 한 줄에 하나씩 Import 합니다.
```python
import os
import sys

from django.conf import settings
```

5.2 다음과 같은 순서로 Import를 합니다.
```python
import os
import sys

from django.conf import settings

from apps.users.models import User
```

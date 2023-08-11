Python
====

## 코드 스타일 도구

기본적으로 [PEP8](https://www.python.org/dev/peps/pep-0008/)의 스타일을 따릅니다. 그리고 저희들은 일관된 스타일을 작성하는데 도움을 주는 다양한 도구들을 활용하고 있습니다.
- [pre-commit](https://pre-commit.com/)
- [black](https://black.readthedocs.io/en/stable/)
- [isort](https://pycqa.github.io/isort/)
- [flake8](https://flake8.pycqa.org/en/latest/)

### black
위의 여러 가지 도구중에 가장 중심이 되는 것은 black입니다. black에서 기본값으로 지정된 설정을 따르고 있습니다.
black에서 만들어주는 코드의 스타일은 [이곳](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html)에서 확인할 수 있습니다.

### isort
[black & isort](https://black.readthedocs.io/en/stable/guides/using_black_with_other_tools.html#isort)
```
[tool.isort]
profile = "black"
```

### flake8
[black & flake8](https://black.readthedocs.io/en/stable/guides/using_black_with_other_tools.html#flake8)
```
max-line-length = 88
extend-ignore = E203
```

## Type Hinting
> [관련 논의](https://github.com/8percent/styleguide/discussions/47)


함수를 정의할 때는 매개변수와 반환형에 대한 타입 힌트도 추가합니다. 타입 힌트의 최대 활용 범위는 코드 작성자 재량에 맡깁니다.

#### Do
```python
def greeting(name: str) -> str:
    words = "Hello " + name
    return words


def greeting(name: str) -> str:
    words: str = "Hello " + name
    return words
```

#### Don't do
```python
def greeting(name):
    return "Hello " + name
```

인스턴스 메서드 첫 인자(self), 클래스 메서드 첫 인자(cls)에는 타입 힌트를 생략합니다.

#### Do
```python
class Car:
    def move(self, goal: str) -> str:
        return "Move to " + goal

    def get_name(cls) -> str:
        return "My Car"
```

return을 통해 값을 반환하지 않는 함수는 반환형 타입 힌트를 생략할 수 있습니다.

#### Do
```python
def show_movie(name: str):
    print("Hello " + name)


def show_movie(name: str) -> None:
    print("Hello " + name)
```


## 이외의 규칙


### Module Level Dunder
모듈 레벨의 던더(`__all__`, `__version__` 등)들은 모듈 Docstring 하단에 그리고 일반적인 모듈 임포트 위에 위치하도록 합니다.

```python
"""Module Docstring Sample

Blah Blah
"""

__all__ = ['A', 'B', 'C']

import sys
import os
```

### Class
클래스를 정의할 때는 최상위 클래스 object 상속을 명시하지 않고 생략합니다.

#### Do
```python
class Movie:
    pass
```

#### Don't
```python
class Movie(object):
    pass
```


### Comments
[Google Python Style Guide](http://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)의 룰을 따릅니다 .
```python
def fetch_smalltable_rows(
    table_handle: smalltable.Table,
    keys: Sequence[bytes | str],
    require_all_keys: bool = False,
) -> Mapping[bytes, tuple[str, ...]]:
    """Fetches rows from a Smalltable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by table_handle.  String keys will be UTF-8 encoded.

    Args:
        table_handle: An open smalltable.Table instance.
        keys: A sequence of strings representing the key of each table
          row to fetch.  String keys will be UTF-8 encoded.
        require_all_keys: If True only rows with values set for all keys will be
          returned.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {b'Serak': ('Rigel VII', 'Preparer'),
         b'Zim': ('Irk', 'Invader'),
         b'Lrrr': ('Omicron Persei 8', 'Emperor')}

        Returned keys are always bytes.  If a key from the keys argument is
        missing from the dictionary, then that row was not found in the
        table (and require_all_keys must have been False).

    Raises:
        IOError: An error occurred accessing the smalltable.
    """
```

### f-string with equal sign
로깅 등의 목적으로 사용될 때, 필요한 경우 등호를 활용합니다. 다만, 다음 경우에 대해서는 직접 포맷팅을 합니다.
- 구분자로 등호가 아닌 기호를 사용해야하는 경우
- 평가되는 식과 다른 내용으로 header를 적어야 할 때

#### Do
```python
logger.info(f"{name = }, {description = }")
```

#### Don't do
```python
logger.info(f"name = {name!r}, description = {description!r}")
```

#### Other case
```python
logger.info(f"name: {name!r}, description: {description!r}") # 등호 대신 쌍점 사용이 필요한 경우
logger.info(f"{a} + {b} = {a + b}")  # 이름이 아닌 실제 값이 쓰여야 해서 {a + b = }로 대체할 수 없는 경우
logger.info(f"result = {a + b}")  # 식(a + b) 대신 별도의 이름이 필요한 경우
```

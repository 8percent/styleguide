Django
====

기본적으로 [Django 공식문서 코드스타일](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/)의 규칙을 따릅니다.

## Model

### Naming

모델의 이름은 프로젝트 내에서 유일하면 좋습니다. 하지만 의미를 명확하게 하는 것이 더 좋고 이에 따른 중복을 허용합니다.

다음과 같은 중복을 허용합니다.
```
apps.transactions.models.Transaction
apps.accounting.models.Transaction
```

중복으로 발생할 수 있는 작은 문제들을 해결하기 위해 다음과같은 Alias를 적극 활용합니다.
```
from apps.accounting.models import Transaction as AccountingTransaction
```

### Meta - Verbose Name
Verbose Name은 최대한 유일한 이름을 갖도록 하여 Admin 화면에서 모델을 효과적으로 분류할 수 있도록 합니다.

### Order
모델 클래스 내부에 클래스, 필드, 메서드들을 정의하는 순서는 다음과 같습니다.

1. Fields
1. Custom Manager Attributes
1. `class Meta`
1. `def __str__()`
1. `def save()`
1. `def get_absolute_url()`
1. Any Custom Methods

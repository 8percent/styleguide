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

### Order
모델 클래스 내부에 클래스, 필드, 메서드들을 정의하는 순서는 다음과 같습니다.

1. Fields
1. Custom Manager Attributes
1. `class Meta`
1. `def __str__()`
1. `def save()`
1. `def get_absolute_url()`
1. Any Custom Methods


### Field

#### ForeignKey
외래키 필드명은 모델명을 따릅니다.
verbose_name 또한 동일하게 유지합니다.

```
user = models.Foreignkey(User, verbose_name='사용자', on_delete=models.CASCADE)
```

OneToOneField, ManyToManyField 도 동일한 규칙을 적용합니다. 단, 복수형이 적절한 경우 복수형을 허용합니다.

#### 비율
- DecimalField를 기본적으로 사용합니다.
- decimal_places를 4로 지정합니다.
- max_digits는 최소 4이상으로 지정합니다.
- 소숫점 형태로 저장합니다. (8.88% (X), 0.0888 (O)
- API의 경우에 DecimalField를 사용합니다.

#### 금액
- DecimalField를 기본적으로 사용합니다.
- decimal_places를 0으로 지정합니다.
- max_digits는 필드의 속성에 맞추어 지정합니다.
- API의 경우에 IntegerField를 사용합니다.

#### 기타
- IntegerField,  BigIntegerField, PositiveIntegerField, PositiveSmallIntegerField 중에 하나를 사용합니다.
- 음수가 기록되어야 하지 않는 경우는 Positive 필드를 사용합니다.

### Meta - Verbose Name
Verbose Name은 최대한 유일한 이름을 갖도록 하여 Admin 화면에서 모델을 효과적으로 분류할 수 있도록 합니다.

### `__str__`
다음과 같은 형식을 따르면서 최대한 짧게 __str__ 을 유지할 수 있도록 합니다.
`[{모델_한글_이름} {pk}] {A}, {B}, {C:D}`

- {모델_한글_이름} 부분은 meta.verbose_name 와 일치시킵니다.
- pk 에는 Primary Key 를 쓴다. 대부분의 경우 id 값이 된다. pk 값이 포함되어 있어야 DB에서 쉽게 조회할 수 있습니다.
- A, B, C 부분에는 필요한 정보를 추가적으로 적어 줍니다. 그 값의 의미가 명확한 경우에는 key 를 포함하지 않습니다. 하지만 그 의미가 명확하지 않은 경우(대부분 숫자)는 key:value 스타일로 적습니다.

```
[사용자 234] eight@8percent.kr
[영화 234] DancingQueen 상영여부:True
```

## Manager
모델과 관련된 비즈니스 로직을 매니저 메서드로 정의합니다.

## Custom Manager
커스텀 매니저를 정의하는 경우 다음과 같은 규칙을 따릅니다.

- `objects`는 항상 존재하여야 한다. `objects`는 장고의 기본 Manager에 접근하는 convention이기 때문에 이것을 지키도록 한다.
- `objects`로 지정된 custom model manager는 get_queryset이 필터링되지 않아야 한다. 즉, `Model.objects.all() == Model._default_manager.all()` 이 되어야 한다.
- 특정 필터링이 필요한 경우, 의미를 전달할 수 있는 명칭을 추가한다. ex) `Transaction.valid.all()`
- 가능하면 복수형을 쓰도록 한다._

### QuerySet Method
데이터베이스와 관련된 행위의 경우 QuerySet 메서드로 추가합니다.

다음 문서에 나열된 메서드들과 유사한 함수는 QuerySet 메서드라고 볼 수 있습니다.
> [장고 공식 문서](https://docs.djangoproject.com/en/dev/ref/models/querysets/)

```
def opened(self):
  return self.filter(is_opened=True)

def get_total_amount(self):
    return self.aggregate(sum=Sum('amount'))['sum']
```

## Signal
기본적으로 Signal의 사용을 지양합니다.
여러 모델에 걸쳐 복잡한 처리가 필요한 것이 아니라면 가급적 init(), save(), delete() 에서 처리하도록 합니다.

#### 사용에 적합한 경우
- 여러 모델을 변경할 때
- 여러 개의 앱에서 발생한 한 종류의 시그널을 공통으로 처리할 때
- 모델이 저장된 이후에 캐시를 지우고 싶을 때
- 콜백이 필요한데 시그널을 제외하고는 방법이 없는 경우
- 외부 패키지 모델의 save(), init() 에 대한 트리거를 구성하고 싶은 경우

#### 적합하지 않은 경우
- 하나의 모델에 연관되어 있으며 save(), delete() 등에서 처리 가능한 경우
- 커스텀 모델 매니저 메소드를 이용할 수 있을 때
- 특정 뷰에만 해당하는 경우

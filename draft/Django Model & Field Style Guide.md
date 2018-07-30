# Django Model 가이드
- 아래에 정의되지 않은 내용은 Django 공식 문서를 따릅니다
- [Django 코딩 스타일](https://docs.djangoproject.com/en/2.1/internals/contributing/writing-code/coding-style/)

## TimeStampedModel
- 모델 생성시 created, modified 필드가 필요한 경우, django_extensions 패키지의 TimeStampedModel 클래스 사용을 권장한다

## 모델 이름
- 전체 App에서 유일하면 좋습니다
- 하지만 의미를 명확하게 하는 것이 더 좋고, 중복을 허용합니다.
- 중복이 있으면 다음을 주의해야 합니다.
1. import 자동완성을 할 때 정확한 것이 import 되었는가?
2. iPython Notebook에서 사용했을 때 원하는 모델이 import 되었는가?
- 이에 대한 대안으로 다음과 같은 코드를 적극 활용합니다.
- from ...models import X as AX

## class Meta의 verbose_name
- Admin에서 효과적으로 분류를 하기 위한 목적을 갖습니다
- 최대한 유일한 이름을 갖도록 합니다

## \_\_str\_\_
- [{모델_한글_이름} {PK}] {A}, {B}, {C:D}
- 모델_한글_이름: meta.verbose_name과 일치시킨다
- PK에는 primary key를 쓴다. 대부분의 경우는 ID값. PK값이 있어야 DB에서 쉽게 조회할 수 있기 때문이다
- A, B, C 부분에는 필요한 정보를 추가적으로 적는다.
- 값의 의미가 명확하다면 key를 쓰지 않아도 된다
- 값의 의미가 불명확하다면(대부분 숫자) key:value 스타일로 적는다
- 최대한 짧게 유지한다

## 내부 클래스의 순서
- All database fields
- custom manager attributes
- class Meta
- def \_\_str\_\_()
- def save()
- def get_absolute_url()
- any custom methods

## 모델 필드
1. 숫자 관련 가이드는 다음 문서를 참고합니다: "숫자 다루기"
- DecimalField: 금액, 소수점이 들어가는 필드
- IntegerField: DRF serialize의 금액 필드
2. FK 필드명은 모델명을 따릅니다. 
- 가급적이면 verbose_name도 동일한 값을 유지합니다.
- OneToOneField, ManyToManyField(복수형이 적절하다면 복수형을 사용해도 됩니다) 도 동일한 규칙을 따릅니다
3. 예외: 
- 복수개의 FK 생성시
- 접두사는 허용됩니다 (from_user, to_user)

## Manager & Class Method
- 모델과 관련된 메서드를 정의할 때 Manager와 Class 메서드 중 어디에 정의해야 할지 구분한다
- 복수개의 반환을 의도: Manager
- 단일 객체 조회: Model
- return이 queryset인가, model의 instance인가에 따라 구분한다

## Signals
1. 적합한 경우
- 여러 모델을 변경할 때
- 여러개의 앱에서 발생한 한 종류의 시그널을 공통적으로 처리할 때
- 모델이 저장된 이후 캐시를 지우고 싶을 때
- 콜백이 필요한데 시그널로만 가능한 경우
- 외부 패키지 모델의 save(), init()에 대한 trigger 구성하고 싶은 경우
2. 부적합한 경우
- 하나의 모델에 연관되어 있으며 save(), delete() 등에서 처리가 가능한 경우
- 커스텀 모델 매니저 메서드를 이용할 수 있을 때
- 특정 뷰에만 해당하는 경우
- 참고: Two Scoops of Django 28장

## Field Tracker
- 필드별로 tracker를 만들지 말고, 1개의 모델에 1개의 tracker를 선언한다
- 이름은 tracker로 한다
- has_changed(), previous()를 사용해 필드별로 체크가 가능하다
- 추적할 필드를 추가할 때, tracker에 추가만 하면 된다
- 참고: [필드 추적기](http://django-model-utils.readthedocs.io/en/latest/utilities.html#field-tracker)


# Django Model Manager 가이드
## Queryset 메서드
- DB와 관련된 행위의 경우 Queryset 메서드로 추가한다
- Queryset 메서드인지 판단은 다음 문서를 참고한다
- [Django Queryset](https://docs.djangoproject.com/en/dev/ref/models/querysets/)
- filter(), order_by(), count() 등이 있다
- 체이닝 메서드가 필요할 경우 Queryset 메서드로 작성한다

## Manager 메서드
- 비즈니스 로직을 담당하며 모델 클래스 메서드로 작성할 것들을 추가한다
- User 모델: create()

## Custom Model Manager 추가시 주의할 점
- objects는 항상 존재해야 한다
- objects는 django의 default manager에 접근하는 기본 컨벤션이므로 지켜야 한다
- objects로 지정된 custom model manager는 get_queryset()이 필터되지 않아야 한다. 
- Model.objects.all() == Model.\_default\_manager.all() 처럼 되게 쓴다
- 특정 필터링이 가능한 경우, 의미를 전달할 수 있는 명칭을 추가한다: Model.valid.all()
- 가능하면 복수형을 쓰도록 한다


# Django Form 가이드
- Django 공식 문서에서 특별한 가이드를 주고 있지 않습니다.

## Field 순서
- Django 모델 개발 가이드에서의 순서를 우선적으로 따릅니다
- All form fields
- class Meta
- def \_\_init\_\_()
- def \_\_str\_\_()
- def save()
- any custom methods


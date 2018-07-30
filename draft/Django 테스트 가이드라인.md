# 테스트 케이스 작성 가이드

## 테스트 메서드 인자의 순서는 메서드가 제시하는 인자에 맞춰서 사용합니다
- 투표에 따라, 새로운 테스트에 대해서는 메서드가 제시하는 표준을 따릅니다

## 명명법
### 클래스명
- 일반적으로 TestCase를 상속받는 클래스의 경우, 클래스명의 마지막에 TestCase를 붙인다

### 메서드명
- test_ 로만 시작하면 동작한다
- 테스트 코드에까지 주석을 다는 것은 번거로우니 test_ 뒷부분을 한글로 하여 설명을 대신한다
- 테스트 메서드명 작성시, 테스트 대상인 함수/메서드명을 포함시켜 만들면 관련된 테스트를 찾기 쉬워진다
- Cmd + Shift + T


## setUpTestData vs. setUp
- fixture를 사용하면 fixture로 정의한 모델 객체가 모든 테스트 시작 전에 생성되는데, setUp에서 factory 생성을 하면 매번 객체를 생성하므로 느려진다
- 테스트에서 read-only 객체의 경우 classmethod인 setUpTestData에서 1번만 생성해 빠르게 만든다
- 가급적 setUp에서 객체를 생성하는 것은 피한다
- 테스트 함수에서 필요한 객체만 생성한다
- 참고: [setUpTestData](https://docs.djangoproject.com/en/2.1/topics/testing/tools/#django.test.TestCase.setUpTestData)


## Factory Boy
### signals 끄기
- factory boy 로 모델 객체를 생성할 때 signal이 호출되는데, signal에 대한 테스트가 아니라면 대부분 실행할 필요가 없다
- 이때 factory.django.mute_signals를 사용해 signal을 끈다
- decorator, context_manager 둘 다 사용 가능하다
- 참고: [시그널 끄기](http://factoryboy.readthedocs.io/en/latest/orms.html#disabling-signals)

## unittest.mock.patch
### decorator
- 일반적으로는 decorator를 사용해 테스트 메서드에 적용한다

### 직접 호출
- 클래스 내의 여러 테스트 메서드 또는 모든 테스트 메서드에서 동일한 patch를 사용한다면, setUp()에서 start()를, tearDown()에서 stop()을 사용한다
- 참고: [패치 메서드-시작과 정지](https://docs.python.org/3/library/unittest.mock.html#patch-methods-start-and-stop)

## datetime 사용시 주의사항
### timezone
- datetime.now(), datetime.today(), datetime.strptime() 등으로 timezone이 없는 naive datetime을 모델의 DateTime 필드에 할당하는 경우: 반드시 aware_time을 해 준다
- 참고: Django Time Zone Problem
- 참고: [국제화-타임존](https://docs.djangoproject.com/en/2.1/topics/i18n/timezones/)

### freezegun.freeze_time
- datetime.datetime.now() 등, 현재 일시를 얻을 때 시간값 고정하기 위해 사용
- 가급적 decorator, context manager를 사용해 특정 클래스/메서드/코드 블록에만 적용한다
- 주의사항: 특정 테스트 케이스 전체에 적용하기 위해 start(), stop() 메서드를 사용하기도 하는데, 이 경우 반드시 stop()을 해줘야 다른 테스트 케이스의 시간값에 영향을 주지 않는다

### 휴무일 관리
- HolidayFactory - Holiday 모델을 만들어 관리한다

## 기타
### 테스트 생략
- 테스트에 문제가 발생해 생략하고 넘어가고 싶다면 @unittest.skip('mocking문제')f를 테스트 메서드를 씌운다
- 주석처리 없이 생략이 가능해진다
- 테스트 결과 콘솔에서도 생략된 테스트를 확인할 수 있다

### 줄바꿈
- 가독성을 향상시킨다
- 데이터 셋업 / 1줄 / 테스트 / 1줄 / 결과 확인













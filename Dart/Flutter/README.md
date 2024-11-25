# Flutter

기본적으로 [Dart Style Guide](https://dart.dev/guides/language/effective-dart/style)를 따르고 있지만, 본 문서에서는 Flutter에 조금 더 특화된 스타일 가이드를 작성합니다.

## 목차

- [코드 스타일](#코드-스타일)
  - [특수한 기능을 가지고 있는 함수, 메소드 및 생성자를 사용합니다](#특수한-기능을-가지고-있는-함수-메소드-및-생성자를-사용합니다)
  - [var 또는 dynamic 사용을 피합니다](#var-또는-dynamic-사용을-피합니다)
  - [상수(Constant)의 Scope를 최소화 합니다](#상수constant의-scope를-최소화-합니다)
  - [명확하지 않은 숫자 또는 Magic Number의 사용을 피합니다](#명확하지-않은-숫자-또는-magic-number의-사용을-피합니다)
  - [initState() 와 dispose() 함수를 Override 하는 경우, `super` 호출 순서를 주의해서 사용합니다](#initstate-와-dispose-함수를-override-하는-경우-super-호출-순서를-주의해서-사용합니다)
- [네이밍](#네이밍)
  - [전역 상수의 이름은 접두사 "k" 로 시작합니다](#전역-상수의-이름은-접두사-k-로-시작합니다)
- [포매팅](#포매팅)
  - [클래스 멤버들을 의미가 있게 정렬하세요](#클래스-멤버들을-의미가-있게-정렬하세요)
  - [생성자 구문](#생성자-구문)
  - [최대 80자의 줄 길이를 선호합니다](#최대-80자의-줄-길이를-선호합니다)
  - [여러줄의 인자 및 파라미터 항목들은 2칸씩 들여쓰기 합니다](#여러줄의-인자-및-파라미터-항목들은-2칸씩-들여쓰기-합니다)
  - [여는 괄호 `(` 뒤에 줄바꿈이 있는 경우, 닫는 괄호 `)` 앞에도 줄바꿈을 추가합니다](#여는-괄호-뒤에-줄바꿈이-있는-경우-닫는-괄호-앞에도-줄바꿈을-추가합니다)
  - [인자들과 파라미터 리스트 또는 List 형식을 표기할 때 Multi-line 으로 작성하는 경우라면, 각 라인의 끝에 쉼표 `,` 를 추가합니다](#인자들과-파라미터-리스트-또는-list-형식을-표기할-때-multi-line-으로-작성하는-경우라면-각-라인의-끝에-쉼표를-추가합니다)
- [Packages](#packages)
  - [Import Convention](#import-convention)
  - [Riverpod](#riverpod)
    - [Notifier의 생성자에 state 초기화 로직을 작성하는것을 피합니다](#notifier의-생성자에-state-초기화-로직을-작성하는것을-피합니다)
  - [Code Generator - build_runner](#code-generator---build_runner)
    - [root 폴더 하위에 build 폴더를 생성하여 관리할 수 있도록 합니다](#root-폴더-하위에-build-폴더를-생성하여-관리할-수-있도록-합니다)

## 코드 스타일

전반적인 Flutter 관련 스타일에 대해 작성합니다. 특정 패키지 관련 스타일은 [Packages](#packages) 문단에서 작성합니다.

### 특수한 기능을 가지고 있는 함수, 메소드 및 생성자를 사용합니다

여러 옵션이 있는 경우, 가장 관련성이 높은 생성자나 메소드를 사용하세요.

```dart
// BAD:
const EdgeInsets.TRBL(0.0, 8.0, 0.0, 8.0);

// GOOD:
const EdgeInsets.symmetric(horizontal: 8.0);
```

### var 또는 dynamic 사용을 피합니다

모든 변수들(variables)과 인자들(arguments)은 타입이 존재합니다. 실제 유형을 파악할 수 있는 경우에는 dynamic 또는 Object 타입의 사용을 피하세요. 가능한 경우 모든 Generic 타입의 유형을 명시할것을 권장합니다. Closure 에서 사용되는 Parameter 또한 타입이 명시되어야 합니다.

모든 경우에서 var 과 dynamic 사용을 피하세요. 만약, 타입을 알 수 없는 경우라면 Object(또는 Object?)를 사용하여 Casting 합니다. 이렇게 하는 이유는 dynamic 의 경우 모든 정적 검사가 비활성화되기 때문입니다.

### 상수(Constant)의 Scope를 최소화 합니다

글로벌 전역 상수를 사용하는 대신, 관련성이 있는 클래스에서 로컬 const 또는 static const 를 사용하세요.
만약, 선언한 상수가 많아진다면 abstract final class 로 래핑하는것을 권장합니다.

### 명확하지 않은 숫자 또는 Magic Number의 사용을 피합니다

코드상의 숫자 및 테스트에서 사용되는 모든 숫자의 경우 명확하게 이해할 수 있어야 합니다. 만약, 숫자의 출처가 명확하지 않은 경우 표현식을 그대로 두거나 명확한 주석을 추가하세요.

예시:

```dart
// BAD:
const double kMaxWidth = 1200;

// GOOD:
/// 기본 값 60에 해상도 조정을 위해 20을 곱한 값
const double kMaxWidth = 60 * 20;
```

### initState() 와 dispose() 함수를 Override 하는 경우, `super` 호출 순서를 주의해서 사용합니다

initState() 의 경우 super.initState() 를 호출 한 후 추가적인 작업을 진행하세요. 반면, dispose() 의 경우 super.dispose() 를 호출 하기 전에 추가적인 작업을 진행하세요.

initState() 의 경우 위젯의 초기화 과정에서 중요한 생명주기 호출입니다. 이를 먼저 호출해야 위젯의 상태가 안정적으로 유지됩니다.

## 네이밍

### 전역 상수의 이름은 접두사 "k" 로 시작합니다

예시:

```dart
const double kMaxWidth = 1200;
const Duration kAnimationDuration = Duration(milliseconds: 200);
```

그러나, 가능한 경우 kDefaultButtonColor 와 같이 전역 상수를 선언하는 대신, Button.defaultColor 와 같이 클래스 내부에 상수를 선언하세요. 필요한 경우 abstract final class 를 사용하여 상수를 선언합니다.

## 포매팅

### 클래스 멤버들을 의미가 있게 정렬하세요

initState 와 dispose 처럼 명확한 라이프사이클이 있는 경우, initState 가 dispose 보다 먼저 오도록 정렬하세요.

예시:

```dart
class MyWidget extends StatefulWidget {
  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  @override
  void initState() {
    super.initState();
  }

  @override
  void build(BuildContext context) {

  }

  @override
  void dispose() {
    super.dispose();
  }
}
```

만약, 순서가 명확하지 않은 경우에는 아래와 같은 순서를 따르며, 각 순서 사이에는 빈 라인을 추가합니다.

1. 생성자에서 초기화되는 final 변수들
2. 생성자 : 생성자는 기본 생성자를 먼저 선언하고, 추가적인 생성자는 그 뒤에 선언합니다.
3. 클래스와 같은 유형의 상수
4. 클래스와 동일한 유형을 반환하는 정적 메서드들
5. 다른 정적 메서드들
6. 정적 속성들(properties)과 상수들
7. 변경 가능한 속성들(properties)은 빈 라인 구분 없이 다음과 같은 순서로 작성합니다.
   - getter
   - private field
   - setter
8. `toString` 과 `build` 를 제외한 메서드들
9. `build` 메서드
10. `hashCode`, `toString` 메서드

생성자가 여러 필드를 받고 있다면, 해당하는 클래스 멤버 변수들의 선언 순서도 동일해야 합니다.

### 생성자 구문

초기화 목록에서 `super()` 를 호출하는 경우, 생성자와 콜론, 그리고 super 키워드 사이에 공백을 유지합니다. super class 에 전달할 인자가 없으면, super 를 호출하지 마세요.

예시:

```dart
// one-line constructor example
class ConstantTween<T> extends Tween<T> {
  ConstantTween(T value) : super(begin: value, end: value);

  // ...
}

// fully expanded constructor example
class ConstantTween<T> extends Tween<T> {
  ConstantTween(
    T value,
  ) : super(
        begin: value,
        end: value,
      );

  // ...
}
```

### 최대 80자의 줄 길이를 선호합니다

최대 줄 길이는 80자를 목표로 하지만, 줄을 나누었을 때 읽기 어려워지거나, 줄과 주변 줄의 일관성이 떨어지는 경우에는 해당 조건을 무시하고 작성합니다.

```Dart
// BAD (breaks after assignment operator)
final List<FooBarBaz> _members =
  <FooBarBaz>[const Quux(), const Qaax(), const Qeex()];

// BETTER (only slightly goes over 80 chars)
final List<FooBarBaz> _members = <FooBarBaz>[const Quux(), const Qaax(), const Qeex()];

// BETTER STILL (fits in 80 chars)
final List<FooBarBaz> _members = <FooBarBaz>[
  const Quux(),
  const Qaax(),
  const Qeex(),
];
```

### 여러줄의 인자 및 파라미터 항목들은 2칸씩 들여쓰기 합니다

예시:

```Dart
Foo f = Foo(
  bar: 1.0,
  quux: 2.0,
);
```

### 여는 괄호 뒤에 줄바꿈이 있는 경우, 닫는 괄호 앞에도 줄바꿈을 추가합니다

여는 괄호 `(` 뒤에 줄바꿈이 있는 경우, 닫는 괄호 `)` 앞에도 줄바꿈을 추가합니다

예시:

```dart
// BAD:
  foo(
    bar, baz);
  foo(
    bar,
    baz);
  foo(bar,
    baz
  );

// GOOD:
  foo(bar, baz);
  foo(
    bar,
    baz,
  );
```

### 인자들과 파라미터 리스트 또는 List 형식을 표기할 때 Multi-line 으로 작성하는 경우라면, 각 라인의 끝에 쉼표를 추가합니다

예시:

```dart
List<int> myList = [
  1,
  2,
];
myList = <int>[3, 4];

foo1(
  bar,
  baz,
);
foo2(bar, baz);
```

항목 중 하나가 multi-line callback, collection literal 또는 switch 표현식인 경우 끝에 쉼표 없이 추가할 수 있습니다.

예시:

```dart
// GOOD:
foo(
  bar,
  baz,
  switch (value) {
    true  => ScrollDirection.forward,
    false => ScrollDirection.reverse,
    null  => ScrollDirection.idle,
  },
);

// also GOOD:
foo(bar, baz, switch (value) {
  true  => ScrollDirection.forward,
  false => ScrollDirection.reverse,
  null  => ScrollDirection.idle,
});

// The same applies to collection literals and callbacks:
foo(<String>[
  'list item 1',
  'list item 2',
  'list item 3',
]);

Future.delayed(Durations.short1, () {
  if (mounted && _shouldOpenDrawer) {
    _drawerController.forward();
  }
});
```

모든것을 한줄로 작성할지, Multi-line 으로 작성할지는 미적인 선택이며, 가장 읽기 쉬운 방법을 선택합니다. 다만, 연속된 리스트에서는 통일된 스타일을 사용할 것을 권장합니다.

예시:

```dart
  // BAD (because the second list is unnecessarily and confusingly different than the others):
  List<FooBarBaz> myLongList1 = <FooBarBaz>[
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
  ];
  List<Quux> myLongList2 = <Quux>[ Quux(1), Quux(2) ];
  List<FooBarBaz> myLongList3 = <FooBarBaz>[
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
  ];

  // GOOD (code is easy to scan):
  List<FooBarBaz> myLongList1 = <FooBarBaz>[
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
  ];
  List<Quux> myLongList2 = <Quux>[
    Quux(1),
    Quux(2),
  ];
  List<FooBarBaz> myLongList3 = <FooBarBaz>[
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
    FooBarBaz(one: firstArgument, two: secondArgument, three: thirdArgument),
  ];
```

### `=>` 의 사용은 짧은 함수 또는 메서드에서 사용할것을 권장합니다

다만, `=>` 를 사용했을 때 모든 내용이 한줄로 작성이 가능한 경우에만 사용합니다.

예시:

```dart
// BAD:
String capitalize(String s) =>
  '${s[0].toUpperCase()}${s.substring(1)}';

// GOOD:
String capitalize(String s) => '${s[0].toUpperCase()}${s.substring(1)}';

String capitalize(String s) {
  return '${s[0].toUpperCase()}${s.substring(1)}';
}
```

### `=>`의 사용은 리터럴 또는 switch 표현식을 반환하는 getter 또는 callback 에서만 사용합니다

예시:

```dart
// GOOD:
List<Color> get favorites => <Color>[
  const Color(0xFF80FFFF),
  const Color(0xFF00FFF0),
  const Color(0xFF4000FF),
  _mysteryColor(),
];

// GOOD:
bool get isForwardOrCompleted => switch (status) {
  AnimationStatus.forward || AnimationStatus.completed => true,
  AnimationStatus.reverse || AnimationStatus.dismissed => false,
};
```

코드가 `return` 명령문만 포함하는 inline closure를 전달하는 경우에도 `=>` 를 사용합니다.
이런 경우에는 `]`, `}` 또는 `)` 괄호는 callback 이 시작하는 줄과 같은 들여쓰기를 같습니다.

예시:

```dart
    // GOOD, but slightly more verbose than necessary since it doesn't use =>
    @override
    Widget build(BuildContext context) {
      return PopupMenuButton<String>(
        onSelected: (String value) { print('Selected: $value'); },
        itemBuilder: (BuildContext context) {
          return <PopupMenuItem<String>>[
            PopupMenuItem<String>(
              value: 'Friends',
              child: MenuItemWithIcon(Icons.people, 'Friends', '5 new'),
            ),
            PopupMenuItem<String>(
              value: 'Events',
              child: MenuItemWithIcon(Icons.event, 'Events', '12 upcoming'),
            ),
          ];
        }
      );
    }

    // GOOD, does use =>, slightly briefer
    @override
    Widget build(BuildContext context) {
      return PopupMenuButton<String>(
        onSelected: (String value) { print('Selected: $value'); },
        itemBuilder: (BuildContext context) => <PopupMenuItem<String>>[
          PopupMenuItem<String>(
            value: 'Friends',
            child: MenuItemWithIcon(Icons.people, 'Friends', '5 new'),
          ),
          PopupMenuItem<String>(
            value: 'Events',
            child: MenuItemWithIcon(Icons.event, 'Events', '12 upcoming'),
          ),
        ]
      );
    }
```

## Packages

### Import Convention

Flutter Framework 관련 패키지를 import 해야하는 경우 다음과 같은 기준으로 작성합니다.

- material : 대부분의 앱 개발시 선택해서 사용
- cupertino : iOS 스타일 앱이나 플랫폼 특화된 iOS UI를 제공할 때 사용
- foundation : UI 위젯 대신 Flutter의 기본 기능이나 상태 관리를 위한 클래스만 필요할 때 사용

### Riverpod

대표적인 상태관리 패키지 중 하나로 riverpod 을 사용하고 있습니다.

#### Notifier의 생성자에 state 초기화 로직을 작성하는것을 피합니다

아직 initialized 되지 않았기에 `ref` 등 기타 프로퍼티들을 사용할 수 없으므로, Notifier에는 생성자가 없어야 합니다.
대신 `build` 메서드에 로직을 작성합니다.

```dart
class MyNotifier extends ... {
  // BAD:
  MyNotifier() {
    state = AsyncValue.data(42);
  }

  // GOOD:
  @override
  Result build() {
    state = AsyncValue.data(42);
  }
}
```

### Code Generator - build_runner

코드 생성 도구 중 하나인 `build_runner` 패키지는 Dart 코드를 사용하여 파일을 생성하는 방법을 제공하는 패키지입니다. 반복적인 작업을 자동화 하기도 하며, [Static Metaprogramming](https://github.com/dart-lang/language/issues/1482) 을 가능하게 해줍니다. (Dart 언어의 경우 어플리케이션을 `컴파일`하는데 추가 단계가 필요하다는 단점이 있어 이를 보완하기 위해 사용합니다.)

많은 패키지들이 build_runner 를 활용하여 코드 생성을 통해 개발 생산성을 높이고 있습니다. 예를 들어, [freezed](https://pub.dev/packages/freezed) 패키지는 데이터 클래스를 생성하는데 사용되고, [json_serializable](https://pub.dev/packages/json_serializable) 패키지는 JSON 직렬화 코드를 생성하는데 사용됩니다.

이처럼 많은곳에서 사용하는 만큼 한번 코드생성을 하게 되면 많은 파일들이 생성됩니다. 이러한 파일들은 프로젝트 내에서 추적이 어렵기 때문에 프로젝트 내에서 특정 폴더에 위치하도록 관리합니다.

#### root 폴더 하위에 build 폴더를 생성하여 관리할 수 있도록 합니다

예시:

```dart
project_root
├── build/
│   └── ...
└── lib/
    └── ...
```

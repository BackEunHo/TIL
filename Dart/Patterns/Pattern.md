# # [Patterns](https://dart.dev/language/patterns)

Dart의 패턴은 값의 형태를 검사하고, 값을 구성 요소로 분해하는 데 사용되는 구문입니다. 패턴은 변수 선언, 변수 할당, switch 문 및 for 루프 등 다양한 곳에서 사용할 수 있습니다.

## 목차

- [Patterns](#patterns)
  - [What patterns do](#what-patterns-do)
    - [Matching](#matching)
    - [Destructuring](#destructuring)
  - [Places patterns can appear](#places-patterns-can-appear)
    - [Variable declaration](#variable-declaration)
    - [Variable assignment](#variable-assignment)
    - [Switch statements and expressions](#switch-statements-and-expressions)
    - [For and for-in loops](#for-and-for-in-loops)
  - [Use cases for patterns](#use-cases-for-patterns)
    - [Destructuring multiple returns](#destructuring-multiple-returns)
    - [Destructuring class instances](#destructuring-class-instances)
    - [Algebraic data types](#algebraic-data-types)
    - [Validating incoming JSON](#validating-incoming-json)

## What patterns do

패턴은 주어진 값이 특정 형태를 가지는지 확인하고, 그 값을 구성 요소로 분해할 수 있게 합니다. 문맥과 패턴의 형태에 따라 패턴은 **매칭** 또는 **디스트럭처링** 또는 둘 다를 수행할 수 있습니다.

### Matching

패턴 매칭은 주어진 값이 특정한 형태를 가지는지 확인하는 과정입니다. 매칭되는지 여부는 패턴의 종류에 따라 다릅니다. 예를 들어, 상수 패턴은 값이 패턴의 상수와 같은지 확인합니다.

```dart
switch (number) {
  // 상수 패턴은 number가 1과 같을 때 매칭됩니다.
  case 1:
    print('one');
}
```

여러 가지 패턴은 하위 패턴을 사용할 수 있으며, 패턴은 재귀적으로 하위 패턴을 매칭합니다. 예를 들어, 리스트 패턴의 개별 필드는 변수 패턴이나 상수 패턴일 수 있습니다.

```dart
const a = 'a';
const b = 'b';
switch (obj) {
  // 리스트 패턴 [a, b]는 obj가 두 개의 필드를 가진 리스트인지 먼저 확인하고,
  // 그런 다음 각 필드가 상수 패턴 'a'와 'b'에 매칭되는지 확인합니다.
  case [a, b]:
    print('$a, $b');
}
```

일부 매칭을 무시하고 싶을 때는 와일드카드 패턴을 사용할 수 있습니다. 리스트 패턴의 경우, rest 요소를 사용할 수 있습니다.

### Destructuring

디스트럭처링은 매칭된 객체의 데이터를 접근하고, 이를 일부 또는 전체로 추출하여 변수에 바인딩하는 과정입니다. 패턴이 객체와 매칭되면, 패턴은 객체의 데이터를 분해할 수 있습니다.

```dart
var numList = [1, 2, 3];
// 리스트 패턴 [a, b, c]는 numList의 세 요소를 분해하여 각각 새로운 변수 a, b, c에 할당합니다.
var [a, b, c] = numList;
print(a + b + c); // 출력: 6
```

패턴 내에 모든 종류의 패턴을 중첩하여 사용할 수 있습니다. 예를 들어, 두 요소로 구성된 리스트 패턴은 첫 번째 요소가 'a' 또는 'b'인지 확인할 수 있습니다.

```dart
switch (list) {
  case ['a' || 'b', var c]:
    print(c);
}
```

## Places patterns can appear

Dart 언어의 여러 위치에서 패턴을 사용할 수 있습니다:

- **로컬 변수 선언 및 할당**
- **for 및 for-in 루프**
- **if-case 및 switch-case**
- **컬렉션 리터럴 내 제어 흐름**

이 섹션에서는 패턴을 매칭 및 디스트럭처링과 함께 사용하는 일반적인 사용 사례를 설명합니다.

### Variable declaration

패턴 변수 선언은 Dart가 로컬 변수 선언을 허용하는 모든 곳에서 사용할 수 있습니다. 패턴은 선언의 오른쪽에 있는 값과 매칭됩니다. 매칭되면, 패턴은 값을 분해하고 새로운 로컬 변수에 바인딩합니다.

```dart
// 새로운 변수 a, b, c를 선언합니다.
var (a, [b, c]) = ('str', [1, 2]);
```

패턴 변수 선언은 `var` 또는 `final`로 시작해야 하며, 뒤에 패턴이 따라옵니다.

### Variable assignment

패턴 변수 할당은 할당의 왼쪽에 위치합니다. 패턴은 매칭된 객체를 디스트럭처링하고, 값을 기존 변수에 할당합니다.

변수의 값을 선언하지 않고도 두 변수의 값을 교환할 수 있습니다:

```dart
var (a, b) = ('left', 'right');
(b, a) = (a, b); // 교환
print('$a $b'); // 출력: "right left"
```

### Switch statements and expressions

모든 case 절에는 패턴이 포함됩니다. 이는 switch 문과 표현식, 그리고 if-case 문에도 적용됩니다. 각 case에서 패턴을 사용하여 객체를 매칭하고 디스트럭처링할 수 있습니다.

패턴은 참조 가능한 한정된 타입을 지원하며, case 패턴은 조건부를 포함할 수 있습니다.

```dart
switch (obj) {
  // number가 1과 같을 때 매칭됩니다.
  case 1:
    print('one');
  
  // obj가 'first' 이상이고 'last' 이하일 때 매칭됩니다.
  case >= first && <= last:
    print('in range');

  // obj가 두 필드를 가진 레코드일 때 매칭되며, 필드를 변수 a와 b에 할당합니다.
  case (var a, var b):
    print('a = $a, b = $b');

  default:
}
```

논리-또는 패턴은 switch 표현식이나 문에서 여러 case가 동일한 본문을 공유하도록 할 때 유용합니다:

```dart
var isPrimary = switch (color) {
  Color.red || Color.yellow || Color.blue => true,
  _ => false
};
```

### For and for-in loops

for 및 for-in 루프에서 패턴을 사용하여 컬렉션의 값을 반복하고 디스트럭처링할 수 있습니다.

다음 예제는 `Map.entries`가 반환하는 `MapEntry` 객체들을 디스트럭처링하는 방법을 보여줍니다:

```dart
Map<String, int> hist = {
  'a': 23,
  'b': 100,
};

for (var MapEntry(key: key, value: count) in hist.entries) {
  print('$key occurred $count times');
}
```

객체 패턴은 `MapEntry`의 `key`와 `value` 게터를 호출하여 각각의 값을 `key`와 `count` 변수에 바인딩합니다.

같은 이름의 변수에 게터 결과를 바인딩할 때는, 패턴을 간소화하여 `key: key`를 `:key`로 표현할 수 있습니다:

```dart
for (var MapEntry(:key, value: count) in hist.entries) {
  print('$key occurred $count times');
}
```

## Use cases for patterns

패턴은 다양한 상황에서 사용할 수 있으며, 다음과 같은 문제를 해결하는 데 유용합니다:

### Destructuring multiple returns

레코드를 사용하여 여러 값을 반환하고, 패턴을 사용하여 이를 로컬 변수에 디스트럭처링할 수 있습니다.

```dart
var (name, age) = userInfo(json);
```

### Destructuring class instances

객체 패턴을 사용하여 클래스 인스턴스의 데이터를 디스트럭처링하고, 게터를 통해 값을 추출할 수 있습니다.

```dart
final Foo myFoo = Foo(one: 'one', two: 2);
var Foo(:one, :two) = myFoo;
print('one $one, two $two');
```

### Algebraic data types

알제브라적 데이터 타입 스타일로 코드를 작성할 때 패턴과 switch 문을 사용하여 타입별로 특정 동작을 수행할 수 있습니다.

```dart
sealed class Shape {}

class Square implements Shape {
  final double length;
  Square(this.length);
}

class Circle implements Shape {
  final double radius;
  Circle(this.radius);
}

double calculateArea(Shape shape) => switch (shape) {
      Square(length: var l) => l * l,
      Circle(radius: var r) => math.pi * r * r
    };
```

### Validating incoming JSON

JSON 데이터의 구조를 패턴을 사용하여 간결하게 검증하고, 필요한 데이터를 추출할 수 있습니다.

```dart
var json = {
  'user': ['Lily', 13]
};

if (json case {'user': [String name, int age]}) {
  print('User $name is $age years old.');
}
```

이렇게 하면 `json`이 특정 형태를 가지는지 검증하고, 동시에 필요한 데이터를 변수에 바인딩할 수 있습니다.

## 관련 자료

- [Dart 패턴 문서](https://dart.dev/language/patterns)

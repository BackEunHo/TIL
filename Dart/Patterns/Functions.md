# [Functions](https://dart.dev/language/functions)

Dart의 함수는 **일급 객체(first-class objects)**로 취급됩니다. 이는 함수가 변수에 할당될 수 있고, 다른 함수의 인자로 전달될 수 있으며, 함수에서 반환될 수 있음을 의미합니다. 함수는 Dart에서 중요한 역할을 하며, 함수형 프로그래밍 스타일을 지원합니다.

## 목차

- [함수 정의](#함수-정의)
  - [일반 함수](#일반-함수)
  - [화살표 함수](#화살표-함수)
- [매개변수](#매개변수)
  - [명명된 매개변수](#명명된-매개변수)
  - [선택적 위치 매개변수](#선택적-위치-매개변수)
- [함수 타입](#함수-타입)
- [익명 함수](#익명-함수)
- [레이크럴 스코프](#레이크럴-스코프)
- [레이클렉스 클로저](#레이클렉스-클로저)
- [테어오프](#테어오프)
- [함수의 동등성 테스트](#함수의-동등성-테스트)
- [반환 값](#반환-값)
- [제너레이터](#제너레이터)
  - [동기 제너레이터](#동기-제너레이터)
  - [비동기 제너레이터](#비동기-제너레이터)
- [외부 함수](#외부-함수)
- [관련 자료](#관련-자료)

## 함수 정의

### 일반 함수

Dart에서 함수를 정의하는 가장 기본적인 방법은 다음과 같습니다:

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

타입 주석을 생략할 수도 있습니다. 하지만 **Effective Dart**는 공용 API에 타입 주석을 사용하는 것을 권장합니다:

```dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

### 화살표 함수

함수가 단일 표현식만을 포함할 경우, 화살표(`=>`) 구문을 사용할 수 있습니다. 이는 `{ return expr; }`의 축약형입니다.

```dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

**주의**: 화살표 구문 뒤에는 표현식만 올 수 있으며, 문(statement)은 올 수 없습니다.

## 매개변수

### 명명된 매개변수

명명된 매개변수는 선택 사항이며, 중괄호 `{}`로 감쌉니다. `required` 키워드를 사용하여 필수로 만들 수도 있습니다.

```dart
void enableFlags({bool? bold, bool? hidden}) {

}

void enableFlagsWithDefaults({bool bold = false, bool hidden = false}) {

}

void enableFlagsRequired({required bool bold, required bool hidden}) {

}
```

함수를 호출할 때는 매개변수 이름을 명시해야 합니다:

```dart
enableFlags(bold: true, hidden: false);
enableFlagsWithDefaults(bold: true);
enableFlagsRequired(bold: true, hidden: true);
```

### 선택적 위치 매개변수

선택적 위치 매개변수는 대괄호 `[]`로 감싸며, 기본값을 지정할 수 있습니다.

```dart
String say(String from, String msg, [String? device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}

String sayWithDefaultDevice(String from, String msg, [String device = 'carrier pigeon']) {
  return '$from says $msg with a $device';
}
```

함수를 호출할 때는 선택적 매개변수를 위치에 따라 제공해야 합니다:

```dart
print(say('Bob', 'Howdy')); // 출력: Bob says Howdy
print(say('Bob', 'Howdy', 'smoke signal')); // 출력: Bob says Howdy with a smoke signal

print(sayWithDefaultDevice('Bob', 'Howdy')); // 출력: Bob says Howdy with a carrier pigeon
```

## 함수 타입

함수는 타입을 가질 수 있습니다. 함수 타입은 인자의 타입과 반환 타입을 명시하여 정의합니다.

```dart
void greet(String name, {String greeting = 'Hello'}) =>
    print('$greeting $name!');

// 함수 타입을 변수에 할당
void Function(String, {String greeting}) g = greet;

g('Dash', greeting: 'Howdy'); // 출력: Howdy Dash!
```

**함수 타입은 함수 시그니처를 명확하게 표현**하므로, 코드의 가독성과 재사용성을 높입니다.

## 익명 함수

익명 함수는 이름이 없는 함수로, 람다 또는 클로저라고도 불립니다.

```dart
const list = ['apples', 'bananas', 'oranges'];

var uppercaseList = list.map((item) {
  return item.toUpperCase();
}).toList();

for (var item in uppercaseList) {
  print('$item: ${item.length}');
}
```

단 하나의 표현식만을 포함하는 경우, 화살표 함수를 사용할 수 있습니다:

```dart
var uppercaseList = list.map((item) => item.toUpperCase()).toList();
uppercaseList.forEach((item) => print('$item: ${item.length}'));
```

## 레이클렉스 클로저

함수가 자신의 외부 범위에 있는 변수에 접근할 수 있는 경우, 그 함수는 클로저가 됩니다.

```dart
/// [makeAdder]는 [addBy]를 캡처하여 함수가 반환될 때도 유지합니다.
Function makeAdder(int addBy) {
  return (int i) => addBy + i;
}

void main() {
  var add2 = makeAdder(2);
  var add4 = makeAdder(4);
  
  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
```

## 테어오프

테어오프는 함수, 메서드 또는 생성자를 호출하지 않고 참조할 때 생성되는 클로저입니다.

```dart
var charCodes = [68, 97, 114, 116];

// 함수 테어오프
charCodes.forEach(print);

// 메서드 테어오프
var buffer = StringBuffer();
charCodes.forEach(buffer.write);
```

테어오프는 람다보다 간결하게 함수를 참조할 수 있습니다.

## 함수의 동등성 테스트

함수는 그 자체로 동등성을 가집니다. 같은 인스턴스를 참조하면 동등하나, 다른 인스턴스는 동등하지 않습니다.

```dart
void foo() {}

class A {
  static void bar() {}
  void baz() {}
}

void main() {
  Function x;
  
  // 최상위 함수 비교
  x = foo;
  assert(foo == x);
  
  // 정적 메서드 비교
  x = A.bar;
  assert(A.bar == x);
  
  // 인스턴스 메서드 비교
  var v = A();
  var w = A();
  var y = w;
  x = w.baz;
  
  // 같은 인스턴스를 참조하므로 동등함
  assert(y.baz == x);
  
  // 다른 인스턴스를 참조하므로 동등하지 않음
  assert(v.baz != w.baz);
}
```

## 반환 값

모든 함수는 값을 반환합니다. 반환 타입을 명시하지 않으면 `null`을 반환합니다.

```dart
foo() {}

assert(foo() == null);
```

여러 값을 반환하려면 레코드를 사용할 수 있습니다.

```dart
(String, int) foo() {
  return ('something', 42);
}
```

## 제너레이터

제너레이터 함수는 일련의 값을 **지연(lazy)**하게 생성합니다. Dart는 **동기**와 **비동기**제너레이터를 지원합니다.

### 동기 제너레이터

동기 제너레이터는 `sync*` 키워드와 `yield`를 사용하여 `Iterable`을 반환합니다.

```dart
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}
```

### 비동기 제너레이터

비동기 제너레이터는 `async*` 키워드와 `yield`를 사용하여 `Stream`을 반환합니다.

```dart
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```

**재귀적인 제너레이터는 `yield*`를 사용**하여 성능을 향상시킬 수 있습니다.

```dart
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
```

## 외부 함수

외부 함수는 선언과 구현이 분리된 함수로, `external` 키워드를 사용하여 정의합니다. 주로 다른 언어와의 인터롭에 사용됩니다.

```dart
external void someFunc(int i);
```

`external` 함수는 Dart 외부에서 구현되며, C나 JavaScript와 같은 다른 언어에서 제공됩니다. 플랫폼에 따라 구현이 다르므로, 해당 인터롭 문서를 참고해야 합니다.

**외부 함수는 다양한 형태로 정의될 수 있습니다:**

- 최상위 함수
- 정적 메서드
- 인스턴스 메서드
- 게터/세터



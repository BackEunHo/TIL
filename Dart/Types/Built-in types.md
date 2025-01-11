# [Dart Built-in Types](https://dart.dev/language/built-in-types)

Dart 언어는 다양한 내장 타입을 지원하며, 이들은 프로그램에서 기본적인 데이터 구조를 나타내는 데 사용됩니다.

## Numbers

- **int**: 정수 값을 나타내며, 플랫폼에 따라 64비트 크기를 가집니다. 네이티브 플랫폼에서는 -2<sup>63</sup>부터 2<sup>63</sup>-1까지의 값을 가질 수 있습니다.
  
  ```dart
  var x = 1;
  var hex = 0xDEADBEEF;
  ```

- **double**: 64비트 부동 소수점 숫자를 나타냅니다.
  
  ```dart
  var y = 1.1;
  var exponents = 1.42e5;
  ```

- **num**: `int`와 `double`의 상위 타입으로, 두 타입의 값을 모두 가질 수 있습니다.
  
  ```dart
  num x = 1;
  x += 2.5;
  ```

## Strings

Dart의 문자열은 UTF-16 코드 유닛의 시퀀스를 보유하는 `String` 객체입니다. 문자열은 작은따옴표 또는 큰따옴표로 생성할 수 있습니다.

```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
```

- **문자열 보간**: `${expression}`을 사용하여 문자열 내에 표현식의 값을 삽입할 수 있습니다. 표현식이 식별자인 경우 중괄호를 생략할 수 있습니다.
  ```dart
  var name = 'Alice';
  var greeting = 'Hello, $name!'; // 중괄호 생략
  print(greeting); // 출력: Hello, Alice!

  var age = 30;
  var message = 'Next year, you will be ${age + 1} years old.';
  print(message); // 출력: Next year, you will be 31 years old.
  ```

- **멀티라인 문자열**: 삼중 따옴표를 사용하여 여러 줄에 걸친 문자열을 생성할 수 있습니다.

  ```dart
  var s1 = '''
  You can create
  multi-line strings like this one.
  ''';
  
  var s2 = """This is also a
  multi-line string.""";
  ```

- **Raw 문자열**: 문자열 앞에 `r`을 붙여 이스케이프 시퀀스를 무시할 수 있습니다.
  
  ```dart
  var s = r'In a raw string, not even \n gets special treatment.';
  print(s); // 출력: In a raw string, not even \n gets special treatment.
  ```

## Booleans

Dart는 `bool` 타입을 사용하여 불리언 값을 나타냅니다. `true`와 `false` 두 가지 리터럴이 있으며, 모두 컴파일 타임 상수입니다.

```dart
var isTrue = true;
var isFalse = false;
```

- **타입 안전성**: Dart의 타입 안전성 덕분에, 불리언이 아닌 값을 조건문에 사용할 수 없습니다. 대신 명시적으로 값을 검사해야 합니다.
  
  ```dart
  var fullName = '';
  assert(fullName.isEmpty); // fullName이 비어 있는지 확인
  ```

## Runes and Grapheme Clusters

Dart에서 런(루네)은 문자열의 유니코드 코드 포인트를 나타냅니다. `characters` 패키지를 사용하면 사용자 인지 문자(그래페메 클러스터)를 쉽게 다룰 수 있습니다.

- **런 사용 예시**:
  
  ```dart
  var heart = '\u2665'; // ♥
  var smile = '\u{1F600}'; // 😀
  print(heart); // 출력: ♥
  print(smile); // 출력: 😀
  ```

- **`characters` 패키지 사용**:
  
  ```dart
  import 'package:characters/characters.dart';
  
  void main() {
    var hi = 'Hi 🇩🇰';
    print(hi); // 출력: Hi 🇩🇰
    print('The end of the string: ${hi.substring(hi.length - 1)}'); // 출력: The end of the string: ??
    print('The last character: ${hi.characters.last}'); // 출력: The last character: 🇩🇰
  }
  ```

## Symbols

`Symbol` 객체는 Dart 프로그램에서 선언된 연산자나 식별자를 나타냅니다. 

```dart
var symbol1 = #radix;
var symbol2 = #bar;
print(symbol1); // 출력: Symbol("radix")
print(symbol2); // 출력: Symbol("bar")
```

심볼 리터럴은 컴파일 타임 상수입니다.

## Object

`Object`는 Dart에서 모든 클래스의 슈퍼클래스입니다. `Null`을 제외한 모든 객체는 `Object`의 인스턴스입니다.

```dart
Object obj = 'Hello';
print(obj); // 출력: Hello

obj = 123;
print(obj); // 출력: 123
```

- **특징**:
  - 모든 클래스는 `Object`를 상속합니다.
  - `Object`는 기본 메서드인 `toString()`, `hashCode`, `==` 등을 제공합니다.

## Enum

`enum`은 관련된 상수 집합을 정의하는 데 사용됩니다.

```dart
enum Color { red, green, blue }

void main() {
  var favoriteColor = Color.blue;
  print(favoriteColor); // 출력: Color.blue
}
```

- **특징**:
  - 각 열거형 값은 `enum` 타입의 인스턴스입니다.
  - 열거형은 `index`와 `toString()` 메서드를 가집니다.

## Future and Stream

- **Future**: 비동기 작업의 결과를 나타냅니다.

  ```dart
  Future<String> fetchUserOrder() {
    return Future.delayed(Duration(seconds: 2), () => 'Large Latte');
  }
  
  void main() async {
    print('Fetching user order...');
    var order = await fetchUserOrder();
    print('Your order is: $order'); // 2초 후 출력: Your order is: Large Latte
  }
  ```

- **Stream**: 비동기 데이터의 연속적인 흐름을 나타냅니다.

  ```dart
  Stream<int> timedCounter(int to) async* {
    for (int i = 1; i <= to; i++) {
      await Future.delayed(Duration(seconds: 1));
      yield i;
    }
  }
  
  void main() async {
    await for (var value in timedCounter(3)) {
      print(value); // 1초 간격으로 1, 2, 3 출력
    }
  }
  ```

## Iterable

`Iterable`은 반복 가능한 컬렉션을 나타냅니다. `for-in` 루프나 `forEach` 메서드와 함께 사용됩니다.

```dart
var list = ['apple', 'banana', 'cherry'];
for (var fruit in list) {
  print(fruit);
}
// 출력:
// apple
// banana
// cherry
```

- **특징**:
  - 다양한 컬렉션 클래스(`List`, `Set`, `Map` 등의 `keys`와 `values`)가 `Iterable`을 구현합니다.
  - `Iterable`은 `map`, `where`, `reduce` 등 다양한 고차 함수 메서드를 제공합니다.

## Never

`Never` 타입은 절대 값을 반환하지 않는 함수의 반환 타입으로 사용됩니다. 주로 예외를 던지거나 무한 루프를 실행하는 함수에서 사용됩니다.

```dart
Never fail() {
  throw Exception('This function always fails');
}

void main() {
  fail(); // 예외 발생
}
```

- **특징**:
  - 함수가 정상적으로 종료되지 않음을 나타냅니다.
  - 타입 체크에서 특정 타입과의 관계를 명확히 할 수 있습니다.

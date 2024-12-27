# Dart Built-in Types

Dart 언어는 다양한 내장 타입을 지원하며, 이들은 프로그램에서 기본적인 데이터 구조를 나타내는 데 사용됩니다.

## Numbers

- **int**: 정수 값을 나타내며, 플랫폼에 따라 64비트 크기를 가집니다. 네이티브 플랫폼에서는 -2^63부터 2^63-1까지의 값을 가질 수 있습니다.
  
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
  assert(fullName.isEmpty);
  ```
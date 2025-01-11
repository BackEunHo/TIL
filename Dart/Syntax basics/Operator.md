# Dart Operators

Dart는 다양한 연산자를 지원하며, 이들은 표현식을 구성하는 데 사용됩니다. 아래는 주요 연산자와 그 설명입니다.

## 산술 연산자

Dart는 일반적인 산술 연산자를 지원합니다.
| 연산자 | 의미                                    |
| ------ | --------------------------------------- |
| +      | 더하기                                  |
| \-     | 빼기                                    |
| \-_expr_ | 단항 마이너스 (부호 반전)              |
| \*     | 곱하기                                  |
| /      | 나누기 (결과는 double)                  |
| \~/    | 나누기 (결과는 int)                     |
| %      | 나머지                                  |

예제:

```dart
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5); // 결과는 double
assert(5 ~/ 2 == 2); // 결과는 int
assert(5 % 2 == 1); // 나머지
```

## 타입 테스트 연산자

`as`, `is`, `is!` 연산자는 런타임에 타입을 검사하는 데 유용합니다.
| 연산자 | 의미                                    |
| ------ | --------------------------------------- |
| as     | 타입 캐스트                                                          |
| is     | 객체가 지정된 타입인지 확인                                          |
| is!    | 객체가 지정된 타입이 아닌지 확인                                     |

예제:

```dart
if (employee is Person) {
  employee.firstName = 'Bob';
}
dynamic value = 'Hello';
String text = value as String; // Cast value to String
print(text); // Output: Hello

var number = 42;
if (number is! String) {
  print('number is not a String.'); // Output: number is not a String.
}
```

## 할당 연산자

값을 할당할 때 `=` 연산자를 사용합니다. 변수에 값이 null일 때만 할당하려면 `??=` 연산자를 사용합니다.

```dart
a = value;
b ??= value; // b가 null일 때만 value 할당
```

## 논리 연산자

불리언 표현식을 반전하거나 결합할 때 논리 연산자를 사용합니다.
| 연산자 | 의미                                    |
| ------ | --------------------------------------- |
| !_expr_ | 표현식 반전 (false를 true로, 반대도 동일) |
| \|\|    | 논리 OR                                 |
| &&     | 논리 AND                                |

## 비트 및 시프트 연산자

Dart에서는 숫자의 개별 비트를 조작할 수 있습니다. 일반적으로 정수와 함께 사용됩니다.

| 연산자 | 의미                                    |
| ------ | --------------------------------------- |
| &      | AND                                     |
| \|     | OR                                      |
| ^      | XOR                                     |
| \~_expr_ | 단항 비트 반전                         |
| <<     | 왼쪽 시프트                             |
| \>>    | 오른쪽 시프트                           |
| \>>>   | unsigned 오른쪽 시프트                  |

예제:

```dart
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // 왼쪽 시프트
assert((value >> 4) == 0x02); // 오른쪽 시프트
```

## 조건부 표현식

Dart는 간결하게 표현식을 평가할 수 있는 두 가지 연산자를 제공합니다.

- 삼항 연산자: `condition ? expr1 : expr2`
- null 병합 연산자: `expr1 ?? expr2`

예제:

```dart
var visibility = isPublic ? 'public' : 'private'; // isPublic이 true면 visibility는 'public', false면 'private'
String playerName(String? name) => name ?? 'Guest'; // name이 null이면 'Guest'를 반환, 아니면 name을 반환
```

## 캐스케이드 표기법

캐스케이드(`..`, `?..`)를 사용하면 동일한 객체에 대해 일련의 작업을 수행할 수 있습니다.

예제:

```dart
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
```

## 스프레드 연산자

스프레드 연산자는 컬렉션을 평가하고, 결과 값을 다른 컬렉션에 삽입합니다.

예제:

```dart
var list = [1, 2, 3];
var newList = [0, ...list];
```


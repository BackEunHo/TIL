# [Dart Pattern Types](https://dart.dev/language/patterns)

Dart의 패턴은 값의 형태를 검사하고, 값을 구성 요소로 분해하는 데 사용되는 다양한 유형을 제공합니다. 각 패턴 유형은 특정한 매칭 및 디스트럭처링 작업을 수행하며, 이를 통해 코드의 가독성과 효율성을 높일 수 있습니다. 이 문서에서는 Dart의 주요 패턴 유형들을 상세히 설명하고, 각 패턴에 대한 코드 예시를 제공합니다.

## 목차

- [Logical-or](#logical-or)
- [Logical-and](#logical-and)
- [Relational](#relational)
- [Cast](#cast)
- [Null-check](#null-check)
- [Null-assert](#null-assert)
- [Constant](#constant)
- [Variable](#variable)
- [Identifier](#identifier)
- [Parenthesized](#parenthesized)
- [List](#list)
- [Rest element](#rest-element)
- [Map](#map)
- [Record](#record)
- [Object](#object)
- [Wildcard](#wildcard)

## Logical-or

**Logical-or 패턴**은 여러 패턴 중 하나와 일치하는지를 확인합니다. 두 개 이상의 패턴을 `||` 연산자로 결합하여 사용합니다.

### 예제

```dart
void checkColor(Color color) {
  switch (color) {
    case Color.red || Color.blue || Color.green:
      print('Primary color');
    default:
      print('Other color');
  }
}

enum Color { red, blue, green, yellow, purple }

void main() {
  checkColor(Color.red);    // 출력: Primary color
  checkColor(Color.yellow); // 출력: Other color
}
```

## Logical-and

**Logical-and 패턴**은 모든 패턴과 일치해야 합니다. Dart에서는 명시적으로 `&&` 연산자를 패턴에 사용할 수 없지만, 복합 조건을 통해 유사한 효과를 낼 수 있습니다.

### 예제

```dart
void checkNumber(int number) {
  if (number > 0 && number < 10) {
    print('Single-digit positive number');
  } else {
    print('Other number');
  }
}

void main() {
  checkNumber(5);  // 출력: Single-digit positive number
  checkNumber(15); // 출력: Other number
}
```

## Relational

**Relational 패턴**은 값이 특정 관계를 만족하는지를 확인합니다. 비교 연산자 (`>`, `<`, `>=`, `<=`)를 사용하여 패턴을 정의합니다.

### 예제

```dart
void categorizeNumber(int number) {
  switch (number) {
    case > 0:
      print('Positive number');
    case < 0:
      print('Negative number');
    default:
      print('Zero');
  }
}

void main() {
  categorizeNumber(10); // 출력: Positive number
  categorizeNumber(-5); // 출력: Negative number
  categorizeNumber(0);  // 출력: Zero
}
```

## Cast

**Cast 패턴**은 값의 타입을 특정 타입으로 캐스팅할 때 사용됩니다. `as` 키워드를 사용하여 타입을 명시적으로 지정할 수 있습니다.

### 예제

```dart
void handleObject(Object obj) {
  if (obj case String s) {
    print('String value: $s');
  } else if (obj case int i) {
    print('Integer value: $i');
  }
}

void main() {
  handleObject('Hello'); // 출력: String value: Hello
  handleObject(42);      // 출력: Integer value: 42
}
```

## Null-check

**Null-check 패턴**은 값이 `null`인지 확인하는 데 사용됩니다. `?` 연산자를 사용하여 null 가능성을 처리할 수 있습니다.

### 예제

```dart
void greet(String? name) {
  switch (name) {
    case String n:
      print('Hello, $n!');
    case null:
      print('Hello, stranger!');
  }
}

void main() {
  greet('Alice'); // 출력: Hello, Alice!
  greet(null);    // 출력: Hello, stranger!
}
```

## Null-assert

**Null-assert 패턴**은 값이 `null`이 아님을 단언(assert)합니다. `!` 연산자를 사용하여 null을 배제할 수 있습니다.

### 예제

```dart
void printLength(String? text) {
  if (text case String t) {
    print('Length: ${t.length}');
  } else {
    print('Text is null');
  }
}

void main() {
  printLength('Dart'); // 출력: Length: 4
  printLength(null);    // 출력: Text is null
}
```

## Constant

**Constant 패턴**은 값이 특정 상수와 일치하는지를 확인합니다. `const` 키워드를 사용하여 상수를 정의할 수 있습니다.

### 예제

```dart
const int maxScore = 100;

void checkScore(int score) {
  switch (score) {
    case maxScore:
      print('Perfect score!');
    default:
      print('Keep trying!');
  }
}

void main() {
  checkScore(100); // 출력: Perfect score!
  checkScore(85);  // 출력: Keep trying!
}
```

## Variable

**Variable 패턴**은 값을 변수에 바인딩합니다. 패턴 매칭 과정에서 값을 분해하여 변수에 할당할 수 있습니다.

### 예제

```dart
void describeNumber(int number) {
  switch (number) {
    case int n when n > 0:
      print('$n is positive');
    case int n when n < 0:
      print('$n is negative');
    default:
      print('$number is zero');
  }
}

void main() {
  describeNumber(10);  // 출력: 10 is positive
  describeNumber(-5);  // 출력: -5 is negative
  describeNumber(0);   // 출력: 0 is zero
}
```

## Identifier

**Identifier 패턴**은 특정 식별자와 일치하는지를 확인합니다. 변수 이름을 지정하여 값을 참조할 수 있습니다.

### 예제

```dart
void identifyValue(Object obj) {
  switch (obj) {
    case String name:
      print('String with name: $name');
    case int age:
      print('Integer with age: $age');
    default:
      print('Unknown type');
  }
}

void main() {
  identifyValue('Bob'); // 출력: String with name: Bob
  identifyValue(25);    // 출력: Integer with age: 25
}
```

## Parenthesized

**Parenthesized 패턴**은 패턴을 괄호로 감싸서 그룹화합니다. 이는 복합 패턴을 명확하게 표현하는 데 유용합니다.

### 예제

```dart
void processPoint(Object point) {
  if (point case (int x, int y)) {
    print('Point at ($x, $y)');
  } else {
    print('Not a valid point');
  }
}

void main() {
  processPoint((10, 20)); // 출력: Point at (10, 20)
  processPoint('Not a point'); // 출력: Not a valid point
}
```

## List

**List 패턴**은 리스트의 구조를 검사하고, 요소를 분해합니다. 패턴 내에서 리스트의 각 요소에 대한 패턴을 정의할 수 있습니다.

### 예제

```dart
void analyzeList(List<dynamic> items) {
  switch (items) {
    case [int a, String b, bool c]:
      print('List with int, String, bool: $a, $b, $c');
    default:
      print('Different list structure');
  }
}

void main() {
  analyzeList([1, 'Dart', true]); // 출력: List with int, String, bool: 1, Dart, true
  analyzeList(['Dart', 2, false]); // 출력: Different list structure
}
```

## Rest element

**Rest element 패턴**은 리스트나 맵에서 나머지 요소를 추출하는 데 사용됩니다. `...` 연산자를 사용하여 나머지를 캡처할 수 있습니다.

### 예제

```dart
void summarizeList(List<int> numbers) {
  switch (numbers) {
    case [int first, ...int rest]:
      print('First: $first, Rest: $rest');
    default:
      print('Empty list');
  }
}

void main() {
  summarizeList([10, 20, 30, 40]); // 출력: First: 10, Rest: [20, 30, 40]
  summarizeList([]); // 출력: Empty list
}
```

## Map

**Map 패턴**은 맵의 키와 값의 구조를 검사하고, 특정 키에 대한 값을 분해합니다.

### 예제

```dart
void describeMap(Map<String, dynamic> data) {
  if (data case {'name': String name, 'age': int age}) {
    print('Name: $name, Age: $age');
  } else {
    print('Invalid data structure');
  }
}

void main() {
  describeMap({'name': 'Alice', 'age': 30}); // 출력: Name: Alice, Age: 30
  describeMap({'name': 'Bob', 'age': 'unknown'}); // 출력: Invalid data structure
}
```

## Record

**Record 패턴**은 레코드 타입의 값을 분해하고, 필드에 접근합니다. 레코드의 각 필드에 대한 패턴을 정의할 수 있습니다.

### 예제

```dart
void handleRecord((String, int) record) {
  switch (record) {
    case ('Alice', 30):
      print('Alice is 30 years old');
    case (String name, int age):
      print('$name is $age years old');
    default:
      print('Unknown record');
  }
}

void main() {
  handleRecord(('Alice', 30)); // 출력: Alice is 30 years old
  handleRecord(('Bob', 25)); // 출력: Bob is 25 years old
}
```

## Object

**Object 패턴**은 객체의 특정 프로퍼티나 메서드를 검사하고, 이를 변수에 바인딩합니다. 객체의 구조와 일치하는지 확인할 수 있습니다.

### 예제

```dart
class Person {
  final String name;
  final int age;

  Person(this.name, this.age);
}

void describePerson(Object obj) {
  if (obj case Person(:String name, :int age)) {
    print('Person: $name, Age: $age');
  } else {
    print('Not a Person');
  }
}

void main() {
  var alice = Person('Alice', 30);
  describePerson(alice); // 출력: Person: Alice, Age: 30
  describePerson('Not a person'); // 출력: Not a Person
}
```

## Wildcard

**Wildcard 패턴**은 특정 값이나 타입을 무시하고 일치 여부만 확인합니다. `_`를 사용하여 와일드카드를 표현할 수 있습니다.

### 예제

```dart
void handleValue(Object value) {
  switch (value) {
    case String _:
      print('A string value');
    case int _:
      print('An integer value');
    default:
      print('Unknown type');
  }
}

void main() {
  handleValue('Hello'); // 출력: A string value
  handleValue(42);      // 출력: An integer value
  handleValue(3.14);    // 출력: Unknown type
}
```
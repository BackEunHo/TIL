# [Dart 제네릭](https://dart.dev/language/generics)

Dart의 제네릭(Generic)은 타입 매개변수를 사용하여 다양한 타입에 대해 재사용 가능한 코드를 작성할 수 있게 해주는 강력한 기능입니다. 제네릭을 사용하면 코드의 타입 안전성을 유지하면서도 유연성을 높일 수 있습니다.

## 제네릭이란?

제네릭은 클래스, 인터페이스, 함수 등을 정의할 때 특정 타입에 의존하지 않고 유연하게 사용할 수 있도록 하는 기능입니다. 타입 매개변수를 사용하여 다양한 타입을 처리할 수 있으며, 컴파일 시점에 타입이 결정됩니다.

### 예제

```dart
// 제네릭 클래스를 정의합니다.
class Box<T> {
  T content;

  Box(this.content);

  void display() {
    print('Box contains: $content');
  }
}

void main() {
  var intBox = Box<int>(123);
  intBox.display(); // 출력: Box contains: 123

  var stringBox = Box<String>('Hello, Dart!');
  stringBox.display(); // 출력: Box contains: Hello, Dart!
}
```

## 제네릭을 사용하는 이유

### 타입 안전성

제네릭을 사용하면 다양한 타입을 지원하면서도 컴파일 시점에 타입 오류를 미리 발견할 수 있어, 런타임 오류를 줄일 수 있습니다.

```dart
List<String> names = ['Alice', 'Bob', 'Charlie'];
// names.add(42); // 오류: int를 String 리스트에 추가할 수 없습니다.
```

### 코드 재사용성

제네릭을 사용하면 동일한 코드 구조를 다양한 타입에 대해 재사용할 수 있어, 중복 코드를 줄일 수 있습니다.

```dart
// 제네릭 함수를 사용하여 두 값을 교환합니다.
(T, U) swap<T, U>(T a, U b) {
  return (b, a);
}

void main() {
  var swapped = swap<int, String>(1, 'one');
  print(swapped); // 출력: (one, 1)
}
```

### 유연성

제네릭은 다양한 타입을 지원하면서도 특정 타입에 대한 제한을 걸어 유연하게 사용할 수 있습니다.

## 제네릭 사용법

### 제네릭 클래스

제네릭 클래스를 정의할 때는 클래스 이름 뒤에 `<T>`와 같이 타입 매개변수를 지정합니다.

```dart
class Pair<T, U> {
  T first;
  U second;

  Pair(this.first, this.second);
}

void main() {
  var pair = Pair<int, String>(1, 'one');
  print('First: ${pair.first}, Second: ${pair.second}');
}
```

### 제네릭 함수

제네릭 함수를 정의할 때는 함수 이름 앞에 `<T>`와 같이 타입 매개변수를 지정합니다.

```dart
T getFirst<T>(List<T> items) {
  return items.first;
}

void main() {
  var numbers = [1, 2, 3];
  var firstNumber = getFirst<int>(numbers);
  print(firstNumber); // 출력: 1

  var words = ['apple', 'banana', 'cherry'];
  var firstWord = getFirst<String>(words);
  print(firstWord); // 출력: apple
}
```

### 제네릭 컬렉션

Dart의 컬렉션 타입(`List`, `Set`, `Map` 등)은 기본적으로 제네릭으로 설계되어 있어, 특정 타입의 데이터를 저장하도록 지정할 수 있습니다.

```dart
List<int> integers = [1, 2, 3];
Set<String> strings = {'apple', 'banana', 'cherry'};
Map<String, int> ages = {'Alice': 30, 'Bob': 25};
```

### 제네릭 생성자

제네릭 클래스의 생성자에서도 타입 매개변수를 사용할 수 있습니다.

```dart
class Container<T> {
  T value;

  Container(this.value);

  void update(T newValue) {
    value = newValue;
  }
}

void main() {
  var container = Container<String>('Initial');
  container.update('Updated');
  print(container.value); // 출력: Updated
}
```

## 제네릭과 클래스/레코드의 차이점

### 클래스와 제네릭 클래스

- **제네릭 클래스**는 다양한 타입을 지원하여 코드 재사용성을 높입니다.
- **클래스**는 특정 타입에 종속적일 수 있어, 타입에 따라 여러 클래스를 만들어야 할 수도 있습니다.

```dart
// 제네릭 클래스
class Box<T> {
  T content;

  Box(this.content);
}

// 특정 타입을 위한 클래스
class IntBox {
  int content;

  IntBox(this.content);
}

class StringBox {
  String content;

  StringBox(this.content);
}
```

### 레코드와 제네릭

레코드는 여러 타입의 값을 묶어 반환하거나 전달하는 간단한 데이터 구조로, 제네릭은 클래스나 함수 등에서 타입 매개변수를 사용하는 기능입니다. 레코드는 익명이고 불변이며, 제네릭은 타입 안전성과 코드 재사용성을 제공합니다.

```dart
// 레코드 예제
(String, int) getUserInfo() {
  return ('Alice', 30);
}

// 제네릭 클래스 예제
class Pair<T, U> {
  T first;
  U second;

  Pair(this.first, this.second);
}
```

## 제네릭 사용 시기

### 다중 반환

함수에서 여러 값을 반환하고자 할 때 제네릭 레코드를 사용하면 유용합니다.

```dart
(String, int) getUserInfo() {
  return ('Alice', 30);
}

void main() {
  var userInfo = getUserInfo();
  print('Name: ${userInfo.$1}, Age: ${userInfo.$2}');
}
```

### 타입 제한

특정 타입의 서브타입만 허용하도록 타입 매개변수를 제한할 때 제네릭을 사용합니다.

```dart
class Repository<T extends Entity> {
  void add(T entity) {
    // 구현
  }
}

class Entity {
  final String id;
  Entity(this.id);
}

class User extends Entity {
  final String name;
  User(String id, this.name) : super(id);
}

void main() {
  var userRepository = Repository<User>();
  userRepository.add(User('1', 'Alice'));
}
```

### 코드 재사용

동일한 로직을 다양한 타입에 대해 재사용하고자 할 때 제네릭을 사용합니다.

```dart
class Stack<T> {
  final List<T> _items = [];

  void push(T item) {
    _items.add(item);
  }

  T pop() {
    return _items.removeLast();
  }

  bool get isEmpty => _items.isEmpty;
}

void main() {
  var intStack = Stack<int>();
  intStack.push(1);
  intStack.push(2);
  print(intStack.pop()); // 출력: 2

  var stringStack = Stack<String>();
  stringStack.push('a');
  stringStack.push('b');
  print(stringStack.pop()); // 출력: b
}
```
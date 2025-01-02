# [Typedefs](https://dart.dev/language/typedefs)

Dart에서 타입 별칭은 주로 `typedef` 키워드를 사용하여 선언되며, 이를 통해 타입을 간단하게 참조할 수 있습니다. 특히 함수 타입을 정의할 때 유용하게 사용됩니다.

## Typedef가 무엇인가요?

`typedef`는 기존 타입에 대한 새로운 이름을 정의할 때 사용되는 구문입니다. 주로 함수 타입에 대한 별칭을 만들기 위해 사용됩니다. 이를 통해 코드의 가독성을 높이고, 동일한 함수 시그니처를 여러 곳에서 재사용할 수 있습니다.

### 예제

```dart
typedef IntList = List<int>;

void main() {
  IntList il = [1, 2, 3];
  print(il); // 출력: [1, 2, 3]
}
```

위 예제에서 `IntList`는 `List<int>` 타입의 별칭으로 정의되었습니다. 이를 통해 코드를 더 간결하고 명확하게 작성할 수 있습니다.

## 타입 별칭의 타입 매개변수

타입 별칭은 타입 매개변수를 가질 수 있습니다. 이는 제네릭 타입을 사용하는 것과 유사하게, 다양한 타입을 지원할 수 있게 합니다.

### 예제

```dart
typedef ListMapper<X> = Map<X, List<X>>;

void main() {
  Map<String, List<String>> m1 = {}; // 포괄적이고 자세한 타입 표기
  ListMapper<String> m2 = {}; // 더 간결하고 명확한 표기
}
```

여기서 `ListMapper<X>`는 `Map<X, List<X>>` 타입의 별칭입니다. 이를 통해 복잡한 타입 표기를 간단하게 만들 수 있습니다.

## 역사적 참고 사항

`Dart 2.13` 이전에는 `typedef`는 함수 타입에만 제한되어 있었습니다. `Dart 2.13`부터는 `typedef`가 더 유연하게 사용될 수 있으며, 다양한 상황에서 타입 별칭을 정의할 수 있습니다.

> **주의**: `Dart 2.13` 이전의 코드를 사용할 경우, 함수 타입 이외의 `typedef`는 지원되지 않습니다.

## 함수 Typedef 예제

함수 타입을 위한 `typedef`는 함수의 시그니처를 간단하게 정의하고 재사용할 수 있게 해줍니다.

### 예제

```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  Compare<int> compareFunction = sort;
  print(compareFunction(10, 5)); // 출력: 5
}
```

위 예제에서 `Compare<T>`는 두 개의 `T` 타입 인자를 받아 `int`를 반환하는 함수 타입의 별칭입니다. 이를 통해 함수의 시그니처를 더 간결하게 정의하고, 여러 곳에서 재사용할 수 있습니다.

## 제네릭 타입과의 관계

제네릭 타입과 `typedef`를 함께 사용하면 더욱 유연하고 재사용 가능한 코드를 작성할 수 있습니다.

### 예제

```dart
typedef Callback<T> = void Function(T value);

void executeCallback<T>(Callback<T> callback, T value) {
  callback(value);
}

void main() {
  Callback<String> printString = (s) => print(s);
  executeCallback<String>(printString, 'Hello, Dart!');
  // 출력: Hello, Dart!
}
```

위 예제에서 `Callback<T>`는 `void Function(T)` 타입의 별칭으로 정의되었습니다. `executeCallback` 함수는 `Callback<T>` 타입의 함수를 받아 실행합니다. 이를 통해 다양한 타입에 맞춰 재사용 가능한 콜백 함수를 정의할 수 있습니다.

## 유형 제한된 Typedef

`typedef`는 구체적인 타입 제한과 함께 사용할 수 있어, 특정 타입 또는 타입의 서브타입만을 허용하는 기능을 제공할 수 있습니다.

### 예제

```dart
typedef EntityCallback<E extends Entity> = void Function(E entity);

class Entity {
  final String id;
  Entity(this.id);
}

class User extends Entity {
  final String name;
  User(String id, this.name) : super(id);
}

void handleEntity(EntityCallback<User> callback) {
  User user = User('1', 'Alice');
  callback(user);
}

void main() {
  handleEntity((user) {
    print('User ID: ${user.id}, Name: ${user.name}');
  });
  // 출력: User ID: 1, Name: Alice
}
```

위 예제에서 `EntityCallback<E extends Entity>`는 `Entity`의 서브타입인 `E` 타입을 인자로 받는 함수 타입의 별칭입니다. 이를 통해 특정 타입에 대한 콜백만을 허용할 수 있습니다.

## `typedef`와 클래스/레코드와의 차이점

### `typedef` vs. 클래스

- **재사용성**: `typedef`는 주로 함수 타입의 재사용을 위해 사용되며, 클래스로는 표현할 수 없는 함수 시그니처의 별칭을 제공합니다.
- **간결성**: 클래스는 데이터와 메서드를 포함할 수 있지만, `typedef`는 단지 타입의 별칭을 제공하여 더 간결하게 사용됩니다.

### `typedef` vs. 레코드

- **목적**: `typedef`는 특정 타입의 별칭을 생성하여 코드 재사용성을 높이는데 사용되고, 레코드는 여러 타입의 값을 하나의 객체로 묶는 데 사용됩니다.
- **용도**: `typedef`는 함수 시그니처나 복잡한 유형의 간결한 표현을 제공하고, 레코드는 데이터를 간결하게 그룹화하는 데 사용됩니다.

```dart
// typedef 예제
typedef IntList = List<int>;

IntList createIntList() {
  return [1, 2, 3];
}

// 레코드 예제
(String, int) getUserInfo() {
  return ('Alice', 30);
}
```

## `typedef` 사용 시기

- **함수 타입의 재사용**: 동일한 함수 시그니처를 여러 곳에서 사용해야 할 때 유용합니다.
  
  ```dart
  typedef Predicate<T> = bool Function(T item);
  
  bool isPositive(int number) => number > 0;
  
  void filterList(List<int> items, Predicate<int> test) {
    return items.where(test).toList();
  }
  ```
  
- **콜백 함수 정의**: 콜백 함수를 정의하고 재사용할 때, 함수 타입의 재사용성을 보장할 수 있습니다.
  
  ```dart
  typedef Callback<T> = void Function(T value);
  
  void performOperation<T>(Callback<T> callback, T value) {
    callback(value);
  }
  ```

- **복잡한 타입 간소화**: 복잡한 제네릭 타입을 간결하게 표현하고 싶을 때 사용합니다.
  
  ```dart
  typedef Mapper<A, B> = B Function(A a);
  
  Mapper<int, String> intToString = (a) => a.toString();
  ```


# Dart Records

Dart의 Records는 익명, 불변, 집합 타입으로, 여러 객체를 하나의 객체로 묶을 수 있게 해줍니다. 다른 컬렉션 타입과 달리, Records는 고정 크기, 이질적이며, 타입이 지정됩니다.

## Record의 특징

- **익명성**: Records는 이름이 없는 타입으로, 간단하게 여러 값을 묶을 수 있습니다.
- **불변성**: Records는 생성 후 변경할 수 없습니다.
- **구조적 타입**: 필드의 타입과 이름에 따라 타입이 결정됩니다.

## Record 사용법

### Record 생성

- **문법**: 괄호 안에 쉼표로 구분된 필드를 사용해 Records를 생성합니다.

  ```dart
  var record = ('first', a: 2, b: true, 'last');
  ```

### Record 타입 주석

- **타입 주석**: 필드의 타입을 괄호 안에 쉼표로 구분하여 지정할 수 있습니다.

  ```dart
  (int, int) swap((int, int) record) {
    var (a, b) = record;
    return (b, a);
  }
  ```

### 필드 접근

- **내장 getter**: 필드는 내장 getter를 통해 접근할 수 있습니다. 위치 필드는 `$<position>` 형식으로 접근합니다.

  ```dart
  var record = ('first', a: 2, b: true, 'last');
  print(record.$1); // 'first'
  print(record.a);  // 2
  print(record.b);  // true
  print(record.$2); // 'last'
  ```


## Record와 Class 비교

### Record

- **간단하고 가벼운 데이터 구조**: 이름 없는 여러 값을 묶는 데 적합합니다.
- **불변성**: 기본적으로 불변으로 작동합니다.
- **상속 불가**: 간단한 데이터 묶음으로 설계되었기 때문에 상속을 지원하지 않습니다.
- **생명주기**: 임시로 데이터를 다루거나 함수 간 데이터를 전달할 때 사용하면 유용합니다.

### Class

- **상속 가능**: 코드 재사용성을 높일 수 있습니다.
- **참조 기반 객체**: 같은 객체를 여러 위치에서 공유하거나, 객체 속성을 수정하는 기능을 제공합니다.
- **데이터 생명주기**: 전역적으로 유지해야 하거나, 긴 생명주기를 가지는 데이터에 적합합니다.
- **캡슐화 및 복잡한 구조**: 데이터와 동작을 결합할 수 있습니다.

## Record 사용 시기

- **다중 반환**: 함수에서 여러 값을 반환할 때 유용합니다.
- **간단한 데이터 묶음**: 간단한 데이터 그룹을 빠르게 처리할 때 적합합니다.

### 예제: 다중 반환

```dart
(String name, int age) getUserInfo() {
  return (42, 'Alice');
}

void main() {
  var userInfo = getUserInfo();
  print(userInfo.$1); // 42
  print(userInfo.$2); // Alice
}
```

Records는 간단한 데이터 구조를 빠르게 처리하고, 여러 값을 효율적으로 반환하는 데 유용합니다.

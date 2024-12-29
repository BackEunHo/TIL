# [Dart Collections](https://dart.dev/language/collections)

Dart의 컬렉션은 리스트, 세트, 맵과 같은 데이터 구조를 제공하여 데이터를 효율적으로 관리하고 조작할 수 있게 해줍니다. 각 컬렉션 타입은 고유한 특성과 사용법을 가지고 있습니다.

## Lists

- **정의**: 리스트는 순서가 있는 객체의 그룹으로, 배열과 유사합니다. Dart에서 리스트는 `List` 객체로 표현됩니다.
- **리터럴**: 리스트 리터럴은 대괄호 `[]`로 감싸고, 쉼표로 구분된 값들로 구성됩니다.

  ```dart
  var list = [1, 2, 3];
  ```

- **특징**: 리스트는 0부터 시작하는 인덱스를 사용하며, `.length` 속성을 통해 리스트의 길이를 얻을 수 있습니다.

  ```dart
  var list = [1, 2, 3];
  assert(list.length == 3);
  assert(list[1] == 2);
  ```

- **상수 리스트**: `const` 키워드를 사용하여 컴파일 타임 상수 리스트를 만들 수 있습니다.

  ```dart
  var constantList = const [1, 2, 3];
  ```

## Sets

- **정의**: 세트는 순서가 없는 고유한 아이템의 컬렉션입니다. Dart에서 세트는 `Set` 타입으로 표현됩니다.
- **리터럴**: 세트 리터럴은 중괄호 `{}`로 감싸고, 쉼표로 구분된 값들로 구성됩니다.

  ```dart
  var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
  ```

- **특징**: 세트는 중복된 값을 허용하지 않으며, `.length` 속성을 통해 아이템의 수를 얻을 수 있습니다.

  ```dart
  var elements = <String>{};
  elements.add('fluorine');
  elements.addAll(halogens);
  assert(elements.length == 5);
  ```

- **상수 세트**: `const` 키워드를 사용하여 컴파일 타임 상수 세트를 만들 수 있습니다.

  ```dart
  final constantSet = const {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
  ```

## Maps

- **정의**: 맵은 키와 값의 쌍으로 구성된 객체입니다. 각 키는 고유하며, 값은 중복될 수 있습니다.
- **리터럴**: 맵 리터럴은 중괄호 `{}`로 감싸고, 콜론 `:`으로 구분된 키-값 쌍으로 구성됩니다.

  ```dart
  var gifts = {
    'first': 'partridge',
    'second': 'turtledoves',
    'fifth': 'golden rings'
  };
  ```

- **특징**: 맵은 키를 사용하여 값을 검색하며, `.length` 속성을 통해 키-값 쌍의 수를 얻을 수 있습니다.

  ```dart
  var gifts = {'first': 'partridge'};
  assert(gifts['first'] == 'partridge');
  ```

- **상수 맵**: `const` 키워드를 사용하여 컴파일 타임 상수 맵을 만들 수 있습니다.

  ```dart
  final constantMap = const {
    2: 'helium',
    10: 'neon',
    18: 'argon',
  };
  ```

## Operators

### Spread Operators

- **정의**: 스프레드 연산자 `...`와 널 인식 스프레드 연산자 `...?`는 리스트, 맵, 세트 리터럴에 여러 값을 삽입하는 간결한 방법을 제공합니다.

  ```dart
  var list = [1, 2, 3];
  var list2 = [0, ...list];
  assert(list2.length == 4);
  ```

### Control-flow Operators

- **Collection `if`**: 조건에 따라 컬렉션에 요소를 추가할 수 있습니다. 특정 조건이 참일 때만 컬렉션에 요소를 포함시키고 싶을 때 유용합니다.

  ```dart
  bool promoActive = true;
  var nav = ['Home', 'Furniture', 'Plants', if (promoActive) 'Outlet'];
  // 'Outlet'은 promoActive가 true일 때만 포함됩니다.
  ```

- **Collection `for`**: 반복적으로 컬렉션에 요소를 추가할 수 있습니다. 다른 컬렉션의 요소를 기반으로 새로운 컬렉션을 만들고 싶을 때 유용합니다.

  ```dart
  var listOfInts = [1, 2, 3];
  var listOfStrings = ['#0', for (var i in listOfInts) '#$i'];
  // listOfStrings는 ['#0', '#1', '#2', '#3']가 됩니다.
  ```


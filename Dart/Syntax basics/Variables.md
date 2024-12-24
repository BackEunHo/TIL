# Dart Variables

Dart에서 변수는 객체에 대한 참조를 저장합니다. 변수의 타입은 명시적으로 선언할 수도 있고, `var` 키워드를 사용하여 Dart가 타입을 추론하도록 할 수도 있습니다.

## Null Safety

Dart는 null 안전성을 보장합니다. 이는 null 참조 오류를 방지하며, 컴파일 시점에 이러한 오류를 감지합니다. 변수 타입에 `?`를 추가하여 null을 허용할 수 있습니다.

```dart
String? name; // Nullable type
String name = 'My Name';  // Non-nullable type
```

## Default Value

초기화되지 않은 nullable 변수는 기본적으로 `null` 값을 가집니다. non-nullable 변수는 사용 전에 반드시 초기화해야 합니다.

```dart
int? lineCount;
assert(lineCount == null);

int lineCount = 0;
```

## Late Variables

`late` 키워드는 변수를 선언 후 초기화하거나, 지연 초기화할 때 사용됩니다. 이는 주로 top-level 변수나 인스턴스 변수를 초기화할 때 유용합니다.

```dart
late String description;

void main() {
  description = 'Feijoada!';
  print(description);
}
```

## Final and Const

- `final`: 런타임에 한 번만 설정할 수 있는 변수입니다.
- `const`: 컴파일 타임에 값이 결정되는 상수로, `final`과 달리 객체와 그 필드 모두 변경할 수 없습니다.

```dart
final name = 'Bob';
const bar = 1000000;
```

`const`는 상수 값을 생성하는 데도 사용됩니다.

```dart
const Object i = 3;
const list = [i as int];
const map = {if (i is int) i: 'int'};
const set = {if (list is List<int>) ...list};
```


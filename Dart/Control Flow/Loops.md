# [Loops](https://dart.dev/language/loops)

Dart에서 루프는 코드의 흐름을 제어하고 반복적인 작업을 수행하는 데 사용됩니다. 루프를 효과적으로 사용하면 효율적이고 가독성 높은 코드를 작성할 수 있습니다. 이 문서에서는 `for` 루프, `while` 및 `do-while` 루프, 그리고 `break`와 `continue`를 사용하는 방법을 다룹니다.

## 목차

- [For 루프](#for-루프)
- [While 및 Do-While 루프](#while-및-do-while-루프)
- [Break 및 Continue](#break-및-continue)
- [중첩 루프](#중첩-루프)
- [레이블이 있는 루프](#레이블이-있는-루프)

## For 루프

`for` 루프는 반복 횟수가 명확할 때 유용하게 사용됩니다. 기본적인 `for` 루프의 구조는 다음과 같습니다:

```dart
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
print(message); // 출력: Dart is fun!!!!
```

### 콜백 캡처 방지

Dart의 `for` 루프 내에서 클로저는 인덱스의 **값**을 캡처하여 JavaScript에서 흔히 발생하는 문제를 방지합니다. 예를 들어:

```dart
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}

for (final c in callbacks) {
  c(); // 출력: 0 그리고 1
}
```

JavaScript에서는 이 코드가 `2`와 `2`를 출력할 수 있으나, Dart에서는 올바르게 `0`과 `1`을 출력합니다.

### For-in 루프

만약 반복할 컬렉션의 인덱스가 필요 없을 경우, `for-in` 루프를 사용하여 코드의 가독성을 높일 수 있습니다:

```dart
for (final candidate in candidates) {
  candidate.interview();
}
```

패턴을 사용하여 `for-in` 루프에서 값을 디스트럭처링할 수도 있습니다:

```dart
class Candidate {
    final String name;
    final int yearsExperience;
    Candidate({required this.name, required this.yearsExperience});
}
void main() {
var candidates = [
    Candidate(name: 'Alice', yearsExperience: 5),
    Candidate(name: 'Bob', yearsExperience: 3),
    Candidate(name: 'Charlie', yearsExperience: 7),
];
for (final Candidate(:name, :yearsExperience) in candidates) {
        print('$name has $yearsExperience years of experience.');
    }
}
/*
Alice has 5 years of experience.
Bob has 3 years of experience.
Charlie has 7 years of experience.
*/
```
익숙하지 않은 이름을 사용할 필요 없이, 패턴을 통해 직접 변수에 값을 할당할 수 있습니다.

### Iterable의 forEach 메서드

`Iterable` 클래스는 `forEach` 메서드를 제공하여 컬렉션의 각 요소에 대해 함수를 실행할 수 있습니다:

```dart
var collection = [1, 2, 3];
collection.forEach(print); // 출력: 1, 2, 3
```

## While 및 Do-While 루프

### While 루프

`while` 루프는 루프가 시작되기 전에 조건을 평가합니다. 조건이 참인 동안 루프 본문이 실행됩니다:

```dart
while (!isDone()) {
  doSomething();
}
```

### Do-While 루프

`do-while` 루프는 루프 본문을 먼저 실행하고, 그 후에 조건을 평가합니다. 최소한 한 번은 루프 본문이 실행됩니다:

```dart
do {
  printLine();
} while (!atEndOfPage());
```

## Break 및 Continue

### Break

`break` 문은 현재 실행 중인 루프를 즉시 종료하고 루프 밖으로 벗어납니다:

```dart
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
```

### Continue

`continue` 문은 현재 반복을 건너뛰고 다음 반복으로 넘어갑니다:

```dart
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

대신, `Iterable`을 사용하는 경우 다음과 같이 작성할 수 있습니다:

```dart
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());
```

`where` 메서드를 사용하면 조건에 맞는 요소만 필터링하여 `forEach`로 처리할 수 있습니다.

## 중첩 루프

중첩 루프는 다차원 데이터 구조를 반복할 때 유용합니다. 예를 들어, 2차원 리스트를 순회하는 경우:

```dart
for (var i = 0; i < outerLimit; i++) {
  for (var j = 0; j < innerLimit; j++) {
    print('i: $i, j: $j');
  }
}
```

## 레이블이 있는 루프

레이블이 있는 루프는 중첩 루프에서 특정 루프를 명시적으로 제어할 때 사용됩니다. 예를 들어, 외부 루프를 종료하거나 건너뛰고 싶을 때:

```dart
outerLoop:
for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    if (j == 1) continue outerLoop;
    print('i: $i, j: $j');
  }
}
```

위 예제에서 `continue outerLoop;`는 내부 루프를 건너뛰고 외부 루프의 다음 반복으로 넘어갑니다.

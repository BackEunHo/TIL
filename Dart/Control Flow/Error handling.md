# [에러 처리](https://dart.dev/language/error-handling)

Dart 코드에서는 예외를 던지고(throw) 잡을 수 있습니다. 예외는 예상치 못한 일이 발생했을 때 발생하는 오류입니다. 예외가 잡히지 않으면 예외를 발생시킨 isolate가 중단되고, 일반적으로 해당 isolate와 프로그램이 종료됩니다.
> isolate는 독립적인 실행 단위로, 각 isolate는 자체적인 메모리 공간과 스레드를 가지고 있습니다. 다른 isolate는 서로 메모리를 공유하지 않아 병렬 처리를 안전하게 수행할 수 있습니다.
Dart의 모든 예외는 체크되지 않은 예외입니다. 메서드는 던질 수 있는 예외를 선언하지 않으며, 예외를 잡을 필요도 없습니다.

Dart는 `Exception`과 `Error` 타입을 제공하며, 다양한 사전 정의된 하위 타입도 있습니다. 물론, 자체적인 예외를 정의할 수도 있습니다. 그러나 Dart 프로그램은 `Exception`과 `Error` 객체뿐만 아니라, 비-널 객체를 예외로 던질 수 있습니다.

## 예외 던지기 (Throw)

예외를 던지는 방법의 예시는 다음과 같습니다:

```dart
throw FormatException('Expected at least 1 section');
```

임의의 객체도 던질 수 있습니다:

```dart
throw 'Out of llamas!';
```

> **참고:** 프로덕션 수준의 코드에서는 보통 `Error`나 `Exception`을 구현하는 타입을 던집니다.

예외를 표현식으로 사용할 수 있으므로, `=>` 문이나 표현식을 허용하는 다른 모든 곳에서 예외를 던질 수 있습니다:

```dart
void distanceTo(Point other) => throw UnimplementedError();
```

## 예외 잡기 (Catch)

예외를 잡으면 예외의 전파가 중단됩니다(예외를 다시 던지지 않는 한). 예외를 잡으면 이를 처리할 기회를 가집니다:

```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  buyMoreLlamas();
}
```

여러 종류의 예외를 잡으려면, 여러 개의 catch 절을 사용할 수 있습니다. 첫 번째로 일치하는 catch 절이 예외를 처리합니다. 타입을 명시하지 않는 catch 절은 모든 타입의 예외를 처리할 수 있습니다:

```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // 특정 예외
  buyMoreLlamas();
} on Exception catch (e) {
  // 다른 모든 Exception 타입
  print('Unknown exception: $e');
} catch (e) {
  // 타입이 명시되지 않아 모든 예외 처리
  print('Something really unknown: $e');
}
```

`on`은 예외 타입을 지정할 때 사용하고, `catch`는 예외 객체가 필요할 때 사용합니다.

`catch`에는 하나 또는 두 개의 매개변수를 지정할 수 있습니다. 첫 번째는 던져진 예외이고, 두 번째는 스택 트레이스(`StackTrace` 객체)입니다.

```dart
try {
  // ...
} on Exception catch (e) {
  print('Exception details:\n $e');
} catch (e, s) {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
```

예외를 부분적으로 처리하고 전파하려면 `rethrow` 키워드를 사용합니다.

```dart
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // 런타임 오류
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    rethrow; // 호출자가 예외를 볼 수 있도록 함
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
```

## 최종 처리 (Finally)

예외가 발생하든 아니든 일부 코드가 항상 실행되도록 하려면 `finally` 절을 사용합니다. 일치하는 `catch` 절이 없다면, 예외는 `finally` 절이 실행된 후 전파됩니다:

```dart
try {
  breedMoreLlamas();
} finally {
  // 예외 발생 여부와 관계없이 클린업(리소스 정리 수행)
  cleanLlamaStalls();
}
```

`finally` 절은 일치하는 모든 `catch` 절 이후에 실행됩니다:

```dart
try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // 먼저 예외 처리
} finally {
  cleanLlamaStalls(); // 그 다음 클린업
}
```

## Assert

개발 중에는 `assert(<조건>, <옵션 메시지>);` 문을 사용하여 불리언 조건이 거짓일 경우 실행을 중단할 수 있습니다.

```dart
// 변수에 비-널 값이 있는지 확인
assert(text != null);

// 값이 100 미만인지 확인
assert(number < 100);

// https URL인지 확인
assert(urlString.startsWith('https'));
```

메시지를 추가하려면 두 번째 인수로 문자열을 전달합니다:

```dart
assert(urlString.startsWith('https'),
    'URL ($urlString) should start with "https".');
```

`assert`의 첫 번째 인수는 불리언 값을 반환하는 표현식이어야 합니다. 조건이 참이면 계속 실행하고, 거짓이면 예외(`AssertionError`)가 발생합니다.

**언제 assert가 작동하나요?**

- Flutter는 디버그 모드에서 assert를 활성화합니다.
- `webdev serve` 같은 개발 전용 도구는 기본적으로 assert를 활성화합니다.
- `dart run`과 `dart compile js`는 `--enable-asserts` 플래그를 통해 assert를 지원합니다.

프로덕션 코드에서는 assert가 무시되며, `assert`의 인수는 평가되지 않습니다.
# 분기(Branches)

Dart 코드의 흐름을 제어하는 방법은 다음과 같습니다:

* `if` 문과 요소
* `if-case` 문과 요소
* `switch` 문과 표현식

또한 Dart에서 제어 흐름을 다음과 같이 조작할 수 있습니다:

* `for` 및 `while`과 같은 루프
* `try`, `catch`, `throw`와 같은 예외 처리

## If

```dart
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```

조건 표현식에서 `if`를 사용하는 방법은 다음과 같습니다:

```dart
var visibility = isPublic ? 'public' : 'private';
// 실행 결과:
// isPublic이 true면 'public', false면 'private'이 할당됩니다.

var weather = 
    if (isRaining()) 'rainy'
    else if (isSnowing()) 'snowy'
    else 'sunny';
// 실행 결과:
// 비가 오면 'rainy'
// 눈이 오면 'snowy'
// 둘 다 아니면 'sunny'가 할당됩니다.

print(if (isLoggedIn) 'Welcome back!' else 'Please log in');
// 실행 결과:
// isLoggedIn이 true면 'Welcome back!'
// false면 'Please log in'을 출력합니다.
```

### If-case

Dart의 `if` 문은 패턴 뒤에 오는 `case` 절을 지원합니다:

```dart
if (pair case [int x, int y]) return Point(x, y);
// 실행 결과:
// pair이 [int x, int y] 패턴과 일치하면 Point(x, y)를 반환합니다.
```

패턴이 값과 일치하면, 패턴이 정의하는 변수들이 범위 내에서 사용할 수 있습니다.

```dart
if (pair case [int x, int y]) {
  print('좌표 배열은 $x,$y 입니다.');
} else {
  throw FormatException('유효하지 않은 좌표입니다.');
}
// 실행 결과:
// pair이 [int x, int y]와 일치하면 '좌표 배열은 x,y 입니다.'를 출력합니다.
// 일치하지 않으면 '유효하지 않은 좌표입니다.' 예외를 발생시킵니다.
```

if-case 문은 단일 패턴과 일치하고 구조를 해체하는 방법을 제공합니다. 여러 패턴을 테스트하려면 `switch`를 사용하세요.

> _참고: if 문에서 `case` 절을 사용하려면 최소 언어 버전 3.0이 필요합니다._

## Switch 문

`switch` 문은 값 표현식을 일련의 `case`와 비교합니다. 각 `case` 절은 값과 일치할 패턴입니다. 모든 종류의 패턴을 `case`로 사용할 수 있습니다.

값이 `case`의 패턴과 일치하면 `case` 본문이 실행됩니다. 비어있지 않은 `case` 절은 완료 후 switch의 끝으로 점프합니다. `break` 문이 필요하지 않습니다. 다른 유효한 종료 방법은 `continue`, `throw`, `return`입니다.

`default` 또는 와일드카드 `_` 절을 사용하여 어떤 `case`도 일치하지 않는 경우 코드를 실행할 수 있습니다:

```dart
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  case 'APPROVED':
    executeApproved();
    break;
  case 'DENIED':
    executeDenied();
    break;
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}
// 실행 결과:
// command가 'OPEN'이면 executeOpen()이 실행됩니다.
// 어떤 case도 일치하지 않으면 executeUnknown()이 실행됩니다.
```

빈 `case`들은 다음 `case`로 넘어갑니다. `case`들이 본문을 공유할 수 있습니다. 건너뛰지 않고 빈 `case`를 사용하려면 `break`를 사용하세요. 비순차적으로 fall-through하려면 `continue`와 레이블을 사용하세요:

```dart
switch (command) {
  case 'OPEN':
    executeOpen();
    continue newCase; // newCase 레이블로 계속 실행합니다.

  case 'DENIED': // 빈 case는 다음 case로 넘어갑니다.
  case 'CLOSED':
    executeClosed(); // 'DENIED'와 'CLOSED' 모두에 대해 실행됩니다.

  newCase:
  case 'PENDING':
    executeNowClosed(); // 'OPEN'과 'PENDING' 모두에 대해 실행됩니다.
}
// 실행 결과:
// 'OPEN'일 경우 executeOpen() 후 executeNowClosed() 실행
// 'DENIED' 또는 'CLOSED'일 경우 executeClosed() 실행
// 'PENDING'일 경우 executeNowClosed() 실행
```

### Switch 표현식

`switch` 표현식은 일치하는 `case`의 표현식 본문에 따라 값을 생성합니다. `switch` 표현식은 Dart가 표현식을 허용하는 모든 곳에서 사용할 수 있습니다. 단, 표현식 문장의 시작점에서는 사용할 수 없습니다:

```dart
var x = switch (y) { 
  // 패턴과 결과
};

print(switch (x) { 
  // 패턴과 결과
});

return switch (x) { 
  // 패턴과 결과
};
// 실행 결과:
// y에 따라 x를 설정하고, x를 출력 및 반환합니다.
```

switch 표현식을 사용하면 switch 문을 다음과 같이 표현할 수 있습니다:

```dart
// slash, star, comma, semicolon 등은 상수 변수입니다...
token = switch (charCode) {
  slash || star || plus || minus => operator(charCode),
  comma || semicolon => punctuation(charCode),
  >= digit0 && <= digit9 => number(),
  _ => throw FormatException('Invalid')
};
// 실행 결과:
// charCode에 따라 token을 할당합니다.
```

`switch` 표현식의 문법은 switch 문과 다릅니다:

- `case`는 `case` 키워드로 시작하지 않습니다.
- `case` 본문은 일련의 문장이 아닌 단일 표현식입니다.
- 각 `case`는 본문을 가져야 합니다. 빈 `case`는 fall-through하지 않습니다.
- `case` 패턴들은 `:` 대신 `=>`를 사용해 본문과 분리됩니다.
- `case`는 `,`로 구분됩니다. (선택적 trailing `,` 허용)
- `default` case는 `_`만 사용할 수 있습니다. `default`와 `_` 둘 다 허용되지 않습니다.

> _참고: switch 표현식은 최소 언어 버전 3.0이 필요합니다._

### Exhaustiveness checking

exhaustiveness checking은 값이 switch에 들어갔을 때 어떤 `case`와도 일치하지 않을 가능성이 있는 경우 컴파일 타임 에러를 보고하는 기능입니다.

```dart
// nullableBool이 null 가능성이 있어 null case가 없는 비완전 switch:
switch (nullableBool) {
  case true:
    print('yes');
    break;
  case false:
    print('no');
    break;
}
// 실행 결과:
// nullableBool이 true면 'yes', false면 'no' 출력
// null인 경우 컴파일 에러 발생
```

`default` case(`default` 또는 `_`)는 switch에 들어갈 수 있는 모든 값을 커버합니다. 이는 어떤 타입의 switch도 완전하게 만듭니다.

enum과 sealed 타입은 어떤 `default` case도 없어도 switch의 가능한 값이 알려져 있고 완전히 열거 가능하기 때문에 특히 유용합니다. `sealed` 수정자를 클래스에 사용하면 그 클래스의 서브타입을 전환할 때 exhaustiveness checking이 활성화됩니다:

```dart
sealed class Shape {}

class Square implements Shape {
  final double length;
  Square(this.length);
}

class Circle implements Shape {
  final double radius;
  Circle(this.radius);
}

double calculateArea(Shape shape) => switch (shape) {
      Square(length: var l) => l * l,
      Circle(radius: var r) => math.pi * r * r
    };
// 실행 결과:
// Shape의 서브타입에 따라 면적을 계산합니다.
// Square인 경우 length * length
// Circle인 경우 π * radius^2
```

누군가 `Shape`의 새로운 서브타입을 추가한다면 이 `switch` 표현식은 완전하지 않습니다. exhaustiveness checking이 누락된 서브타입을 알려줍니다. 이는 Dart를 어느 정도 함수형 대수형 데이터 타입 스타일로 사용할 수 있게 합니다.

## Guard clause

`case` 절 뒤에 옵셔널 guard clause를 설정하려면 `when` 키워드를 사용하세요. guard clause는 `if case` 뒤에 올 수 있고, `switch` 문과 표현식 모두에 올 수 있습니다.

```dart
// Switch 문:
switch (something) {
  case somePattern when some || boolean || expression:
    //             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Guard clause.
    body;
}

// Switch 표현식:
var value = switch (something) {
  somePattern when some || boolean || expression => body,
  //               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Guard clause.
};

// If-case 문:
if (something case somePattern when some || boolean || expression) {
  //                           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Guard clause.
  body;
}
// 실행 결과:
// 각각의 switch와 if-case에서 패턴 매칭 후 guard clause를 평가합니다.
// guard clause가 true면 body를 실행하고, false면 다음 case로 넘어갑니다.
```

guards는 패턴 매칭 후 임의의 boolean 표현식을 평가합니다. 이는 어떤 `case` 본문이 실행되어야 하는지에 대한 추가 제약을 추가할 수 있게 합니다. guard clause가 false로 평가되면, switch 전체를 종료하지 않고 다음 `case`로 넘어갑니다.
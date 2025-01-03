# [Dart 타입 시스템](https://dart.dev/language/type-system)

Dart 언어는 타입 세이프(type safe)하며, 이는 변수의 값이 변수의 정적 타입과 항상 일치하도록 보장하는 **타입 시스템**을 사용합니다. 타입 시스템은 정적 타입 검사와 런타임 검사를 결합하여, 타입 안정성을 유지합니다. 타입은 필수이지만, 타입 주석은 타입 추론 덕분에 선택적입니다.

## Soundness란 무엇인가요?

**Soundness(타입의 건전성)**는 프로그램이 특정 잘못된 상태에 빠지지 않도록 보장하는 개념입니다. 건전한 타입 시스템은 표현식의 정적 타입이 런타임에 실제로 평가되는 값의 타입과 일치하도록 보장합니다. 예를 들어, 표현식의 정적 타입이 `String`이라면, 런타임에 해당 표현식은 항상 `String` 값을 반환하게 됩니다.

Dart의 타입 시스템은 Java와 C#과 유사하게 건전성을 유지합니다. 이는 정적 검사와 런타임 검사를 통해 보장되며, 예를 들어 `String`을 `int`에 할당하는 것은 컴파일 타임 오류를 발생시키고, 객체를 잘못된 타입으로 캐스팅하면 런타임 오류를 발생시킵니다.

## Soundness의 장점

타입 시스템의 건전성은 여러 가지 이점을 제공합니다:

- **타입 관련 버그 조기 발견**: 컴파일 타임에 타입 오류를 발견하여, 런타임 버그를 줄일 수 있습니다.
- **가독성 향상**: 코드가 명확해져, 값이 실제로 지정된 타입을 가지고 있다고 신뢰할 수 있습니다.
- **유지보수성 향상**: 코드의 일부분을 변경할 때, 타입 시스템이 다른 관련 부분에 영향을 미치는지 경고해줍니다.
- **효율적인 AOT 컴파일**: 건전한 타입 시스템은 사전 컴파일(Ahead-of-Time) 최적화를 가능하게 하여, 생성된 코드의 효율성을 높입니다.

## 정적 분석을 통과하기 위한 팁

Dart에서 정적 타입 검사를 통과하기 위해 몇 가지 규칙을 따르는 것이 중요합니다:

### 메서드를 오버라이드할 때 건전한 반환 타입 사용

서브클래스에서 상위 클래스의 메서드를 오버라이드할 때, 반환 타입은 상위 클래스 메서드의 반환 타입과 같거나 그 하위 타입이어야 합니다.

#### 예제

```dart
class Animal {
  Animal get parent => ...;
}

class HoneyBadger extends Animal {
  @override
  HoneyBadger get parent => ...; // 허용됨
}
```

```dart
class HoneyBadger extends Animal {
  @override
  Root get parent => ...; // 오류: 'Root'는 'Animal'의 하위 타입이 아닙니다.
}
```

### 메서드를 오버라이드할 때 건전한 매개변수 타입 사용

오버라이드하는 메서드의 매개변수 타입은 상위 클래스 메서드의 매개변수 타입과 같거나 그 상위 타입이어야 합니다. 이를 통해 타입 안전성을 유지할 수 있습니다.

#### 예제

```dart
class Animal {
  void chase(Animal a) { ... }
}

class HoneyBadger extends Animal {
  @override
  void chase(Object a) { ... } // 허용됨
}
```

```dart
class Mouse extends Animal { ... }

class Cat extends Animal {
  @override
  void chase(Mouse a) { ... } // 오류: 'Mouse'는 'Animal'의 하위 타입이지만, 매개변수를 더 좁게 정의할 수 없습니다.
}
```

### 동적 리스트를 타입된 리스트로 사용하지 않기

`dynamic` 타입의 리스트는 여러 타입의 요소를 포함할 수 있지만, 이를 특정 타입의 리스트로 사용할 수 없습니다. 이는 타입 안전성을 위반할 수 있기 때문에 오류를 발생시킵니다.

#### 예제

```dart
void main() {
  List<Cat> foo = <dynamic>[Dog()]; // 오류: 'Dog'는 'Cat'의 하위 타입이 아닙니다.
  List<dynamic> bar = <dynamic>[Dog(), Cat()]; // 허용됨
}
```

## 런타임 검사

런타임 검사는 컴파일 타임에 감지할 수 없는 타입 관련 문제를 다룹니다. 예를 들어, 다음 코드는 `List<Dog>`를 `List<Cat>`으로 캐스팅하려고 할 때 런타임 오류를 발생시킵니다:

```dart
void main() {
  List<Animal> animals = <Dog>[Dog()];
  List<Cat> cats = animals as List<Cat>; // 런타임 오류 발생
}
```

## 타입 추론

Dart의 타입 추론은 필드, 메서드, 로컬 변수, 그리고 대부분의 제네릭 타입 인수에 대해 타입을 유추할 수 있습니다. 충분한 정보가 없을 경우 `dynamic` 타입을 사용합니다.

### 필드 및 메서드 추론

타입이 명시되지 않은 필드나 메서드는 상위 클래스의 타입을 상속받거나, 초기화된 값에서 타입을 유추합니다.

### 정적 필드 추론

정적 필드는 초기화값을 통해 타입이 추론됩니다. 순환 의존성이 있을 경우 추론이 실패할 수 있습니다.

### 로컬 변수 추론

로컬 변수의 타입은 초기화값을 기반으로 유추됩니다. 이후 대입은 고려되지 않으며, 너무 구체적인 타입이 유추되면 타입 주석을 추가할 수 있습니다.

#### 예제

```dart
var x = 3; // x는 int로 유추됨
x = 4.0; // 오류: double을 int에 할당할 수 없습니다.
```

```dart
num y = 3; // y는 num으로 선언됨, double도 할당 가능
y = 4.0; // 허용됨
```

### 타입 인수 추론

타입 인수는 컨텍스트와 인수에서의 정보를 결합하여 유추됩니다. 필요한 경우 타입 인수를 명시적으로 지정할 수 있습니다.

#### 예제

```dart
List<int> listOfInt = [];
var listOfDouble = [3.0];
var ints = listOfDouble.map((x) => x.toInt()); // Iterable<int>로 유추됨
```

## 타입 대체

### 간단한 타입 대체

타입 대체는 소비자와 생산자의 개념을 통해 이해할 수 있습니다. 소비자는 타입을 흡수하고, 생산자는 타입을 생성합니다.

- **소비자 타입 대체**: 소유자의 타입을 슈퍼타입으로 교체할 수 있습니다.
  
  ```dart
  Animal c = Cat(); // 허용됨
  ```

- **생산자 타입 대체**: 생산자의 타입을 서브타입으로 교체할 수 있습니다.
  
  ```dart
  Cat c = MaineCoon(); // 허용됨
  ```

### 제네릭 타입 대체

제네릭 타입에서도 동일한 규칙이 적용됩니다.

#### 예제

```dart
List<MaineCoon> myMaineCoons = [...];
List<Cat> myCats = myMaineCoons; // 허용됨
```

```dart
List<Animal> myAnimals = [...];
List<Cat> myCats = myAnimals; // 오류: List<Animal>을 List<Cat>에 할당할 수 없습니다.
```

## 메서드에서의 타입 대체

메서드를 오버라이드할 때 생산자와 소비자의 규칙이 여전히 적용됩니다.

### 예제

```dart
class Animal {
  void chase(Animal a) { ... }
  Animal get parent => ...;
}

class HoneyBadger extends Animal {
  @override
  void chase(Object a) { ... } // 허용됨
  @override
  HoneyBadger get parent => ...; // 허용됨
}
```
# Dart Metadata

Dart에서 메타데이터는 코드에 추가 정보를 제공하는 데 사용됩니다. 메타데이터 주석은 `@` 문자로 시작하며, 컴파일 타임 상수나 상수 생성자 호출로 구성됩니다.

## 기본 제공 메타데이터

### `@override`

- **용도**: 상위 클래스의 메서드를 재정의할 때 사용됩니다. 코드의 가독성을 높이고, 의도적으로 메서드를 재정의하고 있음을 명시합니다.
- **예제**:

  ```dart
  class Animal {
    void makeSound() {
      print('Animal sound');
    }
  }

  class Dog extends Animal {
    @override
    void makeSound() {
      print('Bark');
    }
  }
  ```

### `@deprecated`

- **용도**: 더 이상 사용되지 않는 요소에 대해 경고를 표시합니다. 새로운 대체 기능이 있을 때 사용됩니다.
- **예제**:

  ```dart
  class OldClass {
    @deprecated
    void oldMethod() {
      print('This method is deprecated');
    }
  }
  ```

### `@pragma`

- **용도**: 컴파일러에 특별한 지시를 내릴 때 사용됩니다. Dart VM에서 Tree Shaking(불필요한 코드 제거)같은 최적화 작업 중 컴파일러가 사용되지 않는 코드라고 판단해 빌드 결과에서 일부 코드를 제외하거나 런타임에서 네이티브 코드로 대체할 . 수 있습니다. 이를 방지하기 위해 @pragma()와 같은 annotation을 사용해 코드를 유지하도록할 수 있습니다. 
- **예제**:

  ```dart
  @pragma('vm:entry-point')
  void entryPoint() {
    print('This is an entry point');
  }
  ```

## 메타데이터 위치

메타데이터는 라이브러리, 클래스, 타입 정의, 타입 매개변수, 생성자, 팩토리, 함수, 필드, 매개변수, 변수 선언, 그리고 import나 export 지시문 앞에 위치할 수 있습니다.
# Dart의 Sound Null Safety란?

Dart의 **Sound Null Safety**는 코드 작성 시점에 변수와 객체의 null 허용 여부를 명시적으로 구분하는 기능입니다. 이를 통해 컴파일 단계에서 null 관련 에러를 방지할 수 있습니다. "Sound"는 Dart의 null safety가 실행 시점에서도 완벽한 신뢰성을 보장한다는 의미입니다.

---

## **Dart의 Null Safety 개념 및 예시**

### 1. **Non-nullable 변수**

변수 타입에 `?`를 붙이지 않으면 해당 변수는 null 값을 가질 수 없습니다.

```dart
void main() {
  int a = 10; // Non-nullable 변수
  a = null;   // 컴파일 오류 발생
}
```

### 2. **Nullable 변수**

타입 뒤에 `?`를 붙이면 null을 허용할 수 있습니다.

```dart
void main() {
  int? b; // Nullable 변수
  b = null; // 정상 작동
}
```

### 3. **Null 체크 및 Null-aware 연산자**

Nullable 변수는 사용하기 전에 null인지 확인하거나, null-safe 연산자를 사용해야 합니다.

```dart
void main() {
  int? c = null;

  // Null 체크
  if (c != null) {
    print(c + 10);
  }

  // Null-aware 연산자
  print(c?.toString() ?? "null 값입니다."); // c가 null이면 "null 값입니다." 출력
}
```

### 4. **Required 키워드와 Late 초기화**

- **required**: 생성자에서 반드시 초기화해야 하는 nullable 값을 선언할 때 사용.
- **late**: 변수를 나중에 초기화하겠다고 명시.

```dart
class Person {
  final String name;
  late int age;

  Person({required this.name});

  void setAge(int value) {
    age = value; // age는 나중에 초기화 가능
  }
}
```

## **Dart Null Safety의 특징**

- **모든 코드에 Null Safety를 강제 적용**: 프로젝트가 Null Safety를 활성화한 경우, 모든 의존성 코드도 Null Safety를 지원해야 합니다.
- **컴파일 단계에서 null 에러 탐지**: 런타임 에러가 아닌 컴파일 단계에서 null 관련 오류를 방지.

### **차이점(컴파일 단계 vs 런타임 단계)**

| **구분** | **컴파일 단계에서의 null 에러 탐지** | **런타임 단계에서의 null 에러 탐지** |
| --- | --- | --- |
| **에러 발생 시점** | 코드 작성 후 컴파일 시점 | 코드 실행 중 |
| **안정성** | 높음 (코드 실행 전 문제를 해결) | 낮음 (사용자에게 직접 영향을 미칠 가능성 있음) |
| **디버깅 난이도** | 쉬움 (컴파일러가 에러 위치를 명확히 제공) | 어려움 (런타임 중 에러 원인 추적 필요) |
| **개발 유연성** | 낮음 (엄격한 규칙 필요) | 높음 (유연하지만 위험성 존재) |

**예시 (Dart)**:

```dart
void main() {
  String? name = null;
  print(name.length); // 컴파일 오류: name이 null일 수 있음
}
```

**예시 (Java)**:

```java
public class Main {
    public static void main(String[] args) {
        String name = null;
        System.out.println(name.length()); // 런타임에 NullPointerException 발생
    }
}
```

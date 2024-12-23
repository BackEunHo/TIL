# Flutter 위젯

Flutter의 위젯은 Flutter 애플리케이션의 기본 구성 요소입니다. 모든 것이 위젯으로 구성되어 있으며, Flutter에서 UI를 구축하는 데 사용됩니다. 위젯은 화면에 표시되는 시각적 요소뿐만 아니라 레이아웃, 제스처, 애니메이션 등을 포함합니다.

## 위젯의 종류

### StatelessWidget
- 상태가 없는 위젯으로, 데이터가 변경되지 않는 경우에 사용됩니다.

```dart
import 'package:flutter/material.dart';

class MyStatelessWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('Hello, Flutter!');
  }
}
```

### StatefulWidget
- 상태가 있는 위젯으로, 데이터가 변경될 수 있는 경우에 사용됩니다.

```dart
import 'package:flutter/material.dart';

class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        Text('Counter: $_counter'),
        ElevatedButton(
          onPressed: _incrementCounter,
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

### InheritedWidget
- 위젯 트리에서 데이터를 공유하기 위해 사용됩니다. 위젯 트리의 특정 지점에서 데이터를 제공하고 하위 위젯에서 이를 접근할 수 있습니다.
데이터가 변경되면 updateShouldNotify 메서드가 호출되어 하위 위젯들이 다시 빌드될지 여부를 결정합니다. 이는 상태 관리 패턴(Provider, Riverpod 등)에서 사용됩니다.

```dart
import 'package:flutter/material.dart';

class MyInheritedWidget extends InheritedWidget {
  final int data;

  MyInheritedWidget({Key? key, required this.data, required Widget child})
      : super(key: key, child: child);

  @override
  bool updateShouldNotify(MyInheritedWidget oldWidget) {
    return oldWidget.data != data;
  }

  static MyInheritedWidget? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>();
  }
}
```

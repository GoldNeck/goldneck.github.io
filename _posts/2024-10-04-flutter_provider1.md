---
layout: post
title:  "Provider - Flutter에서 효율적인 상태 관리를 위한 Riverpod 소개"
summary: "Riverpod을 사용한 Flutter 상태 관리의 개념과 설정 방법 안내."
author: goldtree
date: '2024-10-04 21:33:12 +0900'
category: 'flutter'
tags: DB
#thumbnail: /assets/img/posts/index_3.jpg
isShowThumbnail: false
keywords: Flutter, Riverpod, 상태관리, FlutterRiverpod, 상태관리패키지, Dart, 프로바이더, ProviderScope, StateProvider, Provider, ConsumerWidget, 의존성주입, 테스트용이성, 성능최적화, 코드분리, 전역상태관리, 모듈화, Flutter개발, 모바일개발, 상태관리방법
usemathjax: false
permalink: /blog/sortjoin_2/
---

## Riverpod이란 무엇인가?
Riverpod는 Flutter 애플리케이션에서 상태 관리를 효율적이고 안전하게 할 수 있도록 도와주는 패키지입니다. 
상태 관리는 애플리케이션의 데이터와 UI를 동기화시키는 중요한 부분인데, Riverpod은 이를 보다 간편하게 처리할 수 있게 해줍니다.

주요 특징
안정성: 컴파일 타임에 에러를 감지하여 런타임 에러를 줄입니다.
유연성: 전역 상태 관리, 의존성 주입, 테스트 등을 쉽게 할 수 있습니다.
성능 최적화: 불필요한 리빌드를 최소화하여 효율적인 성능을 제공합니다.
테스트 용이성: 상태를 쉽게 모킹(mocking)하여 테스트할 수 있습니다.
코드 분리: 비즈니스 로직과 UI 코드를 분리하여 코드의 유지보수성을 높입니다.

### 왜 Riverpod을 사용하나요?
Flutter 애플리케이션이 복잡해질수록 상태 관리의 중요성이 커집니다.
Riverpod은 다음과 같은 이유로 많은 개발자들에게 선호됩니다

전역 상태 관리: 앱 어디에서든 상태에 접근하고 변경할 수 있습니다.
의존성 주입: 프로바이더 간의 의존성을 쉽게 관리할 수 있습니다.
코드의 재사용성: 동일한 로직을 여러 위젯에서 재사용할 수 있습니다.
모듈화: 앱의 각 부분을 모듈화하여 개발 및 유지보수를 용이하게 합니다.
안정성과 확장성: 프로젝트가 커져도 안정적으로 확장할 수 있습니다.

### Riverpod 설정하기

##### 프로젝트 생성

```bash
flutter create riverpod_example
```
이 명령어는 riverpod_example이라는 이름의 새로운 Flutter 프로젝트를 생성합니다. 
```bash
cd riverpod_example
```

#### pubspec.yaml에 Riverpod 추가
Flutter 프로젝트에서 Riverpod을 사용하기 위해 pubspec.yaml 파일에 flutter_riverpod 패키지를 추가해야 합니다.

프로젝트의 루트 디렉토리에 있는 pubspec.yaml 파일을 엽니다.

의존성 추가:
dependencies 섹션에 flutter_riverpod을 추가합니다. 최신 버전을 사용하기 위해 pub.dev에서 확인할 수 있습니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_riverpod: ^2.3.6
```
패키지 설치:
변경 사항을 저장한 후, 터미널에서 다음 명령어를 실행하여 패키지를 설치합니다

```bash
flutter pub get
```

#### main.dart 파일 설정
이제 Riverpod을 사용하기 위해 앱의 진입점을 설정해야 합니다. lib/main.dart 파일을 다음과 같이 수정합니다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

void main() {
  runApp(
    ProviderScope(
      child: MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Riverpod 예제',
      home: HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Riverpod 예제'),
      ),
      body: Center(
        child: Text('Riverpod을 사용해보세요!'),
      ),
    );
  }
}
```

ProviderScope: Riverpod의 프로바이더를 사용할 수 있도록 앱을 감싸는 위젯입니다. 앱의 최상위에 위치해야 합니다.
MyApp: 앱의 루트 위젯으로, 기본적인 MaterialApp 설정을 포함합니다.
HomePage: 기본 홈 화면으로, 나중에 Riverpod을 적용할 위젯을 추가할 것입니다.

#### 간단한 예제: 인사말 표시
간단한 예제를 통해 실제로 사용해보겠습니다. 
사용자 이름을 입력하면 인사말을 표시하는 앱을 만들어보겠습니다.

###### (1) 프로바이더 선언
lib/main.dart 파일에 다음과 같이 프로바이더를 선언합니다.

```dart
// 이름을 저장하는 StateProvider
final nameProvider = StateProvider<String>((ref) => '');

// 인사말을 생성하는 Provider
final greetingProvider = Provider<String>((ref) {
  final name = ref.watch(nameProvider);
  return name.isEmpty ? '안녕하세요!' : '안녕하세요, $name님!';
});
```

StateProvider: 간단한 상태를 관리하는 프로바이더입니다. 여기서는 사용자의 이름을 저장합니다.
Provider: 읽기 전용 데이터를 제공하는 프로바이더입니다. 여기서는 nameProvider의 상태를 기반으로 인사말을 생성합니다.

###### (2) HomePage 위젯 수정
HomePage 위젯을 Riverpod을 활용하도록 수정합니다. ConsumerWidget을 사용하여 프로바이더에 접근할 수 있습니다.

```dart
class HomePage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 프로바이더 구독
    final greeting = ref.watch(greetingProvider);
    final nameController = TextEditingController();

    return Scaffold(
      appBar: AppBar(title: Text('Riverpod 예제')),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          children: [
            Text(
              greeting,
              style: TextStyle(fontSize: 24),
            ),
            TextField(
              controller: nameController,
              decoration: InputDecoration(labelText: '이름을 입력하세요'),
            ),
            SizedBox(height: 10),
            ElevatedButton(
              onPressed: () {
                // 이름 업데이트
                ref.read(nameProvider.notifier).state = nameController.text;
              },
              child: Text('인사말 업데이트'),
            ),
          ],
        ),
      ),
    );
  }
}
```
ConsumerWidget: Riverpod의 프로바이더를 사용할 수 있는 위젯입니다.
WidgetRef: 프로바이더에 접근하고 상태를 읽거나 변경할 수 있는 참조입니다.
ref.watch: 프로바이더의 상태를 구독합니다. 상태가 변경되면 위젯이 자동으로 리빌드됩니다.
ref.read: 프로바이더의 상태를 읽거나 변경할 때 사용합니다. 상태 변경 시 리빌드는 발생하지 않습니다.

(3) 전체 코드 정리
최종적으로 lib/main.dart 파일은 다음과 같습니다:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

void main() {
  runApp(
    ProviderScope(
      child: MyApp(),
    ),
  );
}

// 이름을 저장하는 StateProvider
final nameProvider = StateProvider<String>((ref) => '');

// 인사말을 생성하는 Provider
final greetingProvider = Provider<String>((ref) {
  final name = ref.watch(nameProvider);
  return name.isEmpty ? '안녕하세요!' : '안녕하세요, $name님!';
});

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Riverpod 예제',
      home: HomePage(),
    );
  }
}

class HomePage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 프로바이더 구독
    final greeting = ref.watch(greetingProvider);
    final nameController = TextEditingController();

    return Scaffold(
      appBar: AppBar(title: Text('Riverpod 예제')),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          children: [
            Text(
              greeting,
              style: TextStyle(fontSize: 24),
            ),
            TextField(
              controller: nameController,
              decoration: InputDecoration(labelText: '이름을 입력하세요'),
            ),
            SizedBox(height: 10),
            ElevatedButton(
              onPressed: () {
                // 이름 업데이트
                ref.read(nameProvider.notifier).state = nameController.text;
              },
              child: Text('인사말 업데이트'),
            ),
          ],
        ),
      ),
    );
  }
}
```


###### 앱 실행
터미널에서 다음 명령어를 실행하세요:

```bash
flutter run
```

앱이 실행되면 "안녕하세요!"라는 인사말이 표시됩니다.
텍스트 필드에 이름을 입력하고 "인사말 업데이트" 버튼을 누르면, 입력한 이름을 포함한 인사말로 변경됩니다. 
예를 들어, "홍길동"을 입력하면 "안녕하세요, 홍길동님!"으로 변경됩니다.

###### 정리
Riverpod의 기본 개념과 간단한 설정, 그리고 기본적인 Provider 사용법을 학습했습니다.

ProviderScope: Riverpod을 사용하기 위해 앱을 감싸는 위젯입니다.
Provider: 읽기 전용 데이터를 제공하는 프로바이더입니다.
StateProvider: 간단한 상태를 관리하는 프로바이더입니다.
ConsumerWidget: 프로바이더를 구독하고 사용하는 위젯입니다.
ref.watch: 프로바이더의 상태를 구독하여 상태 변화 시 위젯을 리빌드합니다.
ref.read: 프로바이더의 상태를 읽거나 변경할 때 사용합니다.



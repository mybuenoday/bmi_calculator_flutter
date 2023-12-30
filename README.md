주석 모두 삭제

: ctrl + F → 돋보기 왼쪽 `>` 클릭 →  `.*` 클릭 → //.*\n 입력 → Replace All

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/7aee9fce-24c5-4b4a-a39a-63738e374312/35a1cfdb-4647-4fc9-bdd3-896ed3c71766/Untitled.png)

화면이 2개니까 2개의 디렉토리, main과 result

- TextFormField(): 잘못된 정보를 입력했을 때 에러 처리하는 것을 도와준다. Form() 안에 작성한다.
- Form을 쓰면 form에 key를 설정해줘야 한다. `final _formKey = GlobalKey<FormState>();` form의 상태를 알 수 있다.

```dart
import 'package:flutter/material.dart';

class MainScreen extends StatefulWidget {
  const MainScreen({super.key});

  @override
  State<MainScreen> createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: const Text('비만도 계산기'),
        ),
        body: Padding(
          padding: const EdgeInsets.all(8.0),
          child: Form(
            key: _formKey,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.end,
              children: [
                TextFormField(
                    decoration: const InputDecoration(
                      border: OutlineInputBorder(),
                      hintText: '키',
                    ),
                    keyboardType: TextInputType.number,
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return '키를 입력하세요';
                      }
                      return null;
                    }),
                SizedBox(height: 8),
                TextFormField(
                    decoration: const InputDecoration(
                      border: OutlineInputBorder(),
                      hintText: '몸무게',
                    ),
                    keyboardType: TextInputType.number,
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return '몸무게를 입력하세요';
                      }
                      return null;
                    }),
                SizedBox(height: 8),
                ElevatedButton(
                  onPressed: () {
                    if (_formKey.currentState?.validate() ?? false) {}
                  },
                  child: Text('결과'),
                ),
              ],
            ),
          ),
        ));
  }
}
```

StatelessWidget: 약어 stless. 화면에 변경이 없을 때

Live Templates 코드 약어 설정

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/7aee9fce-24c5-4b4a-a39a-63738e374312/f8d39f14-6636-4b55-bfcf-6404d995fe7f/Untitled.png)

변수가 없으면 const, 변수가 들어오면 const가 아니다.

const가 적재적소에 들어가면 성능상 유리하다.

StaefulWidget에서 dispose는 종료되는 시점

앱을 종료하고 나서도 키와 몸무게가 앱에 남아있게 하려면 값이 임시 저장되어야 한다. 외부 패키지를 사용한다.

## [pub.dev](http://pub.dev) - shared_preferences

- installing 보고 terminal에서 명령어로 설치, readme로 사용 방법 익히기
- Future는 오래 걸리는 일을 하는 함수. async를 동반한다. 다른 Future를 동반하는 메서드는 await을 붙여줘야 순서대로 실행한다.

```dart
// 앱이 종료될 때 저장되도록
void dispose() { save();}; Write
Future save() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setDouble('height', double.parse(_heightController.text));
    await prefs.setDouble('weight', double.parse(_weightController.text));
  }
```

- 결과 부분을 눌러서 화면이 넘어가기 전에 저장이 마저 되도록 save() 위치를 고려한다.

```dart
Future load() async {
    final prefs = await SharedPreferences.getInstance();
    final double? height = prefs.getDouble('height');
    final double? weight = prefs.getDouble('weight');
  }
```

- loading은 future를 return하지 않기 때문에 await을 쓰지 않는다.
- 저장하기 전에 load할 경우 값이 없기 때문에 nullable로 double? height
- initState(): 화면이 처음 시작되는 시점, 여기서 load();
- override: (initState(), dispose()처럼) 특정 생명 주기에 특정 동작을 하는 flutter에서 제공되는 메소드를 재정의. 기존에 있는 클래스를 상속받아(extends) 기능을 재정의하면서 화면을 만든다.
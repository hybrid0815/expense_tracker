# expense_tracker

<p align="center">
  <img src="https://github.com/user-attachments/assets/39015954-6751-4d95-b3b1-2e694a8eaa65" width="200"/>
  <img src="https://github.com/user-attachments/assets/dc8a3764-44ef-44d6-b20d-455581e7f096" width="200"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/422b071e-868b-419b-b431-d122eeab3906" width="200"/>
  <img src="https://github.com/user-attachments/assets/d776e529-5f8f-4050-a822-00ca545d31ef" width="200"/>
</p>


## Widget List
- ListView.builder
- Card
- AppBar
- IconButton
- TextField
- Navigator
- showDatePicker
- showModalBottomSheet
- showDialog
- DropdownButton
- DropdownMenuItem
- Dismissible
- ScaffoldMessenger
- SnackBar

## Packages
```bash
flutter pub add uuid
flutter pub add google_fonts
lutter pub add intl
```   


## 생성자에서 파라미터 없이 초기화
```dart
import 'package:uuid/uuid.dart';

const uuid = Uuid();

class Expense {
  final String id;
  final String title;
  final double amount;
  final DateTime date;

  Expense({
    required this.title,
    required this.amount,
    required this.date,
  }) : id = uuid.v4();
}

```

## enum
```dart
enum Ctegory {
  food,
  travel,
  leisure,
  work,
}
```

## ListView.builder
`ListView.builder`로 만들어진 위젯이 `Column`안에 들어가려면 `Expended` 위젯으로 감싸져야 한다.

## Context
다른 위젯과 통신에 필요한 메타데이터를 담고 있다.

## Navigator
현재 위젯을 화면에서 삭제 
```dart
TextButton(
  onPressed: () {
    Navigator.of(context).pop();
  },
  child: const Text("Cancel"),
),
```

## Future
어떤 함수의 결과가 오래 걸리면 다음 진행이 안된다. 이를 막기위해 우선 Future 타입으로 리턴 해두면
다음 프로세스가 진행이 되고 리턴이 되면 프로세스가 돌아와서 그 진행을 이어 간다.


### Future<String[?]> 상태
1. Future 상태 : 아직 값을 모르는 상태. 비동기 작업이 완료되지 않아 결과 값을 기다리고 있는 상태.
2. String 상태 : Future가 완료되어 결과 값을 받은 상태. 이 결과 값은 String 타입으로 되어 있음.
3. Error 상태 : 비동기 작업 중 에러가 발생하여 결과 값 대신 에러를 받은 상태.
4. Null 상태 : Future가 완료되어 결과 값을 받은 상태. 이 결과 값은 Null 타입으로 되어 있음.

### Method 1
showDatePicker 리턴 값을 기다리지 않고 다음 프로세스 진행을 하고 리턴값이 생기면 다시 프로세스가 돌아온다.

```dart
void _showDatePicker() {
  final now = DateTime.now();
  final firstDate = DateTime(now.year - 1, now.month, now.day);

  showDatePicker(
    context: context,
    initialDate: now,
    firstDate: firstDate,
    lastDate: now,
  ).then((selectedDate) => print(selectedDate));
  print("다른 프로세스 진행")
}

```

### Method 2
showDatePicker 리턴 값을 기다리고 다음 프로세스 진행.

```dart
void _showDatePicker() async {
  final now = DateTime.now();
  final firstDate = DateTime(now.year - 1, now.month, now.day);

  final selectedDate = await showDatePicker(
    context: context,
    initialDate: now,
    firstDate: firstDate,
    lastDate: now,
  );

  print(selectedDate)
}
```

# Responsive & Adaptive

## Locking the Device Orientation
```dart
import 'package:flutter/services.dart';

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  SystemChrome.setPreferredOrientations([
    DeviceOrientation.portraitUp,
  ]).then((_) => runApp(const App()));
}
```

## Get Device Size
```dart
@override
  Widget build(BuildContext context) {
    final double width = MediaQuery.of(context).size.width;
    final keyboardSpace = MediaQuery.of(context).viewInsets.bottom;
    ...

    eturn Scaffold(
      appBar: AppBar(
        title: const Text("Expense Tracker"),
        actions: [
          IconButton(
            icon: const Icon(Icons.add),
            onPressed: _openAddExpenseFOverlay,
          ),
        ],
      ),
      body: width < 600
          ? Column(
              children: [
                Chart(expenses: _registeredExpenses),
                Expanded(
                  child: mainCOntent,
                ),
              ],
            )
          : Row(
              children: [
                Expanded(child: Chart(expenses: _registeredExpenses)),
                Expanded(
                  child: mainCOntent,
                ),
              ],
            ),
    );
  }
  ```

  ## Wisget Size
  각 위젯은 `preferences & parent widget` 두개의 제약을 가진다.
  예를 들면 `Column` 위젯은 높이는 무한대 크기를 가지고 넓이는 부모 위젯의 크기를 가진다.
  반면 `Scaffold` 위젯은 높이와 넓이 모두 디바이스 크기의 최대 값을 가진다.

## LayoutBuilder
스터디 필요

## Adptive Widgets
기기 또는 크롬, 윈도우, MacOS에 따라 위젯 스타일을 변경 할수 있다.
```dart
Platform.isIOS
```
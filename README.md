# thai_utils

> **Status: pre-alpha — under active development. APIs will change until v0.1.0.**
> See [PLAN.md](PLAN.md) for the roadmap.

Pure-Dart Thai helpers for Flutter & Dart: national ID, baht text, Buddhist-era dates, phone numbers, input formatters.

แพ็กเกจ Dart/Flutter สำหรับข้อมูลแบบไทย พร้อม TextInputFormatter สำหรับเลขบัตรประชาชน เบอร์โทร และจำนวนเงิน

## Install
```sh
dart pub add thai_utils
```

## Usage (target API)
```dart
import 'package:thai_utils/thai_utils.dart';

isValidThaiId('1101700230708');
bahtText(1234567.89);
DateTime(2026, 10, 6).toThaiString(); // 6 ตุลาคม 2569
```

## Development
```sh
dart pub get
dart format .
dart analyze --fatal-infos
dart test
```

## Contributing
Issues and PRs welcome — see [CONTRIBUTING.md](CONTRIBUTING.md). Thai or English.

## License
MIT © Chawn and contributors

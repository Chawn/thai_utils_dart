# PLAN — thai_utils (Dart / Flutter)

Status: **M0 scaffolded** · Owner: @Chawn · Last updated: 2026-10-06

## Goal
Pure-Dart port of `thaiutils-go` for Flutter and Dart backends: national ID, baht text,
Buddhist-era dates, Thai phone numbers, Thai digits — plus Flutter `TextInputFormatter`s,
which is where Flutter devs feel the pain most. Success = pub.dev likes/downloads and
dependent packages.

## Non-goals
- Anything requiring `dart:io` or platform channels (must run on web, mobile, desktop, server).
- Full i18n/intl replacement — we complement `package:intl`, not replace it.

## Design principles
1. **Pure Dart**, zero runtime dependencies. The Flutter formatters live in a separate
   library file (`package:thai_utils/flutter.dart`) that imports `package:flutter/services.dart`
   — decide in M5 whether this needs to become a separate package (`thai_utils_flutter`)
   so the core stays Flutter-free. Default plan: **separate package in a `flutter/` subfolder**.
2. API parity with `thaiutils-go` (same names in lowerCamelCase, same test vectors —
   copy `testdata/*.json` from that repo, keep them byte-identical).
3. Aim for 160/160 pub points: docs on every public member, `example/` folder,
   null-safety, platform support for all 6 platforms, `dart format` clean.
4. Package name: `thai_utils` — **check pub.dev availability first**; fallback `thai_toolkit`.

## API design (v0.1.0)
```dart
// thai_id.dart
bool isValidThaiId(String id);
void validateThaiId(String id);              // throws ThaiIdException(reason)
String normalizeThaiId(String id);
String formatThaiId(String id);              // 1-2345-67890-12-1
int thaiIdCheckDigit(String first12);

// baht_text.dart
String bahtText(num amount);                  // rounds half-up to 2 dp
String bahtTextFromString(String decimal);    // exact
String bahtTextFromSatang(int satang);        // exact core
// 0 -> 'ศูนย์บาทถ้วน' ; see thaiutils-go PLAN for all rules

// thai_date.dart
extension ThaiDateTime on DateTime {
  int get beYear;
  String toThaiString({ThaiDateStyle style = ThaiDateStyle.full, bool thaiDigits = false});
}
DateTime parseThaiDate(String input);         // 6 ต.ค. 69 / 6 ตุลาคม 2569 / 06/10/2569
const thaiMonthsFull = [...]; const thaiMonthsShort = [...];

// thai_phone.dart
String normalizeThaiPhone(String input);      // +66812345678
String formatThaiPhone(String input);         // 081-234-5678
ThaiPhoneKind thaiPhoneKind(String input);

// thai_digits.dart
String toThaiDigits(String s); String toArabicDigits(String s);
```

Flutter package (`flutter/` → `thai_utils_flutter`):
```dart
class ThaiIdInputFormatter extends TextInputFormatter   // live 1-2345-67890-12-1 masking
class ThaiPhoneInputFormatter extends TextInputFormatter
class BahtAmountInputFormatter extends TextInputFormatter // 1,234.50
String? thaiIdValidator(String? v);                      // for TextFormField.validator, Thai error messages
```

## Milestones
- [x] **M0 — Scaffold**: pubspec, lints, CI, plan.
- [ ] **M1 — thai_id + thai_digits** with tests.
- [ ] **M2 — baht_text** — all shared vectors pass.
- [ ] **M3 — thai_date** — format/parse round trips.
- [ ] **M4 — thai_phone**.
- [ ] **M5 — Flutter formatters** in `flutter/` package, widget tests for cursor position.
- [ ] **M6 — Release v0.1.0** on pub.dev (verified publisher if Ben's company domain is available), 160 pub points.
- [ ] **M7 — Adoption**: use in Ben's own Flutter apps; post in Flutter Thailand community.

## Definition of done
`dart format`, `dart analyze --fatal-infos`, `dart test` all pass; `dart pub publish --dry-run` clean;
CHANGELOG.md updated; PLAN checkbox ticked.

## Open questions
- Publish under a verified publisher (needs a domain verified in Google Search Console)?
- Package name final choice after pub.dev check.

[https://dart.dev/guides/language/effective-dart/style](https://dart.dev/guides/language/effective-dart/style)에 있는 공식 문서를 번역하고 정리한 글

## 네이밍 스타일

다음의 3가지 스타일(flavor)을 사용한다.
- UpperCamelCase (=PascalCase)
- lowerCamelCase
- lowercase_with_underscores  

- ALL_CAPS_WITH_UNDERSCORES (=SCREAMING_CAPS)은 legacy code에 의한 일관성이 필요할 때만 쓴다.

UpperCamelCase를 사용하는 경우는 다음과 같다.
- class, enum, typedef, 타입 파라미터, 메타데이터 어노테이션

	```dart
	class SliderMenu { ... }

	@Foo(anArg)
	class HttpRequest { ... }

	typedef Predicate<T> = bool Function(T value);
	```

- 단, annotation class의 생성자에 파라미터가 없다면, lowerCamelCase로 쓴다.

	```dart
	const foo = Foo();

	@foo
	class C { ... }
	```

lowercase_with_underscores를 사용하는 경우는 다음과 같다.
- 라이브러리, 패키지, 디렉토리, 소스파일의 이름

	```dart
	library peg_parser.source_scanner;

	import 'file_system.dart';
	import 'slider_menu.dart';
	```

- import prefix

	```dart
	import 'dart:math' as math;
	import 'package:angular_components/angular_components' as angular_components;
	import 'package:js/js.dart' as js;
	```

lowerCamelCase를 사용하는 경우는 다음과 같다.
- 그 이외의 대부분의 식별자

	```dart
	var item;

	HttpRequest httpRequest;

	void align(bool clearItems) {
		// ...
	}
	```

- 상수 (단, 이미 만들어진 code가 `ALL_CAPS_WITH_UNDERSCORES`라면 그쪽으로 통일한다)

	```dart
	const pi = 3.14;
	const defaultTimeout = 1000;
	final urlScheme = new RegExp('^([a-z]+):');

	class Dice {
		static final numberGenerator = new Random();
	}
	```

두문자나 약어 표기법은 다음과 같다.
- 1, 2글자일 때를 제외하고는 모두 일반 단어처럼 표기

	```dart
	HttpConnectionInfo
	uiHandler
	IOStream
	HttpRequest
	Id
	DB
	```

다음은 금지 사항이다.
- '_'로 시작하는 식별자
	- Dart는 '_'로 시작하는 것은 private에 대한 선언으로 사용하고 있음
	- 단, 사용하지 않는 파라미터라는 의미로 '_', '__', '___'등을 사용하는 것은 허용  
	(관용적으로 통용되는 표기이므로)
- 헝가리안식 접두어

## 배치 순서

Directive는 다음의 순서로 배치한다.
- dart:
- external package:
- internal package:
- local source code
- export

그리고 같은 급 안에서는 알파벳 순서로 정렬한다.

```dart
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'package:my_package/util.dart';

import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
```

## Source code 포매팅

[dartfmt](https://github.com/dart-lang/dart_style)을 따른다.

```dart
import 'package:dart_style/dart_style.dart';

main() {
  var formatter = new DartFormatter();

  try {
    print(formatter.format("""
    library an_entire_compilation_unit;

    class SomeClass {}
    """));

    print(formatter.formatStatement("aSingle(statement);"));
  } on FormatterException catch (ex) {
    print(ex);
  }
}
```

한 줄에 80자 이상을 쓰지 않는다.
- 주된 이유는 `아주 긴 class 이름`등인데, 웬만하면 짧게
- URI나 파일 경로가 포함되는 경우는 예외
- 멀티 라인의 문자열의 경우는 예외

항상 중괄호를 쓴다.
```dart
if (isWeekDay) {
  print('Bike to work!');
} else {
  print('Go dancing or read a book!');
}

if (overflowChars != other.overflowChars) {
  return overflowChars < other.overflowChars;
}
```

단, else가 없고 간단하게 표현 가능한 경우는 한 줄로 쓴다.
```dart
if (arg == null) return defaultValue;
```
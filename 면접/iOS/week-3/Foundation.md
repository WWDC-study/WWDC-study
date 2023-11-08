# Foundation

Foundation 프레임워크는 iOS 애플리케이션에 필요한 기본 기능 레이어를 정의합니다

## 하위 클래스

---

이 프레임워크는 원시(primitive) 클래스를 제공하고 Objective-C 런타임 & 언어 또는 Swift 표준 라이브러리 & 언어에서 제공하지 않는 기능과 몇가지 패러다임을 정의합니다.

- [Numbers, Data, and Basic Values](https://developer.apple.com/documentation/foundation/numbers_data_and_basic_values)
    - Cocoa 내에서 사용하는 원시타입 및 기본 타입을 가공합니다.
    - `NSNumber`, `Data`, `URL`, `CGFloat`(CoreFoundation) 등
- [Strings and Text](https://developer.apple.com/documentation/foundation/strings_and_text)
    - 유니코드 문자의 문자열을 생성 및 처리하고, 정규 표현식을 사용하여 패턴을 찾고, 텍스트 자연어 분석을 수행합니다.
    - `Locale` 등
- [Collections](https://developer.apple.com/documentation/foundation/collections)
    - 배열, 딕셔너리, 집합 및 특수 컬렉션을 포함하며 객체 또는 값의 그룹을 저장하고 반복(iterate)합니다.
    - `IndexPath`, `NSCache` 등
- [Dates and Times](https://developer.apple.com/documentation/foundation/dates_and_times)
    - 날짜와 시간을 비교하고 달력 및 시간대 계산을 수행합니다.
    - `Date`, `DateFormatter` 등
- [Units and Measurement](https://developer.apple.com/documentation/foundation/units_and_measurement)
    - 수량에 물리적 서식을 설정하고 단위 간 변환이 가능합니다.
    - `Measurement`, `UnitConverter` 등
- [Filters and Sorting](https://developer.apple.com/documentation/foundation/filters_and_sorting)
    - 술어(predicate), 표현식 및 정렬 설명자를 사용하여 컬렉션 및 기타 서비스의 요소를 검사합니다.
    - `NSPredicate`, `NSExpression` 등

## 정리

---

- **Foundation** 프레임워크는 iOS 애플리케이션에서 필요한 기본 기능을 제공하며 원시 데이터 타입, 문자열, 컬렉션, 날짜 및 시간, 측정 단위를 처리합니다.

## 참고
[Foundation | Apple Developer Documentation](https://developer.apple.com/documentation/foundation)
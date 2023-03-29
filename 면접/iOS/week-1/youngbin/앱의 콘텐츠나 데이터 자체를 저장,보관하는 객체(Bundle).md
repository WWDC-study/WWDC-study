_better on: https://www.notion.so/enebin/1095460191bd44c4bb24cd6dae44167c_

`Bundle`은 앱 또는 프레임워크의 리소스와 코드가 포함된 특수 디렉터리입니다. 

## Bundle

---
`Bundle` file
![](https://velog.velcdn.com/images/enebin777/post/3293a21f-3763-4021-af84-59831654011f/image.png)

`main` 번들
![`main` 번들](https://velog.velcdn.com/images/enebin777/post/3fb11ea5-6cc3-45af-b508-42196610736d/image.png)

`framework` 번들s
![`framework` 번들s](https://velog.velcdn.com/images/enebin777/post/2c85acd1-055e-406b-a64b-4d98a487e2c2/image.png)


*Apple*은 `Bundle`을 사용하여 **앱, 프레임워크, 플러그인 및 기타 여러 특정 유형의 콘텐츠를 나타냅니다**. 

- 번들에는 `info.plist`, dynamic `framework`, `storyboardc`(스토리보드 컴파일된 파일, [참고](https://velog.io/@ryan-son/Storyboard-%EC%BB%B4%ED%8C%8C%EC%9D%BC-%EA%B3%BC%EC%A0%95-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)), 추가한 `asset`이 포함됩니다.
    - `main`번들의 `framework`에는 하위 `framework`의 번들이 포함되어있습니다.
    - static `framework`는 executable 파일에 포함돼 컴파일되므로 포함되지 않습니다.
- 번들은 포함된 리소스를 잘 정의된 하위 디렉토리로 구성하며, 번들 구조는 플랫폼과 번들의 유형에 따라 달라집니다.
- Package vs Bundle?
    
    **Finder는 다음 조건 중 하나라도 해당하는 경우 Directory를 Package로 간주합니다.**
    
    1. Directory가 .app, .bundle, .framework, .plugin, .kext등의 파일 확장자를 가진 파일을 가지고 있는 경우
    2. Directory에 있는 Application이 Package 타입을 나타내는 확장자가 있는 경우
    3. Directory에 Package bit set이 있는 경우

## Bundle 접근

---

모든 실행 파일은 `Bundle` 객체를 사용하여 앱의 번들 내부 또는 다른 곳에 위치한 번들의 리소스를 찾을 수 있습니다. 

1. 번들 디렉터리에 대한 `Bundle` 객체를 생성합니다.
    
    ```swift
    // Get the app's main bundle
    let mainBundle = Bundle.main
    
    // Get the bundle containing the specified private class.
    let myBundle = Bundle(for: NSClassFromString("MyPrivateClass")!)
    ```
    
2. `Bundle` 객체의 메서드를 사용하여 필요한 리소스를 찾거나 로드합니다.
    
    ```swift
    print(Bundle.main.bundleURL) // main bundle의 위치
    print(Bundle.main.bundleIdentifier)
    
    // Bundle에 있는 이미지에 접근
    Bundle.main.url(forResource: "ImageName", withExtension: "jpg")
    ```
    
3. 혹은 다른 시스템 API를 사용하여 리소스와 상호 작용합니다.

***Tips***

- **자주 사용하는 일부 유형의 리소스는 번들 없이도 찾아서 열 수 있습니다.** 예를 들어 이미지를 로드할 때 이미지를 에셋 카탈로그에 저장하고 `UIImage` 또는 `NSImage`의 `init(named:)` 메서드를 사용하여 로드합니다.
- 컨테이너 디렉토리나 다른 File System(ex. `documentsDirectory`)에서 파일을 찾는 데는 번들 객체를 사용하지 않습니다.

## 유의사항

---

**번들 개체는 디스크에서 리소스를 찾을 때 다음 순서로 리소스를 찾습니다.**

1. 글로벌(지역화되지 않은) 리소스
2. 지역별 현지화된 리소스(사용자의 지역 기본 설정에 따라)
3. 언어별 현지화된 리소스(사용자의 언어 기본 설정에 따라)
4. 개발 언어 리소스(번들의 `Info.plist` 파일에 있는 `CFBundleDevelopmentRegion` 키로 지정됨)

***Tips***

- 글로벌 리소스가 언어별 리소스보다 우선하므로 앱에 특정 리소스의 글로벌 버전과 현지화된 버전을 모두 포함해서는 안 됩니다.
- 리소스의 글로벌 버전이 있는 경우 언어별 버전은 반환되지 않습니다.
- 이 우선 순위의 이유는 성능 때문입니다. 지역화된 리소스를 먼저 검색하면 번들 객체가 글로벌 리소스를 반환하기 전에 존재하지 않는 지역화된 리소스를 검색하는 데 시간을 낭비할 수 있습니다.

## 정리

---

- **Bundle은 앱 개발에서 콘텐츠나 데이터를 저장하고 보관하는 객체입니다.** 이 객체는 앱, 프레임워크, 플러그인 및 기타 여러 특정 유형의 콘텐츠를 나타내며, 리소스를 찾거나 로드하는 데 사용됩니다.
- `Bundle`에는 앱의 **info.plist, dynamic framework, storyboardc, 추가한 asset** 등이 포함됩니다.
- **main 번들은 현재 실행 중인 코드가 포함된 번들 디렉터리**를 나타내며, 이를 통해 앱과 함께 제공된 리소스에 액세스할 수 있습니다.

## 참고

---

[Bundle | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/bundle)

[[iOS - swift] 번들과 패키지(Bundle, package), Framework 개념](https://ios-development.tistory.com/339)

[In iOS apps , what's the difference between the main bundle and the documents directory?](https://stackoverflow.com/questions/25471024/in-ios-apps-whats-the-difference-between-the-main-bundle-and-the-documents-di)
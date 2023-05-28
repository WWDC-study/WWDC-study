# @Main에 대해

better on : [@Main에 대해](https://www.notion.so/Main-9fae7b833116429a84597da4a9b9c5c9) 

## @Main

---

iOS에서 `@main`은 Swift 프로그램의 진입점을 표시하는 데 사용되는 *attribute*입니다. Swift 5.3 이후 `@UIApplicationMain`을 대체하기 위해 도입되었습니다.

### 참고

[앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체](https://www.notion.so/main-c-UIApplicationMain-fe697d23c03540a69f52341c1ed03f8f) 

## 상세

---

### @UIApplicationMain와의 차이점

```swift
import UIKit

@main
struct MyMain {
    static func main() -> Void {
         UIApplicationMain(
              CommandLine.argc, 
							CommandLine.unsafeArgv, 
							nil, 
							NSStringFromClass(AppDelegate.self)
         )
    }
}

// look, ma, no @UIApplicationMain!
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]?) -> Bool {
        ...
    }
}
```

[Programming-iOS-Book-Examples/AppDelegate.swift at 604368f458076dd40a8f4d0676a2aa79adb8e390 · mattneub/Programming-iOS-Book-Examples](https://github.com/mattneub/Programming-iOS-Book-Examples/blob/604368f458076dd40a8f4d0676a2aa79adb8e390/bk1ch06p297main2/bk1ch06p297main/AppDelegate.swift)

- `@main`은 Swift 5.3에서 도입된 새로운 기능입니다. 앱의 진입점을 지정하고 `UIApplicationMain` 함수를 자동으로 호출한다는 면에서는 두 attribute가 동일합니다. 즉 `main.swift` 파일을 직접 생성하는 대신 Swift가 생성하도록 할 수 있습니다.
- 단, `@main`을 사용하면 커스텀 타입 중 하나를 `@main`으로 지정한 후 `static` `main` 함수를 선언하여 `main.swift` 파일에서 수행했던 모든 작업을 수행할 수 있습니다. 이를 **type-based program entry point**라고 하며 `@UIApplicationMain`이 할 수 없는 작업입니다.
- 또한 `@main`은 `struct`에도 선언 가능하다는 차이가 있습니다. 이는 진입점이 주로 `struct`로 선언되는 `SwiftUI`의 새로운 라이프사이클에 대응하기 위함입니다.

## 정리

---

- @UIApplicationMain과 @main은 **앱의 진입점을 지정하고 `UIApplicationMain` 함수를 자동으로 호출한다는 면에서 동일**합니다.
- @main은 **type-based program entry point 선언이 가능합니다**
- @main은 `struct`에도 선언 가능하며 따라서 `SwiftUI`에서도 사용이 가능합니다.

## 참고
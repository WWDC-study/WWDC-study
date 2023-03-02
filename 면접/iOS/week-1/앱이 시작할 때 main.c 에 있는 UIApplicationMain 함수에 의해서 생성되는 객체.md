# 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체
*better on : [앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체](https://www.notion.so/main-c-UIApplicationMain-fe697d23c03540a69f52341c1ed03f8f)*

## UIApplication

---

모든 iOS 앱에는 반드시 하나의 `UIApplication` 인스턴스가 있으며 매우 드물게 `UIApplication`의 서브클래스가 존재합니다.

```swift
UIKIT_EXTERN int UIApplicationMain(
    int argc, char * _Nullable argv[_Nonnull],
    NSString * _Nullable principalClassName,
    NSString * _Nullable delegateClassName
);
```

```swift
@interface UIApplication : UIResponder
@property(class, nonatomic, readonly) UIApplication *sharedApplication NS_EXTENSION_UNAVAILABLE_IOS("Use view controller based solutions where appropriate instead.");
@property(nullable, nonatomic, assign) id<UIApplicationDelegate> delegate;
```

- 앱이 실행되면 시스템은 `UIApplicationMain` 함수를 호출합니다. 이 함수는 `shared` 싱글톤 `UIApplication` 객체를 생성합니다.
    - `principalClassName`: `UIApplication` 클래스 또는 서브클래스의 이름입니다. `nil`을 지정하면 `UIApplication`으로 가정합니다.
    - `delegateClassName`: application delegate를 지정할 클래스 이름입니다.  `UIApplication`의 서브클래스를 지정할 경우, 해당 클래스를 `delegate`로 지정 할 수 있습니다

```swift
@main
class AppDelegate: UIResponder, UIApplicationDelegate {
		func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        return true
    }
}
```

- `@main`으로 설정한 클래스(구조체)가 `UIApplicationMain`의 매개변수 `delegateClassName`로 전달됩니다. 이를 통해 해당 객체에 `delegate`를 설정할 수 있습니다.

***참고***

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da0145b0-a175-4363-ad18-644ccf161680/Untitled.png)

### 동작

`UIApplication` 객체는 들어오는 사용자 이벤트를 최초로 처리합니다. 이 객체는 제어 객체(`UIControl` 인스턴스)가 전달한 동작 메시지를 적절한 대상으로 전송합니다.

- `UIApplication` 객체는 열려 있는 창 목록(`UIWindow` 객체)을 관리하며, 이 목록은 앱의 `UIView` 객체를 탐색하는 데 사용할 수 있습니다.
- `UIApplication` 객체는 `UIApplicationDelegate` 프로토콜을 준수하는 `delegate`를 정의하며 프로토콜 메서드 중 일부를 구현(및 실행)합니다.
- `UIApplication` 객체는 앱 시작, 메모리 부족 경고 및 앱 종료와 같은 중요한 런타임 이벤트를 `delegate` 알려주어 적절하게 대응할 수 있는 기회를 제공합니다.
- `UIApplication`은 `open` 메서드를 통해 이메일이나 이미지 파일과 같은 리소스를 처리할 수 있습니다. 예를 들어, 앱이 이메일 URL을 사용하여 `open` 메서드를 호출하면 메일 앱이 실행되어 메시지를 표시합니다.
- `**UIApplication` 클래스의 API를 사용하면 추가적인 기기 동작을 관리할 수 있습니다.**
    - 메서드 목록
        - `startIgnoringInteractionEvents()`: 들어오는 터치 이벤트를 중단
        - `registerForRemoteNotifications()`:  원격 알림 등록
        - `applicationSupportsShakeToEdit`: 흔들어서 실행 취소를 Bool value로 온오프
        - `canOpenURL(_:)`: URL 스킴을 처리할 수 있는지 확인
        - `startBackgroundTask(expirationHandler:)`,  `startBackgroundTask(withName:expirationHandler:)`: 백그라운드에서 할 추가작업을 설정
        - `scheduleLocalNotification`,  `cancelLocalNotification`: 로컬 알림을 예약하고 취소
        - `startReceivingRemoteControlEvents()`, `endReceivingRemoteControlEvents()`: 원격 제어 이벤트 수신을 조절함
        - 상태복구 작업 수행(?)

### 서브클래싱

<aside>
🚧 대부분의 앱은 `UIApplication`을 서브 클래스 할 필요가 없습니다. 
가능한 `AppDelegate`를 사용하여 시스템과 앱 간의 상호 작용을 관리하세요.

</aside>

- 앱이 시스템보다 먼저 수신 이벤트를 처리해야 하는 경우 사용자 지정 이벤트 또는 작업 디스패치 메커니즘을 구현할 수 있습니다.
- 이를 위해선 `UIApplication`을 서브클래싱한 후 `sendEvent` **및/또는 `sendAction(:to:from:for:)` 메서드를 `override` 합니다.
    
    ```swift
    super.sendEvent(event)
    ```
    

## 정리

---

- (Swift 5.7 기준) `**@main` annotation으로 표기된 객체는 앱의 진입점으로 취급되며 `UIApplicationMain`을 호출합니다.**
- `UIApplicationMain`을 호출 시 **`UIApplication`의 싱글톤 객체와 `AppDelegate`를 설정**합니다. `AppDelegate` 객체는 `UIApplicationMain`을 호출한 객체로 전달합니다.
- `UIApplication`은 이벤트의 1차 처리자로서 `**UIWindow`를 관리**하고 시작, 메모리 부족, 종료 등의 **중요한 런타임 이벤트를 전달**하며 open 메서드를 통해 **URL 이벤트를 처리**할 수 있습니다.
- 이외에도 이벤트와 앱 동작에 관한 중요한 메서드를 제공합니다.

## 참고
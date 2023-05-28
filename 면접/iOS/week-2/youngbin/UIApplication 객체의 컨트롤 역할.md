# ***UIApplication 객체의 컨트롤러 역할***

better on : [***UIApplication 객체의 컨트롤러 역할***](https://www.notion.so/UIApplication-8323a444c7034ec0931c42d498f9f1cd) 

## UIApplication

---

**UIApllcation은 다음과 같은 동작을 합니다.** 

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

## UIApplicationMain

---

**UIApllcationMain은 UIApplication을 생성하는 메서드입니다. 앱이 시작하며 최초로 실행하는 메서드입니다.**

- 앱이 실행되면 시스템은 `UIApplicationMain` 함수를 호출합니다. 이 함수는 `shared` 싱글톤 `UIApplication` 객체를 생성합니다.
    - `principalClassName`: `UIApplication` 클래스 또는 서브클래스의 이름입니다. `nil`을 지정하면 `UIApplication`으로 가정합니다.
    - `delegateClassName`: application delegate를 지정할 클래스 이름입니다.  `UIApplication`의 서브클래스를 지정할 경우, 해당 클래스를 `delegate`로 지정 할 수 있습니다
- 이 때 `principalClassName`로 전달한 클래스가 `UIApplication`의 역할을 합니다.
- 이 때 `delegateClassName`로 전달한 클래스가 `UIApplication`로부터 앱 동작을 수신합니다.

## 정리

---

- `UIApplication`은 열려 있는 창 목록(`UIWindow` 객체) 관리, AppDelegate 메서드를 호출하며 openURL같은 메서드를 제공합니다.
- `UIApplicationMain`은 앱 시작과 함께 실행되며 `UIApplication`을 생성하고 `delegate` 클래스를 지정합니다.

## 참고

---

[앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체](https://www.notion.so/main-c-UIApplicationMain-fe697d23c03540a69f52341c1ed03f8f)
# 앱이 foreground에 있을 때와 background에 있을 때의 차이(제약을 중점으로)

better on : [앱이 foreground에 있을 때와 background에 있을 때의 차이(제약을 중점으로)](https://www.notion.so/foreground-background-2c4a7ee709e7432e87ba0974256726cd) 

## foreground와 background

---

앱이 백그라운드에 있는 경우에도 앱은 계속 실행될 수 있습니다.

 **단, 시스템 리소스에 대한 액세스가 일부 제한됩니다.**  곧, background에서 실행하는 작업은 foreground 앱의 우선순위에 밀려 취소될 수 있습니다.

### 워크플로우

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcdcc498-7031-4f2b-a4e7-bc8d324e8183/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5362365-de6e-4e6e-a25f-1c310a9726c5/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/291a861b-938d-4664-a107-55d9f1058f14/Untitled.png)

## 백그라운드 태스크 처리 팁

[Preparing your UI to run in the background | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)

---

### `ResignActive`

**사용자가 앱을 종료하면 시스템은 해당 앱을 백그라운드로 이동하기 전 비활성화 상태를 거칩니다.** 

또한 시스템 경고를 표시하기 위해 일시적으로 앱을 중단해야 하는 경우에도 시스템이 앱을 비활성화합니다. 

**비활성화하는 동안 다음 메서드를 호출합니다:**

- `SceneDelegate`: `sceneWillResignActive(_:)`
- `AppDelegate`: `applicationWillResignActive(_:)`

**비활성화 시 사용자 데이터를 보존하고 주요 작업을 일시 중지하여 앱을 조용한(quiet) 상태로 전환하세요:**

다음의 예시를 들 수 있습니다.

- 사용자 데이터를 디스크에 저장하고 열려 있는 파일을 모두 닫습니다.
- `Dispatch/OperationQueue`를 일시 중단합니다.
    - background Queue는 background 상태와 관련이 없습니다([ref](https://stackoverflow.com/questions/70307882/swift-background-execution)). 백그라운드 task 설정을 위해서는 [`BGAppRefreshTaskRequest`](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjHpfOukrj9AhUwr1YBHZn7AYkQFnoECA4QAQ&url=https%3A%2F%2Fdeveloper.apple.com%2Fdocumentation%2Fbackgroundtasks%2Fbgapprefreshtaskrequest&usg=AOvVaw3ZwGRTb3LhSjDRbv385Gu-) 혹은 `[BGProcessingTaskRequest](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjegfrBkrj9AhWhilYBHWDnAIYQFnoECA8QAQ&url=https%3A%2F%2Fdeveloper.apple.com%2Fdocumentation%2Fbackgroundtasks%2Fbgprocessingtaskrequest&usg=AOvVaw3egcrH6IlLvPbx7vMRBna7)`를 사용하세요.
- 실행을 위해 새 작업을 스케줄하지 않습니다.
- 활성 타이머를 모두 무효화합니다.
- 게임플레이를 자동으로 일시 중지합니다.

### `DidEnterBackground`

**앱이 백그라운드로 전환되면 메모리를 해제하고 앱이 보유하고 있는 모든 shared 리소스를 해제합니다.** 

- Foreground app은 메모리 및 기타 시스템 리소스 접근에 우선순위를 가집니다. 시스템은 이러한 리소스를 사용하기 위해 백그라운드 앱을 종료할 수 있습니다.

**백그라운드로 진입하면 다음 메서드를 호출합니다:**

- `SceneDelegate`: `sceneDidEnterBackground(_:)`
- `AppDelegate`: `applicationDidEnterBackground(_:)`

**백그라운드 전환 중에 다음 작업 중 적절한 작업을 가능한 많이 수행하세요:**

- 파일(file)로부터 로드한 이미지나 미디어를 모두 삭제합니다.
- 디스크로부터 다시 만들거나 로드할 수 있는 모든 대용량 in-memory 개체를 삭제합니다.
- 카메라 및 기타 공유 하드웨어 리소스에 대한 액세스 권한을 해제합니다.
- UI에서 비밀번호와 같은 민감한 정보를 숨깁니다.
- 공유 시스템 데이터베이스에 대한 연결을 닫습니다.
- 알림 및 기타 임시 인터페이스를 해제합니다.

***Tips - 안 해도 되는 것***

- 앱의 에셋 카탈로그에서 로드한 named 이미지(`UIImage(name: “something”)`)는 해제할 필요가 없습니다. 시스템에서 해당 오브젝트를 자동으로 정리합니다.
- 마찬가지로 `NSDiscardableContent` 프로토콜을 채택하거나 `NSCache` 오브젝트를 사용하여 관리하는 오브젝트는 해제할 필요가 없습니다. 시스템에서 해당 오브젝트를 자동으로 정리합니다.
- 백그라운드로 전환한 후에도 카메라 또는 공유 시스템 데이터베이스와 같은 리소스에 계속 접근할 경우 시스템에서 앱을 우선 종료합니다.

### UI 스냅샷 준비

앱이 백그라운드에 진입하고 델리게이트 메서드가 반환되면 **`UIKit`은 앱의 현재 UI의 스냅샷을 생성합니다.** 

- 시스템은 [App switcher](https://support.apple.com/library/content/dam/edam/applecare/images/en_US/iOS/ios15-iphone-se-switch-apps.png)에 결과 이미지를 표시합니다. 또한 앱을 다시 포그라운드로 가져올 때 일시적으로 이미지를 표시합니다.
- 앱의 UI에는 비밀번호나 신용카드 번호와 같은 민감한 사용자 정보가 포함되어서는 안 됩니다.

### 백그라운드에서 이벤트 응답

앱은 일반적으로 백그라운드 진입 후 작업할 수 없습니다. **하지만 `UIKit`은 다음 기능에 한해 작업을 허용합니다.**

- AirPlay를 사용한 오디오 통신 또는 PIP 비디오
- 위치에 민감한 서비스
- IP를 통한 음성 통신
- 외부 액세서리와의 통신
- 블루투스 LE 액세서리와의 통신 또는 장치를 블루투스 LE 액세서리로 변환
- 서버로부터의 정기 업데이트
- Apple 푸시 알림 서비스(APN) 지원

***Tips***

- 앱이 백그라운드 기능을 지원하는 경우 Xcode에서 백그라운드 모드 기능을 활성화합니다.

### 유의사항

- 모든 상태 전환은 UIKit에서 적절한 델리게이트 객체에 알림을 전송합니다:
    - iOS 13 이상 - UISceneDelegate
    - iOS 12 이하 버전 - UIApplicationDelegate
- 앱이 백그라운드에 있을 때는 가능한 한 적은 작업을 수행해야 하며, 가급적 아무것도 수행하지 않는 것이 좋습니다.
- 앱이 이전에 포그라운드에 있었다면 백그라운드 전환을 사용하여 작업을 중지하고 모든 공유 리소스를 해제하세요. 앱이 중요한 이벤트를 처리하기 위해 백그라운드로 전환되는 경우 이벤트를 처리하고 가능한 한 빨리 종료하세요.
- 사용자가 포그라운드 앱을 종료하면 해당 앱은 UIKit이 일시 중단하기 전에 잠시 백그라운드 상태로 이동합니다. 시스템에서 앱을 백그라운드 상태로 직접 실행하거나 일시 중단된 앱을 백그라운드로 이동하여 중요한 작업을 수행할 시간을 줄 수도 있습니다.

### 예시

---

[Download (apple.com)](https://docs-assets.developer.apple.com/published/9decfbca71/RefreshingAndMaintainingYourAppUsingBackgroundTasks.zip)

## 정리

---

앱이 백그라운드에 진입하기까지 앱은 ResignActive, DidEnterBackground, UI snapshot, Background 에서 이벤트 응답의 과정을 거칩니다.

- `ResignActive` : 사용자가 앱을 종료하면 시스템은 해당 앱을 백그라운드로 이동하기 전 비활성화 상태를 거칩니다.
- `DidEnterBackground` : 앱이 백그라운드로 전환되면 메모리를 해제하고 앱이 보유하고 있는 모든 shared 리소스를 해제합니다.
- `UI 스냅샷`: 앱이 백그라운드에 진입하고 델리게이트 메서드가 반환되면 `UIKit`은 앱의 현재 UI의 스냅샷을 생성합니다.
- `백그라운드에서 이벤트 응답`: 앱은 일반적으로 백그라운드 진입 후 작업할 수 없습니다. 하지만 `UIKit`은 몇몇 기능에 한해 작업을 허용합니다.

## 참고

---

[About the background execution sequence | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background/about_the_background_execution_sequence)

### 백그라운드에서 할 만한 작업
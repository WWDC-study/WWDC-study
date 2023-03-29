# AppDelegate & SceneDelegate 메서드들

better on: [AppDelegate & SceneDelegate 메서드들](https://www.notion.so/AppDelegate-SceneDelegate-4a81cd4760414f6fb694892b81db4618) 

## AppDelegate & SceneDelegate

---

`UIAppDelegate`는 앱 내부의 공통 동작을 관리합니다. 앱 델리게이트는 사실상 앱의 루트 오브젝트로써 `UIApplication`과 함께 시스템과의 상호 작용을 관리합니다. 

`UISceneDelegate`는 앱이 foreground로 들어가 active될 때와 background로 들어갈 때를 포함해 Scene 상태 전환에 응답하는 메서드를 정의합니다. 

<aside>
💡 iOS 13 이후로 앱 이벤트를 제외한 Scene과 관련된 메서드의 호출이 `**AppDelegate**`에서 `**SceneDelegate**`로 이전되었습니다. `[applicationDidBecomeActive](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622956-applicationdidbecomeactive)`, `[applicationWillEnterForeground](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623076-applicationwillenterforeground)`, `[applicationWillEnterForeground](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623076-applicationwillenterforeground)`등이 이에 해당합니다.

</aside>

## 상세

---

![https://i.imgur.com/CDwg5Ny.png](https://i.imgur.com/CDwg5Ny.png)

![https://i.imgur.com/D4VfgIv.png](https://i.imgur.com/D4VfgIv.png)

### AppDelegate 메서드들

- `application(_:didFinishLaunchingWithOptions:)`
    
    **앱 실행이 완료 되었을 때 호출**됩니다.
    
    타사 라이브러리 구성이나 앱의 사용자 인터페이스 설정과 같은 초기 설정을 수행하기에 좋습니다.
    
- `applicationWillTerminate(_:)`
    
    **사용자가 앱을 강제 종료하거나 시스템에서 메모리를 회수해야 하는 경우 등 앱이 종료될 때 호출됩니다**. 
    
    앱이 종료되기 전에 최종 정리 작업을 수행하거나 필요한 데이터를 저장하기에 좋습니다.
    
- `applicationDidReceiveMemoryWarning(_:)`
    
    **시스템에서 앱의 메모리가 부족하여 일부 리소스를 확보해야 한다고 판단할 때 호출**됩니다. 
    
    메모리 경고가 수신될 때 불필요한 리소스를 해제하면 앱이 일부 메모리를 확보하여 시스템에 의해 종료되는 것을 방지할 수 있습니다. (참고: [[문서 정리] Preparing your UI to run in the background](https://www.notion.so/Preparing-your-UI-to-run-in-the-background-03e689272d0640a88bcd340836f9bb1f))
    
- etc.

### SceneDelegate 메서드들

scene? → [UIScene | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiscene)

- `scene(_:willConnectTo:options:)`
    
    **이 메서드는 앱이 사용자 인터페이스를 생성하거나 복원할 때 호출**됩니다.
    
    사용자 또는 앱이 새 사용자 인터페이스를 요청하면 `UIKit`은 적절한 `scene` 객체를 생성하고 이를 앱에 연결합니다. 이 메서드를 사용하여 새 `scene` 추가에 응답하고 장면에 표시해야 하는 모든 데이터를 로드하기 시작합니다.
    
- `sceneDidDisconnect(_:)`
    
    **앱에서 `scene`을 제거했을 때 호출**됩니다. 
    
    사용자가 앱을 닫을 때와 같이 장면 연결이 끊어질 때 호출되는 함수입니다. 데이터를 저장하거나 진행 중인 작업을 중지하는 등의 정리 작업을 수행하기에 좋습니다.
    
- `sceneDidEnterBackground(_:)`
    
    사용자가 홈 버튼을 누르거나 다른 앱으로 전환하는 등 앱이 **백그라운드로 들어갈 때 호출**됩니다. 
    
    데이터를 저장하거나 진행 중인 작업을 중지하는 등 장면이 일시 중단되기 전에 수행해야 하는 모든 작업을 수행하기에 좋습니다.
    
- `sceneWillEnterForeground(_:)`
    
    사용자가 앱에 재진입하는 등 **`scene`이 포그라운드로 진입하려고 할 때 호출**됩니다. 
    
- `sceneDidBecomeActive(_:)`
    
    `**scene`이 활성화될 때 호출**됩니다. 
    
    사용자 인터페이스 업데이트 또는 데이터 새로 고침과 같이 장면이 활성화될 때 수행해야 하는 작업을 시작하기에 좋습니다.
    
- etc.

## 정리

---

**AppDelegate는 앱 전반에서 공유하는 이벤트를 전달합니다.** 

- `application(_:didFinishLaunchingWithOptions:)`
- `applicationWillTerminate(_:)`
- `applicationDidReceiveMemoryWarning(_:)`

**SceneDelegate는 `scene`(화면)에 관련한 이벤트를 전달합니다.**

- `scene(_:willConnectTo:options:)`
- `sceneDidDisconnect(_:)`
- `sceneDidEnterBackground(_:)`
- `sceneWillEnterForeground(_:)`
- `sceneDidBecomeActive(_:)`

## 참고
# App의 상태(Not running, Inactive, Active, Background, Suspended)

better on : [App의 상태(Not running, Inactive, Active, Background, Suspended)](https://www.notion.so/App-Not-running-Inactive-Active-Background-Suspended-480a0488f05645a8b76e1b5cafc2e8e7) 

## App state

---

App State 이벤트를 SceneDelegate 혹은 AppDelegate로 수신할 수 있습니다. iOS 13을 기점으로 수신하는 방법에 차이가 있으며 이 문서에서는 이 둘을 구분하여 서술합니다.

### Scene-based-라이프사이클(SceneDelegate)

![https://docs-assets.developer.apple.com/published/8e113a7266/scene-state~dark@2x.png](https://docs-assets.developer.apple.com/published/8e113a7266/scene-state~dark@2x.png)

1. 사용자 또는 시스템이 앱에 새 `scene`을 요청하면 `UIKit`은 `scene`을 생성한 후 **Unattached** 상태로 전환합니다. 
2. 사용자가 요청한 장면은 **Foreground**로 빠르게 이동한 후 화면에 표시됩니다. 
3. 시스템에서 요청한 `scene`은 일반적으로 이벤트를 처리하기 위해 **Background**로 이동합니다. 예컨데 시스템이 위치 이벤트를 처리하는 경우가 있습니다.(cf. [Preparing your UI to run in the background](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background))
4. 사용자가 앱의 UI를 해제하면 `UIKit`은 관련 `scene`을 **Background**로 이동한 다음 **Suspended**로 전환합니다. 
5. `UIKit`은 언제든지 **Background** 또는 **Suspended** `scene`의 연결을 끊고 리소스를 회수할 수 있습니다. 해당 `scene`은 **Unattached** 상태로 돌아갑니다.

### App-based-라이프사이클(AppDelegate)

![https://docs-assets.developer.apple.com/published/e6ac158845/app-state~dark@2x.png](https://docs-assets.developer.apple.com/published/e6ac158845/app-state~dark@2x.png)

1. 실행 후 시스템은 UI가 화면에 표시될지 여부에 따라 앱을 **Inactive** 또는 **Background** 상태로 전환합니다.
2. Foreground로 진행할 때 시스템은 앱을 자동으로 **Active** 상태로 전환합니다. 
3. 그 후에는 앱이 **Suspended**될 때까지 **Active**와 **Background** 상태 사이를 오갑니다.

## 정리

---

어플리케이션은 여러가지 상태를 가질 수 있습니다. 다만 기준을 `scene`과 app 사이 어떤 것으로 잡을지에 따라  종류가 달라질 수 있습니다.

- Scene-based-라이프사이클은 `**scene`의 동작을 기준**으로 상태 흐름을 정의합니다.
    
    <aside>
    💡 Unattched → Foreground inactive → Active → Foreground inactive → Background → Suspended
    
    </aside>
    
- App-based-라이프사이클은 **App의 동작을 기준**으로 상태 흐름을 정의합니다.
    
    <aside>
    💡 Background(혹은 UI 표시 필요에 따라 Inactive) → Inactive → Active → Inactive → Background → Suspended
    
    </aside>
    

## 참고

---

[앱이 Inactive 상태가 되는 시나리오](https://www.notion.so/Inactive-c654e1d6595f4ab892fcc4b513301764) 

### 문서

[Managing your app’s life cycle | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)

[https://www.notion.so/enebin/App-Not-running-Inactive-Active-Background-Suspended-480a0488f05645a8b76e1b5cafc2e8e7?pvs=4](https://www.notion.so/App-Not-running-Inactive-Active-Background-Suspended-480a0488f05645a8b76e1b5cafc2e8e7)
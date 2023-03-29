# 앱이 Inactive 상태가 되는 시나리오

## Inactive status

---
iOS 13 이후
![iOS 13 이후](https://velog.velcdn.com/images/enebin777/post/ec6f26e7-3f08-4c6e-b57d-33b63bed7c48/image.png)

iOS 12 이전
![iOS 12 이전](https://velog.velcdn.com/images/enebin777/post/c3d79d9a-c642-4737-a6c0-5bcb148b38f6/image.png)



- 앱이 Foreground에서 Background로 전환할 때 반드시 inactive 상태를 거칩니다.
- **이 때 `Scene(App)Delegate`에서 [applicationWillResignActive(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622950-applicationwillresignactive)가 호출됩니다.**

## ➕ Suspend status

---

**앱이 suspend 되는 상태는 예측하기 힘듭니다. 다만 다음과 같은 사례가 있습니다**

1. **백그라운드 진입 후 메모리 부족**
    
    백그라운드 진입 후 기기의 메모리가 부족한 경우 시스템에서 리소스를 확보하기 위해 백그라운드에 있는 앱 중 메모리 소모가 큰 앱을 종료할 수 있습니다.
    
2. **앱 충돌**
    
    앱이 충돌하거나 치명적인 오류가 발생하면 앱이 비활성 상태가 되어 실행이 중지됩니다.
    

- **이 경우 `Scene(App)Delegate`에서 [applicationWillTerminate(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623111-applicationwillterminate)가 호출됩니다.**

## 정리

---

- 앱이 Foreground에서 Background로 전환할 때 반드시 inactive 상태를 거치며 이 때 `Scene(App)Delegate`에서 [applicationWillResignActive(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622950-applicationwillresignactive)가 호출됩니다.
- 백그라운드 상태에서 기기에 메모리가 부족하거나 앱이 충돌했을 때 suspend 상태가 될 수 있으며  [applicationWillTerminate(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623111-applicationwillterminate)가 호출됩니다.

## 참고

---

### Inactive

[UIApplication.State | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiapplication/state)

### Suspend
[Reasons of applicationWillTerminate | Apple Developer Forums](https://developer.apple.com/forums/thread/13557)

[When will applicationWillTerminate be called?](https://stackoverflow.com/questions/29416375/when-will-applicationwillterminate-be-called)
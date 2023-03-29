# Inactive 상태가 되는 시나리오

## **앱이 In-Active 상태가 되는 시나리오를 설명해보자**

사용자의 동작에 따라 앱의 상태가 변경된다.

1. 사용자가 앱을 실행한다: Not running » In-Active » Active
2. 앱 실행 도중 홈버튼을 누른다: Active » In-Active » Background
3. 앱을 다시 켠다: Background » In-Active » Active
4. 앱이 백그라운에 있다가 Suspended 상태로 전이: Active » In-Active » Background » Suspended

**메소드 순서로 보면**

**낫 러닝 상태에서 앱 실행하고 포어그라운드 직전에 [applicationDidFinishLaunching(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623053-applicationdidfinishlaunching) 이 호출되고**

**이제 포어그라운드에 진입할 때 [sceneWillEnterForeground(_:)](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197918-scenewillenterforeground) 가 호출되면서 인액티브 상태가 된다.**

**또한 유저가 백그라운드에 진입했을때 [sceneDidEnterBackground(_:)](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197917-scenedidenterbackground?language=objc) 가 호출되고**

**다시 포어그라운드에 들어가게 될 때 sceneDidBecomeActive(_:) 이 호출되는데 여기서 인액티브 상태를 거쳐가게 된다.**

가장 흔한 예시는 우리가 문자를 공유하기 하려할 때 카카오톡 앱이 실행되는데, 이때 메시지 앱은 In-Active 상태가 되어진다.

[Dream Developer | 앱이 In-Active 상태가 되는 시나리오](https://yi-sang.github.io/blog/iOS-InActive)
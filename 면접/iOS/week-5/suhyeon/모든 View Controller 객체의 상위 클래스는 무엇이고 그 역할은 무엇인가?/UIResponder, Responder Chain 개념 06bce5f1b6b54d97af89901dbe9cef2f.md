# UIResponder, Responder Chain 개념

<aside>
👩‍🏫 모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?

</aside>

[iOS의 Responder와 Responder Chain 이해하기](https://seizze.github.io/2019/11/26/iOS%EC%9D%98-Responder%EC%99%80-Responder-Chain-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0.html)

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events)

[UIResponder | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiresponder)

- **앱은 Responder 객체를 통해서 이벤트를 받고 처리한다.**

<aside>
🥸 이벤트?

- 이벤트 정의
    
    터치 이벤트, 모션 이벤트, 원격 조종 이벤트, press 이벤트
    
    특정 이벤트를 처리하기 위해서는, **UIResponder에서 이벤트와 연관된 메서드를 오버라이딩해서 구현**해야 한다.
    
    ex) 터치 이벤트 핸들링 
    
    ![Untitled](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/Untitled.png)
    
    `touchesBegan(_:with:)` : 터치 시작 
    `touchesMoved(_:with:)` : 사용자가 화면에서 손가락을 떼지 않고 드래그
    `touchesEnded(_:with:)` : 사용자가 화면에서 손가락을 뗐을 때 
    `touchesCancelled(_:with:)` : touch이벤트 처리를 취소해달라는 시스템의 인터럽션이 들어어왔을 때, UIKit에서 호출하는 메서드 
    
    - 언제 인터럽트 발생? application이 inactive해질 때 or touch 이벤트를 처리하는 뷰가 window에서 제거될 때 , 전화올 때
    - view의 데이터를 제거하는 작업이 필요
</aside>

```swift
@MainActor class UIResponder : NSObject // UIResponder는 클래스이다.
```

- UIAppliation, UIView, UIVIewController가 UIResponder 클래스를 상속받는다.

### UIResponder가 역할 3가지

1. 이벤트를 처리한다.
2. 이벤트를 다른 UIResponder로 넘긴다. 
3. input view를 통해 custom input을 받을 수 있다. 
- ex) 시스템 키보드
    - UITextField, `[UITextView](https://developer.apple.com/documentation/uikit/uitextview)`를 유저가 탭하면, 이들이 first responder가 되고, input view가 화면에 나타난다.
    - = 활성화된 responsder와 연결된 input view가 화면에 나타남
    responder가 활성화됬을 때 input view를 화면에 보여줄 수 있다.
- 앱이 이벤트를 받으면, UIKit이 이벤트를 UIResponder에게 전달,
이벤트를 최초로 전달받는 UIResponder를 FirstResponder라고 부른다.

## Responder Chain 구조

[Support hardware keyboards in your app - WWDC20 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2020/10109/)

![스크린샷 2022-12-26 오후 3.24.49.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.24.49.jpg)

![Untitled](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/Untitled%201.png)

![Untitled](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/Untitled%202.png)

- 뷰 계층 구조에 따라서 구현됨
- FirstResponder: 이벤트를 가장 처음으로 수신하는 Responder
- Fiirst Responder에서 이벤트를 처리할 수 없으면, Responder Chain에서 상위 뷰(일반적으로는, 슈퍼뷰에 해당)으로 넘어간다.
    - UIView → Root View → RootViewController → UIWindow → UIApplication → UIApplicationDelegate (Responder를 상속받고 있을 때만)
- 마지막까지 처리되지 않으면, 이벤트를 버린다.

## first responder는 어떻게 결정되는가?

---

- 이벤트 유형에 따라 다름

| Event Type | First Responder | 언제 발생 |
| --- | --- | --- |
| Touch Event | 터치가 일어난 뷰 | 유저가 화면 터치  |
| Press Event | 포커스를 가진 뷰 | 게임기 컨트롤러 버튼을 눌렀을 때 |
| Shake-motion events | UIKit이 지정한 객체 또는 직접 지정 |  |
| Remote-control events | UIKit이 지정한 객체 또는 직접 지정 | 다른 액세서리 기기 사용 (헤드셋) |
| Editing menu messages | UIKit이 지정한 객체 또는 직접 지정 |  |
- `canBecomeFirstResponder` 프로퍼티를 false로 바꾸면, view는 first responder가 될 수 없다.
- responder는 타겟이 특정되지 않은 **액션 메시지**를 받을 수 있다.
(액션 메시지는 버튼이나 유저가 상호작용할 수 있는 control로부터 보내진다.)

### 액션 메시지

- 컨트롤은 액션 메시지를 이용하여 타겟 객체(target object)와 커뮤니케이션한다.

```swift
func addTarget(
    _ target: Any?,
    action: Selector,
    for controlEvents: UIControl.Event
)
```

<aside>
🥸 액션 메시지?

- Target - Action 디자인패턴에서 전달되는 메시지
    
    [Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3-SW1)
    
    - 메시지 전송자는 이벤트가 발생했을 때 액션 메시지를 전송
    - 메시지 전송자가 메시지 전송을 위한 정보를 모두 알고 있다.
        - action selector (호출될 메서드 구분자)
        - target (메시지 수신자)
    
    - target은 프레임워크 object 등 어떤 객체든 될 수 있다.
    - 이벤트도 **어떤 객체든 될 수 있다.**
    - ex) gesture recognizer 는 다른 객체에게 action mesage를 보낸다. 
    유저가 control(button, slider 등)을 조작하면, 특정 객체에게 메시지를 보낸다.
    - action selector와 traget은 control object의 프로퍼티이다.
    
    ### action 메서드는 특정한 형태를 갖는다.
    
    ```swift
    (IBAction)doSomething:(id)sender;
    ```
    
    - IBAction = type qualifier (`void` 리턴 타임 메서드에서 사용)
        - 메서드가 `action` 이라는 것을 표시 → interface builder가 알 수 있도록
        - interface builder에서 action method가 나타나기 위해서는 클래스의 헤더 파일에 어떤 클래스가 action message를 수신할 것인지 나타내야 한다.
    - sender: action message를 전송하는 control object
    - `.. 추후 정리 필요`
- 이벤트는 아님.
- 컨트롤의 타겟 객체(target object)가 nil이면, UIKit은 first responder부터 시작해서 responder chain을 돌며, `action` method를 가진 객체를 찾는다.
</aside>

### gesture recognizer가 등록된 경우, 이벤트 전달 순서

1. gesture recognizer
2. view
3. responder chain

[Handling UIKit gestures | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures)

## Touch 이벤트를 처리할 first Responder를 찾는 방법

- UIKit은 hitTesting을 수행
- view 계층을 돌면서, hitTest() 메서드 호출 (방향: super view → subview)
- `hitTest()` 
touch 가 발생한 위치가 view의 bounds에 속하는지 체크, 
touch가 발생한 위치의 가장 상단위 뷰를 찾는다.
- **가장 상단의 뷰(뷰 계층에서 가장 깊이 존재하는 subview)가 firstResponder가 된다.**
- 만일 super view에서 hitTest를 통과 못함 → subview의 hitTest() 호출 X
- 따라서 super view에서 터치 발생, super view의 bounds 바깥에 위치한 subview는 hitTest결과로 반환되지 않는다.
- 사용자가 화면을 터치하면, UIKit은 `UITouch` 인스턴스를 생성.  touch가 발생한 view를 UITouch인스턴스의 `view` 프로퍼티로 할당.
- 터치 위치 같이 터치 구성요소가 바뀌면,
    - UIKit은 UITouch인스턴스의 프로퍼티를 변경 (인스턴스를 새로 생성X)
    - view 프로퍼티는 바뀌지 않음
- 사용자가 화면에서 손가락을 떼면, UITouch 인스턴스 릴리즈.

### hitTest

![스크린샷 2022-12-27 오후 6.25.49.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_6.25.49.jpg)

```swift
func hitTest(
    _ point: CGPoint,
    with event: UIEvent?
) -> UIView?
```

- 이벤트가 발생한 위치에 있는 뷰들 중 가장 상단의 뷰 반환
- 터치이벤트가 어떤 subview에서 발생했는지 알기 위해, 내부에서 `point(inside:with:)` 호출
    - true를 반환한 뷰의 subview를 가지고 point(inside:with:)를 호출
    - 이 방식으로 가장 상단의 뷰를 찾는다.
- **hidden 된 뷰, alpha level이 0.01 이하인 뷰, user interaction이 disableded 된 뷰는 무시한다.**
- 일부분이 투명한 뷰는? 무시X

<aside>
❓ 일부분이 투명한 뷰는 전체 다 무시됨?

</aside>

<aside>
💡 view의 bounds 밖에 위치하는 point는 항상 nil 반환

clipsToBounds가 false이고, 터치가 발생한 subView가 view의 bounds 바깥에 위치할 때 

subview에서 발생한 touch는 무시된다.

</aside>

## Responder Chain 변경

- how? `UIResponder`의  `next` 프로퍼티 변경
- UIKit 클래스들이 next 프로퍼티를 변경해서 사용 중
    - UIView: ViewController의 rootView는 next가 ViewController이다. (일반적으로는 super view)
    - UIViewController: UIViewController가 Root ViewController라면 next가 window다. (일반적으로는 presenting view controller)
    - UIWindow: UIApplication 객체
    - UIApplication 객체: AppDelegate 인스턴스 (AppDelegate가 UIResponder의 인스턴스라면)

## SwiftUI와 Responder Chain

![스크린샷 2022-12-26 오후 3.35.21.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.35.21.jpg)

- UIKit에서 firstResponder = SwiftUI에서 focused view

![스크린샷 2022-12-26 오후 3.35.08.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.35.08.jpg)

- `.focusable`() 을 호출하면, 해당 뷰를 항상 포커즈한다.
- 일부 뷰 (TextField 등)는 항상 focusable

![스크린샷 2022-12-26 오후 3.35.58.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.35.58.jpg)

![스크린샷 2022-12-26 오후 3.36.43.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.36.43.jpg)

- focust view를 변화시킬 수 있다.

## iPadOS 15부터는 responder chain 탐색을 focused cell 부터 시작한다.

[Take your iPad apps to the next level - WWDC21 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2021/10057/?time=1443)

![스크린샷 2022-12-26 오후 3.52.03.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.52.03.jpg)

<aside>
💡 [In iPadOS 15, UIKit is introducing](https://developer.apple.com/videos/play/wwdc2021-10057/?time=1440) [a major change to the responder chain.](https://developer.apple.com/videos/play/wwdc2021-10057/?time=1443)

[When apps adopt keyboard navigation](https://developer.apple.com/videos/play/wwdc2021-10057/?time=1446) [with the focus system,](https://developer.apple.com/videos/play/wwdc2021-10057/?time=1448) [then responder traversal will begin at the focused item](https://developer.apple.com/videos/play/wwdc2021-10057/?time=1450) [rather than the first responder.](https://developer.apple.com/videos/play/wwdc2021-10057/?time=1454)

app이 키보드 네비게이션을 지원하면,  first responder가 아니라, focus된 아이템부터 이벤트를 처리할 수 있는지 체크한다‼️

</aside>

## Action routing

![스크린샷 2022-12-27 오후 4.03.58.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.03.58.jpg)

- 메뉴 바, keycommand action 전달이 first responder부터 시작되기 때문에
- 이벤트의 최종 타겟이 되는 뷰가 first reponsponder가 되고 focus를 받을 수 있도록 해야 한다.

[Because menu bar and key command actions](https://developer.apple.com/videos/play/wwdc2021-10053/?time=716) [are routed starting from the first responder,](https://developer.apple.com/videos/play/wwdc2021-10053/?time=718) [make sure that the views](https://developer.apple.com/videos/play/wwdc2021-10053/?time=721) [that would be the target of those actions](https://developer.apple.com/videos/play/wwdc2021-10053/?time=723) [can become first responder and can accept focus.](https://developer.apple.com/videos/play/wwdc2021-10053/?time=725)

[You can do this by having your views return true](https://developer.apple.com/videos/play/wwdc2021-10053/?time=729) [for the canBecomeFirstResponder and canBecomeFocused properties.](https://developer.apple.com/videos/play/wwdc2021-10053/?time=732)

[let's take a look at how](https://developer.apple.com/videos/play/wwdc2021-10260/?time=175) [to make more elements in your UI focusable.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=177) [canBecomeFocused is the single source of truth.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=181) [It is a read-only property of UIFocusItem.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=184) [Override it and return true to make an item focusable.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=188) [Now, you might be wondering, what is a focus item?](https://developer.apple.com/videos/play/wwdc2021-10260/?time=192) [The backbone of the focus system are the two protocols:](https://developer.apple.com/videos/play/wwdc2021-10260/?time=196) [UIFocusItem and UIFocusEnvironment.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=200) [FocusItems are simply that, items that can be focused.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=204) [FocusEnvironments define the hierarchy](https://developer.apple.com/videos/play/wwdc2021-10260/?time=208) [of focusable items.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=210) [UIView conforms to both of these protocols,](https://developer.apple.com/videos/play/wwdc2021-10260/?time=213) [since any view can be focused itself,](https://developer.apple.com/videos/play/wwdc2021-10260/?time=215) [but it can also contain subviews that can be focused.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=218)

[UIViewController, on the other hand,](https://developer.apple.com/videos/play/wwdc2021-10260/?time=222) [only conforms to UIFocusEnvironment,](https://developer.apple.com/videos/play/wwdc2021-10260/?time=224) [since it doesn't provide any visuals itself.](https://developer.apple.com/videos/play/wwdc2021-10260/?time=226)

## focus item과 first responder를 일치시켜야 한다.

![스크린샷 2022-12-27 오후 4.39.44.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.39.44.jpg)

- ex) collection view에서 focus 되는 cell이 바뀌면, UIKit은 first responder도 같이 변경하려고 한다.
이때 focus된 cell에서 canBecomeFirstResponder가 false라면?
    - responder chain에서 canBecomeFirstResponder가 true인 것을 찾는다.
    - 예시에서는 UIVIewController에 해당!
- firstresponder가 변경되는 경우에도 마찬가지이다. 만일 first responder가 변경된다면, 
system은 새로운 focus 뷰를 first responder 내부에서 찾는다.

![스크린샷 2022-12-27 오후 4.43.26.jpg](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.43.26.jpg)

- ⇒  key event는  focus된 item에 가장 먼저 전달,
 responder chain을 따라서 차례대로 전달

![Untitled](UIResponder,%20Responder%20Chain%20%E1%84%80%E1%85%A2%E1%84%82%E1%85%A7%E1%86%B7%2006bce5f1b6b54d97af89901dbe9cef2f/Untitled%203.png)
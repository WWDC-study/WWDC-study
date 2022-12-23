# UIKit Render

### Refs

- [pdf](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjZpIfNpKP6AhVdQfUHHYuLBIYQFnoECAkQAQ&url=https%3A%2F%2Fdevstreaming-cdn.apple.com%2Fvideos%2Fwwdc%2F2018%2F220f49ijgby0rma%2F220%2F220_high_performance_auto_layout.pdf%3Fdl%3D1&usg=AOvVaw3V9nKwjumyOlPa-XTiEXdj)
- [Demystifying iOS Layout • GameChanger Tech Blog (gc.com)](https://tech.gc.com/demystifying-ios-layout/)

# Constraints & Auto layout

## Constraints

![[High Performance Auto Layout - WWDC18 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/220/)](https://velog.velcdn.com/images/enebin777/post/873a2666-32c8-41f8-8ef6-19a3d0df5ce4/image.png)

[High Performance Auto Layout - WWDC18 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/220/)

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/nslayoutconstraint)

<aside>
💡 from [Documentation](https://developer.apple.com/documentation/uikit/nslayoutconstraint)
The relationship between two user interface objects that must be satisfied by the constraint-based layout system.

</aside>

- Constraint는 constraint-base-system에서 뷰를 구성하는데 사용되는 요소 중 하나다.
- Constraints는 뷰와 뷰 사이의 관계를 수치와 수식으로 정의한다.
- 사칙 연산이 사용될 수 있으며 다음과 같은 형태로 표현된다
    
    ```swift
    button2.leading = 1.0 × button1.trailing + 8.0
    ```
    

## Auto Layout

### 개요

[Understanding Auto Layout](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

<aside>
💡 from [Guide](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)
**Auto Layout dynamically calculates the size and position of all the views in your view hierarchy, based on constraints placed on those views.**
… If the image view’s size or position changes, the button’s position automatically adjusts to match.

</aside>

- `오토 레이아웃`은 화면에 보이는 뷰 사이의 hierarchy와 contstraints를 기반으로 각 뷰의 사이즈와 포지션을 계산한다.
- 각 뷰의 포지션이나 사이즈가 변경될 때마다 변화를 계산해 뷰를 재배치한다.
- 덕분에 다양한 화면의 기종에서도 같은 배치를 유지할 수 있게 해준다.

### 오토 레이아웃의 장점

- 오토 레이아웃은 특히 다음과 같은 상황에 유용하며 런타임에 동적으로 화면을 변화시킬 수 있게 해준다.
1. **External changes(외부 변화)**
    
    *외부 요인으로 인해 물리적으로 바뀌는 경우(스플릿 뷰, 화면 회전 등)*
    
    - The user enters or leaves Split View on an iPad (iOS).
    - The device rotates (iOS).
    - The active call and audio recording bars appear or disappear (iOS).
    - You want to support different size classes.
    - You want to support different screen sizes.
2. **Internal changes(내부 변화)**
    
    *내부 요인으로 인해 배치 혹은 구성이 바뀌는 경우 (뷰 컴포넌트가 사라짐, 다이내믹 폰트 등)*
    
    - The content displayed by the app changes.
    - The app supports internationalization.
    - The app supports Dynamic Type (iOS).

## 뷰 잡는 방법 비교

(***Frame base layout, Autoresizing mask, Auto layout)***

**오토 레이아웃은 전통적으로 사용되던 프레임 베이스 레이아웃과 대비되는 개념이다**

### Frame base layout

- `Frame base 레이아웃`은 origin, width, height 세 수치로 정의한다.
- **가장 빠른 동작방식이며 정확히 원하는 위치에 배치**할 수 있는 장점이 있다.

![프레임 베이스 레이아웃](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled.png)

프레임 베이스 레이아웃

![오토 리사이징 마스크](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%201.png)

오토 리사이징 마스크

- 하지만, 이런 단점이 있다
    - 모든 뷰에 대해 개별적인 설정과 관리가 필요하다.
    - 설계 및 디버그나 유지관리 까다롭다.
- **한마디로  존내 귀찮다.** 이런 문제를 해결하기 위해서 등장한 것이 바로 `autoresizing mask`.

### **Autoresizing mask?**

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622559-autoresizingmask)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%202.png)

- `autoresizing mask`는 슈퍼뷰의 프레임이 변할 때 서브뷰의 프레임이 어떻게 변할지 정의하는 방식으로 bitmask를 이용한다
- View의 bounds가 변경되면 subview들을 각 subview의 autoresizing mask에 해당되는 부분을 자동으로 재설정하는 원리
- UIView.AutoresizingMask에 설명된 상수를 결합하여 값을 조정한다(위 사진 참조)
- 아래와 같이 사용한다

```swift
sampleSubview.autoresizingMask = [.flexibleWidth, .flexibleHeight]
```

**장점**

- 이로서 external changes에 대한 대응이 가능해졌다. (스플릿 뷰로 짜그라들면

**문제**

- 근데 결국 얘도 문제를 다 해결하지 못한다. 슈퍼뷰가 반드시 필요한 특성 상 internal change를 반영할 수 없기 때문이다.

그래서 마지막으로 `auto layout`이 등장한다. 얘는 뷰 사이의 관계로 배치를 정의하기 때문에 이런 문제들을 다소 해결할 수 있었다. 

**cf.**

[https://developer.apple.com/documentation/appkit/nsview/1526961-translatesautoresizingmaskintoco](https://developer.apple.com/documentation/appkit/nsview/1526961-translatesautoresizingmaskintoco)

- 코드 베이스로 뷰를 잡을 때 간혹 보이는 `translatesAutoresizingMaskIntoConstraints`는 오토 리사이징 마스크를 constraint로 바꿔주는 기능을 온오프 할 수 있는 플래그임.
- 그래서 오토 레이아웃이랑 부딪히지 말라고 꺼주는 용도로 사용되기도 함.

## 그래서 Constraint가 무엇

<aside>
💡 from [Guide](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW1)
**The layout of your view hierarchy is defined as a series of linear equations.** Each constraint represents a single equation. Your goal is to declare a series of equations that has one and only one possible solution.

</aside>

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%203.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%204.png)

- 꽤 직관적인 편인데 위 수식의 경우 레드 뷰의 리딩이 블루 뷰의 트레일링의 1배수보다 8 떨어져있다는 의미다.
- 이 방정식은 위에서 보듯 여러개의 파트로 이루어져있다

## Constraint없는 Autolayout

- 도 있다.
- Stack view를 쓰면 할 수 있는데 매우 권장되는 방식이라고 하더라.

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%205.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%206.png)

## Auto layout 적용 과정

![https://velog.velcdn.com/images/enebin777/post/885a0501-4fe6-4d16-9272-74f551b568ca/image.png](https://velog.velcdn.com/images/enebin777/post/885a0501-4fe6-4d16-9272-74f551b568ca/image.png)

![https://velog.velcdn.com/images/enebin777/post/01daad1b-370f-402e-beaf-609881ee0261/image.png](https://velog.velcdn.com/images/enebin777/post/01daad1b-370f-402e-beaf-609881ee0261/image.png)

- iOS가 화면을 업데이트하기 위해 일반기기의 경우 1초에 60번(60Hz), Promotion을 지원하는 일부 기기의 경우 1초에 120번(120Hz) 업데이트 된다.
- 이 루프는 3페이즈를 통해 진행된다.
    - Update constraints (위로 전달): 서브뷰에서부터 Contraint를 업데이트 하고 레이아웃을 계산
    - Layout (아래로 하달): 슈퍼뷰에서부터 이전 단계의 레이아웃 값을 뷰에 입력
    - Display (아래로 하달): 슈퍼뷰에서부터 레이아웃 bounds 내에서 뷰의 사용자 인터페이스를 렌더링

하는 작업이다.

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%207.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%208.png)

## 비효율적인 예시

![https://velog.velcdn.com/images/enebin777/post/87c14d5c-0397-44f7-969f-93fbedba3bb5/image.png](https://velog.velcdn.com/images/enebin777/post/87c14d5c-0397-44f7-969f-93fbedba3bb5/image.png)

- 코드에서 업데이트 시점마다 모든 const를 비활성화하고 삭제하고 있는 것을 볼 수 있다.
- 위는 아래와 같은 짓이다

![https://velog.velcdn.com/images/enebin777/post/e6f26c23-db3f-4888-a6f9-c6f310fbc3c2/image.png](https://velog.velcdn.com/images/enebin777/post/e6f26c23-db3f-4888-a6f9-c6f310fbc3c2/image.png)

- const를 지우면 렌더 엔진 입장에서 처음부터 모든 계산을 재실행해야 하는 추가 태스크가 생긴다.
- 이는 퍼포먼스에 좋지 않다

한 번만 할 수 있게 이렇게 고쳐라.

![https://velog.velcdn.com/images/enebin777/post/65c4541b-0123-49b6-800a-9aa35c3e2531/image.png](https://velog.velcdn.com/images/enebin777/post/65c4541b-0123-49b6-800a-9aa35c3e2531/image.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%209.png)

```swift
var myConstraints: [NSLayoutConstraints] = []

override func updateConstraints() {
    // 불필요하게 모든 constraints를 deactivate/activate 하지않도록 한 번 이상은 작업을 수행하지 않도록한다.

    if myConstraints.isEmpty {

        NSLayoutConstraint.deactivate(myConstraints)
        myConstraints.removeAll()

        let views = ["text1":text1, "text2":text2]
        myConstraints += NSLayoutConstraint.constraints(withVisualFormat: "H:|-[text1]-[text2]",
                                                        options: [.alignAllFirstBaseline],
                                                        metrics: nil, views: views)
        myConstraints += NSLayoutConstraint.constraints(withVisualFormat: "V:|-[text1]-|",
                                                        options: [],
                                                        metrics: nil, views: views)

        NSLayoutConstraint.activate(myConstraints)
    }
    super.updateConstraints()
}
```

***위 작업 이후에 벌어질 일들***

![https://velog.velcdn.com/images/enebin777/post/fb838488-df97-4780-a450-89046080ffa2/image.png](https://velog.velcdn.com/images/enebin777/post/fb838488-df97-4780-a450-89046080ffa2/image.png)

- 렌더루프에서 이런 일들이 일어나는데 렌더루프 먼저 알아보자.

## 렌더 루프

### Refs

[Run Loops](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2010.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2011.png)

- 모든 유저 상호작용은 OS를 거쳐 event queue에 추가된다. (위 그림 참조)
- evene queue에 있는 이벤트들이 하나씩 꺼내져서 어플리케이션 객체에 전달되고 어플리케이션 객체의 코어 오브젝트가 해당 이벤트에 해당하는 이벤트 핸들러를 호출한다.
    - Core obect가 뭔지 찾아봤는데 그냥 UIView, UIViewController처럼 UIKit에서 제공하는 컴포넌트를 퉁쳐서 말하는 듯.([Core Objects of iOS App | Park's Park (wordpress.com)](https://soulpark.wordpress.com/2012/06/14/core-objects-of-ios-app/))
- 하여튼 런루프의 모든 핸들러가 리턴되면 이제 업데이트 사이클을 갖는다. 여기서 뷰가 업데이트 된다.(constraint, layout, display)

## Activating Constraint

![https://velog.velcdn.com/images/enebin777/post/76e19e77-c1c3-46d9-9b0c-47ac371d2379/image.png](https://velog.velcdn.com/images/enebin777/post/76e19e77-c1c3-46d9-9b0c-47ac371d2379/image.png)

![https://velog.velcdn.com/images/enebin777/post/09542c79-ed40-475d-a0df-a2c3a5c9a2bb/image.png](https://velog.velcdn.com/images/enebin777/post/09542c79-ed40-475d-a0df-a2c3a5c9a2bb/image.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2012.png)

![https://velog.velcdn.com/images/enebin777/post/6882a31b-f201-47ee-8f7d-1f922a780e3b/image.png](https://velog.velcdn.com/images/enebin777/post/6882a31b-f201-47ee-8f7d-1f922a780e3b/image.png)

- const가 활성화 되면 엔진에서 각 const의 관계를 통해 배치를 계산한다.
- 위의 예시에서 서로 다른 변수의 관계를 분석해 `test2.width`를 도출해내는 것을 볼 수 있다

![https://velog.velcdn.com/images/enebin777/post/1dd5bb33-9547-4010-a548-f6c8278a2950/image.png](https://velog.velcdn.com/images/enebin777/post/1dd5bb33-9547-4010-a548-f6c8278a2950/image.png)

- 변수에 값이 써지면(변경을 감지하면) 엔진은 슈퍼뷰에 노티를 때린다.
- 그 이후 뷰에서 실행되는 메소드가 바로 `setNeedsLayout()`이다.

![https://velog.velcdn.com/images/enebin777/post/270a8c61-8be2-47ec-9720-47b15d636d0e/image.png](https://velog.velcdn.com/images/enebin777/post/270a8c61-8be2-47ec-9720-47b15d636d0e/image.png)

- 이런 과정을 거친 뒤

![https://velog.velcdn.com/images/enebin777/post/960f9d8a-2756-4a09-a0a2-db9e739804c0/image.png](https://velog.velcdn.com/images/enebin777/post/960f9d8a-2756-4a09-a0a2-db9e739804c0/image.png)

- 슈퍼뷰는 엔진한테 필요한 값을 물어본 뒤 카피해서 엔진에서 가져온다.
- 그 값을 이용해서 `setBounds`나 `setCenter`등의 메소드로 서브뷰를 다시 레이아웃 시킴.
    - 검색해도 안 나오는데 그냥 개념 설명을 위해 언급한 건가 싶다

![https://velog.velcdn.com/images/enebin777/post/c1896bfb-192d-4ec3-a342-c149a3b42893/image.png](https://velog.velcdn.com/images/enebin777/post/c1896bfb-192d-4ec3-a342-c149a3b42893/image.png)

- 레이아웃을 2배로 늘려보자. 이런 케이스는 조금 까다로울 수 있음.
- 서로다른 두 뷰 사이의 레이아웃이 들어가기 때문임.

![https://velog.velcdn.com/images/enebin777/post/aa3d6e7f-4b6c-4e7a-95c7-4c29c1e5daf5/image.png](https://velog.velcdn.com/images/enebin777/post/aa3d6e7f-4b6c-4e7a-95c7-4c29c1e5daf5/image.png)

![https://velog.velcdn.com/images/enebin777/post/8bd1cf15-4c70-4297-a266-9c7768a773c9/image.png](https://velog.velcdn.com/images/enebin777/post/8bd1cf15-4c70-4297-a266-9c7768a773c9/image.png)

- 서로 다른 두 뷰에 대한 레이아웃은 엔진 안에서 완전한 별개의 블록으로 계산된다.
- 2개의 별개의 블록이므로 서로 영향을 미치지 않고
- 그러므로 로드는 그냥 덧셈이 되어 선형증가한다. (서로 영향이 없으므로 조합, 곱셈 이런거 없음)
- *결국 성능 자랑이었음*

## About Engine

![https://velog.velcdn.com/images/enebin777/post/708586d6-e7a1-4609-9e82-ea9b0d047504/image.png](https://velog.velcdn.com/images/enebin777/post/708586d6-e7a1-4609-9e82-ea9b0d047504/image.png)

- 엔진은 현 상태를 기억하고 바뀐 수치만 정확하게 변경한다

![https://velog.velcdn.com/images/enebin777/post/bb4f0a3c-c8ac-4dec-a5fd-99f83cc716f2/image.png](https://velog.velcdn.com/images/enebin777/post/bb4f0a3c-c8ac-4dec-a5fd-99f83cc716f2/image.png)

![https://velog.velcdn.com/images/enebin777/post/4907ddf4-9244-4e7d-af88-43404cc1ad05/image.png](https://velog.velcdn.com/images/enebin777/post/4907ddf4-9244-4e7d-af88-43404cc1ad05/image.png)

![https://velog.velcdn.com/images/enebin777/post/e3246342-26d1-4e30-9495-a3fb6a017281/image.png](https://velog.velcdn.com/images/enebin777/post/e3246342-26d1-4e30-9495-a3fb6a017281/image.png)

- 안 좋은 constraints(두 번째) → 가짜 디펜던시를 만들어낸다
- 레이아웃을 명시적으로 분리, 구현할 것

## Optimize auto layout

![https://velog.velcdn.com/images/enebin777/post/88c09ca6-a937-4b0d-b3ae-89649bd2f3fe/image.png](https://velog.velcdn.com/images/enebin777/post/88c09ca6-a937-4b0d-b3ae-89649bd2f3fe/image.png)

- 아래로 갈수록 비싼 연산이다
    1. 부등호 : 아주 저렴하다. 
    2. Set constants: 
        - `GestureRecognizer`를 예로 들면 움직일 때마다 translation value를 이용해 set constants를 불러주고 그러면 엔진에서 알아서 잘 업데이트 할 것.
        - 엔진은 레이아웃의 캐시로 동작하기 때문이다
        - 이는 매우 빠른 one-step 업데이트임
    3. 우선순위
        
        ![https://velog.velcdn.com/images/enebin777/post/a2dc3b47-6905-4770-a01f-ceaaba47d7e6/image.png](https://velog.velcdn.com/images/enebin777/post/a2dc3b47-6905-4770-a01f-ceaaba47d7e6/image.png)
        
        - 너비가 100인데 다른 파츠때문에 100의 너비를 온전히 못 가져가면?
        - 서로의 관계와 우선순위를 고려한 추가 작업이 필요함
        - 이 경우 simplex algorithm을 이용해서 오류를 최소화한 값 반환한다(**확신 없음**)
    

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2013.png)

**간단한 최적화 4원칙**

- 해제 할당 반복하지 마라
- 부등호, 덧셈, setConstants나 써라
- 엔진은 레이아웃의 캐시로 동작
- 봐가면서 써라

## Intrinsic Content Size

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2014.png)

---

# Interface builder

---

### 잠깐상식

### Window vs View

- [https://developer.apple.com/documentation/uikit/uiwindow](https://developer.apple.com/documentation/uikit/uiwindow)
- [https://developer.apple.com/documentation/uikit/view_controllers/managing_content_in_your_app_s_windows](https://developer.apple.com/documentation/uikit/view_controllers/managing_content_in_your_app_s_windows)

![https://velog.velcdn.com/images/enebin777/post/2c7e51a6-7d66-4ac2-ac1a-49fe37f51411/image.png](https://velog.velcdn.com/images/enebin777/post/2c7e51a6-7d66-4ac2-ac1a-49fe37f51411/image.png)

![https://velog.velcdn.com/images/enebin777/post/72a71c27-4a57-4c3b-9299-9bfb8cad29e7/image.png](https://velog.velcdn.com/images/enebin777/post/72a71c27-4a57-4c3b-9299-9bfb8cad29e7/image.png)

> **Each scene in your app’s UI contains a window object and one or more view objects.** The window serves as an invisible container for the rest of your UI, acts as a top-level container for your views, and routes events to them. The views provide the actual content that users see onscreen, drawing text, images, and other types of custom content. Windows are long-lived objects, and you dismiss them only when tearing down your scene’s entire UI. By contrast, you might change the views in that window frequently, particularly when you want to display new content or information.
> 

Window는 rootViewController로 대변될 수 있는 최상단의 뷰라고 생각하면 된다. **한마디로 요약하면 View hierarchy의 최상단에 있는 객체인 것.** 윈도우는 눈에 보이는 바로 그 '화면'이고 뷰들은 그 안에 옹기종기 모여 사는 주민인 셈이다.

따라서 유일한 윈도우 아래에는 많은 수의 View들이 존재할 수있으며 UIKit은 `UIViewController`라는 객체를 통해 윈도우와 소통하며 이들을 제어할 수 있게 도와준다.

원래는 AppDelegate에서 윈도우를 만들어줬지만 아이패드를 위시한 다중화면 기능이 등장하면서 SceneDelegate에서 여러개의 윈도우를 만들 수 있게 되었다.

### Bound vs Frame

- [https://suragch.medium.com/frame-vs-bounds-in-ios-107990ad53ee](https://suragch.medium.com/frame-vs-bounds-in-ios-107990ad53ee)

위 글에 보면 잘 설명이 되어있는데 Bound는 뷰 내부의 좌표계 중 어느 부분에서 시작할지를 고르는 것, Frame은 뷰 외부(super view)의 좌표계 중 어느 부분에서 시작할지를 고르는 것이다. 크기는 뭐 같은 뜻이다. 말로하면 잘 안 와닿는데 위 블로그에서 사진 설명 보면 바로 이해 된다.

Frame이 변한다고 bound가 변하지 않는다.
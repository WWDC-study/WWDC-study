# 3 ways of layout views

better on : [3 ways of layout views](https://www.notion.so/3-ways-of-layout-views-d5998d1a9afb49dfa0de3dfdd52ac436) 

UI를 코드로 작성하는 3가지 방식

## 코드로 뷰 잡는 방법 비교

***Frame base layout, Autoresizing mask, Auto layout***

### 1. Frame base layout

![프레임 베이스 레이아웃](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/487c478d-0531-48c5-99ba-db53e2716eef/Untitled.png)

프레임 베이스 레이아웃

- `Frame base` 레이아웃은 origin, width, height 세 수치로 정의합니다.

**장점**

- `Frame base`은 가장 빠른 동작방식입니다
- 정확히 원하는 위치에 배치할 수 있습니다

**단점**

- 모든 뷰에 대해 개별적인 설정과 관리가 필요합니다
- 설계 및 디버그나 유지보수가 까다롭습니다

### 2. **Autoresizing mask**

프레임을 이용해 뷰를 그리는 것은 상당히 번거로운 작업이며 ****이런 문제를 해결하기 위해서 등장한 것이 바로 `autoresizing mask`입니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c59a77e0-7e99-4221-b361-856249c8d763/Untitled.png)

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622559-autoresizingmask)

```swift
sampleSubview.autoresizingMask = [.flexibleWidth, .flexibleHeight]
```

- `autoresizing mask`는 **슈퍼뷰의 프레임이 변할 때 서브뷰의 프레임이 어떻게 변할지 정의하는 방식**으로 bitmask를 이용합니다
- View의 bounds가 변경되면 각 subview는 autoresizing mask에 해당되는 부분을 자동으로 재설정합니다.
- `UIView.AutoresizingMask`에 설명한 상수를 결합하여 값을 조정할 수 있습니다

**장점**

- Frame based 레이아웃에서 불가능했던 external changes에 대한 대응이 가능합니다.

**단점**

- 슈퍼뷰가 반드시 필요하며 뷰의 internal change를 반영할 수 없습니다

***Tips***

- 코드 베이스로 뷰를 잡을 때 간혹 보이는 `translatesAutoresizingMaskIntoConstraints`는 오토 리사이징 마스크를 constraint로 바꿔주는 기능을 온오프 할 수 있는 플래그로 오토 레이아웃을 사용할 때 충돌을 방지하기 위해 `false`로 설정하는 것이 좋습니다.

### 3. AutoLayout

앞서 언급한 모든 단점을 보완하기 위해 `auto layout`이 등장했습니다. 

![오토 레이아웃](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d16844ac-1dea-427f-9155-4f02c363ba44/Untitled.png)

오토 레이아웃

- AutoLayout은 뷰 사이에 `Constraint`를 부여해 레이아웃을 정의합니다.
- 그러므로 Super view는 물론 inner view에 대한 변화도 감지가 가능합니다.

### Constraint

💡 from [Guide](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW1) : 
**The layout of your view hierarchy is defined as a series of linear equations.** Each constraint represents a single equation. Your goal is to declare a series of equations that has one and only one possible solution.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e11af310-6729-497b-84e3-ae1fd85c8067/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12ecfdbf-09bf-4b49-a4e0-2b6a8568f18d/Untitled.png)

- Constraint는 수식 관계를 기반으로 뷰 사이의 배치를 결정합니다.

## 정리

---

- UIKit에서 UI를 구성하는 방식은 크게 frame base, auto resizing mask, auto layout 3가지가 있습니다.
- Frame base 방식의 경우 가장 빠르고 정확하나 다른 뷰와의 관계 설정이 힘든 단점이 있습니다.
- Auto resizing mask는 슈퍼뷰와의 관계를 설정할 수 있으나 뷰 내부 변화를 감지할 수 없는 단점이 있습니다.
- Auto layout은 뷰와 뷰 사이의 constraint를 통해 레이아웃을 결정하며 이를 통해 다른 방식의 문제를 해결합니다.

## 참고

---

[](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjZpIfNpKP6AhVdQfUHHYuLBIYQFnoECAkQAQ&url=https://devstreaming-cdn.apple.com/videos/wwdc/2018/220f49ijgby0rma/220/220_high_performance_auto_layout.pdf?dl=1&usg=AOvVaw3V9nKwjumyOlPa-XTiEXdj)

[[iOS - swift] AutoresizingMask, translatesAutoresizingMaskIntoConstraints 개념](https://ios-development.tistory.com/672)

[Auto Layout Guide: Understanding Auto Layout](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

[Demystifying iOS Layout](https://tech.gc.com/demystifying-ios-layout/)
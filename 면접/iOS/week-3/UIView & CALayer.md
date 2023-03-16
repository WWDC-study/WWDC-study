# UIView - Layer

better on : [UIView - Layer](https://www.notion.so/UIView-Layer-83167a6d5e324cfe9522ca0da287c6f0) 

렌더링에 사용하는 뷰의 Core Animation Layer(`CALayer`)입니다

## Layer

---

`**UIView`의 레이어는 뷰의 시각적 콘텐츠를 관리하는 경량(lightweight) 객체입니다.** 

- 레이어는 뷰의 콘텐츠 렌더링 및 애니메이션을 담당하며, 코어 애니메이션 프레임워크 `CALayer`의 서브 클래스입니다.
- 모든 `UIView`는 뷰의 시각적 표현을 관리하는 레이어가 있습니다. 이 레이어는 뷰의 콘텐츠를 그리고, 애니메이션을 처리하고, 그림자, 테두리 및 그라데이션과 같은 다양한 시각 효과를 적용하는 역할을 담당합니다.
- 배경색, 불투명도 및 변환과 같은 레이어 속성을 조작하여 뷰의 모양을 커스텀할 수 있습니다. 또한 레이어는 서브레이어, 마스킹 같은 고급 기능을 지원하므로 복잡한 시각 효과 및 애니메이션을 쉽게 구현할 수 있습니다.

### CALayer

이미지 기반(?)의 콘텐츠를 관리하고 해당 콘텐츠에 애니메이션을 적용할 수 있는 객체입니다.

![https://miro.medium.com/v2/resize:fit:1000/format:webp/0*6nELhZgbTKgD5cL4.png](https://miro.medium.com/v2/resize:fit:1000/format:webp/0*6nELhZgbTKgD5cL4.png)

**레이어는 종종 view를 위한 백그라운드 저장소(store)를 제공하는 데 사용**되지만 **view 없이도 콘텐츠를 표시하는 데 사용할 수 있습니다**.(특수한 경우를 제외하고 권장되지는 않습니다) 

- 레이어의 주요 역할은 사용자가 제공한 비주얼 콘텐츠들을 관리하는 것이지만 레이어 자체에도 배경색, 테두리, 그림자 등 설정할 수 있는 비주얼 속성이 있습니다.
- 레이어는 비주얼 콘텐츠를 관리하는 것 외에도 해당 콘텐츠를 화면에 표시하는 데 사용하는 콘텐츠의 지오메트리(예: location, size 및 transformation) 정보도 관리합니다.
- 레이어의 속성(property)을 수정하면 레이어의 콘텐츠 또는 지오메트리에서 애니메이션을 시작할 수 있습니다.
- 레이어 개체는 레이어의 타이밍 정보를 정의하는 `CAMediaTiming` 프로토콜을 채택하여 레이어와 해당 애니메이션의 지속 시간 및 페이스를 캡슐화합니다.

***Tips***

- 뷰에서 레이어 개체가 만들어질 경우 일반적으로 뷰는 자신을 레이어의 델리게이트(`[CALayerDelegate](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiTttqNhN79AhUTCN4KHTJSApcQFnoECAgQAQ&url=https%3A%2F%2Fdeveloper.apple.com%2Fdocumentation%2Fquartzcore%2Fcalayerdelegate&usg=AOvVaw0zUzhy6I2EpkrpFZMAywh-)`)로 자동 할당합니다. 이를 변경해서는 안 됩니다.
- 레이어를 직접 만들 경우 뷰에 델리게이트 개체를 수동으로 할당합니다. 이후 레이어의 콘텐츠를 동적으로 제공하고 다른 작업을 수행할 수 있습니다.
- 레이어에는 `layoutManager` 객체가 있어 하위 뷰의 레이아웃을 개별적으로 관리할 수도 있습니다.

## UIView vs CALayer

---

```swift
@MainActor open class UIView : 
UIResponder, NSCoding, UIAppearance, UIAppearanceContainer, 
UIDynamicItem, UITraitEnvironment, UICoordinateSpace, 
UIFocusItem, UIFocusItemContainer, CALayerDelegate {
		...
		open var layer: CALayer { get }
}
```

```swift
open class CALayer : NSObject, NSSecureCoding, CAMediaTiming
```

`UIView`는 `CALayer`를 둘러싼 얇은(thin) 래퍼 클래스입니다. `CALayer`로도 뷰를 구성할 수 있으나 [누군가 진행한 실험 결과](https://stackoverflow.com/questions/7826306/what-are-the-differences-between-a-uiview-and-a-calayer)에 따르면 `UIView`에 비해 성능상 큰 이점은 없습니다.

- `UIView`는 `UIResponder` 객체로서 유저와의 상호작용에 응답하는 역할을 갖습니다. 반면 `CALayer`는 시각적 콘텐츠를 처리하는 데에만 집중합니다.
- `UIView`와 달리 `CALayer`는 AutoLayout을 적용할 수 없습니다.
- `CALayer`로만 뷰를 그리는 것은 다음 목적으로 고려해 볼 수 있습니다.
    - 크로스 플랫폼(macOS) 최적화: macOS와 iOS는 각각 `NSView`, `UIView`를 사용하며 약간의 차이가 존재합니다. CorePlot framework를 이용하면 `CALayer`를 통해 거의 차이 없는 뷰를 구성할 수 있습니다.

## 예시

---

각각 다음과 같은 방식으로 뷰를 그릴 수 있습니다

### UIView

```swift
// Create a new UIView with frame (x: 0, y: 0, width: 200, height: 200)
let myView = UIView(frame: CGRect(x: 0, y: 0, width: 200, height: 200))

// Set the background color of the view
myView.backgroundColor = UIColor.blue

// Add the view to the main view
view.addSubview(myView)
```

### CALayer

```swift
let myLayer = CALayer()
myLayer.frame = CGRect(x: 0, y: 0, width: 200, height: 200)

// Set the background color of the layer
myLayer.backgroundColor = UIColor.blue.cgColor

// Add the layer to the main layer of the view controller's view
view.layer.addSublayer(myLayer)
```

## 정리

---

- `UIView`의 레이어는 뷰의 시각적 콘텐츠를 관리하는 경량 `CALayer` 객체입니다.
- `UIView`는 `CALayer`의 래퍼 클래스이며 주로 사용자 상호작용은 `UIView`, 시각적인 콘텐츠 처리는 `CALayer`에서 이루어집니다.

## 참고
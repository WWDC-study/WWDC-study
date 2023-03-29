_better on: https://www.notion.so/enebin/UIView-058e25ce6da249c3a3015087c3339bf9_

`UIView`는 화면의 직사각형 영역에 대한 콘텐츠를 관리하는 객체입니다.

## UIView

---

### 정의

뷰는 앱 사용자 인터페이스의 기본 구성 요소입니다.

다음은 `UIView`가 수행하는 동작입니다.

1. `UIView` 클래스는 **모든 뷰에 공통적으로 적용되는 동작을 정의**합니다.
2. 뷰 객체는 **바운드 사각형 내에서 콘텐츠를 렌더링하고 해당 콘텐츠와의 모든 상호 작용을 처리**합니다. 
3. `UIView` 클래스를 서브클래싱 해 **보다 정교한 콘텐츠를 그릴 수도 있습니다**.

### 할 수 있는 것

`UIView` 객체는 애플리케이션이 사용자와 상호 작용하는 주된 방식이므로 여러가지의 책임을 갖습니다.

다음은 `UIView`가 갖는 책임의 일부입니다:

1. **그리기 및 애니메이션**
    - 뷰는 `UIKit` 또는 `CoreGraphic`을 사용하여 해당 직사각형 영역에 콘텐츠를 그립니다.
    - 일부 뷰 속성을 새 값으로 애니메이션화 할 수 있습니다.
2. **레이아웃 및 하위 뷰 관리**
    - 뷰에는 0개 이상의 `subView`가 포함될 수 있습니다.
    - 뷰는 `subView`의 크기와 위치를 조정할 수 있습니다.
    - `AutoLayout`을 사용하여 뷰 계층 구조의 변경에 따라 뷰의 크기를 조정하고 위치를 변경하는 규칙을 정의할 수 있습니다.
3. **이벤트 처리**
    - 뷰는 `UIResponder`의 하위 클래스이며 터치 및 기타 유형의 이벤트에 응답할 수 있습니다.
    - 뷰는 `GestureRecognizer`를 인스톨 해 일반적인 제스처를 처리할 수 있습니다.

***Tips***

- 뷰는 다른 뷰 안에 중첩하여 뷰 계층을 만들 수 있으며, 이는 관련 콘텐츠를 편리하게 구성할 수 있는 방법을 제공합니다. 뷰를 중첩하면 중첩된 자식 뷰(`subView`)와 상위 뷰(`superView`) 간에 부모-자식 관계가 만들어집니다.
- 상위 뷰에는 하위 뷰가 여러 개 포함될 수 있지만 각 하위 뷰에는 하나의 상위 뷰만 존재합니다.
- 기본적으로 하위 뷰의 표시 영역이 해당 상위 뷰의 범위를 벗어나면 하위 뷰 콘텐츠의 클리핑이 발생하지 않습니다. 이 동작을 변경하려면 `clipsToBounds` 속성을 사용합니다.

### 만들기

일반적으로 스토리보드에서 뷰를 라이브러리에서 캔버스로 드래그하여 뷰를 만듭니다. 물론 코드 기반 방식으로 뷰를 만들 수도 있습니다.

다음은 UIVIew를 만드는 방식입니다.:

1. 뷰를 만들 때 일반적으로 **뷰의 초기 크기와 위치를 향후 슈퍼뷰를 기준으로 지정**합니다. 예를 들어 다음 예제에서는 뷰를 만들고 뷰의 왼쪽 상단 모서리를 상위 뷰 좌표계의 (10, 10) 지점에 배치합니다. ([CGRect?](https://developer.apple.com/documentation/corefoundation/cgrect))
    
    ```swift
    let rect = CGRect(x: 10, y: 10, width: 100, height: 100)
    let myView = UIView(frame: rect)
    ```
    
2. **하위 뷰를 추가하려면 상위 뷰에서 `addSubview` 메서드를 호출**합니다. `addSubview` 메서드를 호출할 때마다 새 뷰가 최상위에 배치됩니다.
3. `insertSubview(:aboveSubview:)` 및 `insertSubview(:belowSubview:)` 메서드를 사용하여 하위 뷰를 추가하여 하위 뷰의 상대적인 z index를 지정할 수 있습니다. 
    - `exchangeSubview(at:withSubviewAt:)` 메서드를 사용하여 이미 추가된 하위 뷰의 위치를 교환할 수도 있습니다.
4. 뷰를 만든 후에는 **`AutoLayout` `constraint`을 만들어 뷰의 크기와 위치가 변경되는 방식을 관리**합니다.

### 그리기

뷰 그리기는 필요에 의해 발생합니다. 뷰를 처음 표시하거나 레이아웃 변경으로 인해 뷰의 전체 또는 일부를 표시하는 경우 시스템에서 뷰에 콘텐츠를 그리도록 요청합니다. 

1. `UIKit` 또는 코어 그래픽을 사용하는 콘텐츠를 포함한 뷰의 경우 시스템에서 뷰의 `draw` 메서드를 호출합니다.
2. 뷰의 실제 콘텐츠가 변경되는 경우 뷰를 다시 그려야 한다는 사실을 시스템에 알리는 것은 사용자의 책임입니다. 이 작업은 뷰의 `setNeedsDisplay` 또는 `setNeedsDisplay` 메서드를 호출하여 수행합니다. 
    - 이 메서드는 다음 draw cycle에 뷰를 업데이트해야 한다는 것을 시스템에 알립니다. 뷰 업데이트는 다음 draw cycle까지 대기 후 이루어집니다. 따라서 여러 뷰의 업데이트를 동시에 실행할 수 있습니다.
    - 관련 문서([About Windows and Views (apple.com)](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009503))

### 애니메이션

[UIKitPlayground/AnimateViewController.swift at f0b4b55572b5d0be7bb3ac48fa6d2cb560207df4 · enebin/UIKitPlayground](https://github.com/enebin/UIKitPlayground/blob/f0b4b55572b5d0be7bb3ac48fa6d2cb560207df4/UIKitPlayground/Week1/AnimateViewController.swift)

뷰 속성에 대한 변경 사항을 애니메이션으로 만들 수 있습니다. 즉, 속성을 변경하면 현재 값에서 시작하여 지정한 새 값에서 끝나는 애니메이션이 생성됩니다. 

애니메이션이 가능한 `UIView` 프로퍼티는 다음과 같습니다:

1. `frame`
2. `bounds`
3. `center`
4. `transform`
5. `alpha`
6. `backgroundColor`

***Tips***

- 변경 사항에 애니메이션을 적용하려면 `UIViewPropertyAnimator` 객체를 생성하고 해당 핸들러 블록을 사용하여 뷰의 속성 값을 변경합니다.
    
    ```swift
    func uiViewPropertyAnimator() {
        let animation = UIViewPropertyAnimator(duration: 3, curve: .easeInOut) {
            self.sampleView.frame.origin.y += 100
            self.sampleView.alpha = 0.5
        }
        animation.startAnimation()
    }
    ```
    
- `UIViewPropertyAnimator` 클래스를 사용하면 애니메이션의 지속 시간과 타이밍을 지정할 수 있지만 실제 애니메이션은 이 클래스가 수행합니다.
- 현재 실행 중인 애니메이션을 중단하고 인터랙티브하게 재시작할 수도 있습니다.

## 정리

---

`UIView`는 iOS 애플리케이션에서 가장 기본적인 UI 객체 중 하나입니다. 

- UIKit의 모든 화면 요소는 `UIView` 객체를 기반으로 만들어지며, 이 객체를 사용하여 화면에 콘텐츠를 배치하고 사용자 상호작용을 처리합니다.
- `UIView` 객체의 정의와 책임, 생성 방법, 그리기 및 레이아웃, 애니메이션, 이벤트 처리에 대해 알아보았습니다.

## 참고
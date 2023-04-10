# 코드로 AutoLayout 작성하는 3가지 방법

better on : [코드로 AutoLayout 작성하는 3가지 방법](https://www.notion.so/AutoLayout-3-e761abd7cb094bd1b8d23ef119309a14) 

## code-based AutoLayout

---

### ****[NSLayoutConstraint](https://developer.apple.com/documentation/uikit/nslayoutconstraint)****

```swift
let label = UILabel()
label.translatesAutoresizingMaskIntoConstraints = false
view.addSubview(label)

let constraint1 = NSLayoutConstraint(item: label, attribute: .leading, relatedBy: .equal, toItem: view, attribute: .leading, multiplier: 1, constant: 20)
let constraint2 = NSLayoutConstraint(item: label, attribute: .top, relatedBy: .equal, toItem: view, attribute: .top, multiplier: 1, constant: 20)

NSLayoutConstraint.activate([constraint1, constraint2])
```

**NSLayoutConstraint**는 AutoLayout constraint를 가장 상세하게 설정할 수 있는 방법입니다. 

- 두 개의 뷰 사이 constraint 관계를 수식의 형태로 지정하여 제약 조건을 만들 수 있습니다
- constraint은 동등 비교에 국한되지 않습니다. 두 속성 간의 관계를 설명하기 위해 >= 또는 <=을 사용할 수도 있습니다.
- constraint에는 1에서 1,000 사이의 우선 순위(priority)도 있습니다. 우선 순위가 1,000인 제약 조건은 필수적임을 나타냅니다. 우선 순위가 1,000 미만인 모든 제약 조건은 선택적임을 나타냅니다. 기본적으로 모든 제약 조건은 필수적입니다(우선순위 = 1,000).
- 필수 constraint을 resolve한 후 자동 레이아웃은 우선순위가 높은 순서부터 낮은 순서로 선택적 constraint을 resolve 시도합니다. 선택적 constraint을 해결할 수 없는 경우 원하는 결과에 최대한 근접하게끔 조정한 다음 constraint으로 이동합니다.

### ****[NSLayoutAnchor](https://developer.apple.com/documentation/uikit/nslayoutanchor)****

```swift
let label = UILabel()
label.translatesAutoresizingMaskIntoConstraints = false
view.addSubview(label)

let constraint1 = label.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20)
let constraint2 = label.topAnchor.constraint(equalTo: view.topAnchor, constant: 20)

NSLayoutConstraint.activate([constraint1, constraint2])
```

****Layout Anchor****는 코드로 AutoLayout constraint를 지정하는 방식 중 하나입니다.

- 레이아웃 앵커는 `leadingAnchor`, `trailingAnchor`, `topAnchor`, `bottomAnchor`와 같은 `UIView`의 속성입니다. 이러한 속성을 이용해 서로 다른 뷰 간에 제약 조건을 생성할 수 있습니다.
- 예제에서 볼 수 있듯이 NSLayoutAnchor 클래스는 NSLayoutConstraint API를 직접 사용하는 것보다  깔끔하고 간결한 코드를 제공합니다.
- NSLayoutConstraint.Attribute 서브클래스는 추가 타입 체킹을 제공하여 유효하지 않은 constraint를 생성하지 않도록 방지합니다.

### ****[Visual Format Language](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html)****

```swift
let label = UILabel()
label.translatesAutoresizingMaskIntoConstraints = false
view.addSubview(label)

let views: [String: Any] = ["label": label]
let metrics: [String: Any] = ["spacing": 20]

let horizontalConstraints = NSLayoutConstraint.constraints(withVisualFormat: "H:|-(spacing)-[label]", options: [], metrics: metrics, views: views)
let verticalConstraints = NSLayoutConstraint.constraints(withVisualFormat: "V:|-(spacing)-[label]", options: [], metrics: metrics, views: views)

NSLayoutConstraint.activate(horizontalConstraints + verticalConstraints)
```

****[Visual Format Language](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html)****(VFL)는 문자열 기반 구문을 사용하여 constraint를 생성하는 방법입니다. 

- VFL을 사용하면 한 줄의 코드에서 여러 뷰에 대한 constraint를 한 번에 지정할 수 있습니다.

## 정리

---

- ****[NSLayoutConstraint](https://developer.apple.com/documentation/uikit/nslayoutconstraint)****: 두 개의 뷰 사이 constraint 관계를 수식의 형태로 지정하여 제약 조건을 만들 수 있습니다
- ****[NSLayoutAnchor](https://developer.apple.com/documentation/uikit/nslayoutanchor)****:  `leadingAnchor`, `trailingAnchor`, `topAnchor`, `bottomAnchor`와 같은 `UIView`의 속성을 이용해 관계를 지정하며 추가 타입 체킹을 제공하여 유효하지 않은 constraint를 생성하지 않도록 방지합니다.
- ****[Visual Format Language](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html)****: 문자열 기반 구문을 사용하여 constraint를 생성합니다

## 참고
[Layout Anchors | Swift by Sundell](https://www.swiftbysundell.com/basics/layout-anchors/)

[NSLayoutAnchor | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/nslayoutanchor)

[NSLayoutConstraint | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/nslayoutconstraint)

[Auto Layout Guide: Visual Format Language](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html)
# Hugging & Compression resistance

## View Priorities

---

### Hugging

Hugging priority가 높을수록 자신의 Intrinsic size 이상으로 사이즈가 커지는 것을 기피합니다. 즉, 사이즈 증가에 반대로 버티며 껴안고 있으려고 하는 성질입니다.

- `[setContentHuggingPriority(_:for:)](https://developer.apple.com/documentation/uikit/uiview/1622485-setcontenthuggingpriority)`를 이용해 설정할 수 있습니다.

### (Compression) Resistance

Compression priority가 높을수록 자신의 Intrinsic size 이하로 사이즈가 작아지는 것을 기피합니다. 즉, 사이즈 축소에 반대로 버티며 저항하려는 성질입니다.

- `[setContentCompressionResistancePriority(_:for:)](https://developer.apple.com/documentation/uikit/uiview/1622526-setcontentcompressionresistancep)`를 이용해 설정할 수 있습니다.

## 예시

---

**다음과 같은 코드를 가정합니다.** 

- `leftLabel`의 Hugging Priority는 `required`로 `rightLabel`의 `defaultLow`보다 높습니다.
- 코드보기
    
    ```swift
    class ViewController: UIViewController {
        private let leftLabel: UILabel = {
            let label = UILabel()
            label.text = "Lorem ipsum dolor sit amet, consectetur"
            label.textColor = .white
            label.backgroundColor = .blue
            return label
        }()
        
        private let rightLabel: UILabel = {
            let label = UILabel()
            label.text = "adipiscing elit. Curabitur commodo aliquam."
            label.textColor = .white
            label.backgroundColor = .orange
            return label
        }()
        
        override func viewDidLoad() {
            super.viewDidLoad()
            
            self.view.addSubview(self.leftLabel)
            self.view.addSubview(self.rightLabel)
            
            self.leftLabel.setContentHuggingPriority(.required, for: .horizontal)
            self.leftLabel.snp.makeConstraints {
                $0.top.equalToSuperview().inset(56)
                $0.left.equalToSuperview()
            }
            
            self.rightLabel.setContentHuggingPriority(.defaultLow, for: .horizontal)
            self.rightLabel.snp.makeConstraints {
                $0.top.equalToSuperview().inset(56)
                $0.left.equalTo(self.leftLabel.snp.right)
                $0.right.equalToSuperview()
            }
        }
    }
    ```
    

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6aea949f-f1d8-450b-be34-b7204e4dc781/Untitled.png)

- `leftLabel`이 사이즈를 지키려는 성질이 더 강하므로 `rightLabel`의 크기가 늘어납니다.

**이번에는 다음과 같이 Compression resistance priority를 번경하고 글자 수를 늘려보겠습니다.**

```swift
self.leftLabel.setContentCompressionResistancePriority(.required, for: .horizontal)
self.rightLabel.setContentCompressionResistancePriority(.defaultLow, for: .horizontal)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65fd79cc-497b-4723-87a6-f9b822f027f9/Untitled.png)

- 이번에도 `leftLabel`이 사이즈를 지키려는 성질이 더 강하므로 `rightLabel`의 크기가 줄어듭니다.

## 정리

---

- Hugging priority가 높을수록 **자신의 크기 이상으로 증가하는 변화에 내한 내성이 강해집니다**.
- Compression resistance priority가 높을수록 **자신의 크기 이하로 감소하는 변화에 내한 내성이 강해집니다**.

## 참고

---

[setContentHuggingPriority(_:for:) | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622485-setcontenthuggingpriority)

[setContentCompressionResistancePriority(_:for:) | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622526-setcontentcompressionresistancep)

[[iOS - swift] 1. Autolayout 고급 (with SnapKit) - Hugging, Compression, priority 개념](https://ios-development.tistory.com/865)
# UIView 커스텀

better on : [Custom UIVIew](https://www.notion.so/enebin/Custom-UIView-971fcfa264784b4fbdb20d60168e7c8a?pvs=4)


`UIView`를 커스텀하기 위해서는 크게 2가지 방식을 사용합니다.

### 1. 스토리보드 또는 XIB 파일 사용하기

![https://velog.velcdn.com/images/enebin777/post/db51e302-88a7-4f43-9341-a0c768f6e459/image.png](https://velog.velcdn.com/images/enebin777/post/db51e302-88a7-4f43-9341-a0c768f6e459/image.png)

![https://velog.velcdn.com/images/enebin777/post/71f552a2-ceb9-486a-b372-a56829676755/image.png](https://velog.velcdn.com/images/enebin777/post/71f552a2-ceb9-486a-b372-a56829676755/image.png)

- 스토리보드 또는 `XIB` 파일을 사용하여 커스텀 `UIView`를 만들 수 있습니다.
- Xcode에서 새 파일을 생성한 다음 Interface builder에서 레이블, 버튼 및 이미지와 같은 다양한 UI 요소를 추가하고 구성하여 `UIView`를 시각적으로 디자인합니다. 커스텀 `UIView`를 디자인한 후에는 스토리보드 또는 `XIB` 파일에서 인스턴스화하여 코드에서 인스턴스를 생성할 수 있습니다.
- 스토리보드 또는 `XIB` 파일을 사용하면 뷰를 시각적으로 디자인할 수 있어 복잡한 레이아웃을 가시적으로 확인하는 데 유용할 수 있습니다.

### 2. 코드베이스로 만들기

```swift
class MyCustomView: UIView {

    private let titleLabel = UILabel()

    override init(frame: CGRect) {
        super.init(frame: frame)
        setupViews()
    }

    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        setupViews()
    }

    private func setupViews() {
        titleLabel.text = "My Custom View"
        titleLabel.textColor = .black
        titleLabel.textAlignment = .center
        addSubview(titleLabel)
    }

    override func layoutSubviews() {
        super.layoutSubviews()
        titleLabel.frame = bounds
    }

}
```

```swift
class ViewController: UIViewController {
		// If using storyboard
    @IBOutlet weak var customView: MyCustomView!

    override func viewDidLoad() {
        super.viewDidLoad()
        customView.titleLabel.text = "Hello World"
				
				// or using code-based method
				let subView = CustomView(frame: CGRect(x: 50, y: 50, width: 100, height: 100)				
				view.addSubview(subView)
    }
}
```

- 코드로 `UIView` 객체를 생성, 서브클래싱 한 후 UI 요소를 추가하는 방식입니다.
- 이 방식은 빌더를 거치지 않고 코드를 바탕으로 뷰를 추론할 수 있다는 장점이 있으나 복잡한 레이아웃의 경우 인터페이스 빌더에 비해 시간이 더 많은 시간을 소요하는 단점이 있습니다.

***Tips***

- `XIB` 파일을 이용한 UIView를 추가할 때 주의해야할 점이 있습니다. 바로 UINib으로 XIB 파일을 로드한 후  `instantiate` 메서드를 이용해 UIView 객체를 얻어야 하는 것입니다.
    
    ```swift
    class CustomView: UIView {
        @IBOutlet var label: UILabel!
        
        override init(frame: CGRect) {
            super.init(frame: frame)
            setup()
        }
        
        required init?(coder aDecoder: NSCoder) {
            super.init(coder: aDecoder)
            setup()
        }
        
        private func setup() {
            let nib = UINib(nibName: "CustomView", bundle: Bundle(for: CustomView.self))
            let view = nib.instantiate(withOwner: self, options: nil).first as! UIView
            addSubview(view)
            view.frame = bounds
        }
    }
    ```
    
    - 위와 같은 작업을 거쳐야 정삭적으로 `UIView`가 표시됩니다.
    

## 정리

---

- `UIView`는 스토리보드(xib)와 코드 베이스의 2가지 방식을 통해 만들고 커스텀할 수 있습니다.
- 스토리보드를 사용할 때는 뷰를 가시적으로 빠르게 만들 수 있는 장점이 있습니다.
- 코드를 사용할 때는 빌더를 거치지 않고 코드를 통해 직관적으로 확인할 수 있다는 장점이 있습니다.

## 참고

---

[UIView | Apple Developer Documentation](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwis6Z2Grtv9AhWP1GEKHe3dAVwQFnoECA0QAQ&url=https://developer.apple.com/documentation/uikit/uiview&usg=AOvVaw1QQF3A1Eh14aS1gN1p4jhI)

[[iOS - swift] Custom View (xib)](https://ios-development.tistory.com/200)
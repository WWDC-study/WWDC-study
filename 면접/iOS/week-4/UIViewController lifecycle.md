# UIViewController - Lifecycle

better on : [UIViewController - Lifecycle](https://www.notion.so/UIViewController-Lifecycle-0188089308b848a78a0d7b6e373bc974) 

UIViewController는 뷰와 뷰 관련 계층 구조를 관리하는 객체입니다

## UIVIewController’s lifecycle

---

![https://static.packt-cdn.com/products/9781783550814/graphics/0814OT_06_02.jpg](https://static.packt-cdn.com/products/9781783550814/graphics/0814OT_06_02.jpg)

뷰 컨트롤러의 라이프사이클은 생성부터 메모리에서 제거까지 여러 단계로 구성됩니다.

### init

뷰 컨트롤러는 코드 베이스 혹은 스토리보드를 이용해 만듭니다. 지정(designated) 이니셜라이저(예: 코드 베이스로 만드는 경우 `init(nibName:bundle:)`, 스토리보드로 만드는 경우 `init(coder:)`)를 호출하여 뷰 컨트롤러의 속성 및 리소스를 초기화합니다.

### [loadView](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621454-loadview)

뷰 컨트롤러는 뷰 속성이 요청되었지만 현재 `nil`인 경우 이 메서드를 호출합니다. 이 메서드는 뷰를 로드하거나 생성한 후 뷰 프로퍼티에 할당합니다. 이 메서드를 직접 호출해서는 안 됩니다. 

뷰 컨트롤러가 스토리보드를 통해 초기화 된 경우 스토리보드 파일에서 뷰 계층 구조를 로드합니다. 그렇지 않은 경우 `loadView()` 메서드를 `override`해 만들 수 있습니다.

### [viewDidLoad](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621495-viewdidload#discussion)

이 메서드는 뷰 컨트롤러가 뷰 계층 구조를 메모리에 로드한 후에 호출됩니다. 이 메서드는 뷰 계층 구조가 `nib` 파일에서 로드되었는지 또는 `loadView` 메서드에서 프로그래밍 방식으로 생성되었는지에 관계없이 호출됩니다. 일반적으로 이 메서드를 `override` 해 `nib` 파일에서 로드한 뷰에 대한 추가 초기화를 수행합니다.

### [viewWillAppear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621510-viewwillappear)

이 메서드는 뷰 컨트롤러의 뷰가 뷰 계층 구조에 추가되기 전, 뷰를 표시하기 위한 애니메이션이 구성되기 전에 호출됩니다. 이 메서드를 `override` 해 뷰 표시와 관련된 사용자 지정 작업을 수행할 수 있습니다. 

예를 들어 이 메서드를 사용해 뷰 또는 상태 표시줄의 방향, 스타일을 변경할 수 있습니다. 이 메서드를 `override`하는 경우 반드시 `super` 메서드를 호출해야 합니다.

### [viewDidAppear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621423-viewdidappear)

이 메서드를 `override`하여 뷰 표시와 관련된 추가 작업을 수행할 수 있습니다. 이 메서드를 `override`하는 경우 반드시 `super` 메서드를 호출해야 합니다.

### [viewWillDisappear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621485-viewwilldisappear)

이 메서드는 뷰 계층 구조에서 뷰가 제거될 때 호출됩니다. 이 메서드는 뷰가 실제로 제거되기 전, 어떠한 애니메이션이 구성되기 전에 호출됩니다.

서브클래스는 이 메서드를 `override`하여 변경 사항을 커밋하거나, 뷰의 first reponder state를 해제하거나, 기타 관련 작업을 수행하는 데 사용할 수 있습니다. 

예를 들어 이 메서드로  `viewDidAppear(_:)` 메서드에서 수행한 상태 표시줄의 방향 또는 스타일 변경을 초기화할 수 있습니다. 이 메서드를 `override`하는 경우 반드시 `super` 메서드를 호출해야 합니다.

### [viewDidDisappear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621477-viewdiddisappear)

이 메서드를 `override`하여 뷰를 해제하거나 숨기는 등의 추가 작업을 수행할 수 있습니다. 이 메서드를 `override`하는 경우 반드시 `super` 메서드를 호출해야 합니다.

### deinit

뷰 컨트롤러가 더 이상 필요하지 않은 경우 뷰 컨트롤러가 할당 해제되고 `deinit` 메서드를 호출합니다. 이 메서드에서 리소스를 해제하는 등의 마무리 작업을 할 수 있습니다.

## 정리

---

UIVIewController의 생명주기는 다음과 같습니다.

1. `init`: 인스턴스가 생성
2. `loadView`: 뷰를 생성하고 view 프로퍼티에 할당
3. `viewDidLoad`: 뷰가 로드 되었음
4. `viewWillAppear`: 뷰가 뷰 계층구조에 할당되기 직전
5. `viewDidAppear`: 뷰가 뷰 계층구조에 할당되었음을 알림
6. `viewWillDisappear`: 뷰가 뷰 계층구조에서 해제되기 직전
7. `viewDidDisappear`: 뷰가 뷰 계층구조에서 해제되었음을 알림
8. `deinit`: 인스턴스가 메모리에서 해제

## 참고

---

[View hierarchy (apple.com)](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/View%20Hierarchy.html)
[UIViewController | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiviewcontroller#1652793)
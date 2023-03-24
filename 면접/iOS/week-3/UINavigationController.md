# UINavigationController

better on : https://www.notion.so/enebin/UINavigationController-40fe8eb29c5843949b7d7dbd707bd403?pvs=4

계층적(hierarchical) 콘텐츠를 탐색하기 위해 스택 기반 뷰 체계를 정의하는 컨테이너 뷰 컨트롤러입니다

## 개요

---

![https://docs-assets.developer.apple.com/published/83ef757907/navigation_interface_2x_8f059f7f-2e2f-4c86-8468-7402b7b3cfe0.png](https://docs-assets.developer.apple.com/published/83ef757907/navigation_interface_2x_8f059f7f-2e2f-4c86-8468-7402b7b3cfe0.png)

`**UINavigationController`는 네비게이션 인터페이스 아래 하나 이상의 자식 뷰 컨트롤러를 관리하는 컨테이너 뷰 컨트롤러입니다.** 

- 이 인터페이스에서는 한 번에 하나의 자식 뷰 컨트롤러만 표시합니다.
- 뷰 컨트롤러에서 항목을 선택하면 애니메이션을 사용하여 새 뷰 컨트롤러가 화면에 푸시되고 이전의 뷰 컨트롤러는 숨겨집니다.
- 인터페이스 상단의 네비게이션 바에서 `back` 버튼을 탭하면 보이는 뷰 컨트롤러를 제거하고 서브 뷰 컨트롤러가 표시됩니다.
- 앱의 요구 사항에 따라 `edit` 또는 `add`와 같은 추가 사용자 지정 버튼을 포함할 수도 있습니다.

### Root view cotroller

`UINavigationController`를 사용하려면 `rootViewController`로 초기화해야 합니다.

- `rootViewController`는 탐색 계층 구조에 표시되는 첫 번째 뷰 컨트롤러입니다.
- 사용자가 앱을 네비게이션할 때 다른 뷰 컨트롤러를 네비게이션 스택으로 푸시하거나 팝아웃하여 이전 뷰 컨트롤러로 돌아갈 수 있습니다.

### Navigation stack

`UINavigationController` 객체는 내비게이션 스택이라고 하는 정렬된 배열을 사용하여 자식 뷰 컨트롤러를 관리합니다. 

- 배열의 첫 번째 뷰 컨트롤러는 루트 뷰 컨트롤러이며 스택의 맨 아래를 나타냅니다.
- 배열의 마지막 뷰 컨트롤러는 스택의 최상위 항목이며 현재 표시 중인 뷰 컨트롤러를 나타냅니다.
- 세그웨이 또는 클래스 메서드를 사용하여 스택에서 뷰 컨트롤러를 추가 및 제거할 수 있습니다.
- 사용자는 네비게이션 바의 뒤로 버튼을 사용하거나 왼쪽 가장자리에서 스와이프 제스처를 사용하여 최상위 뷰 컨트롤러를 제거할 수도 있습니다.

### Bar, Toolbar

![https://velog.velcdn.com/images/enebin777/post/e97bafb4-2cfb-476a-9fcd-68197d80d438/image.png](https://velog.velcdn.com/images/enebin777/post/e97bafb4-2cfb-476a-9fcd-68197d80d438/image.png)

네비게이션 컨트롤러는 인터페이스 상단의 네이베이션 바와 인터페이스 하단의 *옵셔널* 툴바를 관리합니다. 

- 네비게이션 바는 항상 존재하며 네비게이션 컨트롤러가 자체적으로 관리합니다. 네비게이션 컨트롤러는 하위 뷰 컨트롤러에서 제공하는 콘텐츠를 사용하여 네비게이션 바를 업데이트합니다.
- `isToolbarHidden` 속성이 `false`인 경우 탐색 컨트롤러는 마찬가지로 최상위 뷰 컨트롤러에서 제공하는 콘텐츠로 툴바를 업데이트합니다.

### Navigation delegate

![https://docs-assets.developer.apple.com/published/83ef757907/nav_controllers_objects_a8447aef-d652-4ab9-85d1-1eb8e4876e12.jpg](https://docs-assets.developer.apple.com/published/83ef757907/nav_controllers_objects_a8447aef-d652-4ab9-85d1-1eb8e4876e12.jpg)

내비게이션 컨트롤러는 델리게이트 객체로 동작을 조정합니다. 델리게이트 객체는 뷰 컨트롤러의 푸시 또는 팝을 재정의할 수 있고, 커스텀 애니메이션을 사용한 전환을 제공하고, 네비게이션 인터페이스의 방향을 지정할 수 있습니다. 

- 제공하는 델리게이트 객체는 `UINavigationControllerDelegate` 프로토콜을 준수해야 합니다.
- 위 이미지는 내비게이션 컨트롤러와 컨트롤러가 관리하는 객체 간의 관계를 보여줍니다. 이러한 객체에 액세스하려면 내비게이션 컨트롤러의 지정된 속성을 사용합니다.

## Customize

---

네비게이션 컨트롤러는 네비게이션 바 및 툴바의 생성, 구성 및 표시를 관리합니다. 

- 네비게이션 바의 모양 관련 속성을 사용자 정의하는 것은 허용되지만 **프레임, 경계 또는 알파 값을 직접 변경해서는 안 됩니다.**
- `UINavigationBar`를 서브클래싱하는 경우 `init(navigationBarClass:toolbarClass:)`  메서드를 사용하여 네비게이션 컨트롤러를 초기화해야 합니다.
- 네비게이션 바를 숨기거나 표시하려면 `isNavigationBarHidden`  또는 `setNavigationBarHidden(_:animated:)` 메서드를 사용합니다.
- **네비게이션 컨트롤러는 네비게이션 스택의 뷰 컨트롤러와 연결된 `UINavigationItem` 클래스 인스턴스를 사용하여 네비게이션 바의 콘텐츠를 동적으로 빌드합니다.**
- 네비게이션 바의 모양을 커스텀하려면 `UIAppearance` API를 사용합니다. 각 항목에 대한 자세한 내용은 `UINavigationItem`을 참조하십시오.

### Navigation bar update

스택의 최상위 뷰 컨트롤러가 변경될 때마다 네비게이션 컨트롤러는 그에 따라 네비게이션 바를 업데이트합니다. 구체적으로,네비게이션 컨트롤러는 `left`, `middle`, `right`의 세 가지 내비게이션 바 위치 각각에 표시되는 bar button 항목을 업데이트합니다.

- bar button 항목은 `UIBarButtonItem` 클래스의 인스턴스입니다. 필요에 따라 커스텀하거나 기본 항목을 사용할 수도 있습니다.
- 네비게이션 바의 틴트컬러는 자체 속성에 의해 제어됩니다. 바에 있는 아이템의 틴트컬러를 변경하려면 `tintColor` 속성을 사용하고, 바 자체의 색상을 변경하려면 `barTintColor` 속성을 사용합니다.
- *네비게이션 바는 현재 표시된 뷰 컨트롤러에서 `tintColor`를 상속하지 않습니다.*

### Navigate presenting

내비게이션 컨트롤러는 다음 동작을 지원합니다:

- **화면 방향**
    - iPhone에서 네비게이션 컨트롤러는 portrait updside-down을 제외한 모든 방향을 지원합니다.
    - iPad에서는 네비게이션 컨트롤러가 모든 방향을 지원합니다.
    - 네비게이션 컨트롤러에 델리게이트 객체가 있는 경우 델리게이트는 `navigationControllerSupportedInterfaceOrientations(_:)` 메서드를 사용하여 지원되는 다른 방향 집합을 지정할 수 있습니다.
- **프레젠테이션 컨텍스트**
    - 네비게이션 컨트롤러는 모달로 표시되는 뷰 컨트롤러에 대한 프레젠테이션 컨텍스트를 정의합니다.
    - 모달 전환 스타일이 `UIModalPresentationStyle.currentContext` 또는 `UIModalPresentationStyle.overCurrentContext`인 경우 네비게이션 스택에 있는 뷰 컨트롤러의 모달 프레젠테이션은 전체 네비게이션을 덮습니다.

## 상태 저장

---

[Preserving your app’s UI across launches | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches)

네비게이션 컨트롤러의 `restorationIdentifier` 속성에 값을 할당하면 네비게이션 컨트롤러는 자신과 해당 네비게이션 스택의 서브 뷰 컨트롤러를 보존(restoration)하려고 시도합니다. 

- 네비게이션 컨트롤러는 스택의 맨 아래에서 시작하여 위로 이동하면서 유효한 복원 식별자 문자열이 있는 각 뷰 컨트롤러를 인코딩합니다.
- 다음 런치 사이클에 네비게이션 컨트롤러는 보존한 뷰 컨트롤러를 순서대로 네비게이션 스택에 복원합니다.
- 네비게이션 스택에 푸시하는 서브 뷰 컨트롤러는 동일한 `restorationIdentifier`를 사용할 수 있습니다. 네비게이션 컨트롤러는 각 보존 경로가 고유한지 확인하기 위해 추가적인 정보를 자동으로 저장합니다.

## 항상 헷갈리는 Controller vs Bar vs Item

---

[Navigation Controller, Navigation Item and Navigation Bar](https://stackoverflow.com/questions/45734107/navigation-controller-navigation-item-and-navigation-bar)

- `UINavigationController`:
탐색 컨트롤러는 계층적 탐색 기반 인터페이스에서 다른 뷰 컨트롤러 스택을 관리하는 컨테이너 뷰 컨트롤러입니다. 내비게이션 컨트롤러는 뷰 컨트롤러를 내비게이션 스택에 밀어 넣거나 빼서 앱의 여러 화면 간에 내비게이션을 조정합니다. 보기 컨트롤러를 스택에 밀어 넣으면 내비게이션 컨트롤러는 다음을 수행합니다.
- UINavigationBar:
탐색 모음은 화면 상단에 배치되는 가로형 반투명 막대로, 현재 보기의 제목을 표시하며 탐색 및 기타 작업을 위한 버튼도 포함할 수 있습니다. 앱의 여러 보기 사이를 탐색하기 위한 일관된 인터페이스를 제공하는 시각적 구성 요소입니다. 배경색, 제목 색상을 변경하고 사용자 지정 보기를 버튼으로 추가하는 등 탐색 모음의 모양과 동작을 사용자 지정할 수 있습니다.
- UINavigationItem:
탐색 항목은 특정 보기 컨트롤러의 탐색 모음에 표시되는 콘텐츠를 나타내는 개체입니다. 탐색 스택에 푸시되는 각 뷰 컨트롤러에는 고유한 UINavigationItem이 있습니다. 여기에는 제목, 왼쪽 및 오른쪽 막대 버튼 항목, 뒤로 버튼 항목과 같은 속성이 포함되어 있어 해당 뷰 컨트롤러가 표시될 때 탐색 막대의 모양과 동작을 사용자 지정할 수 있습니다. UINavigationItem은 시각적 요소 자체가 아니라 뷰 컨트롤러가 활성화되었을 때 탐색 모음에 표시되어야 하는 콘텐츠를 표현한다는 점에 유의하는 것이 중요합니다.

탐색 항목은 탐색 모음에 표시할 버튼(왼쪽 및 오른쪽) 및 보기(주로 제목 보기)를 관리합니다. 각 탐색 항목은 특정 뷰 컨트롤러를 가리킵니다.

탐색 모음은 탐색 항목의 컨테이너이며 주로 레이아웃, 전환 및 애니메이션을 처리합니다.

### 예시들

### title

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d02d8b7e-9495-4c2e-ae3e-62f92801d1ac/Untitled.png)

```swift
navigationController?.title = "Hello" // not work
navigationItem.title = "Hello" // work
```

- 사실 `navigationController?.title`는 `UIViewController`로서의 타이틀이라 보이는 것과 전혀 상관이 없는 프로퍼티다.

### bgColor

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/071c2962-cfde-4b76-8b28-403b380960a7/Untitled.png)

```swift
navigationItem.titleView?.backgroundColor = .white // not work
navigationController?.navigationBar.backgroundColor = .white // work
```

- `titleView?.backgroundColor`는 `navigationItem` 하위 `UIView`인 `titleView`의 컬러이므로 상관이 없다.
- `navigationBar`는 `navigationController`의 하위 `UIView`로 우리가 보고 있는 바로 그것이고 각 뷰 컨트롤러가 가진 `UINavigationItem`을 가지고 동적으로 요소가 변경된다.

## 정리

---

- `UINavigationController`는 계층적 콘텐츠 탐색을 위한 스택 기반 뷰 구조를 정의하는 컨테이너 뷰 컨트롤러입니다.
- `UINavigationController`는 네비게이션 인터페이스 아래에 하나 이상의 자식 뷰 컨트롤러를 관리합니다. 루트 뷰 컨트롤러는 초기화 시 지정되며 탐색 계층 구조의 첫 번째 뷰 컨트롤러입니다.
- `UINavigationController`는 내비게이션 스택이라는 정렬된 배열을 사용하여 자식 뷰 컨트롤러를 관리하며 네비게이션 바와 옵셔널 툴바를 관리합니다.
- `UINavigationDelegate` 객체는 뷰 컨트롤러의 푸시 또는 팝 동작을 재정의할 수 있습니다.

## 참고
---
[UINavigationController | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationcontroller)

[UINavigationItem | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationitem)

[UINavigationBar | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationbar)
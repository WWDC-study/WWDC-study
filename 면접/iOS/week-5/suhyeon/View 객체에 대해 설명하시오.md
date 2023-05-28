# UIView

```swift
@MainActor class UIView : UIResponder
```

- 화면에 컨텐츠를 나타내기 위해 사용
- 유저와 상호작용하는 주된 수단
- 모든 뷰에 공통된 기능을 포함
1. bounds 내부의 컨텐츠 레렌더링
2. 상속받아서 복잡한 컨텐츠를 그릴 수 있다. (draw 메서드 이용)

### View의 역할

- Drawing and Animation
    - Core Graphics나 UIKit을 활용하여 그린다.
- Layout and subview management
    - subview를 가질 수 있다.
    - view는 subview의 사이즈, 포지션을 조정할 수 있다.
    (auto layout을 사용하여 view의 사이즈 변화에 따라, subview의 사이즈, 포지션을 조정할 수 있다.)
    - auto layout을 사용하여 view의 사이즈 변화에 따라, subview의 사이즈, 포지션을 조정할 수 있다.
- Event handling
    - View가 UIResponder의 자식 클래스이기 때문에, 이벤트 처리 가능
    - gesture recognizer를 통해, 제스처 처리

### View를 중첩시켜서 컨텐츠를 그룹핑하여 관리 가능

- 이때, parentView, subView의 계층 구조가 만들어진다.
- clipsToBounds가 false이면, subview가 parentview의 bounds를 넘어갔을 때, 화면에 보인다.

### View의 크기, 상대적인 위치(geometry, 기하학적 속성)

[Frame ↔ Bounds](https://www.notion.so/Frame-Bounds-baa967274e4a4a75b532c78094189da9)

- `frame`, `bounds` 가 결정
- `frame` : 부모(parent) 뷰의 좌표계에서 위치, 사이즈
- `bounds` : 자신의 좌표계에서 위치, 사이즈

 

### View가 그려지는 방식

- 필요할 때 요청해야지 그려진다. 시스템에서 뷰에 컨텐츠를 그리라고 요청
    1. 뷰가 처음에 화면에 나타날 때
    2. 뷰의 전체 혹은 일부가 hidden → shown 될 때 
    3. 뷰의 setNeedsDisplay(:), setNeedsDispla()를 호출했을 때, 다음 드로잉 사이클에서 그려진다. (즉시X)
    
    <aside>
    💡 setNeedsDisplay(:) : 파라미터로 받은 CGRect() 영역 만큼만 다시 그린다.
    setNeedsDiaplay() : bounds 영역 전체를 다시 그린다.
    
    </aside>
    
    <aside>
    💡 **View의 frame, bounds를 변경하는 것만으로는 뷰가 다시 그려지지 않는다. 
    View의 content mode에 따라서 뷰의 콘텐츠가 조정(adjusted)된다. 
    다시 그리지 않기 때문에 성능이 향상된다.**
    
    </aside>
    
    <aside>
    💡 contentMode: bounds가 변경되었을 때, 뷰의 캐싱된 컨텐츠(bitmap)이 어떻게 처리될 것인지 나타냄. 
    뷰가 캐싱될 것인지 다시 그려질 것인지도 영향
    
    </aside>
    
- UIKit이나 Core Graphics를 이용하여 custom conent를 갖는 뷰는 시스템에서 draw() 메서드를 호출한다. 따라서 draw() 메서드에 custom으로 그릴 내용을 적어두어야 한다.

### 스레드

- View를 가지고 하는 작업은 메인스레드
- 단, View를 생성하는 작업은 메인스레드가 아니어도 된다.

### subclassing시 주의 사항

- 기본 UIView 클래스나 표준 시스템 뷰가 필요한 기능을 제공하지 않는 경우에만 서브클래싱하는 것이 좋습니다. **서브클래스를 사용하면 뷰를 구현하고 성능을 조정하는 데 더 많은 작업이 필요합니다.**
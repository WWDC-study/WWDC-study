# Bounds 와 Frame 의 차이점
_better on: https://enebin.notion.site/Bounds-Frame-f44eebda83174ab885c773c5153efc3e_
Bounds와 Frame은 모두 뷰의 위치와 크기를 지정하는 데 사용되는 속성입니다. 하지만 둘의 차이점이 있습니다.

## Bounds

---

`Bounds`는 뷰의 로컬 좌표계에서의 위치와 크기를 나타냅니다. 즉, 뷰의 원점(0,0)을 기준으로 하는 크기와 위치를 나타내는 것입니다. 

- **예를 들어**, bounds가 (0,0,100,100)으로 설정되어 있다면, 해당 뷰는 원점을 기준으로 x좌표 0부터 100까지, y좌표 0부터 100까지의 크기를 가지게 됩니다.

## Frame

---

`Frame`은 뷰의 슈퍼뷰(상위 뷰)의 좌표계에서의 위치와 크기를 나타냅니다. 즉, 뷰가 슈퍼뷰 내에서 어디에 위치하고 어느 크기를 가지는지를 나타내는 것입니다. 

- **예를 들어**, frame이 (50,50,100,100)으로 설정되어 있다면, 해당 뷰는 슈퍼뷰 내에서 x좌표 50부터 150까지, y좌표 50부터 150까지의 위치와 크기를 가지게 됩니다.

## 차이점

---

`Bounds`는 뷰 자체의 크기와 위치를 나타내며, `Frame`은 슈퍼뷰 내에서의 위치와 크기를 나타냅니다. 

**다음의 예시를 통해 살펴보겠습니다.**

[UIKitPlayground/UIKitPlayground at 86846abbdac113811ed28b3f5ce3bb074b5c5e7a · enebin/UIKitPlayground](https://github.com/enebin/UIKitPlayground/tree/86846abbdac113811ed28b3f5ce3bb074b5c5e7a/UIKitPlayground)

`UIScrollView`에서 이미지를 스크롤할 때, 이미지의 크기는 `ContentSize` 속성이 결정하고, 스크롤뷰의 영역은 `Frame` 속성이 결정합니다. 이렇게 함으로써, 이미지가 스크롤뷰의 영역을 벗어날 때 스크롤이 가능하도록 구현할 수 있습니다.

다음은 `UIScrollView`에서 이미지를 스크롤하는 예시입니다. ([코드](https://github.com/enebin/UIKitPlayground/tree/86846abbdac113811ed28b3f5ce3bb074b5c5e7a/UIKitPlayground))

![Simulator Screen Recording - iPhone 14 Pro - 2023-02-23 at 23.13.55.gif](Bounds%20%E1%84%8B%E1%85%AA%20Frame%20%E1%84%8B%E1%85%B4%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%8C%E1%85%A5%E1%86%B7%20f44eebda83174ab885c773c5153efc3e/Simulator_Screen_Recording_-_iPhone_14_Pro_-_2023-02-23_at_23.13.55.gif)

![Untitled](Bounds%20%E1%84%8B%E1%85%AA%20Frame%20%E1%84%8B%E1%85%B4%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%8C%E1%85%A5%E1%86%B7%20f44eebda83174ab885c773c5153efc3e/Untitled.png)

위에서 관찰한 결과 `Frame`은 변하지 않고 `Bound의 origin.x`만 변화하는 것을 알 수 있습니다.

- `Bounds`를 변경하는 것은 **로컬 좌표계**에서의 해당 위치(x, y)에서 뷰를 다시 그리는 것을 의미합니다.
- 이 결과와 별개로 Bounds는 뷰가 회전, 이동하더라도 그 크기와 위치가 변하지 않습니다.
    
    ![[iOS) Frame vs Bounds 제대로 이해하기 (3/3) (tistory.com)](https://babbab2.tistory.com/46)](Bounds%20%E1%84%8B%E1%85%AA%20Frame%20%E1%84%8B%E1%85%B4%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%8C%E1%85%A5%E1%86%B7%20f44eebda83174ab885c773c5153efc3e/Untitled%201.png)
    
    [iOS) Frame vs Bounds 제대로 이해하기 (3/3) (tistory.com)](https://babbab2.tistory.com/46)
    

## References

---

[frame | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622621-frame)

[bounds | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622580-bounds)

[iOS ) Frame과 Bounds의 차이 (1/2)](https://zeddios.tistory.com/203)

[iOS) Frame vs Bounds 제대로 이해하기 (3/3)](https://babbab2.tistory.com/46)

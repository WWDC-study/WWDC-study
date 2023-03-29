# AppDeleagte, SceneDelegate

- SceneDelgate 정의
    - iOS 13부터 멀티 윈도우를 지원하기 위해 등장
    - UIScene: 하나의 윈도우가 UIScene에 해당. 
    UISceneDelegate: 이 Scene의 라이프 사이클 관리 역할
- 왜 등장?
    - 멀티 윈도우를 지원하기 위해 등장
    - “각 윈도우는 동일한 앱 역할을 할 수 있어야 한다. “
    - → 여러개의 윈도우가 하나의 프로세스를 공유하는 방식
    → 각 윈도우는 독립된 UI 라이프 사이클을 가져야 함
- UIScene
    - 멀티윈도우에서 하나의 윈도우 나타냄
    - UIWindow, UIVIew, UIVIewController를 포함
    - 시스템에서 생성 및 제거


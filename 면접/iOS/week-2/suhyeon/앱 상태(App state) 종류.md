# 앱 상태(App state)  종류

- 5 가지
    1. Not Running
        - 앱이 실행되지 않은 상태 or 유저/시스템이 종료한 이후
    2. Inactive
        - 포그라운드 & 이벤트 수신 중 X
        - 언제 발생? 앱이 다른 상태로 전환 중
            - 전화 수신
            - 다른 앱 실행
    3. Active
        1. 포그라운드 & 이벤트 수신 중 O (유저가 터치중, 네트워크통신 중)
    4. Background
        1. 유저에게 보이지 않음 & 코드 실행 중 (like 음악 실행, 데이터 다운로드)
        2. 이벤트 수신 가능 (위치 업데이트, 푸시 알림 수신)
        3. 언제 발생? 홈화면 이동, 다른 앱으로 전환
    5. Suspended
        1. 백그라운드 & 코드 실행하지 않고 있음
        2. 메모리에 로딩된 상태 
        3. 시스템에서 리소스 확보 위해 앱을 suspended 상태로 만듬, 
        이때 앱은 메모리에 유지되어있어서 유저가 돌아왔을 때 빠르게 실행 재개할 수 있음
- 라이프 사이클 메서드별 상태 전환
    1. Not running → Inactive
    - 앱이 실행되었을 때, 이벤트 수신 중X
    - `application(didFinishLaunchingWithOptions`)
    - 유저와 상호작용하기에 앞서, 이를 준비하는 코드
    - ex) 리소스 초기화, 데이터 로딩, UI 설정
    1. Inactive → Active
    - `applicationDidBecomeActive()`
    - Inactive 상태에서 중지된 작업, 애니메이션을 다시 시작하는 코드
    1. Active → Inactive
    - `applicationWillResignActive()`
    - ex)앱이 유저의 입력을 받지 않는 동안에는 실행될 필요가 없는 작업(Task), 애니메이션을 중지
    1. Inactive → Background
    - `applicationDidEnterBackground`
    - 앱이 유저에게 보이지 않는 동안에도 지속되어야 하는 작업
    
    ```
    - 앱이 background/suspended 상태가 되기 전에 수행해야 하는 작업 
       - ex) 데이터 저장, 네트워크 통신
    ```
    
    - 데이터 저장 
    앱이 다시 실행될 때 유지되어야 하는 데이터 저장 
    ex) preference 데이터, 캐시 데이터
    - 네트워킹 작업 정지
    백그라운드상태에서 유지될 필요 없는 네트워크 작업 일시정지 
    (이를 계속 유지하면, 퍼포먼스 이슈가 발생할 수 있다.)
    - 위치 서비스 업데이트 
    위치 서비스를 업데이트해서, 앱이 백그라운드에 있는 동안에도 위치 정보를 업데이트하도록 함
    - notification 업데이트
    백그라운드에 있는 동안에도 push notification, local notification을 받는 코드를 여기에 위치
    - 영어 원본
        1. Saving data: Developers might use this method to save user data or app state, so that it can be restored when the app is launched again. This might include saving user preferences, cache data, or other relevant information.
        2. Pausing network operations: Developers might use this method to pause ongoing network operations, so that they can resume later without affecting the app's performance. This might include pausing downloads or uploads, or canceling network requests that are no longer needed.
        3. Updating location services: If the app uses location services, developers might use this method to update the app's location information, so that it can continue to receive location updates even when it's in the background.
        4. Updating notifications: Developers might use this method to update the app's notifications, so that it can continue to receive push notifications or local notifications even when it's in the background.
    1. Background → Inactive
    - `applicationWillEnterForeground`
    - 유저의 입력을 받기 전에 수행해야 하는 작업들
    - ex) 업데이트된 데이터를 UI에 반영
    1. system에 의해서 app이 종료될 때
    - `applicationWillTerminate()`
    - 앱이 종료되기 전에 수행해야 하는 작업
    - ex) 앱의 데이터 저장, 실행중이던 작업 종료
- 각 상태별로 개발자가 넣는 코드
    1. NotRunning
        - 앱이 실행중이지 않기 때문에, 넣을 수 있는 코드 없다.
        - launch option을 지정해서, 앱이 실행중이지 않을 때 push notification을 처리하는 방식 등을 지정할 수 있다.
    2. Inactive
        - 유저와 상호작용하기에 앞서, 이를 준비하는 코드
        - ex) 리소스 초기화, 데이터 로딩, UI 설정
        - ex)앱이 유저의 입력을 받지 않는 동안에는 실행될 필요가 없는 작업(Task), 애니메이션을 중지
    3. Active 
        1. 유저의 이벤트를 받아서 처리하는 코드, UI 업데이트 코드
        2. Inactive 상태에서 중지된 작업, 애니메이션을 다시 시작하는 코드
    4. Background
        1. 앱이 유저에게 보이지 않는 동안에도 지속되어야 하는 작업
        2. 서버로부터 데이터 다운/업로드, push notificiation 처리, 위치 정보 업데이트 
    5. Suspended 
        1. 앱이 아무런 코드도 실행하지 않는 상태 
        따라서 개발자가 너흘 수 있는 코드 없음
        2. `applicationWillTerminate()` 메서드를 통해, 시스템이 앱을 종료시킬 때, 앱이 수행할 작업을 지정할 수 있다.
        3. 데이터 저장, clean up 등등
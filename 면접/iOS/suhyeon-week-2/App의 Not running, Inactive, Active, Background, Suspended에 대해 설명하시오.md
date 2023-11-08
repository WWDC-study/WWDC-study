## 5가지 상태
1. Not Running
    - 앱이 실행되지 않은 상태 or 종료된 이후 
2. Inactive
    - 포그라운드 & 이벤트 수신 중 X
    - 언제 발생?
      - 전화 수신
      - 다른 앱으로 전환
3. Active
   - 포그라운드 & 이벤트 수신 중 O 
   - 이벤트? 유저가 터치, 네트워크 통신
4. Background
   - 유저에게 보이지 않는 상태 & 코드 실행 중 (ex. 음악 재생, 데이터 다운로드/업로드)
   - 이벤트 수신 가능 (위치 정보 업데이트, push notification)
   
5. Suspended
    - 유저에게 보이지 않는 상태 & 코드를 실행 중 X
    - 메모리에 로딩된 상태
    - 시스템에서 리소스 확보를 위해, 메모리에서 제거할 수 있다.

<br/>

## 상태 변화와 AppDelegate 메서드
1. Not Running -> Inactive
   - application(didFinishLaunchingWithOptions)
   - 앱이 실행됨 & 이벤트 수신 중 X
   - 유저와 상호작용하기 위한 준비 작업
   - ex) 리소스 초기화, 데이터 로딩, UI 설정
2. Inactive -> Active
   -  applicationDidBecomeActive()
   -  Inactive 상태에서 중지된 작업, 애니메이션을 다시 실행
3. Active -> Inactive
    - applicationWillResignActive()
    - 앱이 유저에게 입력을 받지 않는 동안, 실행될 필요가 없는 작업, 애니메이션을 중지
4. Inactive -> Background
   - applicationDidEnterBackground()
   - 앱이 background/suspended 상태가 되기 전에 수행해야 하는 작업 
   - ex) 백그라운드에서 지속할 필요가 없는 네트워킹 작업 정지 등
5. Background -> Inactive
   - applicationWillEnterForeground()
   - 유저의 입력을 받기 전에 수행해야 하는 작업들
   - ex) 업데이트된 데이터를 UI에 반영
6. system에 의해서 앱이 종료될 때
    - applicationWillTerminate()
    - 앱이 종료되기 전에 수행해야 하는 작업
    - ex) 데이터 저장, 진행중이던 작업 종료
   
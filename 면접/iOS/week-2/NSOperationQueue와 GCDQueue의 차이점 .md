# NSOperationQueue와 GCDQueue의 차이점

- 공통점
    - 멀티 스레딩을 위해 제공
- 차이점
    
    ### NSOperationQueue
    
    ---
    
    - Objective-C 기반의 **high level api**
        - 내부에서 GCD 사용
    - NSOperation 객체가 쌓임
        - NSOperaion 객체의 우선순위와 준비상태에 따라 실행
    - NSOperaionQueue에 쌓인 작업은 실행완료될 때까지 큐에 남아있음. 큐에서 직접 제거 불가
    - 모든 작업이 끝나지 않은 상태에서 NSOperatinoQueue 중지 → 메모리 릭 발생가능
    - GCD보다 약간의 오버헤드 발생
    - but GCD에서 할 수 없는 것(재개, 취소, 중지, KVO 사용가능)을 할 수 있음 GCD를 사용하면 직접 처리해야하는 일을 대신 해줌
    - 한 작업이 수행완료 후, 다른 작업이 수행되도록 하는 의존성을 만들 수 있다.
    - Thread Safety: 여러 스레드에서 안전하게 참조 가능 (동기화, 락 필요 없음)
        - 각 작업을 서로 다른 스레드에서 실행
    
    ### GCDQueue
    
    ---
    
    - C 기반의 low level api
    - ~~NSOperaionQueue보다 동시성 작업을 간단하게 관리 가능~~
    - GCD의 Disaptch Queue에 작업을 쌓아놓으면, GCD에서 queue에 있는 작업을 분배
- GCD Dispatch Queue 종류
    1. main queue
        - serial
        - 메인 스레드 실행
    2. global queue
        - concurrent
        - 백그라운드 스레드 실행
        - qos 지정 가능
    3. custom queue
        - 직접 DispatchQueue 생성자 호출해서
        - 디폴트 serial, concurrent 가능
        - 백그라운드 실행
        - qos 설정 가능, qos 설정 안하면 시스템에서 알아서 추론
        

[[iOS] 차근차근 시작하는 GCD — 5](https://sujinnaljin.medium.com/ios-차근차근-시작하는-gcd-5-c8e6eee3327b)
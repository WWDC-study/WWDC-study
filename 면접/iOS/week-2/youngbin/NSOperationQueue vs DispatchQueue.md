# NSOperationQueue vs DispatchQueue
better on : [NSOperationQueue vs DispatchQueue ](https://www.notion.so/NSOperationQueue-vs-DispatchQueue-a7711a36b76d4ed38c2f35488ec80d02)

## OperationQueue

---

`OperationQueue`는 우선순위 및 준비 상태에 따라 큐에 대기 중인 `operation` 개체를 호출합니다. 

- 큐에 `operation`을 추가한 후에는 완료할 때까지 큐에 남아 있습니다. `operation`을 추가한 후에는 큐에서 제거할 수 없습니다.
- `OperationQueue`는 `operation`이 완료될 때까지 유지하며, 큐 자체는 모든 `operation`이 완료될 때까지 유지됩니다.
- 완료되지 않은 작업이 있는 작업 큐를 일시 중단하면 메모리 누수가 발생할 수 있습니다.

작업 큐 사용에 대한 자세한 내용은 [동시성 프로그래밍 가이드](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)를 참조하십시오.

### 실행 순서

`OperationQueue`는 `isReady`, `queuePriority` 및 Interoperation dependencies에 따라 작업을 구성하고 호출합니다. 

- 큐에 대기 중인 모든 작업의 `queuePriority`가 동일하고 `isReady` 속성이 `true`를 반환하는 경우 큐는 추가한 순서대로 작업을 호출합니다.
- 그렇지 않으면 `OperationQueue`는 항상 다른 준비된 작업 중 우선 순위가 가장 높은 작업을 호출합니다.
- `isReady` 상태가 변경되면 결과 실행 순서가 변경될 수 있습니다. 그러므로 작업의 특정 실행 순서 보장을 큐에 의존해서는 안 됩니다.
- Interoperation dependencies은 `operation`이 서로 다른 큐에 있는 경우에도 `operation`에 대한 절대적인 실행 순서를 정의합니다. 모든 선행 `operation`이 실행을 완료할 때까지 `isReady` 상태를 `true`로 변경하지 않습니다.

우선순위 수준 및 종속성을 설정하는 방법에 대한 자세한 내용은 [종속성 관리하기](https://developer.apple.com/documentation/foundation/operation#1661485)를 참조하세요.

### 작업 취소

작업이 완료되었다는 것은 반드시 해당 작업이 정상적으로 수행되었음을 의미하지 않습니다. 작업은 취소로 인해 완료 상태로 전환할 수도 있습니다. 

- `operation`을 취소하면 가능한 빨리 작업을 중지해야 함을 해당 알립니다. 해당 `operation`는 대기열에 남습니다.
- 현재 실행 중인 `operation`의 경우, 취소 상태를 확인하면 수행 중인 작업을 중지한 후 완료로 표시해야 함을 의미합니다.
- 큐에 대기 중이지만 아직 실행 중이지 않은 `operation`의 경우, 큐는 취소 이벤트를 완료로 처리할 수 있도록 작업 개체의 `start()` 메서드를 계속 호출해야 합니다.

**참고**

작업을 취소하면 해당 `operation`에 존재하는 모든 종속성이 무시됩니다.

- 큐는 가능한 빨리 `operation`의 `start()` 메서드를 호출하고 `start()` 메서드는 작업을 완료 상태로 이동해 큐에서 제거합니다.

### 키-값 관찰을 사용하여 작업 관찰하기

`OperationQueue` 클래스는 KVC 및 KVO를 준수합니다. 이러한 속성을 observe 해 애플리케이션의 다른 부분을 제어할 수 있습니다. 

attribute를 observe하려면 다음 key path를 사용합니다:

- `operations` - 읽기 전용
- `operationCount` - 읽기 전용
- `maxConcurrentOperationCount` - 읽기 및 쓰기 가능
- `isSuspended` - 읽기 및 쓰기 가능
- `name` - 읽기 및 쓰기 가능

### Thread safety

- Lock을 만들지 않고도 여러 스레드에서 단일 `OperationQueue`에 안전하게 접근을 동기화할 수 있습니다.
- 작업 큐는 `Dispatch` 프레임워크를 사용하여 작업을 실행합니다. 따라서 큐는 작업이 동기식, 비동기식에 관계없이 항상 별도의 스레드에서 작업을 호출합니다.

## DispatchQueue와의 차이

---

1. **종속성 관리**
    
    `OperationQueue`는 작업간 종속성을 지원하지만, `DispatchQueue`는 이를 지원하지 않습니다. 
    
    - `OperationQueue`을 사용하면 서로 종속 관계를 설정해 작업을 추가할 수 있으나 `DispatchQueue`는 serial 또는 concurrent 방식으로 작업을 실행하는 것만 지원합니다.
2. **작업 취소**
    
    `OperationQueue`는 작업 취소를 지원하지만, `DispatchQueue`는 그렇지 않습니다. 
    
    - `OperationQueue`을 사용하면 특정 작업을 취소하거나 대기열에 있는 모든 작업을 취소할 수 있습니다. 하지만 `DispatchQueue`는 작업 실행만 지원하며 취소 메커니즘은 제공하지 않습니다.(취소하려면 `DispatchWorkItem`을 이용)
3. **큐 중지**
    
    `OperationQueue`는 큐를 중지시킬 수 있지만 `DispatchQueue`는 그렇지 않습니다. 
    
    - `OperationQueue`의 `isSuspended` 프로퍼티를 사용하면 큐에 있는 작업의 실행을 일시적으로 중지할 수 있습니다. 하지만 `DispatchQueue`는 중지 메커니즘을 제공하지 않습니다.
4. **작업 모니터링**
    
    `OperationQueue`는 큐에 있는 개별 작업의 진행 상황을 모니터링할 수 있는 방법을 제공하지만, `DispatchQueue`는 그렇지 않습니다. 
    
    - `OperationQueue`을 사용하면 KVO(키-값 관찰)를 사용하여 작업의 진행 상황을 추적하고 완료 시 알림을 받을 수 있습니다. `DispatchQueue`는 작업의 진행 상황을 모니터링하기 위한 기본 메커니즘을 제공하지 않습니다.
    

## 정리

---

- `OperationQueue`는 `DispatchQueue`를 래핑해 기능을 추가한 API입니다.
- `OperationQueue`는 작업 간 의존성 설정, 작업 취소, 큐 중지, 작업 모니터링이 가능합니다.

## 참고

---

[OperationQueue | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/operationqueue)

[Operation Queue vs Dispatch Queue for iOS Application](https://stackoverflow.com/questions/7078658/operation-queue-vs-dispatch-queue-for-ios-application)
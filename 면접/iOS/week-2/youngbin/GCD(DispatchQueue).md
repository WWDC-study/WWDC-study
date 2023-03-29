# GCD(DispatchQueue)
better on : [GCD API 동작 방식과 필요성](https://www.notion.so/GCD-API-bfa889d1ae3d4ea28c00fdb1cd62ff13)

---

`DispatchQueue`는 앱의 main 스레드 또는 `background` 스레드에서 작업의 순차적 또는 동시 실행을 관리하는 개체입니다.

### Queue에 관하여

`DispatchQueue`는 애플리케이션이 블록 객체 형태로 작업을 제출하는 FIFO 큐입니다. 

- `DispatchQueue`는 작업을 순차적으로 또는 동시에 실행합니다.
- `DispatchQueue`에 제출한 작업은 시스템에서 관리하는 스레드 풀에서 실행됩니다.
- 앱의 메인 스레드를 나타내는 디스패치 큐(`main`)를 제외하고 시스템은 작업을 실행하는 스레드를 보장하지 않습니다.
- 작업 항목을 동기식 또는 비동기식으로 예약할 수 있습니다.
    - 작업 항목을 동기식으로 예약하면 해당 항목이 실행을 완료할 때까지 코드가 대기합니다. (`sync`)
    - 작업 항목을 비동기적으로 예약하면 작업 항목이 다른 곳에서 실행되는 동안 코드가 계속 실행됩니다. (`async`)

<aside>
⚠️ `main` 큐에서 작업 항목을 동기적으로 실행하려고 하면 교착 상태가 발생합니다.

</aside>

### ⚠️ 과도한 스레드 생성 방지하기

1. **concurrent한 작업을 설계할 때는 현재 실행 스레드를 블록하는 메서드를 호출해서는 안됩니다.**
    - concurrent 디스패치 큐에 의해 예약된 작업이 스레드를 블록하면 시스템은 대기 중인 다른 동시 작업을 실행하기 위해 추가적인 스레드를 생성합니다. 이 경우 가용 스레드가 부족해질 수 있습니다.
2. **private concurrent 디스패치 큐를 너무 많이 생성해서는 안됩니다.**
    - 디스패치 큐는 스레드 리소스를 점유합니다. 따라서 concurrent 디스패치 큐를 추가로 만들면 과도한 스레드 문제가 악화됩니다.
    - private concurrent 디스패치 큐를 만드는 대신 global 디스패치 큐를 사용하세요.
    - Serial한 작업의 경 우도 작업을 처리하는 큐의 타겟을 global 큐로 설정하십시오. ([setTarget(queue:)](https://developer.apple.com/documentation/dispatch/dispatchobject/1452989-settarget)).
        
        ```swift
        // serial A -> targets concurrent B
        
        // concurrent queue A
        let a = DispatchQueue(label: "A", attributes: .concurrent)
        
        // serial queue B targeting concurrent queue A
        let b = DispatchQueue(label: "B", target: a)
        
        // next two blocks will be executed concurrently on A
        a.async {
            print("1")
        }
        a.async {
            print("2")
        }
        
        // next two blocks will be executed serially on A
        b.async {
            print("3")
        }
        b.async {
            print("4")
        }
        ```
        

## GCD의 사용처

---

그랜드 센트럴 디스패치(GCD)는 개발자가 비동기 및 동시 작업을 수행할 수 있도록 Apple이 Swift에서 제공하는 로우레벨 API이며 개발자가 OS의 쓰레드를 확장 가능하고 효율적인 방식으로 관리할 수 있게 돕습니다. 

다음은 Swift에서 GCD가 쓰이는 주요한 이유입니다:

1. **앱 응답성 향상**
    
    개발자는 GCD를 사용하여 백그라운드에서 작업을 수행함으로써 메인 스레드를 UI 처리에 사용할 수 있습니다. 이렇게 하면 앱의 응답성이 향상되고 사용자 경험이 개선됩니다.
    
2. **시스템 리소스의 효율적인 사용**
    
    GCD는 스레드 풀을 사용하여 스레드를 관리하므로 시스템에 과부하가 걸리지 않고 효율적인 작업 실행이 가능합니다. 또한 GCD는 중요도에 따라 작업의 우선순위를 지정할 수 있어 중요한 작업을 빠르게 완료하는 데 도움이 됩니다.
    
3. **비동기 프로그래밍 간소화**
    
    GCD는 작업을 비동기적으로 수행하기 위한 간단하고 사용하기 쉬운 API를 제공합니다. 따라서 개발자는 스레드 및 Lock 관리에 대해 걱정할 필요 없이 비동기 코드를 더 쉽게 작성할 수 있습니다.
    

## 정리

---

GCD는 OS의 쓰레드를 확장 가능하고 효율적인 방식으로 관리할 수 있게 돕는 저수준 API 입니다.

- DispatchQueue는 비동기(`async`)와 동기(`sync`) 두 가지 방법으로 사용할 수 있습니다.
- 백그라운드와 메인 큐의 구분을 통해 UI 응답성을 향상할 수 있습니다.
- 작업의 우선순위를 지정해 시스템 리소스의 효율적인 사용이 가능합니다.
- 스레드와 Lock을 따로 관리하지 않아 비동기 프로그래밍을 간소화합니다.

## 참고

---

[DispatchQueue | Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqueue)

[Dispatch Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html)

[target parameter in DispatchQueue](https://stackoverflow.com/questions/40634586/target-parameter-in-dispatchqueue)
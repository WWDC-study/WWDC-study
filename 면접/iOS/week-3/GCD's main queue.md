# Main thread

better on: [Main thread](https://www.notion.so/Main-thread-9ffe15c4c54d493086827645cde34d9b) 

## GCD

---

[GCD API 동작 방식과 필요성](https://www.notion.so/GCD-API-bfa889d1ae3d4ea28c00fdb1cd62ff13) 

## Main queue

---

GCD의 메인 큐는 단 하나만 존재하는 프로세스의 `main` 스레드와 연결된 디스패치 큐입니다.

- 시스템은 자동으로 `main` 큐를 생성하고 이를 애플리케이션의 `main` 스레드와 연결합니다.
- 앱은 `main` 큐에 제출한 블록을 실행하기 위해 다음 세 가지 접근 방식 중 하나를 사용합니다:
    - `[dispatchMain()](https://developer.apple.com/documentation/dispatch/1452860-dispatchmain)` 호출
    - `UIApplicationMain`(iOS) *또는* `NSApplicationMain`(macOS)을 호출해 앱을 시작
    - 메인 스레드에서 `CFRunLoop` 사용
    
    *→ 위 방식들에 대한 추가 정보가 필요하나 구할 수가 없음..!*
    

***TIP***

글로벌 큐와 마찬가지로, `DispatchQueue.main` 프로퍼티에서 `suspend()`, `resume()`, `dispatch_set_context(*:*:)` 등을 호출해도 아무런 영향을 미치지 않습니다.

### 용도

1. **UI 업데이트**
    
    화면의 텍스트를 변경하는 등 사용자 인터페이스를 업데이트하는 모든 코드는 `main` 큐에서 실행되는 것이 원칙입니다.
    
2. **우선순위가 높은 이벤트 실행**
    
    `main` 큐에 등록되어 있는 작업은 `userInteractive`로 최상위 우선순위를 갖습니다. 
    

### DispatchQueue.main.sync

`main` 큐에서 `sync`를 실행하면 데드락이 발생한다고 알려져 있습니다. 

**예를들어 다음 코드는 데드락이 발생합니다**(`XCTest` 환경) **:** 

```swift
XCTAssertTrue(Thread.current.isMainThread) // Am I on main thread?
        
DispatchQueue.main.sync { // Then crashes
    print("Can execute?")
}

// Above's same with
let queue = DispatchQueue(label: "serial_queue")
queue.sync { // `queue.async` also not working
    queue.sync {
        print("SS")
        exp.fulfill()
    }
}
```

이유는 다음과 같습니다:

- 직렬(serial) 큐는 한 번에 한 개의 작업만 시행할 수 있습니다.
- `sync`는 클로저가 실행되는 동안 큐(스레드)의 제어권을 가져가는 동작입니다. 제어권을 가진 클로저는 반환하기 전까지 다음 클로저(작업)의 실행을 막습니다.
- `sync`로 큐의 제어권을 가져오는 동작을 동일한 큐에서 실행할 때 직렬로 디스패치하는 큐의 경우 데드락이 발생할 수 있습니다. sync를 디스패치 하는 작업은 `sync`의 반환을, `sync`는 디스패치 작업의 반환을 상호 대기하기 때문입니다.
- `main` 큐는 직렬 큐입니다. 따라서 `DispatchQueue.main.sync`의 동작을 main 큐 맥락에서 실행한다면 데드락이 발생합니다.

**따라서 다음 코드는 데드락이 발생하지 않습니다:**

```swift
XCTAssertTrue(Thread.current.isMainThread) // Am I on main thread?

DispatchQueue.global().async { // It works
    DispatchQueue.main.sync {
        print("Can execute?")
    }
}
```

- `DispatchQueue.main.sync`를 다른 큐에서 디스패치 했기 때문에 데드락이 발생하지 않습니다.

## 정리

---

- GCD의 `main` 큐는 단 하나만 존재하는 프로세스의 `main` 스레드와 연결된 디스패치 큐입니다.
- UI 업데이트 혹은 중요한 이벤트의 실행을 위해 사용합니다.
- `main` 스레드에서 `DispatchQueue.main.sync`을 실행하면 데드락이 발생하므로 유의해야 합니다.

## 참고
[main | Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqueue/1781006-main)
# NSCache

better on : [NSCache](https://www.notion.so/NSCache-8b0aff7577904390bbaf9797c94b8c91) 

키-값 쌍을 임시로 저장하는 데 사용하는 변경 가능한(mutable) 컬렉션입니다. 리소스가 부족할 때 비워질 수 있습니다.

## NSCache

---

### 용도

일반적으로 생성 비용이 많이 드는 일시적인 데이터가 포함된 객체를 임시로 메모리에 저장하는 데 `NSCache` 객체를 사용합니다. 

- 객체를 재사용하면 값을 다시 계산할 필요가 없으므로 성능상의 이점을 얻을 수 있습니다.
- 객체는 애플리케이션에 중요하지 않다는 판단 아래 메모리가 부족한 경우 비워질 수 있습니다. 이 때 해당 값을 사용하기 위해서는 다시 적재해야 합니다.

### 삭제 정책 조정

객체를 사용하지 않을 때(대상 객체가 해제되었을 때) 버리는 기능을 가지고 있습니다. 이 경  `NSDiscardableContent` 프로토콜을 채택합니다. 

- 기본적으로 콘텐츠가 해제되면 캐시에 있는 `NSDiscardableContent` 오브젝트 역시 자동으로 제거됩니다. 이는 변경할 수 있습니다.
- `NSDiscardableContent` 오브젝트를 캐시에 넣으면 캐시는 제거 시 해당 오브젝트에 대해 `discardContentIfPossible()`을 호출합니다.

### vs Others(like Dictionary)

Swift에는 여러가지 뮤터블 자료구조가 있습니다. 하지만 `NSCache`는 다른 뮤터블 컬렉션과 몇 가지 차이를 가지고 있습니다.

1. `NSCache` 클래스에는 캐시가 시스템 메모리를 너무 많이 사용하지 않도록 보장하는 다양한 자동 삭제 정책이 내재되어 있습니다. 
    - 다른 애플리케이션에서 메모리가 필요한 경우 정책에 따라 캐시에서 일부 항목을 제거하여 메모리 공간을 절약합니다.
2. 캐시에 `lock`을 사용하지 않고 다른 스레드에서 캐시에 있는 항목을 안전하게 추가, 제거 및 쿼리할 수 있습니다.
3. `NSMutableDictionary` 객체와 달리 캐시는 캐시에 입력된 키 객체를 복사하지 않습니다.

## 예시

---

과연 NSCache는 멀티 쓰레드 데이터레이스에 면역인지 실험을 통해 확인해봅니다.

**다음과 같이 XCTest 환경에서 NSCache와 Dictionary를 선언합니다**

```swift
import XCTest
import Foundation

class CacheVsDictionaryConcurrencyTests: XCTestCase {
    var nsCache: NSCache<NSNumber, NSString>!
    var dictionary: [Int: String]!
    let itemCount = 10_000

		override func setUpWithError() throws {
        nsCache = NSCache<NSNumber, NSString>()
        dictionary = [Int: String]()
    }

    override func tearDownWithError() throws {
        nsCache = nil
        dictionary = nil
    }
}
```

**다음은 멀티쓰레드 환경을 가정하고 concurrent하게 자료구조에 접근하는 코드입니다.** 

먼저 `Dictionary`의 케이스부터 확인합니다. 

```swift

func testDictionaryPerformance() {
    measure {
        let group = DispatchGroup()
        DispatchQueue.concurrentPerform(iterations: concurrentTasks) { _ in
            for i in 0..<self.itemCount {
                group.enter()
                self.dictionary[i] = "Item \(i)"
                group.leave()
            }
        }
        group.wait()
    }
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d5f42b7-45bb-4ce8-816c-580efd70fdd2/Untitled.png)

- 이 경우 메모리 동시접근으로 인한 시스템 크래시가 발생합니다.

다음은 동일하게 concurrent하게 접근하지만 `NSCache`를 사용한 코드입니다.

```swift

func testNSCachePerformance() {
    measure {
        let group = DispatchGroup()
        DispatchQueue.concurrentPerform(iterations: concurrentTasks) { _ in
            for i in 0..<self.itemCount {
                group.enter()
                self.nsCache.setObject("Item \(i)" as NSString, forKey: NSNumber(value: i))
                group.leave()
            }
            
        }
        group.wait()
    }
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1a915214-ed73-4591-96ae-0568fa8561d0/Untitled.png)

- 이 경우 정상적으로 동작합니다.

**결론적으로** `NSCache`는 **멀티 쓰레드 환경에서 안전한 동작을 보장함을 확인했습니다.**

- 다만, 퍼포먼스 면에서 손해가 있으므로 싱글스레드 상황에서는 딕셔너리를 이용하는 것을 고려해볼 수 있습니다.

## 정리

---

- `NSCache`는 키-값 쌍을 임시로 저장하는 데 사용하는 변경 가능한(mutable) 컬렉션입니다. 리소스가 부족할 때 시스템에 의해 일부 비워질 수 있습니다.
- `NSCache`는 다른 뮤터블 컬렉션과 달리 메모리를 너무 많이 사용하지 않도록 보장하는 다양한 자동 삭제 정책이 내재되어 있으며 멀티 스레드 환경에서 항목을 안전하게 추가, 제거 및 쿼리할 수 있고 `NSMutableDictionary` 객체와 달리 캐시에 입력된 키 객체를 복사하지 않습니다.

## 참고
[NSCache | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/nscache)
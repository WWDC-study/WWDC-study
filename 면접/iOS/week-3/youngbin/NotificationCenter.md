# NotificationCenter
better on: [NotificationCenter](https://www.notion.so/enebin/NotificationCenter-2fb9cc7e32eb4f63b7024d0623a05a0b?pvs=4)

NotificationCenter는 등록된 옵저버에게 정보를 브로드캐스트할 수 있는 Notification 발송 메커니즘입니다. 이를 통해 앱의 여러 부분이 서로를 직접 참조하지 않고도 서로 통신할 수 있습니다.

## NotificationCenter

---

`NotificationCenter` 클래스는 `Foundation` 프레임워크의 일부이며 앱의 여러 부분 간에 알림 또는 메시지를 보내고 받는 데 사용됩니다. 

- 이벤트가 발생하면 앱은 `NotificationCenter` 클래스를 사용하여 알림(`NSNotification`)을 보낼 수 있으며, 해당 이벤트 유형을 구독해 알림을 수신할 수 있습니다.
- 알림은 이벤트의 정보를 담고 있으며 구독한 모든 옵저버에게 브로드캐스트됩니다.

### API

`NotificationCenter` 클래스는 알림을 주고 받기 위한 메서드를 제공합니다:

- `post(name:object:userInfo:)`
    - 이 메서드는 지정된 이름, 객체 및 사용자 정보가 포함된 알림을 게시하는 데 사용됩니다.
    - `name`은 이벤트 유형을 식별하는 문자열입니다.
    - `object`는 이벤트에 대한 추가 정보를 제공할 수 있는 옵셔널 객체입니다.
    - `userInfo`는 이벤트에 대한 추가 정보를 제공할 수 있는 옵셔널 딕셔너리입니다.
- `addObserver(_:selector:name:object:)`:
    - 이 메서드는 특정 이름, 개체, 선택자에 대한 알림을 수신할 개체를 등록하는 데 사용됩니다.
    - 지정된 이름과 오브젝트에 대한 알림이 게시되면 등록된 오브젝트에서 `selector`가 호출됩니다.

### 참고

- 실행 중인 각 앱에는 `default` `NotificationCenter`가 있으며, 특정 컨텍스트에 맞게 커스텀하기 위해새 `NotificationCenter`를 만들 수 있습니다.
- `NotificationCenter`는 동일한 프로그램 내에서만 알림을 전달할 수 있으며, 다른 프로세스에 알림을 송 순신 하려면 `DistributedNotificationCenter`를 사용하세요.

### 단점

- **디버깅이 힘들고 스파게티 코드를 유발**
    - 알림을 송신하는 개체와 수신하는 개체가 분리되어 있기 때문에 앱에서 정보의 흐름을 추적하기가 어려워 코드를 디버깅하기가 더 어려워지고 구조가 복잡해질 수 있습니다.
- **Error-prone**
    - 알림 송수신의 식별자가 문자열에 기반하기 때문에 오타 혹은 이름 충돌 등으로 인한 오류가 발생하기 쉽습니다.
- **1 on 1 송수신 보장**
    - 특성 상 한 개의 알림을 여러 옵저버가 구독할 수 있어 때에 따라 혼선을 야기할 수 있습니다.

## 예시

---

**다음과 같이 커스텀 `NotificationCenter`를 만들 수 있습니다:**

```swift
let center = NotificationCenter()
let notificatioName = Notification.Name("not")
```

- `default`를 사용하면 객체를 생성하지 않아도 됩니다.

**다음과 같이  NotificationCenter를 사용할 수 있습니다:**

```swift
center.addObserver(self, selector: #selector(someMethod), name: notificatioName, object: nil)
center.post(name: notificatioName, object: nil)

center.removeObserver(self)
center.post(name: notificatioName, object: nil) // Nobody observes
```

- `addObserver`를 이용해 옵저버를 등록하고 `removeObserver`를 이용해 등록한 옵저버를 해제할 수 있습니다.

## 정리

---

- `NotificationCenter`를 이용하면 클래스를 직접 참조하지 않고 `NotificationCenter`를 매개로 이벤트를 송수신할 수 있습니다.
- 하지만 스파게티 코드, 이름 오류로 인한 에러, 1대 1 연결이 불가능한 구조 등이 단점으로 꼽힙니다.

## 참고
# Delegate와 retain 동작

## Delegate pattern

---

참고: [Delegate 패턴](https://www.notion.so/Delegate-60f3ec5763524c51b96b26bba1f8e673) 

## Delegate의 retain

---

결론부터 말하면 `delegate` 인스턴스가 참조 타입일 경우 retain cycle을 발생시킬 수 있습니다.

![Untitled](https://velog.velcdn.com/images/enebin777/post/3a6ec324-8df3-4099-b94b-3bd5bde5ae29/image.png)

- `delegate`의 경우 `delegator`가 `delegate`의 참조를 저장하고 `delegate` 또한 `delegator` 인스턴스의 참조를 저장합니다. 이로 인해 retain cycle이 발생할 수 있습니다.
- 이를 해결하기 위해서는 `delegate`를 값 타입으로 선언하거나 `weak` 으로 선언해 약한 참조로 선언해 해결할 수 있습니다.
- 위 예시처럼 UIKit의 `delegate`는 내부에서 `weak`으로 선언되어 있습니다.

## 정리

---

- `delegator`와 `delegate` 모두 참조 타입일 경우 상호 참조로 인해 retain cycle이 발생할 수 있습니다.
- 이 문제는 `weak` 선언 혹은 값 타입 사용으로 해결할 수 있습니다.

## 참고
---
[UITableViewDelegate | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableviewdelegate)
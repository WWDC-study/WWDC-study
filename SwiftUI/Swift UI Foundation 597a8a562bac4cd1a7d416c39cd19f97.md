# Swift UI Foundation

### Reference

[https://developer.apple.com/videos/play/wwdc2020/10040/](https://developer.apple.com/videos/play/wwdc2020/10040/)

### Source of truth

![https://velog.velcdn.com/images/enebin777/post/82c39643-6274-4368-91f7-e4114f7f6258/image.png](https://velog.velcdn.com/images/enebin777/post/82c39643-6274-4368-91f7-e4114f7f6258/image.png)

Source of truth는 국어로 번역하면

```
단일 진실 공급원
```

이라고 한다. 쉽게 풀어 말하면 앱의 상태를 관리, 수정하는 데이터의 원천을 말한다.

### View에서 struct의 역할

![https://velog.velcdn.com/images/enebin777/post/76a6222d-c27c-409d-bcdd-3b006c99f9e9/image.png](https://velog.velcdn.com/images/enebin777/post/76a6222d-c27c-409d-bcdd-3b006c99f9e9/image.png)

- SwiftUI는 View 프로토콜을 따르는 struct를 이용해 UI를 렌더링한다. View 프로토콜을 따르는 struct는 내부에 다음 변수를 반드시 포함해야 한다.

```swift
var body: some View
```

- `some` keyword는 프로토콜을 따르는 '그 무언가'를 의미하는 opaque 타입이다.([도큐먼트](https://docs.swift.org/swift-book/LanguageGuide/OpaqueTypes.html))
- 또한 struct는 뷰를 렌더링한 후 곧 사라진다는 걸 알아야 한다. 이를 trasient 하게 동작한다고 말한다.

### @State

![https://velog.velcdn.com/images/enebin777/post/a282b22a-4b8c-4c69-be22-8f79287dbc86/image.png](https://velog.velcdn.com/images/enebin777/post/a282b22a-4b8c-4c69-be22-8f79287dbc86/image.png)

- trasient 하게 동작하는 것은 곧 상태가 어느 곳에도 저장되지 않는다는 것을 뜻한다. 이를 방지하기 위해서는 `@State` wrapper를 사용한다.
- 원래의 View render cycle에 의해 struct 내의 변수는 설계도로만 이용한 후 치워버리겠지만 @State로 선언한 변수의 경우 heap에 저장되어 사라지지 않고 참조의 형태로 남게 된다.
- 이는 View update life cycle을 통해 새로운 뷰로 렌더링 된다.

### 다른 뷰랑 정보를 공유?

![https://velog.velcdn.com/images/enebin777/post/a2b1efd1-45c9-4a3c-ac87-e3e57815e08e/image.png](https://velog.velcdn.com/images/enebin777/post/a2b1efd1-45c9-4a3c-ac87-e3e57815e08e/image.png)

- 이렇게 하면 안됨. 각자 서로의 source of truth를 만들기 때문이다.

![https://velog.velcdn.com/images/enebin777/post/03064f78-4db7-443f-b949-19a8428541e5/image.png](https://velog.velcdn.com/images/enebin777/post/03064f78-4db7-443f-b949-19a8428541e5/image.png)

- 이런 당신을 위해 `@Binding`이 있다.

### SwiftUI 데이터 3원칙

![https://velog.velcdn.com/images/enebin777/post/8dd98e2b-8c3f-4178-b3ad-50b2244b9d8c/image.png](https://velog.velcdn.com/images/enebin777/post/8dd98e2b-8c3f-4178-b3ad-50b2244b9d8c/image.png)

### ObservableObejct

프로토콜 생김새

![https://velog.velcdn.com/images/enebin777/post/7129e21d-4287-487f-8d7e-5341743bd813/image.png](https://velog.velcdn.com/images/enebin777/post/7129e21d-4287-487f-8d7e-5341743bd813/image.png)

![https://velog.velcdn.com/images/enebin777/post/fcd6689f-199b-431a-a050-fd077ad7b11e/image.png](https://velog.velcdn.com/images/enebin777/post/fcd6689f-199b-431a-a050-fd077ad7b11e/image.png)

- 이런 식으로 데이터 모델 관리가 가능하다

![https://velog.velcdn.com/images/enebin777/post/35523416-2e8f-42a3-bb83-4a29a3e316e5/image.png](https://velog.velcdn.com/images/enebin777/post/35523416-2e8f-42a3-bb83-4a29a3e316e5/image.png)

- 원한다면 여러 개로 분리할 수도 있다

### @Published

![https://velog.velcdn.com/images/enebin777/post/53068b4c-cec8-4bbf-becf-f06139831874/image.png](https://velog.velcdn.com/images/enebin777/post/53068b4c-cec8-4bbf-becf-f06139831874/image.png)

- 포인트: Published는 willSet을 트리거한다

### How it works

![https://velog.velcdn.com/images/enebin777/post/1ef019d9-c27b-467e-b2ce-63504c6d12df/image.png](https://velog.velcdn.com/images/enebin777/post/1ef019d9-c27b-467e-b2ce-63504c6d12df/image.png)

- 왜 didSet(did change)에는 업데이트를 안할까요?
- 해당 변수가 언제 변할 지를 알아야 하기 때문이다. 그래야 한 사이클에 합쳐서(coalesce) 전부 업데이트 시킬 수 있으니.

![https://velog.velcdn.com/images/enebin777/post/84229c07-4e4e-4ef3-876e-80d073373289/image.png](https://velog.velcdn.com/images/enebin777/post/84229c07-4e4e-4ef3-876e-80d073373289/image.png)

### StateObject

Initial value를 객체 생성 직전에만 초기화함

### SwiftUI의 View는 저렴한 연산이다

![https://velog.velcdn.com/images/enebin777/post/c83f640e-891b-44cc-a0e5-b250d5332dc1/image.png](https://velog.velcdn.com/images/enebin777/post/c83f640e-891b-44cc-a0e5-b250d5332dc1/image.png)

- 그러므로 작게 여러개를 이렇게 쌓자. 그렇다고 ObservableObject로 막 쌓지는 말고

![https://velog.velcdn.com/images/enebin777/post/cdb51122-db13-4168-9a8a-4ed10cc106db/image.png](https://velog.velcdn.com/images/enebin777/post/cdb51122-db13-4168-9a8a-4ed10cc106db/image.png)

- 그런 당신을 위해 EnvironmentObject라는 좋은 수단이 있다.

![https://velog.velcdn.com/images/enebin777/post/c97fbeec-0959-4989-8888-fa4340d1b5ee/image.png](https://velog.velcdn.com/images/enebin777/post/c97fbeec-0959-4989-8888-fa4340d1b5ee/image.png)

- 뷰모델 구성 팁

### View detail

![https://velog.velcdn.com/images/enebin777/post/8453e165-521b-474c-80f4-a85202266675/image.png](https://velog.velcdn.com/images/enebin777/post/8453e165-521b-474c-80f4-a85202266675/image.png)

![https://velog.velcdn.com/images/enebin777/post/4bfd00ea-cc47-4d15-94d4-1dc55cb90a90/image.png](https://velog.velcdn.com/images/enebin777/post/4bfd00ea-cc47-4d15-94d4-1dc55cb90a90/image.png)

![https://velog.velcdn.com/images/enebin777/post/8205089c-44bd-4bfa-ac9f-50dfa570f5d3/image.png](https://velog.velcdn.com/images/enebin777/post/8205089c-44bd-4bfa-ac9f-50dfa570f5d3/image.png)

바뀐 view는 렌더링에 다시 사용된다.

![https://velog.velcdn.com/images/enebin777/post/b7d06c27-5c89-474b-a0b1-d6f80afce5f9/image.png](https://velog.velcdn.com/images/enebin777/post/b7d06c27-5c89-474b-a0b1-d6f80afce5f9/image.png)

사이클이 돌다 멈춘다. 막기 위해선(백그라운드 큐에서 돌려도 되고)

![https://velog.velcdn.com/images/enebin777/post/fab37739-6899-4bfc-953d-47f77ef149f4/image.png](https://velog.velcdn.com/images/enebin777/post/fab37739-6899-4bfc-953d-47f77ef149f4/image.png)

- 3번을 좀 설명하면 뷰가 얼마나 호출될지 너가 넘겨짚지 말라는 뜻이다.

### Slow update example

![https://velog.velcdn.com/images/enebin777/post/79370364-30c7-41cd-9e56-d92a97c9ceee/image.png](https://velog.velcdn.com/images/enebin777/post/79370364-30c7-41cd-9e56-d92a97c9ceee/image.png)

계속 heap 할당을 함. 그리고 계속 리로드함.

### 해결

예전엔 다른데서 만들고 집어넣어야 했음. 그런데 StateObject로 퉁칠 수 있게 됨.
다른 데서 집어 넣는 걸 인라이닝 한 거임.

![https://velog.velcdn.com/images/enebin777/post/f8536399-441b-4298-b5d0-fa0013130dc7/image.png](https://velog.velcdn.com/images/enebin777/post/f8536399-441b-4298-b5d0-fa0013130dc7/image.png)

### 데이터 라이프사이클

asdf
# QoS

- **DispatchQueue에 쌓여서 비동기로 실행되는 작업의 우선순위**
- 지정방식 2가지
    1. Queue (Global Queue에서만 가능)
    2. Task(작업)

## QoS

- 종류 6가지
    1. userInteractive
    - 사용자와 **직접 상호작용**하는 작업 ex) UI 업데이트, 애니메이션 등
    - 즉각적으로 실행되기를 기대 but 메인 스레드에서 실행하기에 부담되는 작업
    1. userInitiated
    - 즉각적인 결과를 기대하는 작업 ex) 클릭했을 때 문서를 여는 것
    - userInteractive보다 오래 걸릴수 있고, 유저도 이를 인지하고 있음
    1. default
    - 일반적인 작업
    1. ****utility****
    - 프로그래스바와 함께 길게 실행되는 작업 ex) 파일 다운로드
    1. background
    - 유저가 인지하지 않는, 덜 중요한 작업 ex) 동기화, 백업
    1. ****unspecified****
    - qos 정보가 없음을 나타냄
    - 거의 사용 X

- qos 에 따른 각각의 큐에 작업이 담김

- 우선순위가 높은 작업에 더 많은 스레드가 배치됨

![Untitled](QoS%208fda4f5e70384fd697d994cee04b6403/Untitled.png)

![Untitled](QoS%208fda4f5e70384fd697d994cee04b6403/Untitled%201.png)

## Qos와 Task에 서로 다른 QoS 지정를 지정한 경우

```swift
DispatchQueue.global(qos: .background).async(qos: .utility)
```

1. Task qos >  Queue qos
    - 큐의 우선순위가 해당 작업이 있는 동안만,  동일하게 일시적으로 높아짐
2. Task qos < Queue qos
    - 작업의 우선순위가 큐의 우선순위와 동일해짐

*⇒ 더 높은쪽을 따라감*

## QoS 역전

- 높은 우선순위의 작업이 필요한 자원을 낮은 우선순위의 작업에서 사용중일 경우, 
높은 우선순위의 작업이 대기하고 낮은 우선순위의 작업이 먼저 완료되는 현상
- 언제 발생? 2가지
    1. Serial Queue에서 높은 우선순위의 작업이 나중에 들어오는 경우 
    2. 높은 우선순위 작업이 낮은 우선순위 작업이 완료되어야만 실행될 수 있는 경우 
- Qos가 발생하지 않도록 하기 위해, 동일한 공유자원을 사용하는 Task는 동일한 우선순위로 실행해야 함
- QoS가 발생하면,

[[iOS] 차근차근 시작하는 GCD — 5](https://sujinnaljin.medium.com/ios-차근차근-시작하는-gcd-5-c8e6eee3327b)

[[iOS] DispatchQueue와 task의 QoS 가 다를때의 동작 방식](https://sujinnaljin.medium.com/ios-dispatchqueue의-qos와-task의-qos-가-다를때의-동작-방식-e67f0c777db8)
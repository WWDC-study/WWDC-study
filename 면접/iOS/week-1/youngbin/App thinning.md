# App thinning

*better on: [https://www.notion.so/enebin/App-thinning-c1eeb231290341158bd7e11f49c7aef2?pvs=4](https://www.notion.so/App-thinning-c1eeb231290341158bd7e11f49c7aef2)*

App thinning은 iOS 기기에서 앱 설치 및 저장을 최적화하기 위해 iOS 9에 도입된 기능입니다. 이 기능을 사용하면 운영 체제가 특정 기기에 필요한 앱 리소스만 다운로드하여 설치할 수 있어 앱의 크기가 줄어들고 성능이 향상됩니다.

## 구성요소

---

App thinning은 Slicing, On Demand Resources, Bitcode 세 가지 구성 요소를 가집니다.

### Slicing

Slicing은 **앱 바이너리를 디바이스별로 나누어 필요한 요소만 다운로드하는 기술**입니다

- 예를 들어, iPhone 12에서 다운로드하는 경우 iPhone 12에 필요한 부분만, iPhone 8에서 다운로드하는 경우 iPhone 8에 필요한 부분만 다운로드됩니다.

### Bitcode

Bitcode는 앱을 컴파일할 때 생성되는 [중간 표현](https://ko.wikipedia.org/wiki/%EC%A4%91%EA%B0%84_%ED%91%9C%ED%98%84)입니다. 

- 이 중간표현은 새로운 아키텍처의 기기에서 코드를 다시 컴파일 하는 번거로움을 덜어줍니다.
- Bitcode는 iOS에서 선택적으로 도입 가능하나 watchOS, tvOS에서는 의무적으로 도입해야합니다.

### On Demand Resources

On Demand Resources를 사용하면 앱이 이미지, 비디오 또는 사운드 파일과 같은 추가 콘텐츠를 필요할 때만 요청할 수 있습니다. 

- 앱 리소스의 lazy로딩이 가능합니다.
    - 예를 들어, 레벨이 많은 게임에서 사용자는 현재 및 다음 레벨과 관련된 리소스만 필요합니다.
- 거의 사용하지 않는 리소스를 원격으로 저장 후 필요할 때 요청할 수 있습니다.
    - 예를 들어, 앱 튜토리얼은 일반적으로 앱을 처음 연 후 한 번만 표시되며 다시는 사용되지 않을 수 있습니다.
- 앱은 추가 리소스가 포함된 인앱 구매를 제공하며 구매 이전까지 해당 리소스를 제공하지 않습니다.
    - 구매한 모듈의 리소스를 구매가 발생한 후 요청할 수 있습니다.
- 이를 사용하면 앱의 다운로드 크기를 줄이고 앱의 초기 다운로드 시간을 단축할 수 있습니다.

## 정리

---

- App thinning을 통해 앱 사이즈를 줄일 수 있으며  Slicing, On Demand Resources, Bitcode 3가지 기술을 이용합니다.

## 참고

---

[[문서 정리] 앱 크기 측정하고 최적화하기](https://www.notion.so/13514dd6f51f4032b3b6d36b0f1efd8a) 

### App thinning

[App Thinning in Xcode - WWDC15 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2015/404/)

### ODR

[On-Demand Resources Guide: On-Demand Resources Essentials](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html)

### App size
[Reducing your app’s size | Apple Developer Documentation](https://developer.apple.com/documentation/xcode/reducing-your-app-s-size)

[Doing basic optimization to reduce your app’s size | Apple Developer Documentation](https://developer.apple.com/documentation/xcode/doing-basic-optimization-to-reduce-your-app-s-size)
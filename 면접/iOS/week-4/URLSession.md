# URLSession

better on : [URLSession](https://www.notion.so/URLSession-5facd331c194428496189f3b64a2db28) 

네트워크 데이터 전송에 관한 작업을 담당하는 클래스입니다

## URLSession

---

`URLSession`은 `URL`로 표시된 엔드포인트에서 데이터를 다운로드하고 엔드포인트로 데이터를 업로드하기 위한 API를 제공합니다. 

- 이 API를 사용하여 백그라운드 다운로드를 수행할 수 있습니다.
- `URLSessionDelegate` 및 `URLSessionTaskDelegate`를 사용하여 인증(Authentication)을 지원하고 리디렉션 및 완료와 같은 이벤트를 수신할 수 있습니다.

<aside>
⚠️ `URLSession` API은 상당히 복잡한 방식으로 상호작용하는 다양한 클래스가 포함되어 있으므로 문서만 읽을 경우 명확하지 않을 수 있습니다. API를 사용하기 전, [URL 로딩 시스템](https://developer.apple.com/documentation/foundation/url_loading_system) [항목의 개요](https://www.notion.so/URLSession-5facd331c194428496189f3b64a2db28)를 읽어 보세요. [Essentials](https://developer.apple.com/documentation/foundation/url_loading_system#2878017), [Uploading](https://developer.apple.com/documentation/foundation/url_loading_system#3038363)과 [Downloading](https://developer.apple.com/documentation/foundation/url_loading_system#3038364) 섹션의 문서에서는 `URLSession`으로 작업을 수행하는 예제를 제공합니다.

</aside>

### URL 세션 유형

같은 `URLSession` 내의 작업은 단일 호스트에 대한 최대 동시 연결 수, 연결이 셀룰러 네트워크를 사용할 수 있는지 여부 등과 같은 연결 동작을 정의하는 공통적인 세션 설정(configuration) 객체를 공유합니다.

**Shared**

- `URLSession`에는 기본적으로 싱글톤 `shared` 세션(설정 객체가 없음)이 있습니다. 커스텀 세션만큼 자유롭게 설정할 수는 없지만 요구 사항이 매우 제한적인 경우 좋은 출발점이 될 수 있습니다.
- `shared` 클래스 메서드를 호출하여 이 세션에 액세스합니다.

다른 종류의 세션의 경우 세 가지 구성 중 하나를 사용하여 URLSession을 만듭니다:

**Default**

Default 세션은 `shared` 세션과 매우 유사하게 작동하지만 사용자가 커스텀할 수 있습니다. 기본 세션에 델리게이트를 할당하여 데이터를 점진적(incrementally)으로 가져올 수도 있습니다.

**Ephemeral**

Ephemeral 세션은 `shared` 세션과 비슷하지만 캐시, 쿠키 또는 인증서(credentials)를 디스크에 기록하지 않습니다.

**Background**

Background 세션을 사용하면 앱이 실행되지 않는 동안 백그라운드에서 콘텐츠의 업로드 및 다운로드를 수행할 수 있습니다.

### 델리게이트

세션의 작업들은 델리게이트 객체도 공유합니다. 이 델리게이트를 구현하여 다양한 이벤트 정보를 얻고 가공할 수 있습니다:

- 인증 실패
- 서버에서 데이터 수신
- 데이터 캐싱

델리게이트가 제공하는 기능이 필요하지 않은 경우 세션을 만들 때 `nil`을 전달하여 델리게이트를 제공하지 않고  API를 사용할 수 있습니다.

### 비동기 처리

대부분의 네트워킹 API와 마찬가지로 `URLSession` API는 비동기 동작을 지원합니다.

- Swift를 사용하는 경우 `async` 키워드로 표시된 메서드를 사용하여 일반적인 작업을 수행할 수 있습니다.
    - 예를 들어, `data(from:delegate:)`는 데이터를 가져오고 `download(from:delegate:)`는 파일을 다운로드합니다.
    - 호출 지점에서 `await` 키워드를 사용하여 전송이 완료될 때까지 실행을 일시 중단할 수 있습니다.
    - `bytes(from:delegate:)` 메서드를 사용하여 데이터를 `AsyncSequence`로 수신할 수도 있습니다.
    - 이 접근 방식을 사용하면 앱이 데이터를 수신할 때 `for-await-in` 구문을 사용하여 데이터를 반복합니다.
- Swift 또는 Objective-C에서 전송이 완료될 때 실행되는 컴플리션 핸들러 블록을 제공할 수 있습니다.
- Swift 또는 Objective-C에서 전송이 진행되는 동안과 전송이 완료된 직후에 델리게이트 메서드에 대한 콜백을 받을 수 있습니다.

### 기타

네트워크 **프로토콜 지원**

- `URLSession` 클래스는 사용자의 시스템 환경설정에서 설정한 대로 프록시 서버와 `SOCKS` 게이트웨이에 대한 투명성 지원과 함께 데이터, 파일, ftp, http 및 https URL 스키마를 기본적으로 지원합니다.
- `URLSession`은 HTTP/1.1, HTTP/2, HTTP/3 프로토콜을 지원합니다. RFC 7540에 설명된 대로 HTTP/2를 지원하려면 ALPN(Application-Layer Protocol Negotiation)을 지원하는 서버가 필요합니다.
- 또한 `URLProtocol`을 서브클래싱하여 커스텀 네트워킹 프로토콜 및 비공개용 앱 URL 스키마에 대한 지원을 추가할 수도 있습니다.

**App Transport Security**

- iOS 9.0 및 macOS 10.11 이상에서는 `URLSession`으로 이루어진 모든 HTTP 연결에 대해 App Transport Security(ATS)을 사용합니다. ATS를 사용하려면 HTTP 연결이 HTTPS(RFC 2818)를 사용해야 합니다.

**복사**

세션 및 `task` 개체는 다음과 같이 `NSCopying` 프로토콜을 준수합니다:

- 앱이 세션 또는 작업 개체를 복사할 때 동일한 개체를 다시 가져옵니다.
- 앱이 구성 개체를 복사하면 독립적으로 수정할 수 있는 새 복사본을 받습니다.

**스레드 안정성**

`URLSession` API는 Thread-safe 합니다. 

- 모든 스레드 컨텍스트에서 세션과 태스크를 자유롭게 만들 수 있습니다.
- 델리게이트 메서드가 제공한 완료 핸들러를 호출할 때 작업은 올바른 델리게이트 큐에 자동으로 스케줄됩니다.

## URL loading system

---

### 개요

URL 로딩 시스템은 https와 같은 표준 프로토콜 또는 커스텀 프로토콜을 사용하여 URL로 식별되는 리소스에 대한 액세스를 제공합니다. 로딩은 비동기적으로 수행되므로 앱이 응답성을 유지하면서 데이터 또는 오류를 처리할 수 있습니다.

### With URLSession

![https://docs-assets.developer.apple.com/published/4bf9c6d271/6789dd96-afdc-4c18-b8eb-01f9012dc04d.png](https://docs-assets.developer.apple.com/published/4bf9c6d271/6789dd96-afdc-4c18-b8eb-01f9012dc04d.png)

이를 위해서는 `URLSession` 인스턴스를 사용하여 앱에 데이터를 가져와 반환하거나, 파일을 다운로드하거나, 데이터 및 파일을 원격 위치에 업로드할 수 있는 하나 이상의 `URLSessionTask` 인스턴스를 만듭니다. 

- 세션을 구성하려면 캐시 및 쿠키 사용 방법 또는 셀룰러 네트워크 연결 허용 여부와 같은 동작을 제어하는 `URLSessionConfiguration` 객체를 사용합니다.
- 하나의 세션을 반복적으로 사용하여 작업을 만들 수 있습니다. 예를 들어 웹 브라우저에는 일반 브라우징과 비공개 브라우징을 위한 별도의 세션이 있을 수 있으며, 비공개 세션은 데이터를 캐시하지 않습니다. 위 그림은 이러한 구성을 가진 두 세션엣 여러 작업을 처리하는 방법을 보여줍니다.
- 각 세션은 주기적인 업데이트(또는 오류)를 수신하기 위해 델리게이트와 연결됩니다. 기본 델리게이트는 사용자가 제공한 컴플리션 핸들러 클로저를 호출합니다. 단, 커스텀 델리게이트를 제공할 경우 이 블록은 호출되지 않습니다.
- 백그라운드에서 실행되도록 세션을 구성하여 앱이 일시 중지된 동안 시스템이 대신 데이터를 다운로드할 수 있습니다. 작업이 완료되면 결과를 제공하기 위해 앱을 재실행시킬 수 있습니다.

## 정리

---

- **`URLSession`**은 데이터를 업로드하고 다운로드하는 API를 제공하는 클래스로, 백그라운드 다운로드를 지원하고 인증 및 리디렉션과 같은 이벤트를 처리할 수 있습니다.
- 세션 설정을 통해 연결 동작을 제어하며, shared, default, ephemeral 및 background 세션 유형을 제공합니다. 비동기 처리를 지원하고, 델리게이트를 사용하여 이벤트 정보를 얻고 가공할 수 있습니다.

## 참고
[URLSession | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession)
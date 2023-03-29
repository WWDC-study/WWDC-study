## Frame ↔️ Bounds
1. Frame
   - parent view의 좌표계를 기준으로 한 원점, 사이즈를 나타냄
   - origin: parent view의 원점으로부터 떨어진 위치
   - size: 뷰를 감싸는 직사각형의 사이즈 
  
2. Bounds
   - 뷰 자신의 좌표계를 기준으로 한 원점, 사이즈를 나타냄
   - origin: 뷰 자신의 좌표계 원점
   - size: 뷰 자체의 사이즈 

### 특징
1. Frame
    - 뷰를 회전했을 때 사이즈가 변할 수 있다.
2. Bounds
    - origin을 변경하면, subview가 들을 반대방향으로 이동시킨다.
    - 이유: 좌표계의 원점(origin)이 변경되는 것이기 때문이다.


## Frame변경하면 발생하는 일

---

- origin 변경
    
    ex) frame.origin = (50, 50)
    
    - 뷰의 위치가 변경된다.
    - 뷰의 subview도 같이 이동한다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dcaa4b00-3e08-4a4f-b0a8-6ab88d1f05d5/Untitled.png)
    
- size 변경
    
    ex) bounds.origin = (50, 50)
    
    - child 뷰의 좌표가 이동한다.
    - child 뷰의 frame은 부모뷰의 좌표계를 기준으로하는데, 
    뷰 자신의 좌표계의 원점(origin)이 이동하는 것.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/636bb07d-8547-42d1-936c-46bb107d40d8/Untitled.png)
    

## Bounds를 변경하면 발생하는 일

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18ef4897-9f67-4031-bfc8-6936e9f30292/Untitled.png)

- 1) Frame: 슈퍼뷰 상에서내가 어디에 위치할 것인지
    
    2) Bounds: 나의 sub view 들이 어디 영역까지 보일지 결정하는 값
    
- ex) 400 * 200 짜리 이미지 존재, 이 이미지를 파란색 뷰(200 * 200)의 프레임에 추가하는 경우
    
    슈퍼뷰가 200* 200이라서
    이미지가 400*200이더라도
    짤려서 안보이게 된다.
    
    따라서 슈퍼뷰의 바운더리에 따라서 슈퍼뷰 안의 내용물이 보여지는 것이 결정된다.
  

## UIApplicationMain(), @main
  - AppDeleagte가 앱의 시작 포인트이다.
- Swift에서 main.swift 파일이 프로그램의 시작 포인트이다.
- main.swift가 UIApplicationMain() 메서드를 호출한다.
- UIApplicationMain()역할
    1.  app과 appDelegate 인스턴스를 생성한다.
    2. event run rool(Swift 애플리케이션이 실행되는 동안 지속되는 반복문)을 얻는다.

- @main 을 표시하면 자동으로 main.swift 파일이 생성되며, app과 appDelegate 인스턴스가 생성되는 이후 과정이 지속된다.
- @main을 AppDeleagte 클래스에다가 지정함으로써, 어떤 AppDelegate 클래스의 인스턴스를 생성해야 하는지를 지정한다.

- Swift 5.3버전 이전에 @main = @UIApplicationMain

## 실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명하시오
- 할 수 있는 것
    - 나라별 키보드 입력을 할 수 있습니다.
    - 접근성 기능을 사용할 수 있습니다.
    - 지역화 기능을 사용할 수 있습니다.
- 할 수 없는 것
    - 일부 하드웨어 기능(카메라, 마이크, 전화 기능, 블루투스, 가속계와 자이로스코프)
    을 사용할 수 없습니다.
    - 마우스로 시뮬레이터의 터치를 하기 때문에 두 손가락으로 하는 줌인 줌아웃 등의 기능을 테스트 할 수 없습니다.
    - Handoff 기능을 지원하지 않습니다.
- 네트워크 속도 테스트는 가능하다.

## 앱의 콘텐츠나 데이터 자체를 저장/보관하는 특별한 객체를 무엇이라고 하는가? 

## App Thining
- 앱스토어에서 앱을 다운받을 때, 기기별 특성에 맞게 앱 다운로드 과정을 최적화하는 기술
- 장점:
    - 빠른 다운로드
    - 최소한의 디스크 사용
- 종류 3가지
    1. app slicing
    2. bit code
    3. 주문형 리소스
- App Slicing (앱 슬라이싱)
    - 앱스토어에서 디바이스와 OS 버전별로 번들을 생성
    - Asset 카탈로그를 사용할 때, 앱스토어에서 앱 슬라이싱을 한다.
    Asset 카탈로그에서 디바이스와 OS 버전에서 필요한 리소스만 번들에 담는다.
    - 앱을 다운받을 때는, 앱스토어에서 기기와 OS 버전에 맞는 번들을 제공.
    - Xcode에서 빌드할 때도 앱 슬라이싱이 일어난다.
- bit code
    - 기계어로 번역되기 전의 중간 표현에 해당
    - 앱스토어에서 bit code를 컴파일하여 binary code 생성
    - 이 과정에서 디바이스 별로 최적화를 할 수 있다.
    (따라서 앱을 재 제출할 필요 없다.)
    - watchOS,tvOS apps에서 비트코드는 필수
    - CPU가 변경되었을 때, 개발자가 새로운 아키텍처로 컴파일해서 제출해야할 필요 없어짐
    
    <aside>
    💡 과거 새로운 아이폰이 64비트 칩셋으로 바꾼적이 있는데 이 때 bitcode가 적용되어있지 않는 시절이라 앱 개발자들은 코드를 수정하고 앱을 다시 컴파일하여 제출했다고 합니다. bitcode가 적용됬더라면 새로운 아키텍쳐로 컴파일 할 수 있도록 알아서 최적화를 했을텐데 말이죠. ( 출처: [https://zeddios.tistory.com/655](https://zeddios.tistory.com/655) )
    
    </aside>
    
- 주문형 리소스

## 앱 화면의 콘텐츠를 표시하는 로직과 관리를 담당하는 객체를 무엇이라고 하는가? 
UserDefault
- 앱에 디폴트로 제공되는 데이터베이스에 대한 인터페이스로, 
앱이 설치된 동안에 유지된다.
- 용도
    - 대용량 데이터X , 자동로그인 여부 등과 같은 단일 데이터
- 특징
    - 값을 key-value 형태로 저장
    - 런타입에 UserDefault 데이터베이스에 저장된 데이터를 읽어올 수 있다.
    - Thread-Safe하다.
    - 읽어온 값을 캐싱해둔다.
    b) 값이 필요할 때마다, UserDefault 데이터베이스에 접근하는 것을 막기 위해
    - UserDefault 에 저장된 값을 바꾸면, 프로세스에는 동기적으로 반영.
    ↔ UserDefault의 데이터베이스나 다른 프로세스에는 비동기적으로 반영
    - UserDefault의 데이터베이스는 일반적으로 1개의 앱에 1개 생성
- 상세 설명


CoreData
- 큰 데이터 or 객체 참조 데이터베이스를 저장

- SQLite와 달리 테이블 사용 X, 객체를 생성하여 데이터를 관리하기 때문에 더 많은 메모리 필요
→ 속도 더 빠름

- Data Model을 생성 후, Entity 를 생성

- 디바이스에서 데이터를 저장 or 캐시하고 실행 취소를 지원하는 프레임워크
- iCloud를 통해 여러 디바이스에서 앱을 동기화하기 위해, CoreData는 자동으로 app의 scheme을 Cloud kit에 반영한다.

**영구 데이터 저장 즉 데이터 베이스** 기능은 Core Data 기능의 일부분으로 데이터 베이스 기능을 포괄하는 프레임워크이다

### 기능

1. 오프라인 사용을 위한 **영구 데이터 저장**
    - 데이터 모델을 추상화하여 데이터베이스를 직접 관리하지 않고 데이터를 쉽게 저장할 수 있다
    
    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d2f1e72-cc0d-47ed-8e41-6803e183263e/1B7J5e9WtftZnkqB6kVhScw.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d2f1e72-cc0d-47ed-8e41-6803e183263e/1B7J5e9WtftZnkqB6kVhScw.png)
    
    Core Data에서 데이터를 저장할때 3가지 타입의 저장소를 지원한다
    
    1. SQLite (defalut)
        
        SQL 관계형 데이터베이스 저장되고 가장 많이 사용하는 저장소 중 하나이다
        
        SQL Lite를 직접 사용하는 것이 아님으로 SQL을 알필요 없음
        
    2. in-memory 
        
        in-memory , binary 방식은 메모리 사용량 , 성능 측면에서 다른 특성을 가진다
        
        데이터 모델이 작은 경우 (메모리에  모든 데이터를 저장될만큼) 사용된다 
        
        disk 미사용으로 그만큼 빠른 속도를 보장한다   ex) cache
        
    3.  binary store
        
        항상 전체를 읽어오고 , 전체를 기록하는 상황에서 는 binary store를 사용한다
        ex ) csv
        
    
    [https://developer.apple.com/documentation/foundation/nsuserdefaults](https://developer.apple.com/documentation/foundation/nsuserdefaults)
    
    [https://developer.apple.com/documentation/coredata](https://developer.apple.com/documentation/coredata)
    
    [https://baked-corn.tistory.com/49](https://baked-corn.tistory.com/49)
    

# **Core Data**

오프라인 사용을 위해

1) 애플리케이션의 영구 데이터를 저장

2) 임시 데이터를 캐시

3) 단일 기기에서 앱에 실행 취소 기능을 추가하는 프레임워크

Core Data의 데이터 모델 편집기를 통해 데이터의 유형과 관계를 정의하고 각 클래스 정의를 생성합니다. Core Data는 런타임에 개체 인스턴스를 관리하여 다음 기능을 제공 할 수 있습니다

### **Persistence**

- Core Data는 객체(object)를 저장소에 매핑하는 세부 정보를 추상화하여 데이터베이스를 직접 관리하지 않고도 Swift 및 Objective-C의 데이터를 쉽게 저장할 수 있습니다.

![https://docs-assets.developer.apple.com/published/5770f06627/73cb5bb3-06ab-4017-89da-84ddacd20279.png](https://docs-assets.developer.apple.com/published/5770f06627/73cb5bb3-06ab-4017-89da-84ddacd20279.png)

### **개별 또는 일괄 변경 사항 실행 취소 및 다시 실행**

Core Data의 실행 취소 관리자가 변경 사항을 추적하고 개별적으로, 그룹으로 또는 한 번에 모두 롤백 할 수 있으므로 앱에 실행 취소 및 다시 실행 지원을 쉽게 추가 할 수 있습니다.

![https://docs-assets.developer.apple.com/published/843104c2e8/8ff27b5a-9441-4054-91ec-152843782ce2.png](https://docs-assets.developer.apple.com/published/843104c2e8/8ff27b5a-9441-4054-91ec-152843782ce2.png)

### **백그라운드 데이터 작업**

백그라운드에서 JSON을 객체로 구문 분석하는 것과 같이 UI를 blocking시킬 수 있는 데이터 작업을 수행합니다. 그런 다음 결과를 캐시하거나 저장하여 서버 왕복을 줄일 수 있습니다.

![https://docs-assets.developer.apple.com/published/1b9da4c41b/efc8be3c-1e32-4579-9356-4e029b99268a.png](https://docs-assets.developer.apple.com/published/1b9da4c41b/efc8be3c-1e32-4579-9356-4e029b99268a.png)

### 뷰와 데이터 동기화

table view와 collection view에 데이터소스를 제공하여, view와 core data간의 데이터 동기화 기능도 제공합니다.

### 동기화, 버전관리 및 마이그레이션

- 버전 관리 기능, 마이그레이션 기능 제공


Realm 
- 모바일에 최적화된 데이터베이스 라이브러리
- SQLite, Core Data보다 속도빠름. 성능면에서 우수
- 작업을 처리할때, 다른 라이브러리보다 적은 코드로 가능
- 메인 스레드에서 읽기, 쓰기 작업 가능하여 편리
- 용량에 관계 없이 속도, 성능이 유지

SQLite
- 모바일에 최적화된 데이터베이스 라이브러리
- SQLite, Core Data보다 속도빠름. 성능면에서 우수
- 작업을 처리할때, 다른 라이브러리보다 적은 코드로 가능
- 메인 스레드에서 읽기, 쓰기 작업 가능하여 편리
- 용량에 관계 없이 속도, 성능이 유지
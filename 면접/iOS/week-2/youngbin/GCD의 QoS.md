## GCD QoS
better on : [GCD의 QoS 종류와 각각의 의미](better on : @GCD의 QoS 종류와 각각의 의미)

---

![https://velog.velcdn.com/images/enebin777/post/1f5bf4e8-4fdf-4bdd-9674-aa9132957c7d/image.png](https://velog.velcdn.com/images/enebin777/post/1f5bf4e8-4fdf-4bdd-9674-aa9132957c7d/image.png)

- **User Interactive**
    
    메인 스레드에서 수행되는 작업으로, 성능과 응답성에 중점을 둡니다. UI 관련 작업, 사용자 인터페이스 업데이트, 애니메이션 실행 등의 작업을 실행합니다.
    
    - 작업을 User Interactive로 설정할 경우 자동으로 User Initiated로 전환됩니다.
    - quote
        
         [As I mentioned in the previous slide,](https://developer.apple.com/videos/play/wwdc2015-718/?time=890) [dispatch async does automatic propagation of Quality](https://developer.apple.com/videos/play/wwdc2015-718/?time=893) [of Service from the submitting thread to the queue](https://developer.apple.com/videos/play/wwdc2015-718/?time=896) [where you submit the block to execute.](https://developer.apple.com/videos/play/wwdc2015-718/?time=899) [Now, in this case there's a special rule that applies](https://developer.apple.com/videos/play/wwdc2015-718/?time=902) [which is that we automatically translate Quality](https://developer.apple.com/videos/play/wwdc2015-718/?time=907) [of Service Class user interactive to user initiated.](https://developer.apple.com/videos/play/wwdc2015-718/?time=909))
        
- **User Initiated**
    
    메인 쓰레드를 방해하지 않으면서 사용자가 수행한 작업의 결과를 로드하는 경우 사용할 수 있습니다. 주로 스크롤 뷰를 스크롤하여 다음 셀을 불러오거나, 이메일을 탭하여 이메일 내용을 로드하는 등의 작업을 수행한 후 결과를 기다릴 때 주로 사용합니다. 
    
    - 사용자와 상호작용을 유지할 필요가 있는지 고려해야 합니다.
- **Utility**
    
    시간이 많이 필요한 작업을 위해 사용할 수 있습니다. 이미지 다운로드와 같이 완료하는 데 시간이 걸리는 작업을 예로 들 수 있습니다.
    
    - 반응성과 성능, 에너지 효율성 간의 균형을 잡으면서 작업을 수행합니다.
- **Background**
    
    사용자가 진행 상황을 알 필요가 없는 작업을 위해 사용할 수 있습니다. 동기화, 인덱싱, 데이터베이스 비우기 등의 작업을 예로 들 수 있습니다.
    
    언제 수행할 지 사용자가 인식하지 못한다.
    
    - 에너지 효율에 중점을 둡니다.

또한 2개의 추가 QoS가 존재합니다.

- **Default**
    
    **User Initiated**과 **Utility** 사이에 위치하며 이를 직접 사용하는 것은 권장하지 않습니다.. QoS가 할당되지 않은 작업에는 이 우선순위를 할당합니다.
    
- ****Unspecified****
    
    QoS가 존재하지 않는 상태입니다.
    

## 정리

---

- GCD에 작업을 등록할 때 다음 4개의 QoS 중 하나를 선택할 수 있습니다.,
    - User Interactive
    - User Initiated
    - Utility
    - Background
- 다음 2개의 추가적인 QoS가 존재하지만 선택할 수 없습니다.
    - Default
    - Unspecified

## 참고

---

[DispatchQoS | Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqos)

### WWDC

[Building Responsive and Efficient Apps with GCD - WWDC15 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2015/718/)

- Note: [[WWDC2015] - Building Responsive and Efficient Apps with GCD (velog.io)](https://velog.io/@grumpy-sw/Building-Responsive-and-Efficient-Apps-with-GCD)

[Modernizing Grand Central Dispatch Usage - WWDC17 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2017/706/?time=46)

- Note: [Modernizing Grand Central Dispatch Usage | WWDC NOTES](https://www.wwdcnotes.com/notes/wwdc17/706/)
# [2015] Mysteries of Auto Layout, part 2

[wwdc2015-219.txt](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/wwdc2015-219.txt)

## The Layout Cycle

![스크린샷 2022-11-27 오후 4.21.31.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-11-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.21.31.jpg)

1. Application Run Loop
    - 계속해서 동작
    = constraint 가 변해서 layout이 다시 계산되어야 할 때까지 반복문을 돈다.
2. Constraints Change
3. Deferred Layout Pass
    - 새롭게 계산된 layout이 전달된다. (하지만 왜 deferred? 바로 반영되지 않기 때문에 그런것으로 보인다!)
        1. constraint를 변경한다. (레이아웃 엔진가지고 있는, 뷰의 위치 정보는 변경되지만, UI에는 아직 반영되지 않는다.)
        2. 실제로 layout이 전달되면, engine이 가지고 있는 뷰의 위치 정보대로 ui가 변경된다.
    - 뷰 계층을 따라서 layout을 변경한다.

## Constraint Changes

- constraint가 수학식으로 변경되어서, layout engint에 저장된다.
- ex) constant, priority, activate/deative 등이 constraint에 해당한다.

- constraint가 변경되면 발생하는 일
1. layout engine이 layout을 다시 계산한다.
2. expresssion은 sieze, origin과 같은 variable로 이루어져 있기 때문에, 
계산 결과, size, origin과 같은 값이 바뀐다.
3. view들은 알림을 받고, superView.setNeedsLayout()을 호출하여, super view가 다시 배치되어야 한다고 표시한다.
    
    → deferred layout pass가 스케쥴링된다. (미래에 일어나야 한다고 예약해놓는 것)
    
- = 이때가 layout engine 내부에 있는 frame값은 변경되지만, 이 값이 실제 뷰에는 반영되지 않는 상태이다.
1. deferred layout pass가 도착하면, view를 다시 배치한다.

- “a pass”라는 표현은 정확하지는 않다. **b) 뷰계층을 따라서 2가지 pass가 일어난다.**
1. updating constraints 
    1. constraint 변경사항 중에서 아직 반영되지 않고, 보류중인 것이 있는지 확인하기 위한 것이다. 
    2. 뷰를 재배치하기 전에 일어난다. 모든 문제를 해결하기 위해 
2. resassign view frames = view 재배치 

### update constraint

- constraint를 변경하기 위해서, view는 명시적으로 constraint update를 요청해야 한다.
- 어떻게 요청? 메서드를 호출! `setNeedsUpdateConstraints()`
- 프로그래머가 setNeedsUpdateConstraints() → 그후에 자동으로, updateConstraints() 메서드가 호출된다.
- 하지만 대개는 이 메서드를 호출할 필요 없다. 이유? 초기화할 때 이미 지정이 다되고, constraint를 변경할 일이 없기 때문이다!
- constraint초기화작업은 interface builder 내부에서 해주는 것이 가장 좋다. 
프로그래밍으로 constraint를 설할 때는, viewDidLoad와 같은 곳이 좋다.
    - constraint 변경작업을 viewDidLoad가 아니라, 다른 곳에서 한다면 코드를 읽기가 어려워질 수 있다.
- 언제 constriant update를 사용하는 것이 좋은가?
    - 성능적으로! 제약조건을 변경하는 것이 너무 느릴 때, update해라!
    - [It turns out that changing a constraint inside update](https://developer.apple.com/videos/play/wwdc2015-219/?time=313) [constraints is actually faster](https://developer.apple.com/videos/play/wwdc2015-219/?time=316) [than changing a constraint at other times.](https://developer.apple.com/videos/play/wwdc2015-219/?time=318)
    - [The reason for that is because the engine is able](https://developer.apple.com/videos/play/wwdc2015-219/?time=320) [to treat all the constraint changes that happen](https://developer.apple.com/videos/play/wwdc2015-219/?time=322) [in this pass as a batch.](https://developer.apple.com/videos/play/wwdc2015-219/?time=325)
    - [This is the same kind of performance benefit that you get](https://developer.apple.com/videos/play/wwdc2015-219/?time=326) [by calling activate constraints on an entire array](https://developer.apple.com/videos/play/wwdc2015-219/?time=329) [of constraints as opposed to activating each](https://developer.apple.com/videos/play/wwdc2015-219/?time=332) [of those constraints individually.](https://developer.apple.com/videos/play/wwdc2015-219/?time=334)[you have a view](https://developer.apple.com/videos/play/wwdc2015-219/?time=340) [that will rebuild constraints in response to some kind](https://developer.apple.com/videos/play/wwdc2015-219/?time=343) [of a configuration change.](https://developer.apple.com/videos/play/wwdc2015-219/?time=346)
    - ex) 변경된 configuration에 맞춰서 constraint를 다시 설정해야 할 때 setNeedsUpdateConstraints를 사용해라.
    - [It turns out to be very common for clients of these kinds](https://developer.apple.com/videos/play/wwdc2015-219/?time=347) [of views to need to configure more than one property,](https://developer.apple.com/videos/play/wwdc2015-219/?time=350) [so it's very easy for the view, then,](https://developer.apple.com/videos/play/wwdc2015-219/?time=353) [to end up rebuilding its constraints multiple times.](https://developer.apple.com/videos/play/wwdc2015-219/?time=355) [That's just a lot of wasted work.](https://developer.apple.com/videos/play/wwdc2015-219/?time=358) [It's much more efficient in these kinds of situations](https://developer.apple.com/videos/play/wwdc2015-219/?time=360) [to have the view just call setNeedsUpdateConstraints](https://developer.apple.com/videos/play/wwdc2015-219/?time=362) [and then when the update constraints pass comes along,](https://developer.apple.com/videos/play/wwdc2015-219/?time=366) [it can rebuild its constraints once](https://developer.apple.com/videos/play/wwdc2015-219/?time=369) [to match whatever the current configuration is.](https://developer.apple.com/videos/play/wwdc2015-219/?time=371) → 뭔뜻임?
- 모든 뷰에 대해서 constraint를 업데이트가 끝나면 → reposition the view로 넘어간다.

### reposition the views

![스크린샷 2022-12-07 오후 7.53.53.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.53.53.jpg)

- top down 방식으로 뷰 계층을 재배치
- setNeedsLayout()이 호출된 뷰에서, layoutSubViews()를 호출한다.
    - layoutSubViews() = 리시버 포함X, 리시버의 자식 뷰들을 재배치하기 위함
- layout engine이 계산한 frame을 가져다가 subview에 할당한다.
    - iOSl: setBounds, setCenter
- layoutSubViews
    - custom layout을 구현할 때 오버라이딩하는 함수
    - → 이슈가 많이 발생하는 지점
- constraint를 사용해서 표현할 수 있는 Layout이 아닐 때만 layoutSubViews를 오버라이딩해라.
    - layoutSubViews가 호출되는 시점에는 일부는 layout되었고, 일부는 되지 않았을 때라는 것을 알아야 한다.
    - 따라서, 따라야 하는 규칙 존재
        1. **super.layoutSubviews()를 호출해야 한다.**
        2. invalidate layout within your subtree → super.layoutSubviews() 를 호출해야 한다.
        3. **setNeedsUpdateConstraints()를 호출하면 안된다.**
            1. 이미 update constraint pass가 끝났기 때문에,
        4. don;t invalidate the layout of views outside your subtree
        b) layout feedback loops이 발생한다. 
        view를 배치하는 작업이 다시 발생해서, view들의 위치가 잘못될 수 있다.
        무한 반복이 발생할 수도 있다.
        5. constraint를 수정할 수 있다.
         다만, 수정했을 때, 어떤 뷰가 영향을 받을지 예측하기 어렵다.
        subtree 바깥에 존재하는 뷰의 위치를 invalidate 시킬 수 있다. 
- 모든 뷰의 layoutSubView 호출이 끝나면, 레이아웃 사이클 (3개원)이 끝나고, 모든 뷰가 올바른 위치에 존재한다. constraint가 모두 적용된다.

## layout cycle 관련해서 기억할 점

1. constraint를 변경하고, view 의 frame이 바로 바뀌지 않는다!
2. layoutSubviews()를 오버라이딩할 때는 규칙을 꼭 지켜라!
(layout feedback loop이 발생하지 않도록 규칙을 꼭 지켜라!)

## AutoLayout과 Legacy Layout System이 어떻게 같이 동작?

- Legacy Layout System: Frame, autoresizingMask
- frame을 지정해도, AutoLayout으로 지정한 frame값으로 덮어씌워진다.
- layoutSubviews()를 오버라이딩할 때, frame도 지정하고 싶은 경우 발생. 이럴 땐 어떻게해?
- **flag값을 설정해라 = translatesAutoResizingMaskIntoCoinstriants: Bool**
- view가 Legacy Layout system에서 동작하게 해준다.
- translatesAutoResizingMaskIntoConstraints 플래그 동작방식
    1. framework가 frame의 내용을 가지고 constraints를 생성해서, layout engine 이 생성한 frame에 덮어씌운다. (enforce the frame in the layout engine)
    2. = 내부에서는 frame의 내용과 동일한 constraint를 사용한다. 
    3. 이 constraint는 autoresizingMask처럼 동작한다. 
    4. 사실상 내부에서 constraint를 사용하기 때문에, other view가 frame으로 할당된 view에 constraint를 걸 수 있도록 해준다. 
- constraint를 사용할 거면, 이 flag를 꺼야 한다.
    - 이 flag를 끄지 않을 경우? frame 내용과 일치하는 constraint 생성, autolayout constraint와 충돌되면서 에러 발생

<aside>
💡 NSAutoresizingMaskLayoutConstraint
flag가 켜졌을 때 framework에서 생성하는 class

</aside>

⇒ frame으로 지정할 때는, **translatesAutoResizingMaskIntoCoinstriants: Bool 를 true로 설정해라**

## #9. Constraint Creation

---

- 문법이 변경되었다.
- 컴파일 타임 에러 체킹을 해준다.

## Constraining Negative Space

![스크린샷 2022-12-08 오전 12.44.13.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.44.13.jpg)

- 버튼간 간격을 유지
- group으로 center에 놓기
- → 기존 해결책: empty view
- 비효율적임: [And it's also inefficient, especially on iOS,](https://developer.apple.com/videos/play/wwdc2015-219/?time=1084) [where every view has a layer associated with it.](https://developer.apple.com/videos/play/wwdc2015-219/?time=1087)

## NSLayoutGuide, UILayoutGuide

![스크린샷 2022-12-08 오전 12.51.14.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.51.14.jpg)

- LayoutEngine 내부에서 사각형을 나타낸다.
- view 처럼 Constraint을 걸 수 있다.
- 사용 방법
    - UILayoutGuide를 생성해서 view에 추가한다.
    

```swift
var layoutMarginGuide: UILayoutGuide
```

- UIView는 margin의 layout anchor를 노출하지 않는다.
대신 new layout margin guide가 존재한다. (위의 프로퍼티)
- margin에 대한 layout guide는 margin 안에 존재하는 view의 area를 나타낸다.
[This layout guide just represents the area](https://developer.apple.com/videos/play/wwdc2015-219/?time=1140) [of the view inside the margins.](https://developer.apple.com/videos/play/wwdc2015-219/?time=1142)
- 따라서 margin의 layout anchor에 constraint를 걸고 싶으면, layout margin guide를 사용하면 된다.
- layoutMarginGuide를 사용하는 것이, 더 가볍게 해결할 수 있다.
그릴 필요 없는 뷰로 뷰 계층을 혼란스럽게 하지 않아도 된다.

## #11. Unsatifiable Constraints = Debugging Layout

---

1. 가장 하단의 constraint를 보기
    
    ![스크린샷 2022-12-08 오전 12.58.57.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.58.57.jpg)
    
    - layout engine에서 break한 constraint이다.
2. translatesAutoResizingMaskIntoConstraints 가 1번의 view에 설정되어있는지 확인하기
3. log에서 1번의 view에 연관된 constraint를 찾기
    
    ![스크린샷 2022-12-08 오전 12.59.43.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.59.43.jpg)
    
4. 1번의 view와 constraint로 연결되 view를 차직
    
    ![스크린샷 2022-12-08 오전 1.01.05.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.01.05.jpg)
    
    - Saturn의 하단에 label이 위치 ,
    label을 superview의 top에 위치
    Saturn 은 높이가 100보다 크기 때문에
    
    ## constraint에 identifier를 추가하면, 디버깅할 때 어떤 constraint에 대한 log인지 찾기 쉽다.
    
    ![스크린샷 2022-12-08 오전 1.06.23.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.06.23.jpg)
    

![스크린샷 2022-12-08 오전 1.10.42.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.10.42.jpg)

![스크린샷 2022-12-08 오전 1.11.20.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.11.20.jpg)

![스크린샷 2022-12-08 오후 4.07.59.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.07.59.jpg)

## Resolving Ambiguity

---

![스크린샷 2022-12-08 오후 4.30.49.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.30.49.jpg)

![스크린샷 2022-12-08 오후 4.35.31.jpg](%5B2015%5D%20Mysteries%20of%20Auto%20Layout,%20part%202%209462f40cfaca41faa9f6a87faecdf259/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.35.31.jpg)
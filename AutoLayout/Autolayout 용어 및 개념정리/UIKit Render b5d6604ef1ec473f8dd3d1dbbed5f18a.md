# UIKit Render

### Refs

- [pdf](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjZpIfNpKP6AhVdQfUHHYuLBIYQFnoECAkQAQ&url=https%3A%2F%2Fdevstreaming-cdn.apple.com%2Fvideos%2Fwwdc%2F2018%2F220f49ijgby0rma%2F220%2F220_high_performance_auto_layout.pdf%3Fdl%3D1&usg=AOvVaw3V9nKwjumyOlPa-XTiEXdj)
- [Demystifying iOS Layout โข GameChanger Tech Blog (gc.com)](https://tech.gc.com/demystifying-ios-layout/)

# Constraints & Auto layout

## Constraints

![[High Performance Auto Layout - WWDC18 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/220/)](https://velog.velcdn.com/images/enebin777/post/873a2666-32c8-41f8-8ef6-19a3d0df5ce4/image.png)

[High Performance Auto Layout - WWDC18 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/220/)

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/nslayoutconstraint)

<aside>
๐ก from [Documentation](https://developer.apple.com/documentation/uikit/nslayoutconstraint)
The relationship between two user interface objects that must be satisfied by the constraint-based layout system.

</aside>

- Constraint๋ constraint-base-system์์ ๋ทฐ๋ฅผ ๊ตฌ์ฑํ๋๋ฐ ์ฌ์ฉ๋๋ ์์ ์ค ํ๋๋ค.
- Constraints๋ ๋ทฐ์ ๋ทฐ ์ฌ์ด์ ๊ด๊ณ๋ฅผ ์์น์ ์์์ผ๋ก ์ ์ํ๋ค.
- ์ฌ์น ์ฐ์ฐ์ด ์ฌ์ฉ๋  ์ ์์ผ๋ฉฐ ๋ค์๊ณผ ๊ฐ์ ํํ๋ก ํํ๋๋ค
    
    ```swift
    button2.leading = 1.0 ร button1.trailing + 8.0
    ```
    

## Auto Layout

### ๊ฐ์

[Understanding Auto Layout](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

<aside>
๐ก from [Guide](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)
**Auto Layout dynamically calculates the size and position of all the views in your view hierarchy, based on constraints placed on those views.**
โฆ If the image viewโs size or position changes, the buttonโs position automatically adjusts to match.

</aside>

- `์คํ  ๋ ์ด์์`์ ํ๋ฉด์ ๋ณด์ด๋ ๋ทฐ ์ฌ์ด์ hierarchy์ contstraints๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ๊ฐ ๋ทฐ์ ์ฌ์ด์ฆ์ ํฌ์ง์์ ๊ณ์ฐํ๋ค.
- ๊ฐ ๋ทฐ์ ํฌ์ง์์ด๋ ์ฌ์ด์ฆ๊ฐ ๋ณ๊ฒฝ๋  ๋๋ง๋ค ๋ณํ๋ฅผ ๊ณ์ฐํด ๋ทฐ๋ฅผ ์ฌ๋ฐฐ์นํ๋ค.
- ๋๋ถ์ ๋ค์ํ ํ๋ฉด์ ๊ธฐ์ข์์๋ ๊ฐ์ ๋ฐฐ์น๋ฅผ ์ ์งํ  ์ ์๊ฒ ํด์ค๋ค.

### ์คํ  ๋ ์ด์์์ ์ฅ์ 

- ์คํ  ๋ ์ด์์์ ํนํ ๋ค์๊ณผ ๊ฐ์ ์ํฉ์ ์ ์ฉํ๋ฉฐ ๋ฐํ์์ ๋์ ์ผ๋ก ํ๋ฉด์ ๋ณํ์ํฌ ์ ์๊ฒ ํด์ค๋ค.
1. **External changes(์ธ๋ถ ๋ณํ)**
    
    *์ธ๋ถ ์์ธ์ผ๋ก ์ธํด ๋ฌผ๋ฆฌ์ ์ผ๋ก ๋ฐ๋๋ ๊ฒฝ์ฐ(์คํ๋ฆฟ ๋ทฐ, ํ๋ฉด ํ์  ๋ฑ)*
    
    - The user enters or leaves Split View on an iPad (iOS).
    - The device rotates (iOS).
    - The active call and audio recording bars appear or disappear (iOS).
    - You want to support different size classes.
    - You want to support different screen sizes.
2. **Internal changes(๋ด๋ถ ๋ณํ)**
    
    *๋ด๋ถ ์์ธ์ผ๋ก ์ธํด ๋ฐฐ์น ํน์ ๊ตฌ์ฑ์ด ๋ฐ๋๋ ๊ฒฝ์ฐ (๋ทฐ ์ปดํฌ๋ํธ๊ฐ ์ฌ๋ผ์ง, ๋ค์ด๋ด๋ฏน ํฐํธ ๋ฑ)*
    
    - The content displayed by the app changes.
    - The app supports internationalization.
    - The app supports Dynamic Type (iOS).

## ๋ทฐ ์ก๋ ๋ฐฉ๋ฒ ๋น๊ต

(***Frame base layout, Autoresizing mask, Auto layout)***

**์คํ  ๋ ์ด์์์ ์ ํต์ ์ผ๋ก ์ฌ์ฉ๋๋ ํ๋ ์ ๋ฒ ์ด์ค ๋ ์ด์์๊ณผ ๋๋น๋๋ ๊ฐ๋์ด๋ค**

### Frame base layout

- `Frame base ๋ ์ด์์`์ origin, width, height ์ธ ์์น๋ก ์ ์ํ๋ค.
- **๊ฐ์ฅ ๋น ๋ฅธ ๋์๋ฐฉ์์ด๋ฉฐ ์ ํํ ์ํ๋ ์์น์ ๋ฐฐ์น**ํ  ์ ์๋ ์ฅ์ ์ด ์๋ค.

![ํ๋ ์ ๋ฒ ์ด์ค ๋ ์ด์์](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled.png)

ํ๋ ์ ๋ฒ ์ด์ค ๋ ์ด์์

![์คํ  ๋ฆฌ์ฌ์ด์ง ๋ง์คํฌ](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%201.png)

์คํ  ๋ฆฌ์ฌ์ด์ง ๋ง์คํฌ

- ํ์ง๋ง, ์ด๋ฐ ๋จ์ ์ด ์๋ค
    - ๋ชจ๋  ๋ทฐ์ ๋ํด ๊ฐ๋ณ์ ์ธ ์ค์ ๊ณผ ๊ด๋ฆฌ๊ฐ ํ์ํ๋ค.
    - ์ค๊ณ ๋ฐ ๋๋ฒ๊ทธ๋ ์ ์ง๊ด๋ฆฌ ๊น๋ค๋กญ๋ค.
- **ํ๋ง๋๋ก  ์กด๋ด ๊ท์ฐฎ๋ค.** ์ด๋ฐ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด์ ๋ฑ์ฅํ ๊ฒ์ด ๋ฐ๋ก `autoresizing mask`.

### **Autoresizing mask?**

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622559-autoresizingmask)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%202.png)

- `autoresizing mask`๋ ์ํผ๋ทฐ์ ํ๋ ์์ด ๋ณํ  ๋ ์๋ธ๋ทฐ์ ํ๋ ์์ด ์ด๋ป๊ฒ ๋ณํ ์ง ์ ์ํ๋ ๋ฐฉ์์ผ๋ก bitmask๋ฅผ ์ด์ฉํ๋ค
- View์ bounds๊ฐ ๋ณ๊ฒฝ๋๋ฉด subview๋ค์ ๊ฐ subview์ autoresizing mask์ ํด๋น๋๋ ๋ถ๋ถ์ ์๋์ผ๋ก ์ฌ์ค์ ํ๋ ์๋ฆฌ
- UIView.AutoresizingMask์ ์ค๋ช๋ ์์๋ฅผ ๊ฒฐํฉํ์ฌ ๊ฐ์ ์กฐ์ ํ๋ค(์ ์ฌ์ง ์ฐธ์กฐ)
- ์๋์ ๊ฐ์ด ์ฌ์ฉํ๋ค

```swift
sampleSubview.autoresizingMask = [.flexibleWidth, .flexibleHeight]
```

**์ฅ์ **

- ์ด๋ก์ external changes์ ๋ํ ๋์์ด ๊ฐ๋ฅํด์ก๋ค. (์คํ๋ฆฟ ๋ทฐ๋ก ์ง๊ทธ๋ผ๋ค๋ฉด

**๋ฌธ์ **

- ๊ทผ๋ฐ ๊ฒฐ๊ตญ ์๋ ๋ฌธ์ ๋ฅผ ๋ค ํด๊ฒฐํ์ง ๋ชปํ๋ค. ์ํผ๋ทฐ๊ฐ ๋ฐ๋์ ํ์ํ ํน์ฑ ์ internal change๋ฅผ ๋ฐ์ํ  ์ ์๊ธฐ ๋๋ฌธ์ด๋ค.

๊ทธ๋์ ๋ง์ง๋ง์ผ๋ก `auto layout`์ด ๋ฑ์ฅํ๋ค. ์๋ ๋ทฐ ์ฌ์ด์ ๊ด๊ณ๋ก ๋ฐฐ์น๋ฅผ ์ ์ํ๊ธฐ ๋๋ฌธ์ ์ด๋ฐ ๋ฌธ์ ๋ค์ ๋ค์ ํด๊ฒฐํ  ์ ์์๋ค. 

**cf.**

[https://developer.apple.com/documentation/appkit/nsview/1526961-translatesautoresizingmaskintoco](https://developer.apple.com/documentation/appkit/nsview/1526961-translatesautoresizingmaskintoco)

- ์ฝ๋ ๋ฒ ์ด์ค๋ก ๋ทฐ๋ฅผ ์ก์ ๋ ๊ฐํน ๋ณด์ด๋ `translatesAutoresizingMaskIntoConstraints`๋ ์คํ  ๋ฆฌ์ฌ์ด์ง ๋ง์คํฌ๋ฅผ constraint๋ก ๋ฐ๊ฟ์ฃผ๋ ๊ธฐ๋ฅ์ ์จ์คํ ํ  ์ ์๋ ํ๋๊ทธ์.
- ๊ทธ๋์ ์คํ  ๋ ์ด์์์ด๋ ๋ถ๋ชํ์ง ๋ง๋ผ๊ณ  ๊บผ์ฃผ๋ ์ฉ๋๋ก ์ฌ์ฉ๋๊ธฐ๋ ํจ.

## ๊ทธ๋์ Constraint๊ฐ ๋ฌด์

<aside>
๐ก from [Guide](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW1)
**The layout of your view hierarchy is defined as a series of linear equations.** Each constraint represents a single equation. Your goal is to declare a series of equations that has one and only one possible solution.

</aside>

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%203.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%204.png)

- ๊ฝค ์ง๊ด์ ์ธ ํธ์ธ๋ฐ ์ ์์์ ๊ฒฝ์ฐ ๋ ๋ ๋ทฐ์ ๋ฆฌ๋ฉ์ด ๋ธ๋ฃจ ๋ทฐ์ ํธ๋ ์ผ๋ง์ 1๋ฐฐ์๋ณด๋ค 8 ๋จ์ด์ ธ์๋ค๋ ์๋ฏธ๋ค.
- ์ด ๋ฐฉ์ ์์ ์์์ ๋ณด๋ฏ ์ฌ๋ฌ๊ฐ์ ํํธ๋ก ์ด๋ฃจ์ด์ ธ์๋ค

## Constraint์๋ Autolayout

- ๋ ์๋ค.
- Stack view๋ฅผ ์ฐ๋ฉด ํ  ์ ์๋๋ฐ ๋งค์ฐ ๊ถ์ฅ๋๋ ๋ฐฉ์์ด๋ผ๊ณ  ํ๋๋ผ.

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%205.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%206.png)

## Auto layout ์ ์ฉ ๊ณผ์ 

![https://velog.velcdn.com/images/enebin777/post/885a0501-4fe6-4d16-9272-74f551b568ca/image.png](https://velog.velcdn.com/images/enebin777/post/885a0501-4fe6-4d16-9272-74f551b568ca/image.png)

![https://velog.velcdn.com/images/enebin777/post/01daad1b-370f-402e-beaf-609881ee0261/image.png](https://velog.velcdn.com/images/enebin777/post/01daad1b-370f-402e-beaf-609881ee0261/image.png)

- iOS๊ฐ ํ๋ฉด์ ์๋ฐ์ดํธํ๊ธฐ ์ํด ์ผ๋ฐ๊ธฐ๊ธฐ์ ๊ฒฝ์ฐ 1์ด์ 60๋ฒ(60Hz), Promotion์ ์ง์ํ๋ ์ผ๋ถ ๊ธฐ๊ธฐ์ ๊ฒฝ์ฐ 1์ด์ 120๋ฒ(120Hz) ์๋ฐ์ดํธ ๋๋ค.
- ์ด ๋ฃจํ๋ 3ํ์ด์ฆ๋ฅผ ํตํด ์งํ๋๋ค.
    - Update constraints (์๋ก ์ ๋ฌ): ์๋ธ๋ทฐ์์๋ถํฐ Contraint๋ฅผ ์๋ฐ์ดํธ ํ๊ณ  ๋ ์ด์์์ ๊ณ์ฐ
    - Layout (์๋๋ก ํ๋ฌ): ์ํผ๋ทฐ์์๋ถํฐ ์ด์  ๋จ๊ณ์ ๋ ์ด์์ ๊ฐ์ ๋ทฐ์ ์๋ ฅ
    - Display (์๋๋ก ํ๋ฌ): ์ํผ๋ทฐ์์๋ถํฐ ๋ ์ด์์ bounds ๋ด์์ ๋ทฐ์ ์ฌ์ฉ์ ์ธํฐํ์ด์ค๋ฅผ ๋ ๋๋ง

ํ๋ ์์์ด๋ค.

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%207.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%208.png)

## ๋นํจ์จ์ ์ธ ์์

![https://velog.velcdn.com/images/enebin777/post/87c14d5c-0397-44f7-969f-93fbedba3bb5/image.png](https://velog.velcdn.com/images/enebin777/post/87c14d5c-0397-44f7-969f-93fbedba3bb5/image.png)

- ์ฝ๋์์ ์๋ฐ์ดํธ ์์ ๋ง๋ค ๋ชจ๋  const๋ฅผ ๋นํ์ฑํํ๊ณ  ์ญ์ ํ๊ณ  ์๋ ๊ฒ์ ๋ณผ ์ ์๋ค.
- ์๋ ์๋์ ๊ฐ์ ์ง์ด๋ค

![https://velog.velcdn.com/images/enebin777/post/e6f26c23-db3f-4888-a6f9-c6f310fbc3c2/image.png](https://velog.velcdn.com/images/enebin777/post/e6f26c23-db3f-4888-a6f9-c6f310fbc3c2/image.png)

- const๋ฅผ ์ง์ฐ๋ฉด ๋ ๋ ์์ง ์์ฅ์์ ์ฒ์๋ถํฐ ๋ชจ๋  ๊ณ์ฐ์ ์ฌ์คํํด์ผ ํ๋ ์ถ๊ฐ ํ์คํฌ๊ฐ ์๊ธด๋ค.
- ์ด๋ ํผํฌ๋จผ์ค์ ์ข์ง ์๋ค

ํ ๋ฒ๋ง ํ  ์ ์๊ฒ ์ด๋ ๊ฒ ๊ณ ์ณ๋ผ.

![https://velog.velcdn.com/images/enebin777/post/65c4541b-0123-49b6-800a-9aa35c3e2531/image.png](https://velog.velcdn.com/images/enebin777/post/65c4541b-0123-49b6-800a-9aa35c3e2531/image.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%209.png)

```swift
var myConstraints: [NSLayoutConstraints] = []

override func updateConstraints() {
    // ๋ถํ์ํ๊ฒ ๋ชจ๋  constraints๋ฅผ deactivate/activate ํ์ง์๋๋ก ํ ๋ฒ ์ด์์ ์์์ ์ํํ์ง ์๋๋กํ๋ค.

    if myConstraints.isEmpty {

        NSLayoutConstraint.deactivate(myConstraints)
        myConstraints.removeAll()

        let views = ["text1":text1, "text2":text2]
        myConstraints += NSLayoutConstraint.constraints(withVisualFormat: "H:|-[text1]-[text2]",
                                                        options: [.alignAllFirstBaseline],
                                                        metrics: nil, views: views)
        myConstraints += NSLayoutConstraint.constraints(withVisualFormat: "V:|-[text1]-|",
                                                        options: [],
                                                        metrics: nil, views: views)

        NSLayoutConstraint.activate(myConstraints)
    }
    super.updateConstraints()
}
```

***์ ์์ ์ดํ์ ๋ฒ์ด์ง ์ผ๋ค***

![https://velog.velcdn.com/images/enebin777/post/fb838488-df97-4780-a450-89046080ffa2/image.png](https://velog.velcdn.com/images/enebin777/post/fb838488-df97-4780-a450-89046080ffa2/image.png)

- ๋ ๋๋ฃจํ์์ ์ด๋ฐ ์ผ๋ค์ด ์ผ์ด๋๋๋ฐ ๋ ๋๋ฃจํ ๋จผ์  ์์๋ณด์.

## ๋ ๋ ๋ฃจํ

### Refs

[Run Loops](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2010.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2011.png)

- ๋ชจ๋  ์ ์  ์ํธ์์ฉ์ OS๋ฅผ ๊ฑฐ์ณ event queue์ ์ถ๊ฐ๋๋ค. (์ ๊ทธ๋ฆผ ์ฐธ์กฐ)
- evene queue์ ์๋ ์ด๋ฒคํธ๋ค์ด ํ๋์ฉ ๊บผ๋ด์ ธ์ ์ดํ๋ฆฌ์ผ์ด์ ๊ฐ์ฒด์ ์ ๋ฌ๋๊ณ  ์ดํ๋ฆฌ์ผ์ด์ ๊ฐ์ฒด์ ์ฝ์ด ์ค๋ธ์ ํธ๊ฐ ํด๋น ์ด๋ฒคํธ์ ํด๋นํ๋ ์ด๋ฒคํธ ํธ๋ค๋ฌ๋ฅผ ํธ์ถํ๋ค.
    - Core obect๊ฐ ๋ญ์ง ์ฐพ์๋ดค๋๋ฐ ๊ทธ๋ฅ UIView, UIViewController์ฒ๋ผ UIKit์์ ์ ๊ณตํ๋ ์ปดํฌ๋ํธ๋ฅผ ํ์ณ์ ๋งํ๋ ๋ฏ.([Core Objects of iOS App | Park's Park (wordpress.com)](https://soulpark.wordpress.com/2012/06/14/core-objects-of-ios-app/))
- ํ์ฌํผ ๋ฐ๋ฃจํ์ ๋ชจ๋  ํธ๋ค๋ฌ๊ฐ ๋ฆฌํด๋๋ฉด ์ด์  ์๋ฐ์ดํธ ์ฌ์ดํด์ ๊ฐ๋๋ค. ์ฌ๊ธฐ์ ๋ทฐ๊ฐ ์๋ฐ์ดํธ ๋๋ค.(constraint, layout, display)

## Activating Constraint

![https://velog.velcdn.com/images/enebin777/post/76e19e77-c1c3-46d9-9b0c-47ac371d2379/image.png](https://velog.velcdn.com/images/enebin777/post/76e19e77-c1c3-46d9-9b0c-47ac371d2379/image.png)

![https://velog.velcdn.com/images/enebin777/post/09542c79-ed40-475d-a0df-a2c3a5c9a2bb/image.png](https://velog.velcdn.com/images/enebin777/post/09542c79-ed40-475d-a0df-a2c3a5c9a2bb/image.png)

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2012.png)

![https://velog.velcdn.com/images/enebin777/post/6882a31b-f201-47ee-8f7d-1f922a780e3b/image.png](https://velog.velcdn.com/images/enebin777/post/6882a31b-f201-47ee-8f7d-1f922a780e3b/image.png)

- const๊ฐ ํ์ฑํ ๋๋ฉด ์์ง์์ ๊ฐ const์ ๊ด๊ณ๋ฅผ ํตํด ๋ฐฐ์น๋ฅผ ๊ณ์ฐํ๋ค.
- ์์ ์์์์ ์๋ก ๋ค๋ฅธ ๋ณ์์ ๊ด๊ณ๋ฅผ ๋ถ์ํด `test2.width`๋ฅผ ๋์ถํด๋ด๋ ๊ฒ์ ๋ณผ ์ ์๋ค

![https://velog.velcdn.com/images/enebin777/post/1dd5bb33-9547-4010-a548-f6c8278a2950/image.png](https://velog.velcdn.com/images/enebin777/post/1dd5bb33-9547-4010-a548-f6c8278a2950/image.png)

- ๋ณ์์ ๊ฐ์ด ์จ์ง๋ฉด(๋ณ๊ฒฝ์ ๊ฐ์งํ๋ฉด) ์์ง์ ์ํผ๋ทฐ์ ๋ธํฐ๋ฅผ ๋๋ฆฐ๋ค.
- ๊ทธ ์ดํ ๋ทฐ์์ ์คํ๋๋ ๋ฉ์๋๊ฐ ๋ฐ๋ก `setNeedsLayout()`์ด๋ค.

![https://velog.velcdn.com/images/enebin777/post/270a8c61-8be2-47ec-9720-47b15d636d0e/image.png](https://velog.velcdn.com/images/enebin777/post/270a8c61-8be2-47ec-9720-47b15d636d0e/image.png)

- ์ด๋ฐ ๊ณผ์ ์ ๊ฑฐ์น ๋ค

![https://velog.velcdn.com/images/enebin777/post/960f9d8a-2756-4a09-a0a2-db9e739804c0/image.png](https://velog.velcdn.com/images/enebin777/post/960f9d8a-2756-4a09-a0a2-db9e739804c0/image.png)

- ์ํผ๋ทฐ๋ ์์งํํ ํ์ํ ๊ฐ์ ๋ฌผ์ด๋ณธ ๋ค ์นดํผํด์ ์์ง์์ ๊ฐ์ ธ์จ๋ค.
- ๊ทธ ๊ฐ์ ์ด์ฉํด์ `setBounds`๋ `setCenter`๋ฑ์ ๋ฉ์๋๋ก ์๋ธ๋ทฐ๋ฅผ ๋ค์ ๋ ์ด์์ ์ํด.
    - ๊ฒ์ํด๋ ์ ๋์ค๋๋ฐ ๊ทธ๋ฅ ๊ฐ๋ ์ค๋ช์ ์ํด ์ธ๊ธํ ๊ฑด๊ฐ ์ถ๋ค

![https://velog.velcdn.com/images/enebin777/post/c1896bfb-192d-4ec3-a342-c149a3b42893/image.png](https://velog.velcdn.com/images/enebin777/post/c1896bfb-192d-4ec3-a342-c149a3b42893/image.png)

- ๋ ์ด์์์ 2๋ฐฐ๋ก ๋๋ ค๋ณด์. ์ด๋ฐ ์ผ์ด์ค๋ ์กฐ๊ธ ๊น๋ค๋ก์ธ ์ ์์.
- ์๋ก๋ค๋ฅธ ๋ ๋ทฐ ์ฌ์ด์ ๋ ์ด์์์ด ๋ค์ด๊ฐ๊ธฐ ๋๋ฌธ์.

![https://velog.velcdn.com/images/enebin777/post/aa3d6e7f-4b6c-4e7a-95c7-4c29c1e5daf5/image.png](https://velog.velcdn.com/images/enebin777/post/aa3d6e7f-4b6c-4e7a-95c7-4c29c1e5daf5/image.png)

![https://velog.velcdn.com/images/enebin777/post/8bd1cf15-4c70-4297-a266-9c7768a773c9/image.png](https://velog.velcdn.com/images/enebin777/post/8bd1cf15-4c70-4297-a266-9c7768a773c9/image.png)

- ์๋ก ๋ค๋ฅธ ๋ ๋ทฐ์ ๋ํ ๋ ์ด์์์ ์์ง ์์์ ์์ ํ ๋ณ๊ฐ์ ๋ธ๋ก์ผ๋ก ๊ณ์ฐ๋๋ค.
- 2๊ฐ์ ๋ณ๊ฐ์ ๋ธ๋ก์ด๋ฏ๋ก ์๋ก ์ํฅ์ ๋ฏธ์น์ง ์๊ณ 
- ๊ทธ๋ฌ๋ฏ๋ก ๋ก๋๋ ๊ทธ๋ฅ ๋ง์์ด ๋์ด ์ ํ์ฆ๊ฐํ๋ค. (์๋ก ์ํฅ์ด ์์ผ๋ฏ๋ก ์กฐํฉ, ๊ณฑ์ ์ด๋ฐ๊ฑฐ ์์)
- *๊ฒฐ๊ตญ ์ฑ๋ฅ ์๋์ด์์*

## About Engine

![https://velog.velcdn.com/images/enebin777/post/708586d6-e7a1-4609-9e82-ea9b0d047504/image.png](https://velog.velcdn.com/images/enebin777/post/708586d6-e7a1-4609-9e82-ea9b0d047504/image.png)

- ์์ง์ ํ ์ํ๋ฅผ ๊ธฐ์ตํ๊ณ  ๋ฐ๋ ์์น๋ง ์ ํํ๊ฒ ๋ณ๊ฒฝํ๋ค

![https://velog.velcdn.com/images/enebin777/post/bb4f0a3c-c8ac-4dec-a5fd-99f83cc716f2/image.png](https://velog.velcdn.com/images/enebin777/post/bb4f0a3c-c8ac-4dec-a5fd-99f83cc716f2/image.png)

![https://velog.velcdn.com/images/enebin777/post/4907ddf4-9244-4e7d-af88-43404cc1ad05/image.png](https://velog.velcdn.com/images/enebin777/post/4907ddf4-9244-4e7d-af88-43404cc1ad05/image.png)

![https://velog.velcdn.com/images/enebin777/post/e3246342-26d1-4e30-9495-a3fb6a017281/image.png](https://velog.velcdn.com/images/enebin777/post/e3246342-26d1-4e30-9495-a3fb6a017281/image.png)

- ์ ์ข์ constraints(๋ ๋ฒ์งธ) โ ๊ฐ์ง ๋ํ๋์๋ฅผ ๋ง๋ค์ด๋ธ๋ค
- ๋ ์ด์์์ ๋ช์์ ์ผ๋ก ๋ถ๋ฆฌ, ๊ตฌํํ  ๊ฒ

## Optimize auto layout

![https://velog.velcdn.com/images/enebin777/post/88c09ca6-a937-4b0d-b3ae-89649bd2f3fe/image.png](https://velog.velcdn.com/images/enebin777/post/88c09ca6-a937-4b0d-b3ae-89649bd2f3fe/image.png)

- ์๋๋ก ๊ฐ์๋ก ๋น์ผ ์ฐ์ฐ์ด๋ค
    1. ๋ถ๋ฑํธ : ์์ฃผ ์ ๋ ดํ๋ค. 
    2. Set constants: 
        - `GestureRecognizer`๋ฅผ ์๋ก ๋ค๋ฉด ์์ง์ผ ๋๋ง๋ค translation value๋ฅผ ์ด์ฉํด set constants๋ฅผ ๋ถ๋ฌ์ฃผ๊ณ  ๊ทธ๋ฌ๋ฉด ์์ง์์ ์์์ ์ ์๋ฐ์ดํธ ํ  ๊ฒ.
        - ์์ง์ ๋ ์ด์์์ ์บ์๋ก ๋์ํ๊ธฐ ๋๋ฌธ์ด๋ค
        - ์ด๋ ๋งค์ฐ ๋น ๋ฅธ one-step ์๋ฐ์ดํธ์
    3. ์ฐ์ ์์
        
        ![https://velog.velcdn.com/images/enebin777/post/a2dc3b47-6905-4770-a01f-ceaaba47d7e6/image.png](https://velog.velcdn.com/images/enebin777/post/a2dc3b47-6905-4770-a01f-ceaaba47d7e6/image.png)
        
        - ๋๋น๊ฐ 100์ธ๋ฐ ๋ค๋ฅธ ํ์ธ ๋๋ฌธ์ 100์ ๋๋น๋ฅผ ์จ์ ํ ๋ชป ๊ฐ์ ธ๊ฐ๋ฉด?
        - ์๋ก์ ๊ด๊ณ์ ์ฐ์ ์์๋ฅผ ๊ณ ๋ คํ ์ถ๊ฐ ์์์ด ํ์ํจ
        - ์ด ๊ฒฝ์ฐ simplex algorithm์ ์ด์ฉํด์ ์ค๋ฅ๋ฅผ ์ต์ํํ ๊ฐ ๋ฐํํ๋ค(**ํ์  ์์**)
    

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2013.png)

**๊ฐ๋จํ ์ต์ ํ 4์์น**

- ํด์  ํ ๋น ๋ฐ๋ณตํ์ง ๋ง๋ผ
- ๋ถ๋ฑํธ, ๋ง์, setConstants๋ ์จ๋ผ
- ์์ง์ ๋ ์ด์์์ ์บ์๋ก ๋์
- ๋ด๊ฐ๋ฉด์ ์จ๋ผ

## Intrinsic Content Size

![Untitled](UIKit%20Render%20b5d6604ef1ec473f8dd3d1dbbed5f18a/Untitled%2014.png)

---

# Interface builder

---

### ์ ๊น์์

### Window vs View

- [https://developer.apple.com/documentation/uikit/uiwindow](https://developer.apple.com/documentation/uikit/uiwindow)
- [https://developer.apple.com/documentation/uikit/view_controllers/managing_content_in_your_app_s_windows](https://developer.apple.com/documentation/uikit/view_controllers/managing_content_in_your_app_s_windows)

![https://velog.velcdn.com/images/enebin777/post/2c7e51a6-7d66-4ac2-ac1a-49fe37f51411/image.png](https://velog.velcdn.com/images/enebin777/post/2c7e51a6-7d66-4ac2-ac1a-49fe37f51411/image.png)

![https://velog.velcdn.com/images/enebin777/post/72a71c27-4a57-4c3b-9299-9bfb8cad29e7/image.png](https://velog.velcdn.com/images/enebin777/post/72a71c27-4a57-4c3b-9299-9bfb8cad29e7/image.png)

> **Each scene in your appโs UI contains a window object and one or more view objects.** The window serves as an invisible container for the rest of your UI, acts as a top-level container for your views, and routes events to them. The views provide the actual content that users see onscreen, drawing text, images, and other types of custom content. Windows are long-lived objects, and you dismiss them only when tearing down your sceneโs entire UI. By contrast, you might change the views in that window frequently, particularly when you want to display new content or information.
> 

Window๋ rootViewController๋ก ๋๋ณ๋  ์ ์๋ ์ต์๋จ์ ๋ทฐ๋ผ๊ณ  ์๊ฐํ๋ฉด ๋๋ค. **ํ๋ง๋๋ก ์์ฝํ๋ฉด View hierarchy์ ์ต์๋จ์ ์๋ ๊ฐ์ฒด์ธ ๊ฒ.** ์๋์ฐ๋ ๋์ ๋ณด์ด๋ ๋ฐ๋ก ๊ทธ 'ํ๋ฉด'์ด๊ณ  ๋ทฐ๋ค์ ๊ทธ ์์ ์น๊ธฐ์ข๊ธฐ ๋ชจ์ฌ ์ฌ๋ ์ฃผ๋ฏผ์ธ ์์ด๋ค.

๋ฐ๋ผ์ ์ ์ผํ ์๋์ฐ ์๋์๋ ๋ง์ ์์ View๋ค์ด ์กด์ฌํ  ์์์ผ๋ฉฐ UIKit์ `UIViewController`๋ผ๋ ๊ฐ์ฒด๋ฅผ ํตํด ์๋์ฐ์ ์ํตํ๋ฉฐ ์ด๋ค์ ์ ์ดํ  ์ ์๊ฒ ๋์์ค๋ค.

์๋๋ AppDelegate์์ ์๋์ฐ๋ฅผ ๋ง๋ค์ด์คฌ์ง๋ง ์์ดํจ๋๋ฅผ ์์ํ ๋ค์คํ๋ฉด ๊ธฐ๋ฅ์ด ๋ฑ์ฅํ๋ฉด์ SceneDelegate์์ ์ฌ๋ฌ๊ฐ์ ์๋์ฐ๋ฅผ ๋ง๋ค ์ ์๊ฒ ๋์๋ค.

### Bound vs Frame

- [https://suragch.medium.com/frame-vs-bounds-in-ios-107990ad53ee](https://suragch.medium.com/frame-vs-bounds-in-ios-107990ad53ee)

์ ๊ธ์ ๋ณด๋ฉด ์ ์ค๋ช์ด ๋์ด์๋๋ฐ Bound๋ ๋ทฐ ๋ด๋ถ์ ์ขํ๊ณ ์ค ์ด๋ ๋ถ๋ถ์์ ์์ํ ์ง๋ฅผ ๊ณ ๋ฅด๋ ๊ฒ, Frame์ ๋ทฐ ์ธ๋ถ(super view)์ ์ขํ๊ณ ์ค ์ด๋ ๋ถ๋ถ์์ ์์ํ ์ง๋ฅผ ๊ณ ๋ฅด๋ ๊ฒ์ด๋ค. ํฌ๊ธฐ๋ ๋ญ ๊ฐ์ ๋ป์ด๋ค. ๋ง๋กํ๋ฉด ์ ์ ์๋ฟ๋๋ฐ ์ ๋ธ๋ก๊ทธ์์ ์ฌ์ง ์ค๋ช ๋ณด๋ฉด ๋ฐ๋ก ์ดํด ๋๋ค.

Frame์ด ๋ณํ๋ค๊ณ  bound๊ฐ ๋ณํ์ง ์๋๋ค.
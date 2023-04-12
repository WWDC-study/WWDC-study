# Intrinsic content size

뷰 자체가 갖는 고유한 크기

## Intrinsic content size

---

### **대개 contraint를 사용하여 뷰의 위치와 크기를 정의합니다**

그러나 일부 뷰는 별도 설정 없이도 자체적인 크기를 갖습니다. 이를 Intrinsic content size라고 합니다. 예를 들어 버튼의 Intrinsic content size는 제목의 크기에 약간의 여백을 더한 크기입니다.

### **모든 뷰에 고유 콘텐츠 크기가 있는 것은 아닙니다.**

- 고유 콘텐츠 크기가 있는 뷰의 경우 Intrinsic content size는 뷰의 높이, 너비 또는 둘 다를 정의할 수 있습니다. 몇 가지 예시가 표에 나와 있습니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1805106-fb4f-4d68-9ad0-747bee59c887/Untitled.png)

### **Intrinsic content size는 뷰의 현재 콘텐츠를 기반으로 합니다.**

- 레이블 또는 버튼의 크기는 표시되는 텍스트의 양과 사용된 글꼴을 기반으로 합니다.
- 다른 뷰의 경우 크기는 훨씬 더 복잡합니다. 예를 들어 빈 이미지 뷰는 크기가 없습니다. 하지만 이미지를 추가하는 즉시 Intrinsic content size가 이미지의 크기로 설정됩니다.
- 텍스트 뷰의 크기는 콘텐츠, 스크롤이 활성화되어 있는지 여부 및 뷰에 적용된 다른 constraint에 따라 달라집니다.
    - 예를 들어 스크롤이 활성화된 경우 Intrinsic content size가 없습니다.
    - 스크롤을 사용하지 않도록 설정하면 기본적으로 뷰의 Intrinsic content size는 줄 바꿈 없는 텍스트의 크기를 기준으로 계산됩니다.
    - 예를 들어 텍스트에 리턴이 없는 경우 콘텐츠를 한 줄의 텍스트로 레이아웃하는 데 필요한 높이와 너비를 계산합니다.
    - Constraint를 추가하여 뷰의 너비를 지정하는 경우 Intrinsic content size는 너비가 지정된 텍스트를 표시하는 데 필요한 높이를 정의합니다.

### **오토 레이아웃은 각 차원에 대해 한 쌍의 constraint를 사용해 뷰의 Intrinsic content size를 나타냅니다.**

![https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/intrinsic_content_size_2x.png](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/intrinsic_content_size_2x.png)

- 콘텐츠 허깅은 뷰를 안쪽으로 당겨서 콘텐츠 주위에 꼭 맞도록 합니다.
- 컴프레션 저항은 뷰를 바깥쪽으로 밀어내어 콘텐츠가 잘리지 않도록 합니다.
- 이러한 제약 조건은 <목록>에 표시된 부등식을 사용하여 정의됩니다. 여기서 `IntrinsicHeight` 및 `IntrinsicWidth` 상수는 뷰의 Intrinsic content size에서 높이 및 너비 값을 나타냅니다.

```swift
// Compression Resistance
View.height >= 0.0 * NotAnAttribute + IntrinsicHeight
View.width >= 0.0 * NotAnAttribute + IntrinsicWidth
 
// Content Hugging
View.height <= 0.0 * NotAnAttribute + IntrinsicHeight
View.width <= 0.0 * NotAnAttribute + IntrinsicWidth
```

### **Constraint은 각각 고유한 우선순위를 가질 수 있습니다**

- 기본적으로 뷰는 콘텐츠 허깅에 250의 우선순위를 사용하고 컴프레션 저항에 750의 우선순위를 사용합니다.
- 따라서 뷰를 축소하는 것보다 뷰를 늘리는 것이 더 먼저고 이는 바람직한 동작입니다.
    - 예를 들어, 버튼을 원래 콘텐츠 크기보다 크게 늘리는 것과 비교해 크기가 축소되면 콘텐츠가 잘릴 수 있습니다.
    - 인터페이스 빌더는 때때로 이러한 우선 순위를 자체 수정하여 동일한 우선순위를 방지합니다.

<aside>
⚠️ **가능하면 레이아웃에서 Intrinsic content size를 사용하십시오.** 
이를 이용하면 뷰의 콘텐츠가 변경될 때 레이아웃을 동적으로 조정할 수 있습니다. 단, 이 경우 필요한 Constraint의 수를 줄일 수 있지만 뷰의 CHCR(콘텐츠 허깅 및 컴프레션 저항) 우선 순위를 관리해야 합니다.

</aside>

### **다음은 Intrinsic content size를 처리하기 위한 몇 가지 지침입니다**

- 공간을 채우기 위해 뷰를 늘릴 때 모든 뷰의 콘텐츠 허깅 우선 순위가 동일하면 레이아웃이 모호해집니다. 오토 레이아웃이 어떤 뷰를 늘려야 하는지 알 수 없기 때문입니다.
    - 일반적인 예로는 레이블과 텍스트 필드 쌍이 있습니다.
    - 일반적으로 레이블은 Intrinsic content size를 유지하면서 텍스트 필드를 늘려 여분의 공간을 채우기를 원합니다. 이를 보장하려면 텍스트 필드의 가로 콘텐츠 허깅 우선순위가 레이블보다 낮은지 확인해야 합니다.
    - 실제 이 예제는 매우 일반적이므로 인터페이스 빌더가 자동으로 처리해 모든 레이블의 콘텐츠 허깅 우선순위를 251로 설정합니다.
    - 코드 베이스로 레이아웃을 만드는 경우 콘텐츠 허깅 우선 순위를 직접 수정해야 합니다.
- 버튼이나 레이블과 같이 보이지 않는 배경이 있는 뷰가 Intrinsic content size를 초과해 늘어나면 예기치 않은 레이아웃이 발생할 수 있습니다.
- 베이스라인 constraint는 Intrinsic content size에 있는 뷰에서만 작동합니다. 뷰가 세로로 늘어나거나 압축되면 베이스라인 constraint을 따라 제대로 정렬되지 않습니다.
- 뷰에 필수 CHCR 우선 순위를 부여하지 마십시오.
    - 일반적으로 뷰의 크기가 잘못되어 충돌이 발생하는 것보다 뷰의 크기가 잘못된 것이 더 낫습니다.
    - 뷰가 항상 Intrinsic content size여야 하는 경우 대신 매우 높은 우선 순위(999)를 사용하는 것이 좋습니다.
    - 이 접근 방식은 일반적으로 뷰가 늘어나거나 압축되는 것을 방지하지만 뷰가 예상보다 크거나 작은 환경에 표시되는 경우의 대비책을 제공합니다.

## Intrinsic content size vs Fitting Size

---

**Intrinsic content size는 오토 레이아웃에 대한 입력으로 작동합니다.**

- 뷰에 Intrinsic content size가 있는 경우 시스템에서 해당 크기를 나타내는 constraint를 생성하고 이 것이 레이아웃을 계산하는 데 사용됩니다.

**반면에 피팅 사이즈는 오토 레이아웃 엔진의 출력입니다.** 

- 뷰의 제약 조건을 기반으로 계산된 크기입니다. 뷰가 자동 레이아웃을 사용하여 하위 뷰를 레이아웃하는 경우 시스템에서 뷰의 콘텐츠를 기반으로 뷰에 맞는 크기를 계산할 수 있습니다.

**스택 뷰가 좋은 예시입니다.** 

다른 constraint이 없으면 시스템은 스택 뷰의 콘텐츠 및 속성을 기반으로 스택 뷰의 크기를 계산합니다. 여러 가지 면에서 스택 뷰는 마치 Intrinsic content size가 있는 것처럼 작동합니다: 

- 하나의 수직 및 하나의 수평 constraint만으로 위치를 정의하여 유효한 레이아웃을 만들 수 있습니다.
- 그러나 그 크기는 오토 레이아웃에 의해 계산된 출력입니다. 오토 레이아웃에 대한 입력이 아닙니다.
- 따라서 스택 뷰에는 Intrinsic content size가 없으므로 스택 보기의 CHCR 우선 순위를 설정해도 아무런 영향을 미치지 않습니다.

스택 뷰 외부의 항목에 대한 스택 뷰의 크기를 조정해야 하는 경우 명시적 contraint을 만들어 이러한 관계를 캡처하거나 스택 외부 항목에 대한 스택 콘텐츠의 CHCR 우선 순위를 수정합니다.

## 정리

---

- Intrinsic content size는 뷰는 별도 설정 없이도 갖는 자체적인 크기를 의미합니다. 예를 들어 버튼의 Intrinsic content size는 제목의 크기에 약간의 여백을 더한 크기입니다.

## 참고

---

[Auto Layout Guide: Anatomy of a Constraint](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW21)

[intrinsicContentSize | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview/1622600-intrinsiccontentsize)
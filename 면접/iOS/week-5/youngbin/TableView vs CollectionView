# Table view vs Collection view

`UITableView`와 `UICollectionView`는 모두 스크롤 가능한 컨테이너에 일련의 데이터 리스트를 표시하기 위한 컴포넌트입니다. 

## 비교

---

### 레이아웃

`**UITableView**`

- 데이터를 단일 열의 세로 목록으로 표시합니다.
- 각 행은 단일 데이터 항목을 나타내며 하나 이상의 셀로 구성됩니다.
- 섹션의 헤더, 푸터를 지원하며 각 셀에 대한 Separator를 지원합니다.

간단한 목록과 계층적 데이터를 표시하는 데 이상적입니다.

`**UICollectionView**`

- 보다 유연한 레이아웃 시스템을 제공하며 여러 열과 행에 데이터를 표시할 수 있습니다.
- 커스텀 레이아웃을 지원해 Grid 혹은 Mansory 같은 복잡한 레이아웃을 만들 수 있습니다.

단순한 목록 이상의 기능이 필요하거나 시각적으로 풍부한 방식으로 데이터를 표시하려는 상황에 적합합니다.

### Cell

```swift
// ✅ Work
UITableViewCell().textLabel?.text = "brbr.."

// 🚫 Error
UICollectionViewCell().textLabel?.text = "brbr.."
```

`**UITableViewCel**`

- `UITableViewCell`은 단일한 열 리스트 레이아웃을 위해 설계되었습니다.
- 일반적으로 기본 텍스트 레이블과 디테일 텍스트 레이블, 이미지 보기 및 액세서리 뷰(옵셔널)가 포함된 단순한 구조를 갖습니다.
- 이러한 요소의 레이아웃은 미리 정의되어 있습니다. 텍스트 레이블과 이미지 뷰는 왼쪽에, 액세서리 뷰는 오른쪽에 위치합니다.
- 이러한 기본 제공 요소의 모양을 커스텀하거나 서브뷰를 추가하여 **단일한 열 리스트 레이아웃 한도 내**에서 더 복잡한 레이아웃을 만들 수 있습니다.

`**UICollectionViewCell`** 

- `UICollectionViewCell`은 보다 복잡하고 유연한 레이아웃을 위해 설계되었습니다.
- `UITableViewCell`과 달리 텍스트 레이블, 이미지 뷰 또는 액세서리 뷰가 내장되어 있지 않습니다.
- 대신 `UICollectionViewCell`은 빈 캔버스 역할을 합니다. 그러므로 필요에 따라 사용자 지정 서브뷰를 추가하고 셀의 레이아웃을 디자인할 수 있습니다.

## 정리

---

- **공통적으로,** `UITableView`와 `UICollectionView`는 모두 데이터 목록을 표시하는 강력한 컴포넌트입니다.
- **다른 점으로,** `UITableView`는 단순한 단일 열 목록 및 계층적 데이터에 적합한 반면, `UICollectionView`는 레이아웃 및 사용자 지정에 더 큰 유연성을 제공합니다. 그러므로 복잡하고 시각적으로 풍부한 프레젠테이션에 더 적합합니다.

## 참고
[When to use UICollectionView instead of UITableView?](https://stackoverflow.com/questions/23078847/when-to-use-uicollectionview-instead-of-uitableview)
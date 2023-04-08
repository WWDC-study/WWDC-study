# TableView & DataSource

better on : [TableView & DataSource](https://www.notion.so/TableView-DataSource-1580a32e83bc446b9186747a9d29b1f2) 

`UITableView`는 단일 열(컬럼)에 여러개의 행을 사용해 데이터를 나타내는 뷰 입니다

## UITableView

---

![https://docs-assets.developer.apple.com/published/6ac735d0f5/3148900~dark@2x.png](https://docs-assets.developer.apple.com/published/6ac735d0f5/3148900~dark@2x.png)

iOS의 테이블 뷰는 세로로 스크롤할 수 있는 콘텐츠 행을 단일 열에 표시합니다. 표의 각 행에는 앱의 콘텐츠가 하나씩 포함됩니다. 

- 예를 들어 연락처 앱에는 연락처의 이름에 해당하는 부분이 각 행에 표시되고 설정 앱에서는 사용 가능한 설정의 그룹이 표시됩니다.
- 단일한 행 리스트를 표시하도록 테이블을 구성하거나 관련된 행을 `섹션`으로 그룹화할 수 있습니다.

### 셀

`UITableView`는 테이블의 생김새를 관리하지만, 실제 콘텐츠는 앱에서 제공하는 셀(`UITableViewCell` 객체)에서 표시합니다. 

- 기본 셀은 텍스트와 이미지의 간단한 조합을 표시하지만 원하는 콘텐츠를 표시하는 사용자 지정 셀을 정의할 수 있습니다.
- 헤더 및 푸터를 제공해 셀 그룹에 대한 추가 정보를 제공할 수도 있습니다.

### 상태 저장

`UITableView`는 UIKit 앱 restoration을 지원합니다. 

- 테이블의 데이터를 저장하고 복원하려면 테이블 보기의 `restoreIdentifier` 속성에 값을 할당합니다.
- 상위 뷰 컨트롤러를 저장하면 테이블 뷰가 현재 선택 및 표시된 행의 `IndexPath`를 자동으로 저장합니다.
- 테이블의 데이터 소스 개체가 `UIDataSourceModelAssociation` 프로토콜을 채택하는 경우 테이블은 `IndexPath` 대신 해당 항목에 대해 사용자가 제공한 고유한 ID를 저장합니다.

## UITableViewDatasources

---

테이블 뷰는 데이터의 표현만 관리하며 데이터 자체는 관리하지 않습니다. 데이터를 관리하려면 테이블에 데이터 소스 객체(`UITableViewDataSource` 프로토콜을 구현하는 객체)를 제공합니다. 

- 데이터 소스 개체는 테이블의 데이터 관련 요청에 응답합니다. 또한 테이블의 데이터를 직접 관리하거나 앱의 다른 부분과 상호작용하며 해당 데이터를 관리합니다.

### 기능

데이터 소스 개체의 책임은 다음과 같습니다:

- 테이블의 섹션 및 행 수 제공
- 표의 각 행에 대한 셀 제공
- 섹션 헤더 및 푸터 타이틀 제공
- 테이블의 인덱스(있는 경우) 구성
- 데이터 변경이 필요한 업데이트에 응답합니다. 업데이트는 사용자 혹은 테이블 자체에 의해 실행될 수 있습니다.

### 구현

이 프로토콜은 두 가지 메서드를 반드시 구현해야 합니다:

```swift
// Return the number of rows for the table.     
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
   return 0
}

// Provide a cell object for each row.
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Fetch a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "cellTypeIdentifier", for: indexPath)
   
   // Configure the cell’s contents.
   cell.textLabel!.text = "Cell text"
       
   return cell
}
```

### 위치 얻기

![https://docs-assets.developer.apple.com/published/b6d7a0587b/row-section-indexes~dark@2x.png](https://docs-assets.developer.apple.com/published/b6d7a0587b/row-section-indexes~dark@2x.png)

테이블 뷰는 `NSIndexPath` 개체의 행 및 섹션 속성을 사용하여 셀의 위치를 사용자에게 전달합니다.

- 행 및 섹션 인덱스는 0을 기준으로 시작합니다.
- 마찬가지로 각 섹션 인덱스 역시 0을 기준으로 시작합니다. 따라서 행을 고유하게 식별하려면 섹션과 행 값이 모두 필요합니다.
- 테이블에 섹션이 없는 경우 행 값만 필요합니다.

## 정리

---

- 테이블 뷰는 세로로 스크롤할 수 있는 콘텐츠 행을 단일 열에 표시합니다. 표의 각 행에는 앱의 콘텐츠를 하나씩 포함하며 아이템을 표시하는 것은 셀이 담당합니다.
- 테이블 뷰는 뷰를 표현하는 것만 담당하며 데이터를 제공하는 것은 데이터소스가 담당합니다.
- 데이터 소스는 `numberOfRowsInSection`, `cellForRowAt` 두 가지 메서드를 반드시 구현해야 합니다.

## 참고

---

[UITableView | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableview)

[UITableViewDataSource | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableviewdatasource)

[https://www.notion.so/enebin/TableView-DataSource-1580a32e83bc446b9186747a9d29b1f2?pvs=4](https://www.notion.so/TableView-DataSource-1580a32e83bc446b9186747a9d29b1f2)
# Prepare for reuse
better on : [prepareForReuse](https://www.notion.so/prepareForReuse-cfa1571b8ca141fdb9a0d64789066178) 

테이블 뷰 델리게이트가 재사용할 수 있도록 재사용 가능한(reusable) 셀을 준비합니다

## prepareForReuse

---

`UITableViewCell` 객체에 재사용 식별자가 있는 경우 테이블 뷰는 `UITableView` 메서드에서 객체를 반환하기 직전에 이 메서드 `dequeueReusableCell(withIdentifier:)`을 호출합니다. 

- 잠재적인 성능 문제를 방지하려면 콘텐츠와 관련이 없는 **셀의 속성(예: 알파, 편집 및 선택 상태)만 재설정해야 합니다**. 셀을 재사용할 때 테이블 뷰 델리게이트의 `tableView(_:cellForRowAt:)`에서 반드시 셀 콘텐츠를 초기화해야 합니다.
- 셀 개체에 연결된 재사용 식별자가 없거나 기존 셀의 내용을 업데이트하기 위해 `reconfigureRows(at:)`를 사용하는 경우 테이블 뷰는 `prepareForReuse` 메서드를 호출하지 않습니다.
- 이 메서드를 `override`하는 경우 반드시 수퍼클래스 메서드를 호출해야 합니다.

## ****reconfigureRows(at:)****

---

지정한 인덱스 경로에서 기존 셀을 유지하면서 행의 데이터를 업데이트합니다.

### 개요

- 새 셀로 대체하지 않고 기존(미리 가져온 셀 포함) 셀의 내용을 업데이트하려면 `reloadRows(at:with:)` 대신 이 메서드를 사용합니다.
- 최적의 성능을 위해 기존 셀을 명시적으로 새 셀로 교체해야 하는 경우가 아니라면 행을 다시 로드하는 대신 행을 `reconfigureRows`****를**** 선택하세요.

### 동작

셀 프로바이더는 제공된 인덱스 경로에 대해 동일한 유형의 셀을 dequeue해야 하며, 주어진 `indexPath`에 대해 동일한 기존의 셀을 반환해야 합니다. 

- 이 메서드는 기존 셀을 재구성하기 때문에 테이블 뷰는 각 셀에 대해 `prepareForReuse()`를 호출하지 않습니다.
- `indexPath`에 대해 다른 유형의 셀을 반환해야 하는 경우 대신 `reloadRows(at:with:)`를 사용하십시오.
- 셀 크기가 조정되는 경우 테이블 뷰는 셀을 `reconfigure`한 후 셀 크기를 조정합니다.

### 기타

- 기본적으로 테이블 뷰는 `reconfigure`의 결과로 발생하는 모든 크기 또는 레이아웃 변경에 애니메이션을 적용합니다. 애니메이션 없이 셀을 재구성하려면 이 메서드를 호출할 때 `UIView`의 `performWithoutAnimation(:)`을 사용합니다*.*
- 테이블 뷰에서 커스텀 `UITableViewDataSource` 구현을 사용하는 경우 이 메서드를 사용합니다?
- 테이블 뷰에서 `diffableDataSource`를 사용하는 경우 대신 `NSDiffableDataSourceSnapshot`에서 `reconfigureItems(_:)`를 사용하세요.

## 예시

---

### `prepareForReuse`(Old version)

```swift
class ViewController: UIViewController {
    var number = 0
    
    var tableView: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        
        tableView = UITableView(frame: view.bounds)
        tableView.delegate = self
        tableView.dataSource = self
        tableView.register(CustomCell.self, forCellReuseIdentifier: "CustomCell")
        
        view.addSubview(tableView)
    }
}

extension ViewController: UITableViewDelegate {
    
}

extension ViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        50
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "CustomCell", for: indexPath) as? CustomCell else {
            return UITableViewCell()
        }
        
        cell.textLabel?.text = "ddddd"
        
        return cell
    }
    
    
}

class CustomCell: UITableViewCell {
    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        self.backgroundColor = .red
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func prepareForReuse() {
        super.prepareForReuse()
        
        print("Prepare")
        self.backgroundColor = .gray
    }
}
```

[http://www.giphy.com/gifs/z3wUvJggxADnap8CMn](http://www.giphy.com/gifs/z3wUvJggxADnap8CMn)

### ****`reconfigureRows(at:)`(New version)**

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    guard let cell = tableView.dequeueReusableCell(withIdentifier: "CustomCell", for: indexPath) as? CustomCell else {
        return UITableViewCell()
    }
    
    cell.textLabel?.text = "ddddd"
    cell.backgroundColor = .gray

    tableView.reconfigureRows(at: [indexPath])
    return cell
}
```

## 정리

---

- `prepareForReuse`: 테이블 뷰에서 재사용 가능한(reusable) 셀을 초기화하기 위해 호출되는 메서드이며, 콘텐츠와 관련된 속성만 초기화해야 합니다.
- `reconfigureRows(at:)`: 기존 셀을 유지하면서 데이터를 업데이트하기 위해 사용되는 메서드이며, `reloadRows(at:with:)` 대신 사용할 수 있습니다.
- `reconfigureItems(_:)`: `NSDiffableDataSourceSnapshot`에서 사용되며, `reconfigureRows(at:)`와 유사하지만 `diffableDataSource`를 사용하는 경우 사용해야 합니다.

## 참고

---

[prepareForReuse() | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableviewcell/1623223-prepareforreuse)

[reconfigureRows(at:) | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableview/3801923-reconfigurerows)
# One ViewController w/ multi TableViewController
better on : https://www.notion.so/enebin/One-ViewController-w-multi-TableViewController-441eebb2bc0648daa83a1a437b196095?pvs=4

## Multiple Table view

---

한 뷰 컨트롤러에 2개 이상의 테이블 뷰를 표시하는 방법에는 여러가지가 있습니다.

### UITableViewDatasources, UITableViewDelegate 공유

하나의 뷰 컨트롤러에 `UITableViewDatasources`, `UITableViewDelegate`를 구현한 뒤 이를 여러 개의 테이블 뷰와 공유합니다.

- 아무런 처리를 하지 않을 경우 모두 같은 데이터 소스와 동작을 공유합니다.
- 따라서 다음과 같이 테이블 뷰 객체에 따른 분기처리를 구현합니다.
    
    ```swift
    class MultipleTableViewController: 
    UIViewController, 
    UITableViewDelegate,
    UITableViewDataSource {
    
        let tableView1 = UITableView()
        let tableView2 = UITableView()
        
        override func viewDidLoad() {
            super.viewDidLoad()
            
            setupTableViews()
        }
        
        private func setupTableViews() {
            tableView1.delegate = self
            tableView1.dataSource = self
            
            tableView2.delegate = self
            tableView2.dataSource = self
        }
    }
    ```
    
    ```swift
    // MARK: - UITableViewDataSource
    func numberOfSections(in tableView: UITableView) -> Int {
        return 1
    }
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        if tableView === tableView1 {
            // Return the number of rows for tableView1
            return 10
        } else if tableView === tableView2 {
            // Return the number of rows for tableView2
            return 20
        }
        
        return 0
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: .default, reuseIdentifier: "cell")
        
        if tableView === tableView1 {
            // Configure the cell for tableView1
            cell.textLabel?.text = "TableView1: Row \(indexPath.row)"
        } else if tableView === tableView2 {
            // Configure the cell for tableView2
            cell.textLabel?.text = "TableView2: Row \(indexPath.row)"
        }
        
        return cell
    }
    ```
    
- 공유된 데이터 소스를 가지고 여러 개의 바리에이션을 만들 수 있다는 장점이 있습니다.
- 구현이 길어져 가독성을 해칠 우려가 있습니다.

### Sub 뷰 컨트롤러

테이블 뷰 당 하나의 뷰 컨트롤러를 할당한 뒤 루트 뷰 컨트롤러에서 자식 뷰 컨트롤러로서 관리합니다.

- 다음과 같이 테이블 뷰 컨트롤러를 서브클래싱한 후 각각 프로토콜을 관리합니다.

```swift
class FirstTableViewController: UITableViewController {
    // Your implementation for the first table view
		
		// FirstTableViewController
		override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
		    // Return the number of rows for the first table view
		    return 10
		}
		
		override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
		    let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
		    cell.textLabel?.text = "FirstTableView: Row \(indexPath.row)"
		    return cell
		}
}

class SecondTableViewController: UITableViewController {
    // Your implementation for the second table view

		// SecondTableViewController
		override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
		    // Return the number of rows for the second table view
		    return 10
		}
		
		override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
		    let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
		    cell.textLabel?.text = "SecondTableView: Row \(indexPath.row)"
		    return cell
		}
}
```

- 테이블 뷰에 대한 책임이 개별 객체로 분리되어 SRP를 지킬 수 있게 됩니다.
- 구현이 조금 귀찮습니다.

## 정리

---

한 뷰 컨트롤러에 2개 이상의 테이블 뷰를 표시하는 방법에는 크게 `**UITableViewDatasources`와 `UITableViewDelegate`를 공유해 하나의 뷰 컨트롤러에서 모두 관리**하는 방법과 **Sub 뷰 컨트롤러를 여러 개 둔 뒤 자식 뷰 컨트롤러로 관리**하는 2가지 방법을 예로 들 수 있습니다.

## 참고
[UITableView | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableview)
---
title: UITableView
categories:
 - Swift5-UI
tags:
---

### 纯代码创建

#####初始化

```swift
lazy var tableView : UITableView = {
    let tableView = UITableView.init()
    tableView.separatorStyle = .none
    tableView.rowHeight = UITableView.automaticDimension
    tableView.estimatedRowHeight = kFitWidth(200)
    tableView.backgroundColor = UIColor.dynamic(light: mainBGColor_gray, dark: mainBGColor_dark)
    tableView.delegate = self
    tableView.dataSource = self
    tableView.pd_registerCellClass(MineOrderCell.self)
    tableView.tableFooterView = UIView()
    return tableView
}()
```



##### func 实现

```swift
	var pageNo = 1
    override func initialize(_ anyModel: Any?) {
        view.addSubview(tableView)
        tableView.snp.makeConstraints { (make) in
            
            make.top.left.right.bottom.equalToSuperview()
        }
        
        
        
        tableView.es.addPullToRefresh {[weak self] () in
            self?.pageNo = 1
            self?.loadData()
        }
        tableView.es.addInfiniteScrolling {[weak self] () in
            self?.pageNo += 1
            self?.loadData()
        }
                    loadData()
    }
 func loadData() {
}

```

##### datasource

```swift

extension MineOrderListVC: UITableViewDelegate, UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return dataArr.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.pd_dequeueReusableCell(MineOrderCell.self) as MineOrderCell
        
         return cell
    }
 
}
```



##### cell

```swift
{

    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }

    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)

        // Configure the view for the selected state
    }

    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        selectionStyle = .none
        initSubViews()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    
    
    func initSubViews() {
        
    }
}

```



### 2. 局部刷新


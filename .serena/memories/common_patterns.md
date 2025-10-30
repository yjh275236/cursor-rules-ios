# 常用代码模式

## 网络请求模式

### 定义请求类

所有网络请求继承 `SSBaseRequest`：

```swift
class SSLoginRequest: SSBaseRequest {
    var phoneNum: String?
    var codeForLogin: String?
    
    override func requestUrl() -> String {
        return "/app/interface/TimVay/user/auth/userLogin"
    }
    
    override func requestMethod() -> YTKRequestMethod {
        return .POST
    }
    
    override func requestArgument() -> Any? {
        return ["phoneNum": phoneNum, "codeForLogin": codeForLogin]
    }
}
```

### 发送请求（使用 Promise）

```swift
let request = SSLoginRequest()
request.phoneNum = phone
request.codeForLogin = code

request.fetch().done { [weak self] data in
    guard let self = self, let dict = data as? [String: Any] else { return }
    
    if let model = dict.kj.model(SSLoginModel.self) {
        AppShared.User.login(with: model)
    }
}.catch { error in
    if let ssError = error as? SSError {
        print(ssError.message)
    }
}
```

---

## UI 组件创建模式

### ViewController 结构

```swift
class CustomViewController: SSBaseViewController {
    
    // MARK: - UI Components
    
    private lazy var titleLabel: UILabel = {
        let label = UILabel()
        label.font = .systemFont(ofSize: 18, weight: .semibold)
        label.textColor = UIColor(hexString: "#333333")
        return label
    }()
    
    private lazy var actionButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("确认", for: .normal)
        button.backgroundColor = UIColor(hexString: "#322EE3")
        button.layer.cornerRadius = 8
        button.addTarget(self, action: #selector(handleAction), for: .touchUpInside)
        return button
    }()
    
    // MARK: - Lifecycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        loadData()
    }
    
    // MARK: - UI Setup
    
    private func setupUI() {
        view.backgroundColor = .white
        view.addSubview(titleLabel)
        view.addSubview(actionButton)
        
        titleLabel.snp.makeConstraints { make in
            make.top.equalTo(view.safeAreaLayoutGuide).offset(20)
            make.left.right.equalToSuperview().inset(16)
        }
        
        actionButton.snp.makeConstraints { make in
            make.top.equalTo(titleLabel.snp.bottom).offset(20)
            make.centerX.equalToSuperview()
            make.size.equalTo(CGSize(width: 200, height: 44))
        }
    }
    
    // MARK: - Actions
    
    @objc private func handleAction() {
        // 处理按钮点击
    }
    
    // MARK: - Data Loading
    
    private func loadData() {
        // 加载数据
    }
}
```

### 自定义 View

```swift
class CustomView: UIView {
    
    // MARK: - UI Components
    
    private let titleLabel: UILabel = {
        let label = UILabel()
        label.font = .systemFont(ofSize: 16, weight: .medium)
        return label
    }()
    
    // MARK: - Properties
    
    var title: String? {
        didSet { titleLabel.text = title }
    }
    
    // MARK: - Initialization
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        setupUI()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    // MARK: - UI Setup
    
    private func setupUI() {
        backgroundColor = .white
        addSubview(titleLabel)
        
        titleLabel.snp.makeConstraints { make in
            make.center.equalToSuperview()
        }
    }
}
```

### 导航栏配置

```swift
// 隐藏导航栏
override func base_hideNavigationBar() -> Bool {
    return true
}

// 自定义导航栏颜色
override func viewDidLoad() {
    super.viewDidLoad()
    updateNavigationBar(foregroundColor: .white, backgroundColor: .white)
}

// 自定义返回逻辑
override func base_backClicked() {
    // 自定义返回处理
    navigationController?.popViewController(animated: true)
}
```

---

## 数据处理模式

### JSON 解析（KakaJSON）

```swift
// 定义模型
struct UserModel: Convertible {
    var id: Int?
    var name: String?
    var phone: String?
    var token: String?
}

// 解析单个对象
if let dict = data as? [String: Any],
   let user = dict.kj.model(UserModel.self) {
    // 使用 user
}

// 解析数组
if let array = data["list"] as? [[String: Any]] {
    let users = array.kj.modelArray(UserModel.self)
}
```

### 本地存储（SSAppCache）

```swift
// 保存数据
SSAppCache.saveDataToLocalCache(data: value, for: "key")

// 读取数据
if let value = SSAppCache.getLocalCacheData(for: "key") as? String {
    // 使用 value
}

// 清除用户数据
SSAppCache.clearUserData()
```

### 缓存策略

```swift
func loadData(forceRefresh: Bool = false) -> Promise<Model> {
    let cacheKey = "data_key"
    
    return Promise { seal in
        // 先尝试从缓存获取
        if !forceRefresh,
           let cachedData = SSAppCache.getLocalCacheData(for: cacheKey) as? Data,
           let model = try? JSONDecoder().decode(Model.self, from: cachedData) {
            seal.fulfill(model)
            return
        }
        
        // 请求网络数据
        request.fetch().done { data in
            if let dict = data as? [String: Any],
               let model = dict.kj.model(Model.self),
               let jsonData = try? JSONEncoder().encode(model) {
                SSAppCache.saveDataToLocalCache(data: jsonData, for: cacheKey)
                seal.fulfill(model)
            }
        }.catch { error in
            // 网络失败时尝试从缓存获取
            if let cachedData = SSAppCache.getLocalCacheData(for: cacheKey) as? Data,
               let model = try? JSONDecoder().decode(Model.self, from: cachedData) {
                seal.fulfill(model)
            } else {
                seal.reject(error)
            }
        }
    }
}
```

---

## 异步编程模式

### PromiseKit 链式调用

```swift
firstly {
    getUserInfo()
}.then { user in
    getOrderList(userId: user.id)
}.done { orders in
    updateUI(orders: orders)
}.catch { error in
    handleError(error)
}
```

### Promise 包装网络请求

```swift
func getUserInfo() -> Promise<UserModel> {
    return Promise { seal in
        let request = GetUserInfoRequest()
        request.fetch().done { data in
            if let dict = data as? [String: Any],
               let user = dict.kj.model(UserModel.self) {
                seal.fulfill(user)
            } else {
                seal.reject(SSError(code: -1, message: "解析失败"))
            }
        }.catch { error in
            seal.reject(error)
        }
    }
}
```

---

## 列表刷新模式

### ESPullToRefresh 下拉刷新

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    setupRefresh()
}

private func setupRefresh() {
    // 下拉刷新
    tableView.es.addPullToRefresh { [weak self] in
        self?.refreshData()
    }
    
    // 上拉加载
    tableView.es.addInfiniteScrolling { [weak self] in
        self?.loadMoreData()
    }
}

private func refreshData() {
    currentPage = 1
    loadData { [weak self] in
        self?.tableView.es.stopPullToRefresh()
        self?.tableView.es.resetNoMoreData()
    }
}

private func loadMoreData() {
    currentPage += 1
    loadData { [weak self] hasMore in
        if hasMore {
            self?.tableView.es.stopLoadingMore()
        } else {
            self?.tableView.es.stopLoadingMore(pullToRefreshView: nil, completion: true)
        }
    }
}
```

---

## 用户状态管理

### 检查登录状态

```swift
// 检查登录状态
if AppShared.User.isLogin {
    // 已登录
} else {
    // 未登录，显示登录弹窗
    AppShared.User.loginAlert()
}

// 获取用户信息
let userId = AppShared.User.userId
let token = AppShared.User.token
```

### 用户登录/登出

```swift
// 登录成功（会自动更新 AppsFlyer 用户ID 并记录事件）
AppShared.User.login(with: loginResponse)

// 登出（会自动清除用户数据并更新界面）
AppShared.User.logOut()

// Token 失效处理（会自动清除用户信息并跳转登录页）
AppShared.User.accessTokenInvalid(text: "登录已过期")
```

### 检查认证状态

```swift
AppShared.User.checkAuthStatusAndNavigate(from: self) { isAuthenticated in
    if isAuthenticated {
        // 已完成认证，继续操作
    } else {
        // 未完成认证，已自动跳转到认证页面
    }
}
```

---

## 提示和弹窗模式

### MBProgressHUD 加载提示

```swift
// 显示加载中
let hud = MBProgressHUD.showAdded(to: view, animated: true)
hud.label.text = "加载中..."

// 隐藏加载
MBProgressHUD.hide(for: view, animated: true)
```

### JXPopupView 弹窗

```swift
// 显示弹窗
let popup = Bundle.main.loadNibNamed("PopupName", owner: nil, options: nil)?.first as? CustomPopup
let popupView = PopupView(containerView: view, contentView: popup!)
popupView.isDismissible = true
popupView.display(animated: true, completion: nil)

// 关闭弹窗
popupView.dismiss(animated: true, completion: nil)

// 设置关闭回调
popup?.closeHandler = {
    popupView.dismiss(animated: true, completion: nil)
}
```

---

## 图片加载模式

### HanekeSwift 加载图片

```swift
// 基本使用
imageView.hnk_setImageFromURL(URL(string: imageURL)!)

// 带占位图和回调
imageView.hnk_setImageFromURL(
    URL(string: imageURL)!,
    placeholder: UIImage(named: "placeholder"),
    success: { image in
        // 图片加载成功
    },
    failure: { error in
        // 图片加载失败
    }
)
```

### URLSession 直接加载

```swift
if let url = URL(string: imageURL) {
    URLSession.shared.dataTask(with: url) { [weak self] data, response, error in
        guard let self = self,
              let data = data,
              let image = UIImage(data: data),
              error == nil else { return }
        
        DispatchQueue.main.async {
            self.imageView.image = image
        }
    }.resume()
}
```

---

## 错误处理模式

### 统一错误处理

```swift
request.fetch().done { data in
    // 成功处理
}.catch { error in
    // 统一错误处理
    if let ssError = error as? SSError {
        if ssError.code == 1000 {
            // Token 失效，AppShared.User 会自动处理
            return
        }
        print(ssError.message)
    } else {
        print(error.localizedDescription)
    }
}
```

### 自定义错误类型

```swift
struct SSError: Error, LocalizedError {
    let code: Int
    let message: String
    
    init(code: Int, message: String) {
        self.code = code
        self.message = message
    }
    
    var errorDescription: String? {
        return message
    }
}
```

---

## WebView 交互模式

### SSHTMLController 使用

```swift
class SSHTMLController: SSBaseViewController {
    
    private lazy var webView: WKWebView = {
        let config = WKWebViewConfiguration()
        config.userContentController.add(self, name: "native")
        return WKWebView(frame: .zero, configuration: config)
    }()
    
    // JS 调用 Native 方法
    func userContentController(_ userContentController: WKUserContentController, 
                              didReceive message: WKScriptMessage) {
        if message.name == "native",
           let body = message.body as? [String: Any],
           let method = body["method"] as? String {
            
            switch method {
            case "close":
                navigationController?.popViewController(animated: true)
            case "login":
                // 处理登录
                break
            default:
                break
            }
        }
    }
    
    // Native 调用 JS
    private func callJS(method: String, params: [String: Any]) {
        guard let jsonData = try? JSONSerialization.data(withJSONObject: params),
              let jsonString = String(data: jsonData, encoding: .utf8) else { return }
        
        let script = "\(method)(\(jsonString))"
        webView.evaluateJavaScript(script, completionHandler: nil)
    }
}
```

---

## 富文本标签模式

### NantesLabel 使用

```swift
private lazy var privacyLabel: NantesLabel = {
    let label = NantesLabel()
    label.delegate = self
    label.isUserInteractionEnabled = true
    return label
}()

// 设置富文本和链接
let text = "Tôi đã đọc và đồng ý Chính sách bảo mật"
let attrString = NSMutableAttributedString(string: text)
attrString.addAttribute(.font, value: UIFont.systemFont(ofSize: 14), 
                       range: NSRange(location: 0, length: text.count))

let policyRange = (text as NSString).range(of: "Chính sách bảo mật")
attrString.addAttribute(.underlineStyle, value: NSUnderlineStyle.single.rawValue, 
                       range: policyRange)
attrString.addAttribute(.foregroundColor, value: UIColor(hexString: "#322EE3")!, 
                       range: policyRange)

privacyLabel.attributedText = attrString
privacyLabel.addLink(to: URL(string: "privacyPolicy://")!, withRange: policyRange)

// 实现委托方法
func attributedLabel(_ label: NantesLabel, didSelectLink link: URL) {
    if link.scheme == "privacyPolicy" {
        // 处理链接点击
    }
}
```
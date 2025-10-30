# 技术栈详情

## 第三方库列表

### YTKNetwork - 网络请求库

**用途**: 基于 AFNetworking 的网络请求框架

**官方文档**: https://github.com/yuantiku/YTKNetwork

**项目中的使用**:
- 所有请求继承 `SSBaseRequest`（继承自 YTKRequest）
- 使用 `fetch()` 方法返回 Promise 对象
- 请求头在 SSBaseRequest 中自动添加（包括 Token、设备信息等）
- Token 失效（errorCode=1000）会自动跳转登录页

**请求配置**:
- `baseUrl()`: 自动返回 `AppShared.Config.Domain`
- `requestUrl()`: 返回相对路径
- `requestMethod()`: 返回 `.GET` 或 `.POST`
- `requestArgument()`: 返回请求参数字典

---

### KakaJSON - JSON 解析

**用途**: Swift JSON 序列化/反序列化

**官方文档**: https://github.com/kakaopensource/KakaJSON

**项目中的使用**:
- 模型需实现 `Convertible` 协议
- 使用 `.kj.model()` 解析单个对象
- 使用 `.kj.modelArray()` 解析数组
- 支持可选属性，无需手动处理 JSON key 映射

---

### SnapKit - 自动布局

**用途**: 声明式自动布局 DSL

**官方文档**: https://github.com/SnapKit/SnapKit

**常用约束**:
```swift
view.snp.makeConstraints { make in
    make.top.equalTo(view.safeAreaLayoutGuide).offset(20)
    make.left.right.equalToSuperview().inset(16)
    make.height.equalTo(44)
}
```

---

### HanekeSwift - 图片加载

**用途**: 异步图片下载和缓存

**官方文档**: https://github.com/Haneke/HanekeSwift

**基本使用**:
```swift
imageView.hnk_setImageFromURL(
    URL(string: imageURL)!,
    placeholder: UIImage(named: "placeholder")
)
```

---

### IQKeyboardManagerSwift - 键盘管理

**用途**: 自动处理键盘弹出和隐藏

**官方文档**: https://github.com/hackiftekhar/IQKeyboardManager

**配置**:
```swift
// AppDelegate 中配置
IQKeyboardManager.shared.isEnabled = true
IQKeyboardToolbarManager.shared.isEnabled = true
```

---

### PromiseKit - 异步编程

**用途**: Promise/Future 异步编程模式

**官方文档**: https://github.com/mxcl/PromiseKit

**项目中的使用**:
- 与 SSBaseRequest 结合，通过 `fetch()` 返回 Promise
- 支持链式调用（`then`, `done`, `catch`）
- 统一异步错误处理

---

### ESPullToRefresh - 下拉刷新

**用途**: 列表下拉刷新和上拉加载

**官方文档**: https://github.com/eggswift/pull-to-refresh

**基本使用**:
```swift
tableView.es.addPullToRefresh { [weak self] in
    self?.loadData()
}

tableView.es.addInfiniteScrolling { [weak self] in
    self?.loadMoreData()
}

// 停止刷新
tableView.es.stopPullToRefresh()
tableView.es.stopLoadingMore()
```

---

### SwifterSwift - Swift 扩展

**用途**: Swift 标准库扩展，提供便利方法

**官方文档**: https://github.com/SwifterSwift/SwifterSwift

**常用扩展**:
```swift
// UIColor 扩展
let color = UIColor(hexString: "#333333")

// String 扩展
let md5String = "hello".md5
```

---

### StorageManager - 本地存储

**用途**: 键值对存储管理

**项目中的使用**: 通过 SSAppCache 封装
```swift
// 存储数据
SSAppCache.saveDataToLocalCache(data: value, for: "key")

// 读取数据
SSAppCache.getLocalCacheData(for: "key")

// 清除用户数据
SSAppCache.clearUserData()
```

---

### RAMAnimatedTabBarController - 动画标签栏

**用途**: 支持动画效果的标签栏控制器

**官方文档**: https://github.com/Ramotion/animated-tab-bar

**项目中的使用**:
```swift
let tabBarItem = RAMAnimatedTabBarItem(
    title: "首页", 
    image: unselectedImage, 
    selectedImage: selectedImage
)
tabBarItem.animation = RAMLeftRotationAnimation()
nav.tabBarItem = tabBarItem
```

---

### MBProgressHUD - 加载提示

**用途**: 显示加载指示器

**官方文档**: https://github.com/jdg/MBProgressHUD

**基本使用**:
```swift
// 显示加载中
let hud = MBProgressHUD.showAdded(to: view, animated: true)
hud.label.text = "加载中..."

// 隐藏
MBProgressHUD.hide(for: view, animated: true)
```

---

### Spring - UI 动画库

**用途**: 动画和 UI 组件

**官方文档**: https://github.com/MengTo/Spring

**项目中的使用**:
```swift
// UIImageView 扩展 - 加载网络图片
imageView.setImage(
    urlString: imageURL, 
    placeholderImage: UIImage(named: "placeholder")
)
```

---

### Nantes - 富文本标签

**用途**: 支持链接、样式等的 UILabel 替代品

**官方文档**: https://github.com/instacart/Nantes

**基本使用**:
```swift
let label = NantesLabel()
label.delegate = self
label.attributedText = attrString
label.addLink(to: URL(string: "scheme://")!, withRange: range)
```

---

### JXPopupView - 弹窗

**用途**: 自定义弹窗显示

**官方文档**: https://github.com/pujiaxin33/JXPopupView

**基本使用**:
```swift
let popup = Bundle.main.loadNibNamed("PopupName", owner: nil, options: nil)?.first as? CustomPopup
let popupView = PopupView(containerView: view, contentView: popup!)
popupView.isDismissible = true
popupView.display(animated: true, completion: nil)
```

---

### 其他工具库

- **BadgeHub**: UIView 角标显示
- **TimeIntervals**: 时间间隔格式化
- **SwiftGen**: 类型安全的资源访问代码生成（通过 swiftgen.yml 配置）
- **SteviaLayout**: 布局 DSL（项目中主要使用 SnapKit）

---

## Firebase 集成

### Firebase Analytics
- **用途**: 用户行为分析
- **配置**: GoogleService-Info.plist
- **使用**: `Analytics.setUserID()`, `Analytics.logEvent()`

### Firebase Crashlytics
- **用途**: 崩溃报告
- **注意**: 不要上传敏感信息到崩溃日志

### Firebase Cloud Messaging
- **用途**: 推送通知
- **配置**: 在 AppDelegate 注册推送、实现 UNUserNotificationCenterDelegate
- **使用**: `Messaging.messaging().fcmToken`

---

## AppsFlyer 集成

**用途**: 归因分析和用户追踪

**配置**:
```swift
// AppDelegate 中配置
AppsFlyerLib.shared().appsFlyerDevKey = AppShared.Config.AFKey
AppsFlyerLib.shared().appleAppID = AppShared.Config.AppID
AppsFlyerLib.shared().waitForATTUserAuthorization(timeoutInterval: 60)
AppsFlyerLib.shared().customerUserID = AppShared.User.customerID
AppsFlyerLib.shared().start()
```

**记录事件**:
```swift
AppsFlyerLib.shared().logEvent(AFEventLogin, withValues: nil)
AppsFlyerLib.shared().logEvent(AFEventCompleteRegistration, withValues: nil)
```

---

## MCKayeriou SDK 集成

**用途**: 第三方 SDK（来自本地 lib 目录）

**配置**:
```swift
// AppDelegate 中初始化
application.start { config in
    config.key = "your_key"
    config.bundleID = "com.appstart.dev"
}
```

---

## 依赖管理

### CocoaPods
**Podfile 关键配置**:
```ruby
source 'https://cdn.cocoapods.org/'
use_frameworks!
platform :ios, '14.0'

target 'SeasysStart' do
  # 核心依赖
  pod 'Firebase/Analytics'
  pod 'Firebase/Messaging'
  pod 'Firebase/Crashlytics'
  pod 'AppsFlyerFramework', '6.17.6'
  
  # UI 框架
  pod 'SnapKit'
  pod 'IQKeyboardManagerSwift'
  pod 'MBProgressHUD'
  pod 'ESPullToRefresh'
  pod 'RAMAnimatedTabBarController'
  
  # 数据处理
  pod 'KakaJSON'
  pod 'SwifterSwift/SwiftStdlib'
  pod 'SwifterSwift/Foundation'
  
  # 异步和网络
  pod 'PromiseKit/CorePromise'
  
  # 图片加载
  pod 'HanekeSwift', :git => 'https://github.com/Haneke/HanekeSwift.git'
  
  # 其他
  pod 'StorageManager'
  pod 'Spring', :git => 'https://github.com/Mengjing/Spring.git'
  pod 'SwiftGen', '~> 6.0'
  pod 'BadgeHub'
  pod 'Nantes'
  pod 'TimeIntervals'
  pod 'JXPopupView'
  pod 'MCKayeriou', :path => 'lib'
end
```

**常用命令**:
```bash
pod install           # 安装依赖
pod update            # 更新所有依赖
pod update [PodName]  # 更新特定依赖
pod outdated          # 查看过期依赖
```

**注意事项**:
- 所有 Pod 最低部署目标版本统一设置为 iOS 14.0
- MCKayeriou 通过本地路径引入
- HanekeSwift 和 Spring 从 GitHub 仓库引入
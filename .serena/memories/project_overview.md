# 项目概览

## 项目基本信息
- **项目名称**: 
- **Bundle ID**: 
- **项目目的**: 金融借贷类移动应用，提供账户管理、订单处理、身份验证、银行信息等功能
- **最低支持版本**: iOS 14.0
- **开发语言**: Swift 5.0
- **当前版本**: 1.0.0

## 技术栈概览
- **开发语言**: Swift
- **UI 框架**: UIKit
- **架构模式**: MVC
- **依赖管理**: CocoaPods
- **核心第三方库**: 
  - YTKNetwork - 网络请求（基于 AFNetworking）
  - KakaJSON - JSON 解析
  - SnapKit - 自动布局
  - HanekeSwift - 图片加载和缓存
  - IQKeyboardManagerSwift - 键盘管理
  - Firebase/Analytics - 用户行为分析
  - Firebase/Messaging - 推送通知
  - Firebase/Crashlytics - 崩溃报告
  - AppsFlyerFramework - 归因分析
  - PromiseKit - 异步编程
  - ESPullToRefresh - 下拉刷新
  - SwifterSwift - Swift 标准库扩展
  - StorageManager - 本地存储管理
  - RAMAnimatedTabBarController - 动画标签栏
  - MCKayeriou - 第三方 SDK

## 目录结构
```
SeasysStart/
├── AppDelegate.swift              # 应用入口
├── Source/                         # 源代码目录
│   ├── Foundation/                # 基础控制器
│   │   ├── SSBaseViewController  # 基类 VC
│   │   ├── SSHTMLController      # WebView 控制器
│   │   ├── SSNavBarController     # 导航控制器
│   │   └── SSTabBarController     # 标签栏控制器
│   ├── Global/                    # 全局配置
│   │   └── AppShared.swift        # 应用共享配置和用户管理
│   ├── Misc/                      # 工具类
│   │   ├── Extensions/            # 扩展
│   │   ├── SSBaseRequest.swift    # 网络请求基类
│   │   ├── SSCache.swift          # 缓存管理
│   │   ├── SSDeviceTool.swift     # 设备工具
│   │   ├── SSKeyChainTool.swift   # Keychain 工具
│   │   └── SDK/                   # 第三方 SDK
│   └── Pages/                     # 业务模块
│       ├── Login/                 # 登录模块
│       ├── Home/                  # 首页模块
│       ├── Order/                 # 订单模块
│       ├── OrderRepayment/        # 还款模块
│       ├── Bank/                  # 银行模块
│       ├── Message/               # 消息模块
│       └── Mine/                  # 我的模块
├── Assets.xcassets/               # 资源文件
├── GoogleService-Info.plist      # Firebase 配置
└── Info.plist                     # 应用配置
```

## 核心架构

### 基类说明
- **SSBaseViewController**: 所有 VC 的基类，处理导航栏返回按钮和右滑返回手势
- **SSNavBarController**: 自定义导航控制器，统一导航栏样式
- **SSTabBarController**: 主标签栏控制器，使用 RAMAnimatedTabBarController

### 全局配置
- **AppShared**: 全局配置和用户管理
  - `AppShared.Config`: 存储 API 域名、Bundle ID、密钥等配置
    - `Domain`: API 基础地址
    - `BundleID`: 应用 Bundle ID (com.appstart.dev)
    - `Key`: API 密钥
  - `AppShared.User`: 用户状态管理
    - `isLogin`: 登录状态
    - `userId`: 用户ID
    - `token`: 访问令牌
    - `login()`: 登录成功处理
    - `logOut()`: 登出处理
    - `accessTokenInvalid()`: Token 失效处理

### 数据存储
- **本地存储**: 使用 SSAppCache（基于 StorageManager）
  - `getLocalCacheData(for:)`: 获取持久化数据
  - `saveDataToLocalCache(data:for:)`: 保存持久化数据
  - `clearUserData()`: 清除用户数据
- **缓存策略**: 支持内存缓存和磁盘缓存

### 网络请求架构
- **SSBaseRequest**: 继承 YTKRequest，统一处理网络请求
  - 自动添加请求头（Bundle ID、Token、设备信息等）
  - 统一错误处理（Token 失效自动跳转登录）
  - 返回 Promise 对象，支持链式调用
- **请求基类方法**:
  - `baseUrl()`: 返回 API 基础地址
  - `requestUrl()`: 返回请求路径
  - `requestMethod()`: 返回请求方法（GET/POST）
  - `requestArgument()`: 返回请求参数
  - `fetch()`: 返回 Promise，用于发送请求

## 开发环境

### 构建和运行
1. 安装依赖: `pod install`
2. 打开工作空间: `SeasysStart.xcworkspace`
3. 选择目标设备运行

### 必要配置
- **Firebase**: GoogleService-Info.plist 配置文件
- **AppsFlyer**: AFKey 和 AppID 配置（在 AppShared.Config 中）
- **API 域名**: 在 AppShared.Config.Domain 中配置

## 关键约定
- **类名前缀**: SS（如 SSBaseViewController, SSLoginViewController）
- **网络请求**: 继承 SSBaseRequest，使用 `fetch()` 返回 Promise
- **响应模型解析**: 使用 KakaJSON 进行 JSON 解析（`.kj.model()`）
- **UI 布局**: 使用 SnapKit 进行自动布局
- **异步编程**: 使用 PromiseKit 处理异步操作
- **缓存管理**: 使用 SSAppCache 进行本地数据存储

## 主要功能模块
1. **登录认证**: 短信验证码登录
2. **首页**: 应用主界面，身份认证流程
3. **订单管理**: 订单列表和详情，订单还款
4. **银行信息**: 银行列表选择，银行卡绑定
5. **身份验证**: OCR 身份证识别、基础信息填写、活体验证
6. **消息中心**: 系统消息
7. **我的**: 个人信息和设置
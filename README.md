# swift-ios-ui

> 一个 Claude Code Skill，根据 UI 截图、设计稿或文字描述，自动生成规范的 iOS UIKit Swift 代码。

---

## 特性

- 📸 **支持多种输入**：UI 截图 / Figma 描述 / 文字需求，按需生成代码
- 📐 **SnapKit 布局**：全程使用 Auto Layout DSL，禁止 frame 和 Storyboard
- 🪟 **底部弹窗**：使用标准 SwiftEntryKit 配置，支持 home indicator 适配
- 🎨 **规范颜色**：统一使用 `AppColor` enum + HEX 值管理
- 🔤 **PingFangSC 字体**：六种字重完整覆盖，禁止使用 systemFont
- 🖼️ **Kingfisher 图片加载**：含 placeholder、淡入动画、Cell 复用取消
- 📦 **SwiftyJSON 数据模型**：统一 `init(json: JSON)` 解析范式

---

## 安装

### 方式一：npx 一键安装（推荐）

```bash
npx skills add https://github.com/qunwang6/swift-ios-ui
```

### 方式二：手动安装

将 `swift-ios-ui/SKILL.md` 复制到以下目录：

```bash
# 当前项目生效
your-ios-project/.claude/skills/swift-ios-ui/SKILL.md

# 全局所有项目生效
~/.claude/skills/swift-ios-ui/SKILL.md
```

---

## 使用方式

在 iOS 项目目录下启动 Claude Code，然后直接描述需求：

```bash
cd your-ios-project
claude
```

### 上传截图

```
帮我根据这张截图实现 Swift 界面
```

拖入设计图，Claude 会自动分析布局、颜色、组件，生成完整代码。

### 文字描述

```
做一个商品详情页：
- 顶部轮播图
- 商品名称（semibold 18pt）、价格（medium 16pt）
- 底部固定"立即购买"按钮
```

### 底部弹窗

```
做一个底部弹窗，包含标题、选项列表和取消按钮
```

---

## 生成输出示例

| 文件 | 说明 |
|---|---|
| `ProductDetailViewController.swift` | 主界面控制器 |
| `ProductCell.swift` | 自定义列表 Cell |
| `ProductBottomSheetView.swift` | 底部弹窗视图 |
| `ProductModel.swift` | SwiftyJSON 数据模型 |

---

## 技术栈

| 用途 | 库 | CocoaPods |
|---|---|---|
| 布局 | SnapKit | `pod 'SnapKit'` |
| 弹窗 / Toast | SwiftEntryKit | `pod 'SwiftEntryKit'` |
| 图片加载 | Kingfisher | `pod 'Kingfisher'` |
| 颜色 | Hue + SwiftHEXColors | `pod 'Hue'` / `pod 'SwiftHEXColors'` |
| JSON 解析 | SwiftyJSON | `pod 'SwiftyJSON'` |

### Podfile 示例

```ruby
target 'YourApp' do
  use_frameworks!

  pod 'SnapKit'
  pod 'SwiftEntryKit', '2.0.0'
  pod 'Kingfisher'
  pod 'Hue'
  pod 'SwiftHEXColors'
  pod 'SwiftyJSON'
end
```

---

## 代码规范说明

### 布局结构

每个文件统一使用 `MARK` 分区：

```swift
// MARK: - Properties
// MARK: - Lifecycle
// MARK: - Setup
// MARK: - Layout (SnapKit)
// MARK: - Actions
// MARK: - Data / Network
// MARK: - Helpers
```

### 字体

```swift
// ✅ 正确
titleLabel.font = .pingFangSC(.semibold, size: 18)
bodyLabel.font  = .pingFangSC(.regular,  size: 14)

// ❌ 禁止
titleLabel.font = .systemFont(ofSize: 18, weight: .semibold)
```

### 颜色

```swift
enum AppColor {
    static let primary    = UIColor(hexString: "#007AFF")
    static let background = UIColor(hexString: "#F2F2F7")
    static let text       = UIColor(hexString: "#1C1C1E")
    static let subtext    = UIColor(hexString: "#8E8E93")
}
```

### 底部弹窗（固定配置）

```swift
var attributes = EKAttributes()
attributes.position = .bottom
attributes.displayDuration = .infinity
attributes.screenBackground = .color(
    color: .init(light: UIColor(white: 0, alpha: 0.4),
                 dark:  UIColor(white: 0, alpha: 0.4))
)
attributes.entryBackground = .clear
attributes.screenInteraction = .dismiss
attributes.entryInteraction = .forward
attributes.scroll = .disabled
attributes.positionConstraints.size = .init(width: .fill, height: .intrinsic)
attributes.positionConstraints.safeArea = .overridden
attributes.positionConstraints.verticalOffset = 0
SwiftEntryKit.display(entry: popupView, using: attributes)
```

> ⚠️ 底部弹窗不使用 `EKAttributes.bottomFloat`，必须使用以上完整配置。

---

## 文件结构

```
swift-ios-ui/
├── SKILL.md       # Skill 主文件（Claude Code 读取）
└── README.md      # 本文件
```

---

## License

MIT
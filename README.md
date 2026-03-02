# swift-ios-ui

> A Claude Code Skill that automatically generates clean iOS UIKit Swift code from UI screenshots, design specs, or text descriptions.

---

## Features

- 📸 **Multiple input types**: UI screenshots / Figma descriptions / text requirements
- 📐 **SnapKit layout**: Full Auto Layout DSL — no `frame`, no Storyboard
- 🪟 **Bottom sheets**: Standard SwiftEntryKit config with home indicator support
- 🎨 **Inline colors**: Hex values written directly via `UIColor.colorWithHexString(hex:)` — no `AppColor` enum
- 🔤 **PingFangSC fonts**: All six weights supported — `systemFont` is forbidden
- 🖼️ **Kingfisher image loading**: Placeholder, fade animation, cell reuse cancellation
- 📦 **SwiftyJSON models**: Unified `init(json: JSON)` parsing pattern
- 🌐 **Localization**: `LocalizableManager.localValue("key")` with auto-output EN / Simplified Chinese / Traditional Chinese

---

## Installation

### Option 1: npx (recommended)

```bash
npx skills add https://github.com/qunwang6/swift-ios-ui
```

### Option 2: Manual

Copy `swift-ios-ui/SKILL.md` to one of the following directories:

```bash
# Project-level (current project only)
your-ios-project/.claude/skills/swift-ios-ui/SKILL.md

# Global (all projects)
~/.claude/skills/swift-ios-ui/SKILL.md
```

---

## Usage

Start Claude Code inside your iOS project directory and describe what you need:

```bash
cd your-ios-project
claude
```

### Upload a screenshot

```
Implement this Swift screen based on the screenshot
```

Drop in a design image — Claude will analyze the layout, colors, and components and generate complete code.

### Text description

```
Build a product detail page:
- Top image carousel
- Product name (semibold 18pt), price (medium 16pt)
- Fixed "Buy Now" button at the bottom
```

### Bottom sheet

```
Build a bottom sheet with a title, option list, and cancel button
```

---

## Generated Output

| File | Description |
|---|---|
| `ProductDetailViewController.swift` | Main view controller |
| `ProductCell.swift` | Custom table view cell |
| `ProductBottomSheetView.swift` | Bottom sheet view |
| `ProductModel.swift` | SwiftyJSON data model |
| `en.lproj/Localizable.strings` | English localization |
| `zh-Hans.lproj/Localizable.strings` | Simplified Chinese localization |
| `zh-Hant.lproj/Localizable.strings` | Traditional Chinese localization |

---

## Tech Stack

| Purpose | Library | CocoaPods |
|---|---|---|
| Layout | SnapKit | `pod 'SnapKit'` |
| Popups / Toast | SwiftEntryKit | `pod 'SwiftEntryKit'` |
| Image Loading | Kingfisher | `pod 'Kingfisher'` |
| JSON Parsing | SwiftyJSON | `pod 'SwiftyJSON'` |

### Podfile Example

```ruby
target 'YourApp' do
  use_frameworks!

  pod 'SnapKit'
  pod 'SwiftEntryKit', '2.0.0'
  pod 'Kingfisher'
  pod 'SwiftyJSON'
end
```

---

## Code Conventions

### File Structure

Each file uses consistent `MARK` sections:

```swift
// MARK: - Properties
// MARK: - Lifecycle
// MARK: - Setup
// MARK: - Layout (SnapKit)
// MARK: - Actions
// MARK: - Data / Network
// MARK: - Helpers
```

### Fonts

```swift
// ✅ Correct
titleLabel.font = .pingFangSC(.semibold, size: 18)
bodyLabel.font  = .pingFangSC(.regular,  size: 14)

// ❌ Forbidden
titleLabel.font = .systemFont(ofSize: 18, weight: .semibold)
```

### Colors

Write hex values **inline** — do NOT define an `AppColor` enum or any named color constants.

```swift
// ✅ Correct — inline hex values
label.textColor       = UIColor.colorWithHexString(hex: "#2D2F35")
button.backgroundColor = UIColor.colorWithHexString(hex: "#149D93")
view.backgroundColor  = UIColor.colorWithHexString(hex: "#F8F8F8")

// ❌ Forbidden — no AppColor enum
enum AppColor {
    static let primary = UIColor.colorWithHexString(hex: "#149D93")
}
label.textColor = AppColor.primary
```

### Layout Spacing

Write spacing values **as inline literals** in SnapKit constraints — do NOT define an `enum Layout`.

```swift
// ✅ Correct — inline literals
titleLabel.snp.makeConstraints { make in
    make.top.equalToSuperview().offset(26)
    make.leading.trailing.equalToSuperview().inset(30)
}

// ❌ Forbidden — no enum Layout
enum Layout {
    static let sideInset: CGFloat = 30
}
make.leading.trailing.equalToSuperview().inset(Layout.sideInset)
```

### Localization

```swift
// ✅ Correct
titleLabel.text = LocalizableManager.localValue("register_email")

// ❌ Forbidden
titleLabel.text = "Email"
titleLabel.text = "邮箱"
```

Output all three translation files alongside every generated UI file:

**en.lproj/Localizable.strings**
```
"register_email" = "Email";
```

**zh-Hans.lproj/Localizable.strings**
```
"register_email" = "邮箱";
```

**zh-Hant.lproj/Localizable.strings**
```
"register_email" = "郵箱";
```

### Bottom Sheet (Fixed Config)

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

> ⚠️ Never use `EKAttributes.bottomFloat` — always use the full config above.

---

## File Structure

```
swift-ios-ui/
├── SKILL.md       # Main skill file (read by Claude Code)
└── README.md      # This file
```

---

## License

MIT
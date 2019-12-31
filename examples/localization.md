# Localization

## MVPR

- Since `MVPR` finds it important that `V`, the objects that actually make use of the drawing API, stay passive, text is bound to the `V` through `P`.
  Therefore, any hard coded strings that need to be localized are localized within `P`, allowing the `V` to untouched.

```swift
// Before localization
class TitlePresenter {
  weak var titleLabel: UILabel! {
    didSet { titleLabel.text = "Foo" }
  }
}

// After localization
protocol LocalizationService {
  func localizedString(key: String) -> String?
}

class TitlePresenter {
  var localizer: LocalizationService!

  weak var titleLabel: UILabel! {
    didSet { titleLabel.text = localizer.localizedString(key: "Foo") }
  }
}
```

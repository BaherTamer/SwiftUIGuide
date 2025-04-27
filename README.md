# Style Guide:

### 💠 Use `Label` Instead of `HStack` for Icon + Text Combinations

**Why?**
`Label` is a semantic, built-in SwiftUI component specifically designed for pairing an icon with text. It improves accessibility, reduces boilerplate, and adapts better to dynamic type, right-to-left languages, and other system-driven layout changes.

``` swift
// Don't
HStack(spacer: 6) {
    Image(systemName: "circle")
    Text("Task")
}
```

``` swift
// Do
Label(
    title: titleText,
    icon: circleIcon
)

private func titleText() -> some View {
    Text("Task")
}

private func circleIcon() -> some View {
    Image(systemName: "circle")
}
```

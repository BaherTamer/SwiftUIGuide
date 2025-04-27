# SwiftUI Style Guide:

### 💠 Use `Label` Instead of `HStack` for Icon + Text Combinations

**Why?**
> `Label` is a semantic, built-in SwiftUI component specifically designed for pairing an icon with text. It improves accessibility, reduces boilerplate, and adapts better to dynamic type, right-to-left languages, and other system-driven layout changes.

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

<br>

---

<br>

### 💠 Use `LabeledContent` Instead of `HStack` for Label + Value Layouts

**Why?**
> `LabeledContent` is a purpose-built SwiftUI view for showing a label and a value. It improves consistency, accessibility, dynamic type handling, and aligns better with system styles, especially in forms and settings screens.

``` swift
// Don't
HStack {
    Text("Price")
    Spacer()
    Text("500 EGP")
}
```

``` swift
// Do
LabeledContent(
    content: amountText,
    label: priceText
)

private func priceText() -> some View {
    Text("Price")
}

private func amountText() -> some View {
    Text("500 EGP")
}
```

<br>

---

<br>

### 💠 Prefer Explicit Parameters Over Trailing Closures for Callbacks

**Why?**
> When a function or initializer has one or more callbacks, using explicit parameter labels improves clarity. It makes the code easier to read, reason about, and maintain — especially when scanning quickly or working with unfamiliar code.

``` swift
// Don't
ContentView {
    // Submit Action
} onDismiss: {
    // Dismiss Action
}
```

``` swift
// Do
TestView(
    onSubmit: {
        // Submit Action
    },
    onDismiss: {
        // Dismiss Action
    }
)
```

<br>

---

<br>

### 💠 Prefer Passing Function Names Instead of Closures

**Why?**
> Passing a function name directly makes your code cleaner, shorter, and more readable. It avoids unnecessary closures, improves clarity.

``` swift
// Don't
.onAppear {
    test()
}
```

``` swift
// Do
.onAppear(perform: test)
```

<br>

---

<br>

### 💠 Avoid Using Outer Padding for Reusable Components

**Why?**
> Reusable components should focus only on their internal layout and content, not their external spacing. This keeps components flexible and adaptable to different contexts and gives the parent view full control over padding and layout.

``` swift
// Don't
struct ReusableView: View {
    var body: some View {
        VStack {
            // Content
        }
        // Modifiers
        .padding(.horizontal, 25)
    }
}

struct ParentView: View {
    var body: some View {
        ReusableView()
    }
}
```

``` swift
// Do
struct ReusableView: View {
    var body: some View {
        VStack {
            // Content
        }
        // Modifiers
    }
}

struct ParentView: View {
    var body: some View {
        ReusableView()
            .padding(.horizontal, 25)
    }
}
```

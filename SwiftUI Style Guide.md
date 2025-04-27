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

<br>

---

<br>

### 💠 Use `LocalizedStringKey` as the Input Type for Reusable Components

**Why?**
> Using `LocalizedStringKey` ensures that your custom SwiftUI views support localization seamlessly. This enables the use of localized strings without extra manual effort.

``` swift
// Don't
struct ReusableView: View {
    let title: String

    var body: some View {
        // Content
    }
}
```

``` swift
// Do
struct ReusableView: View {
    let title: LocalizedStringKey

    var body: some View {
        // Content
    }
}
```

<br>

---

<br>

### 💠 Use `String.LocalizationValue` for Localized Entity Properties

**Why?**
> Using `String.LocalizationValue` ensures that your models or entities with localized strings are compatible with the localization system in Swift. This type allows automatic handling of localized values and ensures that your app is ready for different languages without extra code.

``` swift
// Don't
struct Toast {
    let title: String
    let message: String
}
```

``` swift
// Do
struct Toast {
    let title: String.LocalizationValue
    let message: String.LocalizationValue
}
```

<br>

---

<br>

### 💠 Use `verbatim` Parameter for `Text` When Displaying Non-Localized Strings

**Why?**
> The `verbatim` parameter should be used when you need to display a non-localized string. By default, `Text` uses `LocalizedStringKey`, which is designed for localized strings. Using `verbatim` ensures that your string is treated as raw text and not mistakenly processed for localization and displayed in `Localizable` table.

``` swift
// Don't
Text("\(product.title) - \(product.price)")
```

``` swift
// Do
Text(verbatim: "\(product.title) - \(product.price)")
```

<br>

---

<br>

### 💠 Use `ImageResource` Instead of `String` for Image Names

**Why?**
> Using `ImageResource` ensures type safety, avoids typos, and leverages Swift’s compiler checks to catch errors early. This improves maintainability, readability, and prevents runtime errors when working with images.

``` swift
// Don't
Image("circle.fill")
```

``` swift
// Do
Image(.circleFill)
```

<br>

---

<br>

### 💠 Use `ColorResource` Instead of `String` for Colors

**Why?**
> Using `ColorResource` ensures type safety, avoids typos, and leverages Swift’s compiler checks to catch errors early. This improves maintainability, readability, and prevents runtime errors when working with colors.

``` swift
// Don't
Color("blue.light")
```

``` swift
// Do
Color(.blueLight)
```

<br>

---

<br>

### 💠 Use `Text` `format` Parameter Instead of String Interpolation or Custom Formatters

**Why?**
> Using the `format` parameter with `Text` allows you to leverage Swift’s built-in formatting capabilities, ensuring consistent, locale-aware formatting and reducing the need for custom logic or string interpolation. It improves code clarity, maintainability, and ensures the proper handling of various data types like numbers, dates, and currencies.

**Tip:**
> When fetching date strings from a backend, convert the `String` to a `Date` in your model.

**Resource:**
> Check this website for all [SwiftUI Format Styles](https://goshdarnformatstyle.com/) in details and TONS of examples.

``` swift
// Don't
Text("\(12)%")
Text("\(500) EGP")
Text("\(dateFormatter.string(from: someDate))")
```

``` swift
// Do
Text(12, format: .percent) // 12%
Text(500, format: .currency(code: "egp")) // EGP 500
Text(.now, format: .dateTime.day().month(.abbreviated)) // 27 Apr
```

# SwiftUI Guidelines:

### 💠 Use `Label` Instead of `HStack` for Icon + Text Combinations

**Why?**
> `Label` is a semantic, built-in SwiftUI component specifically designed for pairing an icon with text. It improves accessibility, reduces boilerplate, and adapts better to dynamic type, right-to-left languages, and other system-driven layout changes.

``` swift
// Avoid
HStack(spacer: 6) {
    Image(systemName: "circle")
    Text("Task")
}
```

``` swift
// Use
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
// Avoid
HStack {
    Text("Price")
    Spacer()
    Text("500 EGP")
}
```

``` swift
// Use
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
// Avoid
SheetView {
    // Submit Action
} onDismiss: {
    // Dismiss Action
}
```

``` swift
// Use
SheetView(
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
// Avoid
.onAppear {
    viewOnAppear()
}
.onChange(of: state) { oldValue, newValue in
    // Action Code
}
```

``` swift
// Use
.onAppear(perform: viewOnAppear)
.onChange(of: state, stateDidChange)

private func viewOnAppear() {}
private func stateDidChange(_ oldValue: Int, newValue: Int) {}
```

<br>

---

<br>

### 💠 Avoid Using Outer Padding for Reusable Components

**Why?**
> Reusable components should focus only on their internal layout and content, not their external spacing. This keeps components flexible and adaptable to different contexts and gives the parent view full control over padding and layout.

``` swift
// Avoid
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
// Use
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
// Avoid
struct ReusableView: View {
    let title: String

    var body: some View {
        // Content
    }
}
```

``` swift
// Use
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

### 💠 Use `verbatim` Parameter for `Text` When Displaying Non-Localized Strings

**Why?**
> The `verbatim` parameter should be used when you need to display a non-localized string. By default, `Text` uses `LocalizedStringKey`, which is designed for localized strings. Using `verbatim` ensures that your string is treated as raw text and not mistakenly processed for localization and displayed in `Localizable` table.

``` swift
// Avoid
Text("\(product.title) - \(product.price)")
```

``` swift
// Use
Text(verbatim: "\(product.title) - \(product.price)")
```

<br>

---

<br>

### 💠 Use `ImageResource` Instead of `String` for Image Names

**Why?**
> Using `ImageResource` ensures type safety, avoids typos, and leverages Swift’s compiler checks to catch errors early. This improves maintainability, readability, and prevents runtime errors when working with images.

``` swift
// Avoid
Image("circle.fill")
```

``` swift
// Use
Image(.circleFill)
```

<br>

---

<br>

### 💠 Use `ColorResource` Instead of `String` for Colors

**Why?**
> Using `ColorResource` ensures type safety, avoids typos, and leverages Swift’s compiler checks to catch errors early. This improves maintainability, readability, and prevents runtime errors when working with colors.

``` swift
// Avoid
Color("blue.light")
```

``` swift
// Use
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
// Avoid
Text("\(12)%")
Text("\(500) EGP")
Text("\(dateFormatter.string(from: someDate))")
```

``` swift
// Use
Text(12, format: .percent) // 12%
Text(500, format: .currency(code: "egp")) // EGP 500
Text(.now, format: .dateTime.day().month(.abbreviated)) // 27 Apr
```

<br>

---

<br>

### 💠 Use Custom Modifiers Only When Necessary — Otherwise, Use `View` Extensions

**Why?**
> Custom modifiers should be used only when you need to work with SwiftUI state, environment properties, or manage complex view-specific logic. For simple visual modifications, creating a `View` extension function is a cleaner, simpler approach that improves readability and reduces unnecessary complexity in your code.

``` swift
// No need to be used as custom modifier
extension View {
    func roundedBorders(
        radius: CGFloat,
        color: Color,
        lineWidth: CGFloat = 1
    ) -> some View {
        self
            .clipShape(.rect(cornerRadius: radius))
            .overlay {
                RoundedRectangle(cornerRadius: radius)
                    .stroke(color, lineWidth: lineWidth)
            }
    }
}
```

``` swift
// Need to be used as custom modifier
struct ColorBackgroundModifier: ViewModifier {
    @Environment(\.colorScheme) private var colorScheme
    
    private var color: Color {
        colorScheme == .dark ? .blue : .cyan
    }
    
    func body(content: Content) -> some View {
        content
            .background(color)
    }
}
```

<br>

---

<br>

### 💠 Follow Consistent Naming Conventions for SwiftUI Views

**Why?**
> Using consistent naming conventions for SwiftUI views helps maintain clarity, improves code readability, and makes it easier to identify the purpose of a view at a glance. These naming rules allow developers to quickly differentiate between screens, reusable views, and smaller UI components.

<br>

1️⃣ When creating a SwiftUI screen (a top-level view that represents a full screen), use the `Screen` suffix to clearly indicate its role.

``` swift
// Avoid
OrderDetailsView()
CartView()
```

``` swift
// Use
OrderDetailsScreen()
CartScreen()
```

<br>

2️⃣ When creating a general reusable views that are not full screens but still serve as major components, use the `View` suffix.

``` swift
// Use
ProductCardView()
BannerView()
```

<br>

3️⃣ When creating a small, reusable UI components (such as buttons, labels, tags, and sections), omit the View suffix to keep the name clean and concise.

``` swift
// Avoid
UnderlinedButtonView()
SectionHeaderView()
```

``` swift
// Use
UnderlinedButton()
SectionHeader()
```

<br>

4️⃣ When creating small reusable components with names similar to built-in SwiftUI views (e.g., Slider, Picker), use the App prefix to distinguish your custom components from SwiftUI’s built-in ones.

``` swift
// Use
AppSlider()
AppPicker()
```

<br>

---

<br>

### 💠 When to Encapsulate a Component into Its Own `View` Struct?

**Why?**
> Encapsulating components into their own View structs improves code organization, readability, maintainability, and reusability.

**Apple's SwiftUI Team Says:**
> Since views are declarative descriptions, splitting a view into multiple smaller views does not impact performance, allowing developers to organize their code without compromising efficiency.

<br>

**1️⃣ When It Contains Complex Logic**
* If the view involves complex logic (such as multiple states, interactions, or calculations), encapsulating it into a separate View struct keeps your code cleaner and more modular.

<br>

**2️⃣ When the View Is Reusable**
* Encapsulate views into their own structs when they need to be reused in multiple places. This improves maintainability and ensures consistency across your app.

<br>

**3️⃣ When the View Code Is Big**
* If a view has a lot of code or multiple layers of nested views, it’s a good idea to encapsulate it. This makes the code easier to manage, debug, and maintain.

<br>

**4️⃣ When the View Needs to Handle Its Own State**
* If a view has state management or complex interactions (e.g., sliders, or toggles with unique behaviors), it’s a good practice to encapsulate it. This ensures the view has full control over its internal state without affecting other views.

<br>

**5️⃣ When the View Has Its Own Business Logic**
* If a view has its own small business logic (like fetching data, or reacting to local changes), it should encapsulate that logic inside itself rather than pushing it up to the parent view. This keeps the parent view simpler and the logic closely tied to the UI it affects.

<br>

**6️⃣ When You Need to Handle Animation or Transitions**
* For views that handle animations or complex transitions, encapsulating them helps organize animation logic separately, making it easier to tweak or reuse.

<br>

**7️⃣ When You Need to Apply Specific Modifiers to a View**
* If a view requires several specific modifiers (such as padding, background, or cornerRadius), encapsulating the view ensures that all the necessary modifiers are applied in a clean and reusable way.

<br>

---

<br>

### 🌟 Avoid Using `AnyView` — Prefer `@ViewBuilder` When Needed

**Why?**
> Using `AnyView` hides the real type of your SwiftUI views, making it harder for SwiftUI to optimize rendering, diffing, and animation. It forces SwiftUI to treat the view as opaque, losing all the benefits of type-safety, performance optimizations, and compile-time checks. Instead, use `@ViewBuilder` to return different view types while preserving SwiftUI’s ability to optimize.

**Apple's SwiftUI Team Says:**
> `AnyView` makes it hard for SwiftUI to optimize and identify each view because it sees the return value for all as `AnyView`, avoid it as much as possible, use `@ViewBuilder` if needed.

``` swift
// Avoid
func makeContent() -> AnyView {
    if isPremium {
        return AnyView(...)
    } else {
        return AnyView(...)
    }
}
```

``` swift
// Use
@ViewBuilder
func makeContent() -> some View {
    if isPremium {
        ...
    } else {
        ...
    }
}
```

<br>

---

<br>

### 🌟 Use Lazy Stacks With Scrollable Content

**Why?**
> When you have a scrollable view that contains a list of items, you should use lazy containers (`LazyVStack`, `LazyHStack`) instead of regular stacks. Lazy stacks only create the views currently needed for display, improving performance, memory usage, and scrolling smoothness, especially with large data sets.

``` swift
// Avoid
ScrollView {
    VStack {
        ForEach(0..<1000) { index in
            ...
        }
    }
}
```

``` swift
// Use
ScrollView {
    LazyVStack {
        ForEach(0..<1000) { index in
            ...
        }
    }
}
```

<br>

---

<br>

### 🌟 Prefer Modifying View Properties Instead of Splitting Views With `if-else`

**Why?**
> In SwiftUI, an `if-else` statement creates two separate view identities behind the scenes. This can cause unnecessary view invalidation, animation glitches, and state resets when toggling between the conditions. Instead, it’s better to modify view properties based on the condition while keeping the same view identity. SwiftUI can then differentiate and update the view efficiently.

**Apple's SwiftUI Team Says:**
> If else statement and inert modifier are forms of structure identity. However, From SwiftUI point of view, the `if-else` statement represents two different views with distinct identities. To make those views the same identity, we’d need to apply the condition in other ways, for example via an inert view modifier. Both of these strategies can work, but SwiftUI generally recommends the second approach. By default, try to preserve identity and provide more fluid transitions: this also helps preserve your view’s lifetime and state.

``` swift
// Avoid
if condition {
    contentView
        .foregroundStyle(.green)
        .frame(maxHeight: .infinity, alignment: .top)
} else {
    contentView
        .foregroundStyle(.red)
        .frame(maxHeight: .infinity, alignment: .bottom)
}
```

``` swift
// Use
contentView
    .foregroundStyle(condition ? .green : .red)
    .frame(
        maxHeight: .infinity,
        alignment: condition ? .top : .bottom
    )
```

<br>

---

<br>

### 🌟 Don’t Instantiate State Properties

**Why?**
> Dynamic property instantiation (e.g., creating a `@StateObject` or `initializing` @State values inside a view) can lead to slow updates, unexpected reinitializations, and inefficient UI rebuilding. Instead, inject dependencies and bind values using `@ObservedObject` or `@Binding` to make your views lighter, faster, and more predictable.

**Apple's SwiftUI Team Says:**
> Common sources of slow updates: Dynamic Property instantiation, such as allocating and initializing a state object or initializing state.

``` swift
// Avoid
struct ContentView: View {
    @StateObject var viewModel: ContentVM
}

struct ContentView: View {
    @State var age: Int
}
```

``` swift
// Use
struct ContentView: View {
    @ObservedObject var viewModel: ContentVM
}

struct ContentView: View {
    @Binding var age: Int
}
```

<br>

---

<br>

### 🌟 Avoid Expensive Computations Inside The View Body

**Why?**
> If a property is computed directly inside a view’s body, it can make the view expensive to evaluate. SwiftUI recomputes the body frequently, so heavy calculations inside it can cause slow UI updates, stuttering animations, and poor performance. Instead, precompute values outside the body using computed properties.

**Apple's SwiftUI Team Says:**
> Avoid inline filtering: `ForEach(dogs.filter(…))`. The inline filter here is linear over the collection. This might work in a prototype, but when the collection scales, this operation can quickly become expensive, leading to a slow update. It's better to move it out to the model.

**Apple's SwiftUI Team Says:**
> Common sources of slow updates:
> 1. Expensive view body: a dynamic property could be computed from a view’s body, making the view expensive to evaluate.
> 2. Make sure to check for expensive string interpolation or operations like data filtering and other work inside of body.

``` swift
// Avoid
struct ContentView: View {
    var body: some View {
        List(allItems.filter({$0.isActive})) { item in
            ...
        }
    }
}
```

``` swift
// Use
struct ContentView: View {
    private var filteredItems: [Item] {
        allItems.filter({$0.isActive}))
    }

    var body: some View {
        List(filteredItems) { item in
            ...
        }
    }
}
```

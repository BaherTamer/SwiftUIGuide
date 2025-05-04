# SwiftUI Guidelines:
SwiftUI is a powerful declarative framework, but writing efficient, maintainable, and performant code requires following best practices. This document provides guidelines to help you:
1. Improve code readability & maintainability
2. Enhance performance & reduce unnecessary re-renders
3. Follow Apple’s recommended SwiftUI patterns
4. Avoid common pitfalls that lead to bugs or sluggish UIs

These guidelines are based on Apple’s official SwiftUI team recommendations and real-world experience.

<br>

# Table of Contents
**Core SwiftUI Components & Best Practices**
* [Use `Label` Instead of `HStack` for Icon + Text Combinations](#use-label-instead-of-hstack-for-icon--text-combinations)
* [Use `LabeledContent` Instead of `HStack` for Label + Value Layouts](#use-labeledcontent-instead-of-hstack-for-label--value-layouts)
* [Use `LocalizedStringKey` for Reusable Components](#use-localizedstringkey-for-reusable-components)
* [Use `verbatim` Parameter for Non-Localized `Text`](#use-verbatim-parameter-for-non-localized-text)
* [Use `ImageResource` Instead of `String` for Image Names](#use-imageresource-instead-of-string-for-image-names)
* [Use `ColorResource` Instead of `String` for Colors](#use-colorresource-instead-of-string-for-colors)
* [Use `Text` `format` Parameter for Consistent Formatting](#use-text-format-parameter-for-consistent-formatting)
* [Apply Scrolling Only When Content is Large](#apply-scrolling-only-when-content-is-large)

<br>

**View Composition & Structure**
* [Avoid Using Outer Padding for Reusable Components](#avoid-using-outer-padding-for-reusable-components)
* [Follow Consistent Naming Conventions for SwiftUI Views](#follow-consistent-naming-conventions-for-swiftui-views)
* [When to Encapsulate a Component into Its Own `View` Struct?](#when-to-encapsulate-a-component-into-its-own-view-struct)
* [Avoid using `UIScreen` Bounds](#avoid-using-uiscreen-bounds)

<br>

**State Management & Data Flow**
* [How to Choose Property Keywords for Your `View`?](#how-to-choose-property-keywords-for-your-view)
* [Eliminate Unnecessary `View` Dependencies](#eliminate-unnecessary-view-dependencies)

<br>

**Performance Optimization**
* [Avoid Using `AnyView` — Prefer `@ViewBuilder`](#avoid-using-anyview--prefer-viewbuilder)
* [Use Lazy Stacks for Scrollable Content](#use-lazy-stacks-for-scrollable-content)
* [Avoid Expensive Computations Inside The View Body](#avoid-expensive-computations-inside-the-view-body)
* [Prefer Modifying View Properties Over `if-else` Splits](#prefer-modifying-view-properties-over-if-else-splits)
* [Use if Statements to Add or Remove Views Dynamically](#use-if-statements-to-add-or-remove-views-dynamically)
* [Avoid if Conditions Inside `ForEach`](#avoid-adding-if-conditions-inside-foreach)
* [Dynamic Data in `ForEach` Must Be `Hashable`](#dynamic-data-in-foreach-must-be-hashable)
* [Use Stable & Unique Identifiers for `ForEach`](#use-stable--unique-identifiers-for-foreach)
* [Don’t Instantiate State Properties](#dont-instantiate-state-properties)
* [Don’t Initialize `@ObservedObject`](#dont-initialize-observedobject)
* [Use `.task` for Long-Running Work in `ObservableObject`](#use-task-for-long-running-work-in-observableobject)
* [Avoid Escaping Closures for `View` Content](#avoid-escaping-closures-for-view-content)

<br>

**Code Clarity & Maintainability**
* [Prefer Explicit Parameters Over Trailing Closures for Callbacks](#prefer-explicit-parameters-over-trailing-closures-for-callbacks)
* [Prefer Passing Function Names Instead of Closures](#prefer-passing-function-names-instead-of-closures)
* [Use Custom Modifiers Only When Necessary](#use-custom-modifiers-only-when-necessary)
* [State Should Be Marked `private`](#state-should-be-marked-private)

<br>

# Resources

| Title | Link |
| ----- | ---- |
| SwiftUI Essentials 2019 | [Video Link](https://developer.apple.com/videos/play/wwdc2019/216) |
| Data Flow Through SwiftUI | [Video Link](https://developer.apple.com/videos/play/wwdc2019/226) |
| Building Custom View with SwiftUI | [Video Link](https://developer.apple.com/videos/play/wwdc2019/237) |
| Data Essentials in SwiftUI | [Video Link](https://developer.apple.com/videos/play/wwdc2020/10040) |
| Introduction to SwiftUI | [Video Link](https://developer.apple.com/videos/play/wwdc2020/10119) |
| Demystify SwiftUI | [Video Link](https://developer.apple.com/videos/play/wwdc2021/10022) |
| The Craft of SwiftUI API Design: Progressive Disclosure | [Video Link](https://developer.apple.com/videos/play/wwdc2022/10059) |
| Demystify SwiftUI Performance | [Video Link](https://developer.apple.com/videos/play/wwdc2023/10160) |
| SwiftUI Essentials 2024 | [Video Link](https://developer.apple.com/videos/play/wwdc2024/10150) |
| Don't Use Escaping Closures in SwiftUI | [Article Link](https://rensbr.eu/blog/swiftui-escaping-closures/) |

<br>

# Tools

| Title | Link | Description |
| ----- | ---- | ----------- |
| Format Styles | [Website Link](https://goshdarnformatstyle.com) | All SwiftUI format styles with examples |
| SwiftSafeUI | [GitHub Link](https://github.com/BaherTamer/SwiftSafeUI) - [Documentation Link](https://bahertamer.github.io/SwiftSafeUI/documentation/swiftsafeui/) | Encapsulates SwiftUI deprecation handling logic. |
| Inject | [GitHub Link](https://github.com/krzysztofzablocki/Inject) - [Tutorial Link](https://www.youtube.com/watch?v=lbsXuCYTpkY&t=297s) | Hot Reloading for Swift applications |

<br>

###### Made with 💙 by Baher Tamer

---

<br>

<!---------------------------------------------------------------------------------------------------------------------------->

# 💠 Core SwiftUI Components & Best Practices

<br>

### Use `Label` Instead of `HStack` for Icon + Text Combinations

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
// Prefer
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

### Use `LabeledContent` Instead of `HStack` for Label + Value Layouts

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
// Prefer
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

### Use `LocalizedStringKey` for Reusable Components

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
// Prefer
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

### Use `verbatim` Parameter for Non-Localized `Text`

**Why?**
> The `verbatim` parameter should be used when you need to display a non-localized string. By default, `Text` uses `LocalizedStringKey`, which is designed for localized strings. Using `verbatim` ensures that your string is treated as raw text and not mistakenly processed for localization and displayed in `Localizable` table.

``` swift
// Avoid
Text("\(product.title) - \(product.price)")
```

``` swift
// Prefer
Text(verbatim: "\(product.title) - \(product.price)")
```

<br>

---

<br>

### Use `ImageResource` Instead of `String` for Image Names

**Why?**
> Using `ImageResource` ensures type safety, avoids typos, and leverages Swift’s compiler checks to catch errors early. This improves maintainability, readability, and prevents runtime errors when working with images.

``` swift
// Avoid
Image("circle.fill")
```

``` swift
// Prefer
Image(.circleFill)
```

<br>

---

<br>

### Use `ColorResource` Instead of `String` for Colors

**Why?**
> Using `ColorResource` ensures type safety, avoids typos, and leverages Swift’s compiler checks to catch errors early. This improves maintainability, readability, and prevents runtime errors when working with colors.

``` swift
// Avoid
Color("blue.light")
```

``` swift
// Prefer
Color(.blueLight)
```

<br>

---

<br>

### Use `Text` `format` Parameter for Consistent Formatting

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
// Prefer
Text(12, format: .percent) // 12%
Text(500, format: .currency(code: "egp")) // EGP 500
Text(.now, format: .dateTime.day().month(.abbreviated)) // 27 Apr
```

<br>

---

<br>

### Apply Scrolling Only When Content is Large

> SwiftUI’s `.scrollBounceBehavior(.basedOnSize)` ensures that bounce effects are only applied when the content is larger than the container, making your scroll views feel more natural and efficient.

``` swift
// Not Optimal
ScrollView {
    VStack {
        // content
    }
}
```

``` swift
// Optimal
ScrollView {
    VStack {
        // content
    }
}
.scrollBounceBehavior(.basedOnSize)
```

---

<br>

<!---------------------------------------------------------------------------------------------------------------------------->

# 💠 View Composition & Structure

<br>

### Avoid Using Outer Padding for Reusable Components

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
// Prefer
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

### Follow Consistent Naming Conventions for SwiftUI Views

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
// Prefer
OrderDetailsScreen()
CartScreen()
```

<br>

2️⃣ When creating a general reusable views that are not full screens but still serve as major components, use the `View` suffix.

``` swift
// Prefer
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
// Prefer
UnderlinedButton()
SectionHeader()
```

<br>

4️⃣ When creating small reusable components with names similar to built-in SwiftUI views (e.g., Slider, Picker), use the App prefix to distinguish your custom components from SwiftUI’s built-in ones.

``` swift
// Prefer
AppSlider()
AppPicker()
```

<br>

---

<br>

### Avoid Using `UIScreen` Bounds

**Why?**
> Your view should not rely on `UIScreen.main.bounds` to determine its layout. Instead, design components to be flexible and adaptable to the space they are given. Also, `UIScreen.main.bounds` will not take into account any scaling or rotation of the device.
> 
> **Use instead layout tools like:**
> * `Spacer()` to naturally fill or push content
> * `GeometryReader` to adapt to available bounds
> * `frame(maxWidth: .infinity)` to grow within flexible containers

``` swift
// Avoid
.frame(width: UIScreen.main.bounds.width - 40)
```

``` swift
// Prefer
.frame(maxWidth: .infinity)
.padding(.horizontal, 20)

// Or
GeometryReader { proxy in
    VStack {
        ...
    }
    .frame(width: proxy.size.width * 0.9)
}
```

<br>

---

<br>

### When to Encapsulate a Component into Its Own `View` Struct?

**Why?**
> Encapsulating components into their own View structs improves code organization, readability, maintainability, and reusability.

**Apple's SwiftUI Team Says:**
> Since views are declarative descriptions, splitting a view into multiple smaller views does not impact performance, allowing developers to organize their code without compromising efficiency.

**Apple's SwiftUI Team Says:**
> **A general principle of SwiftUI:** Which is to prefer smaller, single-purpose views, because these kinds of simpler views are easier to understand and also easier to maintain over time. The entire SwiftUI framework is oriented around composition of small pieces and you should organize your code in the same way.

**Apple's SwiftUI Team Says:**
> You should never hesitate to re-factor your SwiftUI code because extracting a subview has virtually no runtime overhead.

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

---

<br>

<!---------------------------------------------------------------------------------------------------------------------------->

# 💠 State Management & Data Flow

<br>

### How to Choose Property Keywords for Your `View`?

**Why?**
> Choosing the right property wrapper (`let`, `var`, `@State`, `@Binding`, `ObservableObject`, etc.) ensures proper data flow, state management, and reusability in SwiftUI.

<br>

**1️⃣ Use `let` for Read-Only Data**
* When the view only displays data and does not modify it.
* Great for reusable components that receive static values.

<br>

**2️⃣ Use `@State` for View-Owned Mutable Data**
* When the view owns and manages its own transient state (e.g., toggle state, text input).
* Great for expandable toggles, text inputs, or button highlights.

<br>

**3️⃣ Use `@Binding` for Mutating Parent-Owned Values**
* When the view needs to modify data owned by a parent view or external source.
* Great for reusable components like switches or sliders.

<br>

**4️⃣ Use `ObservableObject` for External, Complex Data Models**
* When the data is controlled outside SwiftUI and your view observes the changes.
* Great for service or view model.
* Use `@StateObject` when the view owns the data model and creates it.
* Use `@ObservedObject` when the data model is created outside the view and the view observes the changes.

<br>

**5️⃣ Use `@EnvironmentObject` for Global or Deeply-Shared Models**
* When you need to avoid manually passing models through multiple views.

<br>

**Apple's SwiftUI Team Says:**
> When start on a new view in SwiftUI, there are three key questions to think about.
> 1. What data does this view need to do its job?
> 2. How will the view manipulate that data?
> 3. Where will the data come from?

<br>

**Apple's SwiftUI Team Says:**
> When you make your views into reusable components, you'll probably notice that most of the time when you're using data in your views, you probably don't need to mutate it. And so, read-only access is preferred when you can get away with that.

<br>

**Apple's SwiftUI Team Says:**
> One of the great uses of `State` that we have in our framework is `Button`. `Button` uses `State` to track whether the user is pressing on it and highlight appropriately. And what's great about using `State` for `Button` is that when you create a `Button`, you don't need to care about the highlight state. That is data that is truly owned by the `Button`. So when you're reaching for `State`, consider do I have a case that's like `Button`? And if you do, `State` might be a great tool. But if not, consider using one of the other powerful tools for using data in SwiftUI.

<br>

---

<br>

### Eliminate Unnecessary `View` Dependencies

**Why?**
> Passing entire models or complex data structures into child views make components harder to be predictable, testable, and easier to reuse. Instead, pass only the data the child view actually needs.

``` swift
// Avoid
struct ProductCard: View {
    let product: Product

    var body: some View {
        VStack(alignment: .leading) {
            Text(product.barCode)
            Text(product.title)
        }
    }
}
```

``` swift
// Prefer
struct ProductCard: View {
    let title: String
    let barcode: String

    var body: some View {
        VStack(alignment: .leading) {
            Text(barCode)
            Text(title)
        }
    }
}
```

---

<br>

<!---------------------------------------------------------------------------------------------------------------------------->

# 💠 Performance Optimization

<br>

### Avoid Using `AnyView` — Prefer `@ViewBuilder`

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
// Prefer
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

### Use Lazy Stacks for Scrollable Content

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
// Prefer
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

### Avoid Expensive Computations Inside The View Body

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
// Prefer
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

<br>

---

<br>

### Prefer Modifying View Properties Over `if-else` Splits

**Why?**
> In SwiftUI, an `if-else` statement creates two separate view identities behind the scenes. This can cause unnecessary view invalidation, animation glitches, and state resets when toggling between the conditions. Instead, it’s better to modify view properties based on the condition while keeping the same view identity. SwiftUI can then differentiate and update the view efficiently.

**Apple's SwiftUI Team Says:**
> If else statement and inert modifier are forms of structure identity. However, From SwiftUI point of view, the `if-else` statement represents two different views with distinct identities. To make those views the same identity, we’d need to apply the condition in other ways, for example via an inert view modifier. Both of these strategies can work, but SwiftUI generally recommends the second approach. By default, try to preserve identity and provide more fluid transitions: this also helps preserve your view’s lifetime and state.

**Apple's SwiftUI Team Says:**
> You should try to push your conditions into your modifiers as much as possible. Because that will help SwiftUI detect those changes and give you better animations. That if statement syntax really great if your intention is to actually add or remove views from your hierarchy. 

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
// Prefer
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

### Use if Statements to Add or Remove Views Dynamically

**Why?**
> Using an if statement to conditionally add or remove views creates a clear and stable view hierarchy that SwiftUI can easily manage and optimize. It’s better than hiding views with `opacity` or setting them empty manually using `EmptyView`.

**Tip:**
> To create smooth animations, combine the if condition with `animation` or `transition` modifiers.

``` swift
// Prefer
@ViewBuilder
func saleTag(_ value: Int?) -> some View {
    if let value {
        // Content
            .transition(...)
            .animation(...)
    }
}
```

<br>

---

<br>

### Avoid Adding if Conditions Inside `ForEach`

**Why?**
> This leads to dynamic view counts (either 0 or 1 view per item) to determine whether it's displayed or not. SwiftUI has to build all possible views just to figure out the identifiers and layout, which hurts performance, especially in large lists. Instead, filter the array first, then pass the filtered result into `ForEach`.

**Apple's SwiftUI Team Says:**
> It might be tempting to add a filter using a conditional view. The number of views became variable, it's either one or zero. This is bad because it results in list needing to build all the views to retrieve the row identifiers because it doesn't know how many views each element resolves to.

``` swift
// Avoid
struct ContentView: View {
    var body: some View {
        ...
        ForEach(allItems) { item in
            if item.isActive {
                ...
            }
        }
        ...
    }
}
```

``` swift
// Prefer
struct ContentView: View {
    private var filteredItems: [Item] {
        allItems.filter({$0.isActive}))
    }

    var body: some View {
        ...
        ForEach(filteredItems) { item in
            ...
        }
        ...
    }
}
```

<br>

---

<br>

### Dynamic Data in `ForEach` Must Be `Hashable`

**Why?**
> SwiftUI relies on each item’s hash value to assign a unique identity. This allows SwiftUI to efficiently detect changes and update only the necessary views instead of rebuilding the entire hierarchy.

**Apple's SwiftUI Team Says:**
> Any dynamic collection of data used in `ForEach` this property must be hashable because SwiftUI is going to use its value to assign an identity to all the views generated from the elements of the collection.

``` swift
// Prefer
struct User: Hashable {
    let id: String
    let name: String
}

ForEach(users, id: \.id) { user in
    ...
}
```

<br>

---

<br>

### Use Stable & Unique Identifiers for `ForEach`

**Why?**
> When using `ForEach`, you must provide a stable (not changing) and unique identifier for each element. This helps SwiftUI track view identity correctly. Avoid using `.self` or iterating over indices, as these often lead to unstable or duplicate Ids.

**Apple's SwiftUI Team Says:**
> Use stable unique identifiers for items in a collection to be used as id in `ForEach`. This ensures that animations look great, performance is smooth, and the dependencies of your hierarchy are reflected in the most efficient form.

``` swift
// Avoid
ForEach(users, id: \.self) { user in
    ...
}

ForEach(items.indices) { index in
    ...
}
```

``` swift
// Prefer
ForEach(users, id: \.email) { user in
    ...
}
```

<br>

---

<br>

### Don’t Instantiate State Properties

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
// Prefer
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

### Don’t Initialize `@ObservedObject`

**Why?**
> Initializing an object with @ObservedObject directly in a SwiftUI view causes the object to be recreated every time the view reloads.

``` swift
// Avoid
struct ContentView: View {
    @ObservedObject var data = DataProvider()
}
```

``` swift
// Prefer
struct ContentView: View {
    @StateObject private var data = DataProvider()
}

// Or
struct ContentView: View {
    @ObservedObject var data: DataProvider
}
```

<br>

---

<br>

### Use `.task` for Long-Running Work in `ObservableObject`

**Why?**
> Running long operations during initialization or synchronously inside a model can block the main thread, causing slow updates, blank screens, or UI delays.

**Apple's SwiftUI Team Says:**
> **Common sources of slow updates:** Not using `.task` modifier for a function that takes long time in `observableObject`.

``` swift
// Avoid
@MainActor
final class ProductListVM: ObservableObject {
    @Published var products: [Product] = []

    init() {
        loadProducts()
    }

    private func loadProducts() {
        // Takes a long time
    }
}

struct ProductListView: View {
    @StateObject private var viewModel = ProductListVM()

    var body: some View {
        ProductsListView()
    }
}
```

``` swift
// Prefer
@MainActor
final class ProductListVM: ObservableObject {
    @Published var products: [Product] = []

    func loadProducts() async {
        // Takes a long time
    }
}

struct ProductListView: View {
    @StateObject private var viewModel = ProductListVM()

    var body: some View {
        ProductsListView()
            .task {
                await viewModel.loadProducts()
            }
    }
}
```

<br>

---

<br>

### Avoid Escaping Closures for `View` Content

**Why?**
> When you store a closure that builds a view instead of evaluating it immediately and storing the resulting view, SwiftUI can’t optimize rendering efficiently because it can’t compare closures for equality. This breaks memoization and can even cause glitches when state changes due to state sync issues.

``` swift
// Avoid
struct Collapsable<Content: View>: View {
    @State private var collapsed = true
    let content: () -> Content // Escaping

    init(@ViewBuilder content: @escaping () -> Content) {
        self.content = content
    }

    var body: some View {
        VStack {
            if !collapsed {
                content() // evaluated dynamically
            }
            Button(collapsed ? "↓ open" : "↑ close") {
                collapsed.toggle()
            }
        }
    }
}
```

``` swift
// Prefer
struct Collapsable<Content: View>: View {
    @State private var collapsed = true
    let content: Content // Stored as value

    init(@ViewBuilder content: () -> Content) {
        self.content = content() // evaluated immediately
    }

    var body: some View {
        VStack {
            if !collapsed {
                content
            }
            Button(collapsed ? "↓ open" : "↑ close") {
                collapsed.toggle()
            }
        }
    }
}
```

---

<br>

<!---------------------------------------------------------------------------------------------------------------------------->

# 💠 Code Clarity & Maintainability

<br>

### Prefer Explicit Parameters Over Trailing Closures for Callbacks

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
// Prefer
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

### Prefer Passing Function Names Instead of Closures

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
// Prefer
.onAppear(perform: viewOnAppear)
.onChange(of: state, stateDidChange)

private func viewOnAppear() {}
private func stateDidChange(_ oldValue: Int, newValue: Int) {}
```

<br>

---

<br>

### Use Custom Modifiers Only When Necessary

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

### State Should Be Marked `private`

**Why?**
> This makes it clear what external data the view depends on, versus what it creates by itself, and makes code auto completion of the generated initializer more accurate.

``` swift
// Prefer
@State private var item = Item()
@StateObject private var object = Object()
@AppStorage("flag") private var flag = false
@Environment(\.colorScheme) private var colorScheme
@EnvironmentObject private var globalObject: GlobalObject
```

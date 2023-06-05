---
layout: post
title: SwiftUI 前端
date: 2023-05-28 02:17 +0000
---

## SwiftUI 相比于 UIKit 的优势

SwiftUI 不仅用于 iOS，也可以应用在其他的 Apple 平台。在 SwiftUI 推出之前，你需要使用平台特定的 UI 框架来开发使用者介面，例如：开发 macOS App 要使用 AppKit，开发 tvOS App 要使用 TVUIKit，开发 watchOS App 要使用 WatchKit。

在 SwiftUI 中的同一段程序，可以产生不同的 UI 控制。举例而言，以下这段程序为声明一个切换开关：

```swift
Toggle(isOn: $isOn) {
    Text("Toggle")
}
```

在 iOS 上，这段程序会产生一个 UISwitch 控制。在 macOS 上，这段程序会产生一个 NSButton 控制。在 watchOS 上，这段程序会产生一个 WKInterfaceSwitch 控制。在 tvOS 上，这段程序会产生一个 UISwitch 控制。

因此，对于 iOS 与 iPadOS，Toggle 会渲染为开关。而在 macOS 上，SwiftUI 会将这个控制组件（control ）渲染为核取方块样式。


**UIKit 与 SwiftUI 的兼容**：若你有一个视图是使用 UIKit 开发，你可以采用 UIViewRepresentable 协议，来让它相容 SwiftUI。

## 文字处理

你可以参考[这份文件](https://developer.apple.com/documentation/swiftui/font)来找出所有 font 修饰器所支持的值。

你也可以使用 font 修饰器来指定字体设计，譬如说，你想要字体圆润。你可以将修饰器撰写如下：
```swift
.font(.system(.largeTitle, design: .rounded))
```
这里你指定使用系统字体，文字样式为 largeTitle ，以及 rounded 设计。预览画布应该会立即对修改做出反应，并显示圆润的文字。

字体大小：
```swift
.font(.system(size: 24))
```

字体颜色：
```swift
.font(.system(.largeTitle, design: .rounded))
.foregroundColor(.red)
```

Text 默认支持多行文字，要将文字居中对齐，可插入 `multilineTextAlignment` 修饰器，并设定值为 `.center`：
```swift
.multilineTextAlignment(.center)
```

Text 也支持设定行数，若你想要限制文字只显示一行，可以插入 `lineLimit` 修饰器，并设定值为 `1`：
```swift
.lineLimit(1)
```

Text 默认显示多行文字的原因正是 SwiftUI 框架默认 `lineLimit` 修饰器 为 `nil`，你可以将 `.lineLimit` 设定为 `nil` 来看一下结果：
```swift
.lineLimit(nil)
```

Text 也支持设定行间距，若你想要增加行间距，可以插入 `lineSpacing` 修饰器，并设定值为 `10`：
```swift
.lineSpacing(10)
```

系统默认设定是截断字尾。要修改文字的截断模式，你可以使用 `truncationMode` 修饰器，并设定它的值为 `.head` 或 `.middle` ，如下所示：
```swift
.truncationMode(.head)
```

Text 也支持设定文字的最大宽度，若你想要限制文字的最大宽度，可以插入 `fixedSize` 修饰器，并设定值为 `true`：
```swift
.fixedSize(horizontal: true, vertical: true)
```

文字的旋转：使用 `rotateEffect` 修饰器，并传入旋转角度：
```swift
.rotationEffect(.degrees(45))
```

以特定点来旋转：
```swift
.rotationEffect(.degrees(20), anchor: UnitPoint(x: 0, y: 0))
```

你不止可以进行 2D 旋转， SwiftUI 提供一个称作 `rotation3DEffect` 修饰器，可以让你建立 3D 效果。这个修饰器有两个参数：“旋转角度”与“旋转轴”，譬如你要建立透视文字特效，程序可以这样写：
```swift
.rotation3DEffect(.degrees(60), axis: (x: 1, y: 0, z: 0))
```

你还可以插入以下这行程序来对透视文字建立阴影效果
```swift
.shadow(color: .gray, radius: 2, x: 0, y: 15)
```
这个 shadow 修饰器将会对文字应用阴影效果，你只需要指定颜色与阴影半径。另外，你也可以告诉系统 x 与 y 值来指定阴影位置。

你可以使用 `padding` 修饰器来设定文字的内外边距：
```swift
.padding(20)
```

在默认情况下，所有文字都是使用系统字体显示的。 假设你在 [Google Fonts](https://fonts.google.com/specimen/Nunito) 上找到了一种免费字体。那如何在 App 中使用自订字体？

假设你已经下载了字体文件，跟着就要将字体档加到你的 Xcode 项目中。 方法很简单，只要将字体档拖到项目导航器中并将它们放进你的项目文件夹（这里示例是 SwiftUIText）下。 字体文件一般是 `.ttf`（例如 Nunito-Regular.ttf）。 如需使用粗体或斜体，请添加相应的文件。

添加字体后，Xcode 即显示一个选项框。 请确保你选取了 Copy items if needed 和你的 target。

添加字体文件后，还未能直接使用字体。 Xcode 要求开发者在项目配置中设定字体。 在项目导航器中，先选择 SwiftUIText，然后点击 Targets 下的 SwiftUIText。 选择显示项目配置的 Info 选项。

你可以右点 Bundle 并选择 Add row。 将 Key 设置为“Fonts provided by application”。 接下来，点击披露指示器以展开条目。 在 item 0，将值设置为 Nunito-Regular.ttf，这是你刚刚添加的字体档。 如果你有多个字体档，你可以点击“+”按钮添加另一个项目。

使用：
```swift
.font(.custom("Nunito-Regular", size: 24))
```

上面的代码没有使用系统字体样式（只需要 `.` 例如 `.title`），而是使用 .custom 并指定字体名称（需要输入字体名字例如 `.custom("meko-font")`）。 字体名称可以在应用程序“Font Book”中找到。

SwiftUI 内置了对 Markdown 的支持。也就是说，你可以在 Text 中使用 Markdown 语法，例如：
```swift
Text("This is **bold**")
```

## 图片处理

SwiftUI 提供了一个 Image 视图，可以用来显示图片。 Image 视图的使用方式与 Text 视图类似，你可以在 Image 视图中插入图片名称，如下所示：
```swift
Image("swiftui")
```

## 堆叠布局

SwiftUI 中使用自动布局，所有东西都是堆叠，包括了 VStack、HStack 与 ZStack。

- HStack 水平排列视图。
- VStack 垂直排列视图。
- ZStack 在一个视图重叠在其他视图之上。

.padding([.top, .horizontal])指定要添加的填充方向。使用[.top, .horizontal]作为参数，将在顶部和水平方向（左右两侧）添加填充。

# CardView 和 ScrollView

如果将多个卡片视图放入 VStack 中，那么这些卡片视图将被挤压，以填满屏幕，因为 VStack 是不可滚动的。

要解决这个问题，可以将 VStack 包装在 ScrollView 中。这样，VStack 将会自动滚动，以便用户可以查看所有内容。
```swift
ScrollView {
    VStack {
        CardView()
        CardView()
        CardView()
    }
}
```

## 按钮

```swift
Button {
    // 所需运行的内容
} label: {
    // 按钮的外观设定
}
```

所需运行的内容加不加 action 都是一样的

```swift
Button{
    print("Hello")
}label: {
    ButtonView(buttonText: "Meko", color: .indigo)
}
Button(action: {
    print("Hello Meko")
}){
    ButtonView(buttonText: "Meko", color: .indigo)
}
Button(action: {
    print("Delete button tapped!")
}) {
    Image(systemName: "trash")
        .font(.largeTitle)
        .foregroundColor(.red)
}
```

可以使用 Label 来替代 HStack 来实现同样的布局：

```swift
Button(action: {
    print("Delete button tapped!")
}) {
    HStack(spacing: 10) {
        Image(systemName: "trash")
            .font(.largeTitle)
        Text("Delete")
            .fontWeight(.semibold)
            .font(.title)
    }
    .foregroundColor(.red)
}

Button(action: {
    print("Delete button tapped!")
}) {
    Label("Delete", systemImage: "trash")
        .font(.largeTitle)
        .foregroundColor(.red)
}
```

SwiftUI 框架有几个内建的渐层效果，上列的代码从左（ .leading ）至右（ .trailing ）应用了线性渐层，其从左侧的红色开始，至右侧的蓝色结束：

```swift
LinearGradient(gradient: Gradient(colors: [.red, .blue]), startPoint: .leading, endPoint: .trailing)
```

```swift
Button(action: {
    print("Delete button tapped!")
}) {
    HStack(spacing: 10) {
        Image(systemName: "trash")
            .font(.largeTitle)
        Text("Delete")
            .fontWeight(.semibold)
            .font(.title)
    }
    .padding()
    .foregroundColor(.white)
    .background(LinearGradient(gradient: Gradient(colors: [.red, .blue]), startPoint: .leading, endPoint: .trailing))
    .cornerRadius(40)
}
```

## State

在 action 闭包（closure ）中，我们调用 toggle() 方法来将布林值从 false 切换为 true，或者从 true 切换为 false。


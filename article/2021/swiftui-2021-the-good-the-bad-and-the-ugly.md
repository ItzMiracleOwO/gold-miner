> * 原文地址：[SwiftUI in 2021: The Good, the Bad, and the Ugly](https://betterprogramming.pub/swiftui-2021-the-good-the-bad-and-the-ugly-458c6ee768f9)
> * 原文作者：[Chrys Bader](https://medium.com/@chrysb)
> * 译文出自：[掘金翻译计划](https://github.com/xitu/gold-miner)
> * 本文永久链接：[https://github.com/xitu/gold-miner/blob/master/article/2021/swiftui-2021-the-good-the-bad-and-the-ugly.md](https://github.com/xitu/gold-miner/blob/master/article/2021/swiftui-2021-the-good-the-bad-and-the-ugly.md)
> * 译者：[ItzMiracleOwO](https://github.com/itzmiracleowo)
> * 校对者：

# 2021 的 SwiftUI : 好处、坏处以及丑处

> 有生产力的 SwiftUI? **依旧不行.**

![由 [Maxwell Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 发布的图片](https://cdn-images-1.medium.com/max/12000/0*Wexfei50ZXms6ocU)

过去 8 个月我一直在用 SwiftUI 开发复杂的应用程序, 包括我们最近发布到 AppStore 的应用 —— [Fave](https://apps.apple.com/us/app/fave-close-friends-only/id1541952688)。尽管我遇到了无数此的限制，但我仍然找到了大多数问题的解决方法。

简而言之，SwiftUI 是一个美妙且非常有前途的框架。我认为 SwiftUI 就是未来。但是，要达到与 UIKit 相同的可靠性和稳健性，可能还需要 3 到 5 年的时间。但是，这并不意味着你今天不应该使用 SwiftUI。我的目标是帮助你权衡其优点和缺点，以便你可以就 SwiftUI 是否适合你的下一个项目做出更明智的决定。

## 优点

### 1.SwiftUI 編寫起來很有趣，而且你可以十分快速地进行構建

`addSubview` 和 `sizeForItemAtIndexPath` 的日子已经一去不复返了，它们苦苦计算帧、与约束搏斗以及手动构建视图层次结构。SwiftUI 的声明式和反应式设计模式使创建响应式布局和 React 一样简单，而且他还使用了 Apple 强大的 UIKit。他构建视图并开始运行的速度非常快。

### 2. SwiftUI 简化了跨平台开发

我最兴奋的一件事是，你可以编写一次 SwiftUI，然后在 iOS（iPhone 和 iPad）、WatchOS 和 macOS 上使用它。虽然你必须为 Android 和 Windows 开发和维护单独的代码库，这已经够麻烦了，但是任何一点的简化都有助于减少不同代码库的数量。还有一些缺点，我将在 '缺点' 的部分分享。

### 3. 你可以免费获得漂亮的过渡动画、动画和组件

你可以将 SwiftUI 视为一个实际提供了制作具有专业外观的应用程序所需的所有构建模块的 UI 套件。如果你熟悉 CSS 轉場，SwiftUI 有自己的版本，可以非常轻松地创建精美的交互。声明式语法的美妙之处在于事情能够 "正常工作" 并且看起来很神奇，但他也有一些缺点，我也会在后面介绍。

### 4. UI 完全是状态驱动和反应式的

如果你熟悉 React，SwiftUI 的工作原理是一样的。当你观看整个 UI "反应"、动画和所有内容时，你将会高兴地更改 `@State`、`@Binding` 和 `@Published` 属性，而不是回调地狱。你可以利用 `Combine` 与 `ObservableObject` 和 `@StateObject` 的强大功能。这方面是 UIKit 最酷的变化之一，而且它非常强大。

### 5. 社区正在拥抱 SwiftUI

几乎每个人都对 SwiftUI 感到兴奋。有很多资源可用于学习 SwiftUI，从 WWDC，到书籍，再到博客 —— 信息都在那里，你只需要搜索它。或者，我在这里汇总了一份最佳社区资源列表。

拥有一个充满活力和支持的社区将加速学习和开发，大量新库的出现将使 SwiftUI 更加通用。

## The Bad

### 1. Not everything is available in SwiftUI yet

There are a number of components that are missing, incomplete, or overly simple, some of which I’ll go into more detail below.

There is a solution by using`UIViewRepresentable` `UIViewControllerRepresentable` and `UIHostingController`. The former two allow you to embed UIKit views and controllers into a SwiftUI view hierarchy. The latter allows you to embed a SwiftUI view in UIKit. The same three exist for Mac development (`NSViewRepresentable`, etc).

These bridges are a great stopgap to make up for the missing functionality that SwiftUI has, but it’s not always a seamless experience. Furthermore, while the cross-platform promise of SwiftUI is great, if something’s not available you’ll still need to implement bridge code twice for iOS and Mac.

### 2. NavigationView 还没有真正存在

如果你想隐藏导航栏并且仍然可以使用滑动手势，你不能这么做。迫于无奈，我最后参考了一些我找到的代码，并制作了一个 [UINavigationController 包装器](https://gist.github.com/chrysb/d7d85e20d8c94fd3e0b753a4abd1c941)。他可以正常工作但是这不是一个长远的解决办法。

If you want to have a SplitView on iPad, you can’t show the master and detail view together yet in portrait mode. They chose an awkward use of a button to reveal a drawer, which is closed by default. Apparently, you can solve this problem by adding padding, which highlights the kind of things you have to do when wrangling SwiftUI.

`NavigationLink` is kind of funky when you want to navigate programmatically. Here’s an [interesting discussion](https://forums.swift.org/t/implementing-complex-navigation-stack-in-swiftui-and-the-composable-architecture/39352).

### 3. Text input is very limited

`TextField` and `TextEditor` are too simplistic right now, and you’ll end up falling back on UIKit. I had to build my own `UIViewRepresentable` for `UITextField` and `UITextView` (with auto-growing support). The gists are below:

### 4. The compiler struggles

When your view gets a little heftier and you’ve factored it out the best you can, the compiler can still huff and puff, telling you:

> The compiler couldn’t type-check this expression in a reasonable amount of time…

This has slowed me down a number of times. I’ve gotten good at commenting out code and narrowing it down to the line that’s causing the issue, but it feels really backward to be debugging code like this in 2021.

### 5. matchedGeometryEffect

When I first discovered [this](https://developer.apple.com/documentation/swiftui/view/matchedgeometryeffect(id:in:properties:anchor:issource:)), I thought it was amazing. It’s supposed to help you transition two views with different identities more seamlessly by matching their geometry as one appears and another disappears. I thought this would help make beautiful transitions from View A to View B.

I kept wanting it to work. Ultimately, I stay away from this because it’s imperfect and it causes crashes when you use it in a `List` or `ScrollView` with a lot of items. I would only recommend using this for simple transitions within the same view. Things start to get weird when you’re sharing a namespace between multiple distinct views, including view clipping during transitions.

### 6. Gestures are limited

SwiftUI comes with a new set of gestures (i.e. `DragGesture`, `LongPressGesture`) that can be added to a view using the `gesture` convenience modifiers like `tapGesture` and `longPressGesture`. They work okay until you want to do more complex interactions.

For example, `DragGesture` doesn’t interact well with `ScrollView`. Putting one within a ScrollView prevents scrolling, even with the `simultaneousGesture` modifier. In other situations, a drag gesture can get canceled without any notification, leaving gestures in an incomplete state.

To solve this, I built my own `GestureView` which allows me to use UIKit gestures in SwiftUI. I’ll share this in my next post about the best SwiftUI libraries and workarounds.

### 7. SwiftUI in Share Extension

I could be wrong, but Share Extensions still use UIKit. I built a share extension using SwiftUI by leveraging `UIHostingController` and there was a noticeable delay when the share extension loaded, creating a poor user experience. You can try to mask it by animating the view in, but it still has about a 500ms delay.

### Honorable mentions

* No access to the status bar (can’t change the color or intercept taps)
* `@UIApplicationDelegateAdaptor` required as `App` is still lacking
* No backward compatibility
* `UIVisualEffectsView` causes scroll lag in \< iOS15 (h/t @[AlanPegoli](https://twitter.com/alanpegoli?lang=en))

## The Ugly

### 1. ScrollView

This is one of the biggest drawbacks to date. Anyone who has built a more bespoke iOS app knows how much we rely on ScrollView to power interaction.

* **Major deal-breaker:** `LazyVStack` within a [ScrollView causes stutters, jitters, and unexpected behavior](https://stackoverflow.com/questions/66523786/swiftui-putting-a-lazyvstack-or-lazyhstack-in-a-scrollview-causes-stuttering-a/67895804). LazyVStacks are critical for long lists of mixed content that need to scroll, like a news feed. **This alone makes SwiftUI not production-ready:** Apple has confirmed to me that this is a bug in SwiftUI. It’s unclear when they’ll fix it, but when they do, it’ll be a big win.
* **Scroll state:** No native support for understanding the state of scrolling (is the scroll view dragging? scrolling? what’s the offset?). Though there are some workarounds for this, but they can be finicky and unstable.
* **Paging:** No native support for paging scroll views. So, forget about doing something like a swipeable media gallery (but use `[SwiftUIPager](https://github.com/fermoya/SwiftUIPager)` if you want something close). You can technically use `TabView` with `PageTabViewStyle`, but I think it’s more intended for a few elements and not large datasets.
* **Performance:** Using a `List`is the most performant and avoids the stuttering issue that `LazyVStack` has, but is still not ideal for showing variably sized content due to the way transitions work. For example, when building a chat view, the transitions are weird and clip the children, and you have no control over the insertion animation style.

## The Verdict

I think you should definitely learn SwiftUI, understand it for yourself, and experience the joy. Just hold off on fully adopting it.

SwiftUI is more than ready for simple applications, but at the time of this writing (iOS 15, beta 4), I don’t think SwiftUI is production-ready yet for complex applications, mainly due to the issues with `ScrollViews` and the heavy reliance on `UIViewRepresentable`. It breaks my heart. Particularly for things like messaging products, news feeds, and products that rely heavily on complex views or want to create gesture-driven bespoke experience will not want to use SwiftUI just yet.

If you want fine-grain control and limitless possibilities, I recommend sticking to UIKit for the foreseeable future. You can still reap the benefit of SwiftUI for some views (like settings pages) by using `UIHostingController` to include SwiftUI views.

## What Does The Future Behold?

As we begin to embark on the next big iteration of our project. I know the scope of interactions for this new project is outside of the scope of what SwiftUI currently supports. It breaks my heart to know that SwiftUI falls short in some critical ways, but I’m not quite ready to go back to UIKit, knowing how much of a joy it is to build in SwiftUI when it works. It’s just so much faster.

Will SwiftUI ever match UIKit? If so, we’re looking at maybe another 3–5 years to port over all of the essential UIKit APIs. If not, then you’ll always be able to drop down into UIKit and wrap it with SwiftUI.

What I’m curious about is how invested Apple is in SwiftUI. Is their long-term plan to have all developers fully adopt SwiftUI, or will it just become another Interface Builder? I really hope not. I hope that they go all in on SwiftUI because the promise of it is just amazing.

## More Perspectives

* [Is SwiftUI Ready?](https://www.jessesquires.com/blog/2021/07/01/is-swiftui-ready/)
* [SwiftUI Drawbacks: Why SwiftUI Is Not Ready for Production Yet](https://www.iosapptemplates.com/blog/swiftui/swiftui-drawbacks)
* [My takeaway from working with SwiftUI](https://link.medium.com/isXKLhaaCib)

> 如果发现译文存在错误或其他需要改进的地方，欢迎到 [掘金翻译计划](https://github.com/xitu/gold-miner) 对译文进行修改并 PR，也可获得相应奖励积分。文章开头的 **本文永久链接** 即为本文在 GitHub 上的 MarkDown 链接。

---

> [掘金翻译计划](https://github.com/xitu/gold-miner) 是一个翻译优质互联网技术文章的社区，文章来源为 [掘金](https://juejin.im) 上的英文分享文章。内容覆盖 [Android](https://github.com/xitu/gold-miner#android)、[iOS](https://github.com/xitu/gold-miner#ios)、[前端](https://github.com/xitu/gold-miner#前端)、[后端](https://github.com/xitu/gold-miner#后端)、[区块链](https://github.com/xitu/gold-miner#区块链)、[产品](https://github.com/xitu/gold-miner#产品)、[设计](https://github.com/xitu/gold-miner#设计)、[人工智能](https://github.com/xitu/gold-miner#人工智能)等领域，想要查看更多优质译文请持续关注 [掘金翻译计划](https://github.com/xitu/gold-miner)、[官方微博](http://weibo.com/juejinfanyi)、[知乎专栏](https://zhuanlan.zhihu.com/juejinfanyi)。

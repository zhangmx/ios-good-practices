<!-- 
iOS Good Practices
==================

_Just like software, this document will rot unless we take care of it. We encourage everyone to help us on that – just open an issue or send a pull request!_

Interested in other mobile platforms? Our [Best Practices in Android Development][android-best-practices] and [Windows App Development Best Practices][windows-app-development-best-practices] documents have got you covered.

[android-best-practices]: https://github.com/futurice/android-best-practices
[windows-app-development-best-practices]: https://github.com/futurice/windows-app-development-best-practices

-->

iOS 最佳实践
==================

_就像软件一样，除非我们维护这份文档，否则它也会过时。我们鼓励每个人帮助我们做到这一点 - 只需打开一个问题或发送一个拉取请求即可！_

对其他移动平台感兴趣吗？我们的[《Android 开发最佳实践》][android-best-practices]和[《Windows 应用开发最佳实践》][windows-app-development-best-practices]文档已经为您准备好了。 

[android-best-practices]: https://github.com/futurice/android-best-practices
[windows-app-development-best-practices]: https://github.com/futurice/windows-app-development-best-practices

<!-- 
## Why?

Getting on board with iOS can be intimidating. Neither Swift nor Objective-C are widely used elsewhere, the platform has its own names for almost everything, and it's a bumpy road for your code to actually make it onto a physical device. This living document is here to help you, whether you're taking your first steps in Cocoaland or you're curious about doing things "the right way". Everything below is just suggestions, so if you have a good reason to do something differently, by all means go for it!

-->

## 啥?

加入 iOS 开发可能会让人感到畏惧。Swift 和 Objective-C 都不是广泛使用的语言，平台几乎为所有事物都有自己的命名，而且要让你的代码真正运行在物理设备上是一条坎坷之路。这份活生生的文档旨在帮助你，不管你是正在迈出在 Cocoaland 的第一步，还是好奇于寻找“正确的做事方式”。下面的所有内容都只是建议，所以如果你有充分的理由以不同的方式做某事，那么尽管去做吧！

<!-- 
## Contents

If you are looking for something specific, you can jump right into the relevant section from here.

1. [Getting Started](#getting-started)
1. [Common Libraries](#common-libraries)
1. [Architecture](#architecture)
1. [Stores](#stores)
1. [Assets](#assets)
1. [Coding Style](#coding-style)
1. [Security](#security)
1. [Diagnostics](#diagnostics)
1. [Analytics](#analytics)
1. [Building](#building)
1. [Deployment](#deployment)
1. [In-App Purchases (IAP)](#in-app-purchases-iap)
1. [License](#license)

-->

## 内容

如果你正在寻找特定的内容，你可以直接从这里跳转到相关部分。

1. [入门](#入门)
1. [常用库](#常用库)
1. [架构](#架构)
1. [存储](#存储)
1. [资源](#资源)
1. [编码风格](#编码风格)
1. [安全](#安全)
1. [诊断](#诊断)
1. [分析](#分析)
1. [构建](#构建)
1. [部署](#部署)
1. [应用内购买（IAP）](#应用内购买iap)
1. [许可证](#许可证)

<!-- 
## Getting Started

### Human Interface Guidelines

If you're coming from another platform, do take some time to familiarize yourself with Apple's [Human Interface Guidelines][ios-hig] for the platform. There is a strong emphasis on good design in the iOS world, and your app should be no exception. The guidelines also provide a handy overview of native UI elements, technologies such as 3D Touch or Wallet, and icon dimensions for designers.

[ios-hig]: https://developer.apple.com/ios/human-interface-guidelines/

-->

## 入门

### 人机界面指南

如果您来自另一个平台，请花些时间熟悉苹果公司为该平台制定的[人机界面指南][ios-hig]。在iOS世界中，非常重视良好的设计，您的应用也应当不例外。这些指南还为设计师提供了本地UI元素、如3D Touch或Wallet等技术，以及图标尺寸的便捷概览。

[ios-hig]: https://developer.apple.com/ios/human-interface-guidelines/

<!-- 
### Xcode

[Xcode][xcode] is the IDE of choice for most iOS developers, and the only one officially supported by Apple. There are some alternatives, of which [AppCode][appcode] is arguably the most famous, but unless you're already a seasoned iOS person, go with Xcode. Despite its shortcomings, it's actually quite usable nowadays!

To install, simply download [Xcode on the Mac App Store][xcode-app-store]. It comes with the newest SDK and simulators, and you can install more stuff under _Preferences > Downloads_.

[xcode]: https://developer.apple.com/xcode/
[appcode]: https://www.jetbrains.com/objc/
[xcode-app-store]: https://itunes.apple.com/us/app/xcode/id497799835

-->

### Xcode

[Xcode][xcode] 是大多数iOS开发者的首选集成开发环境（IDE），也是苹果官方唯一支持的。虽然有一些替代品，其中[AppCode][appcode]可能是最著名的，但除非你已经是一个经验丰富的iOS开发者，否则建议使用Xcode。尽管它有一些缺点，但现在实际上是相当好用的！

要安装，只需在[Mac App Store 上下载Xcode][xcode-app-store]。它附带了最新的SDK和模拟器，你还可以在_首选项 > 下载_下安装更多东西。

[xcode]: https://developer.apple.com/xcode/
[appcode]: https://www.jetbrains.com/objc/
[xcode-app-store]: https://itunes.apple.com/us/app/xcode/id497799835

<!-- 

### Project Setup

A common question when beginning an iOS project is whether to write all views in code or use Interface Builder with Storyboards or XIB files. Both are known to occasionally result in working software. However, there are a few considerations:
-->

### 项目设置

开始iOS项目时一个常见的问题是，是使用代码编写所有视图，还是使用Interface Builder配合故事板或XIB文件。这两种方法都有可能偶尔生成可运行的软件。然而，有一些考虑因素：

<!-- 

#### Why code?
* Storyboards are more prone to version conflicts due to their complex XML structure. This makes merging much harder than with code.
* It's easier to structure and reuse views in code, thereby keeping your codebase [DRY][dry].
* All information is in one place. In Interface Builder you have to click through all the inspectors to find what you're looking for.
* Storyboards introduce coupling between your code and UI, which can lead to crashes e.g. when an outlet or action is not set up correctly. These issues are not detected by the compiler.

[dry]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

-->

#### 为什么选择代码？

* 由于故事板的复杂XML结构，它们更容易引起版本冲突。这使得与代码相比，合并变得更加困难。
* 在代码中结构化和复用视图更容易，从而保持你的代码库[DRY（不要重复自己）][dry]。
* 所有信息都在一个地方。在Interface Builder中，你必须点击所有的检查器才能找到你正在寻找的内容。
* 故事板引入了代码和UI之间的耦合，这可能导致崩溃，例如，当一个出口或动作没有正确设置时。这些问题不会被编译器检测到。

[dry]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

<!-- 

#### Why Storyboards?
* For the less technically inclined, Storyboards can be a great way to contribute to the project directly, e.g. by tweaking colors or layout constraints. However, this requires a working project setup and some time to learn the basics.
* Iteration is often faster since you can preview certain changes without building the project.
* Custom fonts and UI elements are represented visually in Storyboards, giving you a much better idea of the final appearance while designing.
* For [size classes][size-classes], Interface Builder gives you a live layout preview for the devices of your choice, including iPad split-screen multitasking.

[size-classes]: http://futurice.com/blog/adaptive-views-in-ios-8

-->

#### 为什么选择故事板？
* 对于技术能力较弱的人来说，故事板可以是直接参与项目的好方法，例如，通过调整颜色或布局约束。然而，这需要一个工作中的项目设置和一些时间来学习基础知识。
* 迭代通常更快，因为你可以在不构建项目的情况下预览某些更改。
* 在故事板中，自定义字体和UI元素在视觉上得到了呈现，这让你在设计时对最终外观有了更好的认识。
* 对于[尺寸类][size-classes]，Interface Builder为你提供了所选设备的实时布局预览，包括iPad分屏多任务处理。

[size-classes]: https://developer.apple.com/design/human-interface-guidelines/layout


<!-- 

#### Why not both?
To get the best of both worlds, you can also take a hybrid approach: Start off by sketching the initial design with Storyboards, which are great for tinkering and quick changes. You can even invite designers to participate in this process. As the UI matures and reliability becomes more important, you then transition into a code-based setup that's easier to maintain and collaborate on.

-->

#### 为什么不两者兼顾？
为了兼得两个世界的优点，你也可以采取一种混合方法：首先用故事板草绘初始设计，这对于调整和快速更改来说非常有用。你甚至可以邀请设计师参与这一过程。随着UI的成熟和可靠性变得更加重要，你然后过渡到基于代码的设置，这样更易于维护和协作。

<!-- 

### Ignores

A good first step when putting a project under version control is to have a decent `.gitignore` file. That way, unwanted files (user settings, temporary files, etc.) will never even make it into your repository. Luckily, GitHub has us covered for both [Swift][swift-gitignore] and [Objective-C][objc-gitignore].

[swift-gitignore]: https://github.com/github/gitignore/blob/master/Swift.gitignore
[objc-gitignore]: https://github.com/github/gitignore/blob/master/Objective-C.gitignore

-->
### Ignores
确保项目在进行版本控制时拥有一个合适的 `.gitignore` 文件是一个很好的第一步。这样，不需要的文件（用户设置、临时文件等）就不会进入你的仓库。幸运的是，GitHub 为 [Swift][swift-gitignore] 和 [Objective-C][objc-gitignore] 都提供了支持。

[swift-gitignore]: https://github.com/github/gitignore/blob/master/Swift.gitignore
[objc-gitignore]: https://github.com/github/gitignore/blob/master/Objective-C.gitignore

<!-- 

### Dependency Management

#### CocoaPods

If you're planning on including external dependencies (e.g. third-party libraries) in your project, [CocoaPods][cocoapods] offers easy and fast integration. Install it like so:

    sudo gem install cocoapods

To get started, move inside your iOS project folder and run

    pod init

This creates a Podfile, which will hold all your dependencies in one place. After adding your dependencies to the Podfile, you run

    pod install

to install the libraries and include them as part of a workspace which also holds your own project. For reasons stated [here][committing-pods-cocoapods] and [here][committing-pods], we recommend committing the installed dependencies to your own repo, instead of relying on having each developer run `pod install` after a fresh checkout.

Note that from now on, you'll need to open the `.xcworkspace` file instead of `.xcproject`, or your code will not compile. The command

    pod update

will update all pods to the newest versions permitted by the Podfile. You can use a wealth of [operators][cocoapods-pod-syntax] to specify your exact version requirements.

[cocoapods]: https://cocoapods.org/
[cocoapods-pod-syntax]: http://guides.cocoapods.org/syntax/podfile.html#pod
[committing-pods]: https://www.dzombak.com/blog/2014/03/including-pods-in-source-control.html
[committing-pods-cocoapods]: https://guides.cocoapods.org/using/using-cocoapods.html#should-i-check-the-pods-directory-into-source-control

-->

### 依赖管理

#### CocoaPods

如果你计划在你的项目中包含外部依赖（例如，第三方库），[CocoaPods][cocoapods]提供了一个简单快捷的集成方式。像这样安装它：

```
sudo gem install cocoapods
```

开始时，移动到你的iOS项目文件夹内并运行

```
pod init
```

这将创建一个Podfile，它将在一个地方保存所有你的依赖。在Podfile中添加了你的依赖后，你运行

```
pod install
```

来安装库，并将它们作为工作区的一部分包含进来，这个工作区也包含了你自己的项目。出于[这里][committing-pods-cocoapods]和[这里][committing-pods]所述的原因，我们推荐将安装的依赖提交到你自己的仓库，而不是依赖于让每个开发者在新检出后运行`pod install`。

注意，从现在开始，你需要打开`.xcworkspace`文件而不是`.xcproject`，否则你的代码将无法编译。命令

```
pod update
```

将根据Podfile允许的更新所有pods到最新版本。你可以使用大量的[操作符][cocoapods-pod-syntax]来指定你的确切版本要求。

[cocoapods]: https://cocoapods.org/
[cocoapods-pod-syntax]: http://guides.cocoapods.org/syntax/podfile.html#pod
[committing-pods]: https://www.dzombak.com/blog/2014/03/including-pods-in-source-control.html
[committing-pods-cocoapods]: https://guides.cocoapods.org/using/using-cocoapods.html#should-i-check-the-pods-directory-into-source-control

<!-- 

#### Carthage

[Carthage][carthage] takes the ["simple, not easy"][simple-made-easy] approach by building your dependencies into binary frameworks, without magically integrating them with your project in any way. This also greatly reduces build times, because your dependencies have already been compiled by the time you start building.

There is no centralized repository of projects, which means any library that can be compiled into a framework supports Carthage out of the box.

To get started, follow the [instructions][carthage-instructions] in Carthage's documentation.

[carthage]: https://github.com/Carthage/Carthage
[simple-made-easy]: http://www.infoq.com/presentations/Simple-Made-Easy
[carthage-instructions]: https://github.com/Carthage/Carthage#installing-carthage
-->

#### Carthage

[Carthage][carthage] 采取了["简单，而非易用"][simple-made-easy]的方法，通过将你的依赖构建成二进制框架，而不是以任何方式神奇地将它们与你的项目集成。这也大大减少了构建时间，因为你开始构建时你的依赖已经被编译过了。

没有中央化的项目仓库，这意味着任何可以被编译成框架的库都默认支持 Carthage。

要开始，遵循 Carthage 文档中的[说明][carthage-instructions]。

[carthage]: https://github.com/Carthage/Carthage
[simple-made-easy]: http://www.infoq.com/presentations/Simple-Made-Easy
[carthage-instructions]: https://github.com/Carthage/Carthage#installing-carthage

<!-- 

### Project Structure

To keep all those hundreds of source files from ending up in the same directory, it's a good idea to set up some folder structure depending on your architecture. For instance, you can use the following:

    ├─ Models
    ├─ Views
    ├─ Controllers (or ViewModels, if your architecture is MVVM)
    ├─ Stores
    ├─ Helpers

First, create them as groups (little yellow "folders") within the group with your project's name in Xcode's Project Navigator. Then, for each of the groups, link them to an actual directory in your project path by opening their File Inspector on the right, hitting the little gray folder icon, and creating a new subfolder with the name of the group in your project directory.

-->

### 项目结构

为了防止成百上千的源文件最终都放在同一个目录下，根据你的架构设置一些文件夹结构是个好主意。例如，你可以使用以下结构：

    ├─ Models
    ├─ Views
    ├─ Controllers（或 ViewModels，如果你的架构是 MVVM）
    ├─ Stores
    ├─ Helpers

首先，在Xcode的项目导航器中，以你的项目名称所在的组（小黄色“文件夹”）内创建它们作为组。然后，对每个组，通过打开它们在右侧的文件检查器，点击小灰色文件夹图标，并在你的项目目录中以组的名称创建新的子文件夹，将它们链接到项目路径中的实际目录。

<!-- 
#### Localization

Keep all user strings in localization files right from the beginning. This is good not only for translations, but also for finding user-facing text quickly. You can add a launch argument to your build scheme to launch the app in a certain language, e.g.

    -AppleLanguages (Finnish)

For more complex translations such as plural forms that depending on a number of items (e.g. "1 person" vs. "3 people"), you should use the [`.stringsdict` format][stringsdict-format] instead of a regular `localizable.strings` file. As soon as you've wrapped your head around the crazy syntax, you have a powerful tool that knows how to make plurals for "one", some", "few" and "many" items, as needed [e.g. in Russian or Arabic][language-plural-rules].

Find more information about localization in [these presentation slides][l10n-slides] from the February 2012 HelsinkiOS meetup. Most of the talk is still relevant.

[stringsdict-format]: https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/StringsdictFileFormat/StringsdictFileFormat.html
[language-plural-rules]: http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html
[l10n-slides]: https://speakerdeck.com/hasseg/localization-practicum

-->

#### 本地化

从一开始就将所有用户字符串保存在本地化文件中。这不仅对翻译有好处，也便于快速找到面向用户的文本。你可以向你的构建方案添加一个启动参数，以特定语言启动应用，例如：

    -AppleLanguages (Finnish)

对于更复杂的翻译，比如取决于项目数量的复数形式（例如，“1 person”与“3 people”），你应该使用[`.stringsdict`格式][stringsdict-format]而不是常规的`localizable.strings`文件。一旦你弄懂了那些复杂的语法，你就拥有了一个强大的工具，它知道如何根据需要为“one”，“some”，“few”和“many”项目创建复数形式[例如在俄语或阿拉伯语中][language-plural-rules]。

从2012年2月赫尔辛基OS聚会的[这些演示文稿][l10n-slides]中找到更多关于本地化的信息。大部分讲话内容仍然相关。

[stringsdict-format]: https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/StringsdictFileFormat/StringsdictFileFormat.html
[language-plural-rules]: http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html
[l10n-slides]: https://speakerdeck.com/hasseg/localization-practicum
<!-- 

#### Constants

Keep your constants' scope as small as possible. For instance, when you only need it inside a class, it should live in that class. Those constants that need to be truly app-wide should be kept in one place. In Swift, you can use enums defined in a `Constants.swift` file to group, store and access your app-wide constants in a clean way:

```swift

enum Config {
    static let baseURL = NSURL(string: "http://www.example.org/")!
    static let splineReticulatorName = "foobar"
}

enum Color {
    static let primaryColor = UIColor(red: 0.22, green: 0.58, blue: 0.29, alpha: 1.0)
    static let secondaryColor = UIColor.lightGray

    // A visual way to define colours within code files is to use #colorLiteral
    // This syntax will present you with colour picker component right on the code line
    static let tertiaryColor = #colorLiteral(red: 0.22, green: 0.58, blue: 0.29, alpha: 1.0)
}

```

When using Objective-C, keep app-wide constants in a `Constants.h` file that is included in the prefix header.

Instead of preprocessor macro definitions (via `#define`), use actual constants:

    static CGFloat const XYZBrandingFontSizeSmall = 12.0f;
    static NSString * const XYZAwesomenessDeliveredNotificationName = @"foo";

Actual constants are type-safe, have more explicit scope (they’re not available in all imported/included files until undefined), cannot be redefined or undefined in later parts of the code, and are available in the debugger.


-->

#### 常量

尽可能缩小常量的作用范围。例如，当你只在一个类内部需要它时，它应该存在于那个类中。那些真正需要在整个应用范围内使用的常量应该保存在一个地方。在Swift中，你可以使用定义在`Constants.swift`文件中的枚举以一种干净的方式来分组、存储和访问你的应用范围内的常量：

```swift
enum Config {
    static let baseURL = NSURL(string: "http://www.example.org/")!
    static let splineReticulatorName = "foobar"
}

enum Color {
    static let primaryColor = UIColor(red: 0.22, green: 0.58, blue: 0.29, alpha: 1.0)
    static let secondaryColor = UIColor.lightGray

    // 在代码文件中以可视化的方式定义颜色是使用#colorLiteral
    // 这种语法将在代码行上为你提供一个颜色选择器组件
    static let tertiaryColor = #colorLiteral(red: 0.22, green: 0.58, blue: 0.29, alpha: 1.0)
}
```

在使用Objective-C时，将应用范围内的常量保存在一个`Constants.h`文件中，并在前缀头文件中包含它。

不要使用预处理器宏定义（通过`#define`），而应使用实际的常量：

```objective-c
static CGFloat const XYZBrandingFontSizeSmall = 12.0f;
static NSString * const XYZAwesomenessDeliveredNotificationName = @"foo";
```

实际的常量是类型安全的，具有更显式的作用域（它们不会在所有导入/包含的文件中可用，直到未定义），不能在代码的后面部分被重新定义或取消定义，并且在调试器中可用。

<!-- 
### Branching Model

Especially when distributing an app to the public (e.g. through the App Store), it's a good idea to isolate releases to their own branch with proper tags. Also, feature work that involves a lot of commits should be done on its own branch. [`git-flow`][gitflow-github] is a tool that helps you follow these conventions. It is simply a convenience wrapper around Git's branching and tagging commands, but can help maintain a proper branching structure especially for teams. Do all development on feature branches (or on `develop` for smaller work), tag releases with the app version, and commit to master only via

    git flow release finish <version>

[gitflow-github]: https://github.com/nvie/gitflow

-->

### 分支模型

特别是在向公众分发应用程序（例如，通过App Store）时，将发布版本隔离到它们自己的分支并使用适当的标签是一个好主意。此外，涉及到许多提交的功能工作也应该在它们自己的分支上进行。[`git-flow`][gitflow-github]是一个帮助你遵循这些惯例的工具。它只是Git分支和标签命令的便利包装器，但可以帮助特别是团队维护适当的分支结构。所有开发工作都在功能分支上进行（或在`develop`上进行较小的工作），用应用版本标记发布，并且只通过以下方式提交到master分支：

```
git flow release finish <version>
```

这种方法提倡使用特定的分支来处理开发、特性添加、发布准备和修补工作。使用git-flow，你可以保持你的项目历史清晰、有序，同时也提高了代码发布的整体质量。它鼓励进行代码审查和测试，因为所有新功能都在独立的分支上进行开发，而不是直接在主分支上进行。

遵循git-flow模型需要团队成员熟悉Git命令和工作流程的概念。虽然它增加了一些复杂性，特别是对于新手来说，但长期来看，它为项目带来的结构和纪律性的好处是显而易见的。它也使得回滚到旧版本、管理热修补（hotfixes）和准备发布变得更加简单和直接。

[gitflow-github]: https://github.com/nvie/gitflow

<!-- 

### Minimum iOS Version Requirement

It’s useful to make an early decision on the minimum iOS version you want to support in your project: knowing which OS versions you need to develop and test against, and which system APIs you can rely on, helps you estimate your workload, and enables you to determine what’s possible and what’s not.

Use these resources to gather the data necessary for making this choice:

* Official “first-party” resources:
    * [Apple’s world-wide iOS version penetration statistics](https://developer.apple.com/support/app-store/): The primary public source for version penetration stats. Prefer more localized and domain-specific statistics, if available.
* Third-party resources:
    * [iOS Support Matrix](http://iossupportmatrix.com): Useful for determining which specific device models are ruled out by a given minimum OS version requirement.
    * [DavidSmith: iOS Version Stats](https://david-smith.org/iosversionstats/): Version penetration stats for David Smith’s Audiobooks apps.
    * [Mixpanel Trends: iOS versions](https://mixpanel.com/trends/#report/ios_frag): Version penetration stats from Mixpanel.

-->

### 最低iOS版本要求

在项目初期就决定您想要支持的最低iOS版本是非常有用的：了解您需要针对哪些操作系统版本进行开发和测试，以及您可以依赖哪些系统API，有助于您估计工作量，并使您能够确定什么是可能的，什么是不可能的。

使用以下资源来收集做出这一选择所需的数据：

* 官方“第一方”资源：
    * [Apple的全球iOS版本普及率统计](https://developer.apple.com/support/app-store/)：版本普及率的主要公共来源。如果可能，更倾向于使用更本地化和领域特定的统计数据。
* 第三方资源：
    * [iOS支持矩阵](http://iossupportmatrix.com)：用于确定由于给定的最低操作系统版本要求而被排除的特定设备型号非常有用。
    * [DavidSmith: iOS版本统计](https://david-smith.org/iosversionstats/)：David Smith的有声书应用的版本普及率统计。
    * [Mixpanel趋势：iOS版本](https://mixpanel.com/trends/#report/ios_frag)：来自Mixpanel的版本普及率统计。


<!-- 

## Common Libraries

Generally speaking, make it a conscious decision to add an external dependency to your project. Sure, this one neat library solves your problem now, but maybe later gets stuck in maintenance limbo, with the next OS version that breaks everything being just around the corner. Another scenario is that a feature only achievable with external libraries suddenly becomes part of the official APIs. In a well-designed codebase, switching out the implementation is a small effort that pays off quickly. Always consider solving the problem using Apple's extensive (and mostly excellent) frameworks first!

Therefore this section has been deliberately kept rather short. The libraries featured here tend to reduce boilerplate code (e.g. Auto Layout) or solve complex problems that require extensive testing, such as date calculations. As you become more proficient with iOS, be sure to dive into the source here and there, and acquaint yourself with their underlying Apple frameworks. You'll find that those alone can do a lot of the heavy lifting.

-->
## 常用库

一般来说，将外部依赖添加到您的项目中应是一个有意识的决定。当然，这个整洁的库现在解决了您的问题，但也许稍后它会陷入维护的不确定状态，而下一个操作系统版本可能会打破一切，就在拐角处。另一个情况是，只能通过外部库实现的功能突然成为官方API的一部分。在一个设计良好的代码库中，更换实现是一项微小的努力，但很快就能得到回报。始终首先考虑使用Apple提供的广泛（且大多数情况下优秀的）框架来解决问题！

因此，这一部分被有意保持相对较短。这里介绍的库倾向于减少样板代码（例如，自动布局）或解决需要广泛测试的复杂问题，如日期计算。随着您对iOS越来越熟练，一定要时不时深入了解源代码，并熟悉它们底层的Apple框架。您会发现，仅凭这些就可以完成很多繁重的工作。
<!-- 

### AFNetworking/Alamofire

The majority of iOS developers use one of these network libraries. While `NSURLSession` is surprisingly powerful by itself, [AFNetworking][afnetworking-github] and [Alamofire][alamofire-github] remain unbeaten when it comes to actually managing queues of requests, which is pretty much a requirement of any modern app. We recommend AFNetworking for Objective-C projects and Alamofire for Swift projects. While the two frameworks have subtle differences, they share the same ideology and are published by the same foundation.

[afnetworking-github]: https://github.com/AFNetworking/AFNetworking
[alamofire-github]: https://github.com/Alamofire/Alamofire

-->
### AFNetworking/Alamofire

绝大多数iOS开发者会使用这两个网络库中的一个。尽管`NSURLSession`本身就异常强大，[AFNetworking][afnetworking-github]和[Alamofire][alamofire-github]在真正管理请求队列方面仍然无人能敌，这几乎是任何现代应用的必需功能。我们推荐Objective-C项目使用AFNetworking，Swift项目使用Alamofire。虽然这两个框架有细微的差别，但它们分享相同的理念，并由同一基金会发布。

[afnetworking-github]: https://github.com/AFNetworking/AFNetworking
[alamofire-github]: https://github.com/Alamofire/Alamofire

<!-- 

### DateTools
As a general rule, [don't write your date calculations yourself][timezones-youtube]. Luckily, in [DateTools][datetools-github] you get an MIT-licensed, thoroughly tested library that covers pretty much all your calendar needs.

[timezones-youtube]: https://www.youtube.com/watch?v=-5wpm-gesOY
[datetools-github]: https://github.com/MatthewYork/DateTools

-->
### DateTools
通常规则是[不要自己编写日期计算][timezones-youtube]。幸运的是，在[DateTools][datetools-github]中，你可以得到一个MIT许可的、经过彻底测试的库，几乎涵盖了你所有的日历需求。

[timezones-youtube]: https://www.youtube.com/watch?v=-5wpm-gesOY
[datetools-github]: https://github.com/MatthewYork/DateTools

<!-- 

### Auto Layout Libraries
If you prefer to write your views in code, chances are you've heard of either Apple's awkward syntaxes – the regular `NSLayoutConstraint` factory or the so-called [Visual Format Language][visual-format-language]. The former is extremely verbose and the latter based on strings, which effectively prevents compile-time checking. Fortunately, they've addressed the issue in iOS 9, allowing [a more concise specification of constraints][nslayoutanchor].

If you're stuck with an earlier iOS version, [Masonry/SnapKit][snapkit-github] remedies the problem by introducing its own [DSL][dsl-wikipedia] to make, update and replace constraints. [PureLayout][purelayout-github] solves the same problem using Cocoa API style. For Swift, there is also [Cartography][cartography-github], which builds on the language's powerful operator overloading features. For the more conservative, [FLKAutoLayout][flkautolayout-github] offers a clean, but rather non-magical wrapper around the native APIs.

[visual-format-language]: https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html
[nslayoutanchor]: https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html
[snapkit-github]: https://github.com/SnapKit/
[purelayout-github]: https://github.com/PureLayout/PureLayout
[dsl-wikipedia]: https://en.wikipedia.org/wiki/Domain-specific_language
[cartography-github]: https://github.com/robb/Cartography
[flkautolayout-github]: https://github.com/floriankugler/FLKAutoLayout

-->
### 自动布局库
如果你更喜欢用代码编写视图，那么你可能听说过苹果的一些笨拙语法——常规的`NSLayoutConstraint`工厂或所谓的[可视化格式语言][visual-format-language]。前者极其冗长，而后者基于字符串，有效地阻止了编译时检查。幸运的是，他们在iOS 9中解决了这个问题，允许[更简洁地指定约束][nslayoutanchor]。

如果你被困在早期的iOS版本中，[Masonry/SnapKit][snapkit-github]通过引入自己的[DSL][dsl-wikipedia]来解决问题，以便创建、更新和替换约束。[PureLayout][purelayout-github]使用Cocoa API风格解决了同样的问题。对于Swift，还有[Cartography][cartography-github]，它利用了语言强大的操作符重载特性。对于更保守的人来说，[FLKAutoLayout][flkautolayout-github]提供了一个干净但相对非魔法的原生API封装。

[visual-format-language]: https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html
[nslayoutanchor]: https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html
[snapkit-github]: https://github.com/SnapKit/
[purelayout-github]: https://github.com/PureLayout/PureLayout
[dsl-wikipedia]: https://en.wikipedia.org/wiki/Domain-specific_language
[cartography-github]: https://github.com/robb/Cartography
[flkautolayout-github]: https://github.com/floriankugler/FLKAutoLayout


<!-- 

## Architecture

* [Model-View-Controller-Store (MVCS)][mvcs]
    * This is the default Apple architecture (MVC), extended by a Store layer that vends Model instances and handles the networking, caching etc.
    * Every Store exposes to the view controllers either `signals` or `void` methods with custom completion blocks.
* [Model-View-ViewModel (MVVM)][mvvm]
    * Motivated by "massive view controllers": MVVM considers `UIViewController` subclasses part of the View and keeps them slim by maintaining all state in the ViewModel.
    * To learn more about it, check out Bob Spryn's [fantastic introduction][sprynthesis-mvvm].
* [View-Interactor-Presenter-Entity-Routing (VIPER)][viper]
    * Rather exotic architecture that might be worth looking into in larger projects, where even MVVM feels too cluttered and testability is a major concern.

[mvcs]: http://programmers.stackexchange.com/questions/184396/mvcs-model-view-controller-store
[mvvm]: https://www.objc.io/issues/13-architecture/mvvm/
[sprynthesis-mvvm]: http://www.sprynthesis.com/2014/12/06/reactivecocoa-mvvm-introduction/
[viper]: https://www.objc.io/issues/13-architecture/viper/

-->
## 架构

* [模型-视图-控制器-存储 (MVCS)][mvcs]
    * 这是苹果默认的架构（MVC），通过增加一个存储层来扩展，该层提供模型实例并处理网络请求、缓存等。
    * 每个存储向视图控制器暴露`信号`或带有自定义完成块的`void`方法。
* [模型-视图-视图模型 (MVVM)][mvvm]
    * 由“庞大的视图控制器”问题激发：MVVM将`UIViewController`子类视为视图的一部分，并通过在视图模型中保持所有状态来保持它们的精简。
    * 要了解更多信息，请查看Bob Spryn的[精彩介绍][sprynthesis-mvvm]。
* [视图-交互器-呈现器-实体-路由 (VIPER)][viper]
    * 相当异国情调的架构，可能值得在大型项目中探讨，在这些项目中，即使是MVVM也感觉太杂乱，且可测试性是主要关注点。

[mvcs]: http://programmers.stackexchange.com/questions/184396/mvcs-model-view-controller-store
[mvvm]: https://www.objc.io/issues/13-architecture/mvvm/
[sprynthesis-mvvm]: http://www.sprynthesis.com/2014/12/06/reactivecocoa-mvvm-introduction/
[viper]: https://www.objc.io/issues/13-architecture/viper/

<!-- 

### “Event” Patterns

These are the idiomatic ways for components to notify others about things:

* __Delegation:__ _(one-to-one)_ Apple uses this a lot (some would say, too much). Use when you want to communicate stuff back e.g. from a modal view.
* __Callback blocks:__ _(one-to-one)_ Allow for a more loose coupling, while keeping related code sections close to each other. Also scales better than delegation when there are many senders.
* __Notification Center:__ _(one-to-many)_ Possibly the most common way for objects to emit “events” to multiple observers. Very loose coupling — notifications can even be observed globally without reference to the dispatching object.
* __Key-Value Observing (KVO):__ _(one-to-many)_ Does not require the observed object to explicitly “emit events” as long as it is _Key-Value Coding (KVC)_ compliant for the observed keys (properties). Usually not recommended due to its implicit nature and the cumbersome standard library API.
* __Signals:__ _(one-to-many)_ The centerpiece of [ReactiveCocoa][reactivecocoa-github], they allow chaining and combining to your heart's content, thereby offering a way out of "callback hell".


-->
### “事件”模式

这些是组件通知其他组件事情的惯用方法：

* __代理：__ _(一对一)_ 苹果大量使用这种方式（有些人说，用得太多了）。当你想要从模态视图等回传信息时使用。
* __回调块：__ _(一对一)_ 允许更松散的耦合，同时保持相关代码部分彼此靠近。当有许多发送者时，也比代理更好地扩展。
* __通知中心：__ _(一对多)_ 可能是对象向多个观察者发出“事件”的最常见方式。非常松散的耦合——甚至可以在不引用分发对象的情况下全局观察到通知。
* __键值观察 (KVO)：__ _(一对多)_ 只要被观察的对象对被观察的键（属性）遵守_Key-Value Coding (KVC)_ 即可，不需要被观察的对象显式地“发出事件”。通常不推荐使用，因为它的隐式性质和标准库API的繁琐性。
* __信号：__ _(一对多)_ [ReactiveCocoa][reactivecocoa-github]的中心概念，它们允许按照你的意愿进行链式和组合操作，从而提供了一种摆脱“回调地狱”的方法。

<!-- 

### Models

Keep your models immutable, and use them to translate the remote API's semantics and types to your app. For Objective-C projects, Github's [Mantle](https://github.com/Mantle/Mantle) is a good choice. In Swift, you can use structs instead of classes to ensure immutability, and use Swift's [Codable][codableLink] to do the JSON-to-model mapping. There are also few third party libraries available. [SwiftyJSON][swiftyjson] and [Argo][argo] are the popular among them.

[swiftyjson]: https://github.com/SwiftyJSON/SwiftyJSON
[argo]: https://github.com/thoughtbot/Argo
[codableLink]: https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types


-->
### 模型

保持你的模型不可变，并使用它们将远程API的语义和类型转换为你的应用程序所需的格式。对于Objective-C项目，Github的[Mantle](https://github.com/Mantle/Mantle)是一个不错的选择。在Swift中，你可以使用结构体而不是类来确保不可变性，并使用Swift的[Codable][codableLink]来进行JSON到模型的映射。还有一些第三方库可供选择。[SwiftyJSON][swiftyjson]和[Argo][argo]是其中较为流行的。

[swiftyjson]: https://github.com/SwiftyJSON/SwiftyJSON
[argo]: https://github.com/thoughtbot/Argo
[codableLink]: https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types

<!-- 

### Views

With today's wealth of screen sizes in the Apple ecosystem and the advent of split-screen multitasking on iPad, the boundaries between devices and form factors become increasingly blurred. Much like today's websites are expected to adapt to different browser window sizes, your app should handle changes in available screen real estate in a graceful way. This can happen e.g. if the user rotates the device or swipes in a secondary iPad app next to your own.

Instead of manipulating view frames directly, you should use [size classes][size-classes] and Auto Layout to declare constraints on your views. The system will then calculate the appropriate frames based on these rules, and re-evaluate them when the environment changes.

Apple's [recommended approach][wwdc-autolayout-mysteries] for setting up your layout constraints is to create and activate them once during initialization. If you need to change your constraints dynamically, hold references to them and then deactivate/activate them as required. The main use case for `UIView`'s `updateConstraints` (or its `UIViewController` counterpart, `updateViewConstraints`) is when you want the system to perform batch updates for better performance. However, this comes at the cost of having to call `setNeedsUpdateConstraints` elsewhere in your code, increasing its complexity.

If you override `updateConstraints` in a custom view, you should explicitly state that your view requires a constraint-based layout:

Swift:
```swift
override class var requiresConstraintBasedLayout: Bool {
    return true
}
```

Objective-C:
```objective-c
+ (BOOL)requiresConstraintBasedLayout {
    return YES
}
```

Otherwise, you may encounter strange bugs when the system doesn't call `updateConstraints()` as you would expect it to. [This blog post][edward-huynh-requiresconstraintbasedlayout] by Edward Huynh offers a more detailed explanation.

[wwdc-autolayout-mysteries]: https://developer.apple.com/videos/wwdc/2015/?id=219
[edward-huynh-requiresconstraintbasedlayout]: http://www.edwardhuynh.com/blog/2013/11/24/the-mystery-of-the-requiresconstraintbasedlayout/


-->

### 视图

在苹果生态系统中，由于屏幕尺寸多样化以及iPad上出现的分屏多任务处理功能，设备与形态因素之间的界限变得越来越模糊。就像今天的网站需要适应不同的浏览器窗口尺寸一样，你的应用也应该以优雅的方式处理可用屏幕空间的变化。例如，当用户旋转设备或在你的应用旁边滑动一个次级iPad应用时，就会发生这种情况。

你应该使用[尺寸类][size-classes]和自动布局来声明对视图的约束，而不是直接操作视图框架。系统将根据这些规则计算适当的框架，并在环境变化时重新评估它们。

苹果[推荐的][wwdc-autolayout-mysteries]设置布局约束的方法是在初始化期间创建并激活它们。如果你需要动态更改约束，保持对它们的引用，然后根据需要激活/停用它们。`UIView`的`updateConstraints`（或其`UIViewController`对应项，`updateViewConstraints`）的主要用途是当你希望系统执行批量更新以提高性能时。然而，这需要在代码中的其他地方调用`setNeedsUpdateConstraints`，增加了代码的复杂性。

如果你在自定义视图中覆写了`updateConstraints`，你应该明确声明你的视图需要基于约束的布局：

Swift:
```swift
override class var requiresConstraintBasedLayout: Bool {
    return true
}
```

Objective-C:
```objective-c
+ (BOOL)requiresConstraintBasedLayout {
    return YES
}
```

否则，当系统不按你预期调用`updateConstraints()`时，你可能会遇到奇怪的bug。Edward Huynh的[这篇博客文章][edward-huynh-requiresconstraintbasedlayout]提供了更详细的解释。

[size-classes]: https://developer.apple.com/documentation/uikit/uitraitcollection/1652813-sizeclasses
[wwdc-autolayout-mysteries]: https://developer.apple.com/videos/wwdc/2015/?id=219
[edward-huynh-requiresconstraintbasedlayout]: http://www.edwardhuynh.com/blog/2013/11/24/the-mystery-of-the-requiresconstraintbasedlayout/


<!-- 

### Controllers

Use dependency injection, i.e. pass any required objects in as parameters, instead of keeping all state around in singletons. The latter is okay only if the state _really_ is global.

Swift:
```swift
let fooViewController = FooViewController(withViewModel: fooViewModel)
```

Objective-C:
```objective-c
FooViewController *fooViewController = [[FooViewController alloc] initWithViewModel:fooViewModel];
```

Try to avoid bloating your view controllers with logic that can safely reside in other places. Soroush Khanlou has a [good writeup][khanlou-destroy-massive-vc] of how to achieve this, and architectures like [MVVM](#architecture) treat view controllers as views, thereby greatly reducing their complexity.

[khanlou-destroy-massive-vc]: http://khanlou.com/2014/09/8-patterns-to-help-you-destroy-massive-view-controller/

-->

### 控制器

使用依赖注入，即将所需的对象作为参数传入，而不是在单例中保留所有状态。只有当状态 _确实_ 是全局的时候，后者才是可以接受的。

Swift:
```swift
let fooViewController = FooViewController(withViewModel: fooViewModel)
```

Objective-C:
```objective-c
FooViewController *fooViewController = [[FooViewController alloc] initWithViewModel:fooViewModel];
```

尽量避免让你的视图控制器充满了可以安全放在其他地方的逻辑。Soroush Khanlou有一篇[很好的文章][khanlou-destroy-massive-vc]介绍了如何实现这一点，而像[MVVM](#架构)这样的架构将视图控制器视为视图，从而大大减少了它们的复杂性。

[khanlou-destroy-massive-vc]: http://khanlou.com/2014/09/8-patterns-to-help-you-destroy-massive-view-controller/

<!-- 


## Stores

At the "ground level" of a mobile app is usually some kind of model storage, that keeps its data in places such as on disk, in a local database, or on a remote server. This layer is also useful to abstract away any activities related to the vending of model objects, such as caching.

Whether it means kicking off a backend request or deserializing a large file from disk, fetching data is often asynchronous in nature. Your store's API should reflect this by offering some kind of deferral mechanism, as synchronously returning the data would cause the rest of your app to stall.

If you're using [ReactiveCocoa][reactivecocoa-github], `SignalProducer` is a natural choice for the return type. For instance, fetching gigs for a given artist would yield the following signature:

Swift + ReactiveSwift:
```swift
func fetchGigs(for artist: Artist) -> SignalProducer<[Gig], Error> {
    // ...
}
```

ObjectiveC + ReactiveObjC:
```objective-c
- (RACSignal<NSArray<Gig *> *> *)fetchGigsForArtist:(Artist *)artist {
    // ...
}
```

Here, the returned `SignalProducer` is merely a "recipe" for getting a list of gigs. Only when started by the subscriber, e.g. a view model, will it perform the actual work of fetching the gigs. Unsubscribing before the data has arrived would then cancel the network request.

If you don't want to use signals, futures or similar mechanisms to represent your future data, you can also use a regular callback block. Keep in mind that chaining or nesting such blocks, e.g. in the case where one network request depends on the outcome of another, can quickly become very unwieldy – a condition generally known as "callback hell".

-->
## 存储

在移动应用的“底层”通常有某种形式的模型存储，它将其数据保存在诸如磁盘、本地数据库或远程服务器等地方。这一层也有助于抽象出与模型对象的分发相关的任何活动，例如缓存。

无论是启动后端请求还是从磁盘反序列化一个大文件，获取数据的过程往往是异步的。你的存储API应该通过提供某种延迟机制来反映这一点，因为同步返回数据会导致你的应用的其余部分停滞。

如果你使用的是[ReactiveCocoa][reactivecocoa-github]，`SignalProducer` 是返回类型的自然选择。例如，获取给定艺术家的演出可能会产生以下签名：

Swift + ReactiveSwift:
```swift
func fetchGigs(for artist: Artist) -> SignalProducer<[Gig], Error> {
    // ...
}
```

ObjectiveC + ReactiveObjC:
```objective-c
- (RACSignal<NSArray<Gig *> *> *)fetchGigsForArtist:(Artist *)artist {
    // ...
}
```

在这里，返回的`SignalProducer`仅仅是获取演出列表的一个“配方”。只有当订阅者，例如一个视图模型，启动它时，它才会执行获取演出的实际工作。在数据到达之前取消订阅，则会取消网络请求。

如果你不想使用信号、未来或类似机制来表示你的未来数据，你也可以使用常规的回调块。请记住，链式或嵌套这样的块，例如在一个网络请求依赖于另一个的结果的情况下，可以很快变得非常笨重 - 这通常被称为“回调地狱”。

[reactivecocoa-github]: https://github.com/ReactiveCocoa/ReactiveCocoa
<!-- 

## Assets

[Asset catalogs][asset-catalogs] are the best way to manage all your project's visual assets. They can hold both universal and device-specific (iPhone 4-inch, iPhone Retina, iPad, etc.) assets and will automatically serve the correct ones for a given name. Teaching your designer(s) how to add and commit things there (Xcode has its own built-in Git client) can save a lot of time that would otherwise be spent copying stuff from emails or other channels to the codebase. It also allows them to instantly try out their changes and iterate if needed.

[asset-catalogs]: http://help.apple.com/xcode/mac/8.0/#/dev10510b1f7

-->
## 资源

[资产目录][asset-catalogs] 是管理项目中所有视觉资产的最佳方式。它们可以同时容纳通用资产和特定设备的资产（iPhone 4英寸、iPhone Retina、iPad 等），并会自动为给定名称提供正确的资产。教会你的设计师如何在那里添加和提交内容（Xcode 有自己的内置 Git 客户端）可以节省很多时间，这些时间否则会花在将内容从电子邮件或其他渠道复制到代码库上。它还允许他们立即尝试他们的更改，并在需要时进行迭代。

[asset-catalogs]: http://help.apple.com/xcode/mac/8.0/#/dev10510b1f7
<!-- 
### Using Bitmap Images

Asset catalogs expose only the names of image sets, abstracting away the actual file names within the set. This nicely prevents asset name conflicts, as files such as `button_large@2x.png` are now namespaced inside their image sets. Appending the modifiers `-568h`, `@2x`, `~iphone` and `~ipad` are not required per se, but having them in the file name when dragging the file to an image set will automatically place them in the right "slot", thereby preventing assignment mistakes that can be hard to hunt down.

-->

### 使用位图图像

资产目录仅暴露图像集的名称，抽象出了集合内的实际文件名。这样很好地避免了资产名称冲突，因为像 `button_large@2x.png` 这样的文件现在被命名空间封装在它们的图像集内部。本身并不要求附加修饰符 `-568h`、`@2x`、`~iphone` 和 `~ipad`，但在将文件拖动到图像集时如果文件名中包含这些修饰符，将自动将它们放置在正确的“槽”中，从而防止分配错误，这种错误可能很难追踪。
<!-- 
### Using Vector Images

You can include the original [vector graphics (PDFs)][vector-assets] produced by designers into the asset catalogs, and have Xcode automatically generate the bitmaps from that. This reduces the complexity of your project (the number of files to manage.)

[vector-assets]: http://martiancraft.com/blog/2014/09/vector-images-xcode6/

-->

### 使用矢量图像

你可以将设计师制作的原始[矢量图形（PDFs）][vector-assets]包含到资产目录中，并让 Xcode 自动从中生成位图。这减少了项目的复杂度（要管理的文件数量）。

[vector-assets]: http://martiancraft.com/blog/2014/09/vector-images-xcode6/

<!-- 
### Image optimisation

Xcode automatically tries to optimise resources living in asset catalogs (yet another reason to use them). Developers can choose from lossless and lossy compression algorithms. App icons are an exception: Apps with large or unoptimised app icons are known to be rejected by Apple. For app icons and more advanced optimisation of PNG files we recommend using [pngcrush][pngcrush-website] or [ImageOptim][imageoptim-website], its GUI counterpart.

[pngcrush-website]: http://pmt.sourceforge.net/pngcrush/
[imageoptim-website]:https://imageoptim.com/mac

-->
### 图像优化

Xcode 会自动尝试优化存储在资产目录中的资源（这是使用它们的另一个理由）。开发者可以从无损和有损压缩算法中选择。应用图标是一个例外：已知苹果会拒绝带有大型或未优化应用图标的应用。对于应用图标和 PNG 文件的更高级优化，我们推荐使用 [pngcrush][pngcrush-website] 或其图形界面对应物 [ImageOptim][imageoptim-website]。

[pngcrush-website]: http://pmt.sourceforge.net/pngcrush/
[imageoptim-website]:https://imageoptim.com/mac
<!-- 


## Coding Style

### Naming

Apple pays great attention to keeping naming consistent. Adhering to their [coding guidelines for Objective-C][cocoa-coding-guidelines] and [API design guidelines for Swift][swift-api-design-guidelines] makes it much easier for new people to join the project.

[cocoa-coding-guidelines]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html
[swift-api-design-guidelines]: https://swift.org/documentation/api-design-guidelines/

Here are some basic takeaways you can start using right away:

A method beginning with a _verb_ indicates that it performs some side effects, but won't return anything:
`- (void)loadView;`
`- (void)startAnimating;`

Any method starting with a _noun_, however, returns that object and should do so without side effects:
`- (UINavigationItem *)navigationItem;`
`+ (UILabel *)labelWithText:(NSString *)text;`

It pays off to keep these two as separated as possible, i.e. not perform side effects when you transform data, and vice versa. That will keep your side effects contained to smaller sections of the code, which makes it more understandable and facilitates debugging.

-->
## 编码风格

### 命名

Apple 非常注重保持命名的一致性。遵循他们的 [Objective-C 编码指南][cocoa-coding-guidelines] 和 [Swift API 设计指南][swift-api-design-guidelines]，可以让新成员更容易加入项目。

[cocoa-coding-guidelines]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html
[swift-api-design-guidelines]: https://swift.org/documentation/api-design-guidelines/

以下是一些你可以立即开始使用的基本要点：

以 _动词_ 开头的方法表示它将执行一些副作用，但不会返回任何内容：
`- (void)loadView;`
`- (void)startAnimating;`

然而，任何以 _名词_ 开头的方法都会返回那个对象，并且应该在没有副作用的情况下这样做：
`- (UINavigationItem *)navigationItem;`
`+ (UILabel *)labelWithText:(NSString *)text;`

尽可能保持这两者分开，即在转换数据时不执行副作用，反之亦然。这将使你的副作用限制在代码的较小部分，从而使代码更易于理解和调试。
<!-- 
### Structure

`MARK:` comments (Swift) and [pragma marks][nshipster-pragma-marks] (Objective-C) are a great way to group your methods, especially in view controllers. Here is a Swift example for a common structure that works with almost any view controller:

```swift

import SomeExternalFramework

class FooViewController : UIViewController, FoobarDelegate {

    let foo: Foo

    private let fooStringConstant = "FooConstant"
    private let floatConstant = 1234.5

    // MARK: Lifecycle

    // Custom initializers go here

    // MARK: View Lifecycle

    override func viewDidLoad() {
        super.viewDidLoad()
        // ...
    }

    // MARK: Layout

    private func makeViewConstraints() {
        // ...
    }

    // MARK: User Interaction

    func foobarButtonTapped() {
        // ...
    }

    // MARK: FoobarDelegate

    func foobar(foobar: Foobar, didSomethingWithFoo foo: Foo) {
        // ...
    }

    // MARK: Additional Helpers

    private func displayNameForFoo(foo: Foo) {
        // ...
    }

}
```

The most important point is to keep these consistent across your project's classes.

[nshipster-pragma-marks]: http://nshipster.com/pragma/

-->

### 结构

`MARK:` 注释（Swift）和 [pragma marks][nshipster-pragma-marks]（Objective-C）是对你的方法进行分组的绝佳方式，尤其是在视图控制器中。这里有一个几乎适用于任何视图控制器的 Swift 示例结构：

```swift
import SomeExternalFramework

class FooViewController : UIViewController, FoobarDelegate {

    let foo: Foo

    private let fooStringConstant = "FooConstant"
    private let floatConstant = 1234.5

    // MARK: 生命周期

    // 自定义初始化器在这里

    // MARK: 视图生命周期

    override func viewDidLoad() {
        super.viewDidLoad()
        // ...
    }

    // MARK: 布局

    private func makeViewConstraints() {
        // ...
    }

    // MARK: 用户交互

    func foobarButtonTapped() {
        // ...
    }

    // MARK: FoobarDelegate

    func foobar(foobar: Foobar, didSomethingWithFoo foo: Foo) {
        // ...
    }

    // MARK: 额外辅助功能

    private func displayNameForFoo(foo: Foo) {
        // ...
    }

}
```

最重要的一点是在你项目的类中保持这些的一致性。

[nshipster-pragma-marks]: http://nshipster.com/pragma/
<!-- 
### External Style Guides

Futurice does not have company-level guidelines for coding style. It can however be useful to peruse the style guides of other software companies, even if some bits can be quite company-specific or opinionated.

* GitHub: [Swift](https://github.com/github/swift-style-guide) and [Objective-C](https://github.com/github/objective-c-style-guide)
* Ray Wenderlich: [Swift](https://github.com/raywenderlich/swift-style-guide) and [Objective-C](https://github.com/raywenderlich/objective-c-style-guide)
* Google: [Objective-C](https://google.github.io/styleguide/objcguide.xml)
* The New York Times: [Objective-C](https://github.com/NYTimes/objective-c-style-guide)
* Sam Soffes: [Objective-C](https://gist.github.com/soffes/812796)
* Luke Redpath: [Objective-C](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)

-->

### 外部风格指南

Futurice 没有公司级别的编码风格指南。然而，浏览其他软件公司的风格指南可能会有所帮助，即使其中一些内容可能非常具有公司特色或带有个人观点。

* GitHub：[Swift](https://github.com/github/swift-style-guide) 和 [Objective-C](https://github.com/github/objective-c-style-guide)
* Ray Wenderlich：[Swift](https://github.com/raywenderlich/swift-style-guide) 和 [Objective-C](https://github.com/raywenderlich/objective-c-style-guide)
* Google：[Objective-C](https://google.github.io/styleguide/objcguide.xml)
* The New York Times：[Objective-C](https://github.com/NYTimes/objective-c-style-guide)
* Sam Soffes：[Objective-C](https://gist.github.com/soffes/812796)
* Luke Redpath：[Objective-C](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)

<!-- 

## Security

Even in an age where we trust our portable devices with the most private data, app security remains an often-overlooked subject. Try to find a good trade-off given the nature of your data; following just a few simple rules can go a long way here. A good resource to get started is Apple's own [iOS Security Guide][apple-security-guide].

### Data Storage

If your app needs to store sensitive data, such as a username and password, an authentication token or some personal user details, you need to keep these in a location where they cannot be accessed from outside the app. Never use `NSUserDefaults`, other plist files on disk or Core Data for this, as they are not encrypted! In most such cases, the iOS Keychain is your friend. If you're uncomfortable working with the C APIs directly, you can use a wrapper library such as [SSKeychain][sskeychain] or [UICKeyChainStore][uickeychainstore].

When storing files and passwords, be sure to set the correct protection level, and choose it conservatively. If you need access while the device is locked (e.g. for background tasks), use the "accessible after first unlock" variety. In other cases, you should probably require that the device is unlocked to access the data. Only keep sensitive data around while you need it.

-->
## 安全

即使在我们信任携带设备保存最私密数据的时代，应用安全仍然是一个经常被忽视的话题。鉴于你数据的性质，尝试找到一个好的权衡方案；在这里遵循几条简单的规则就能走很长的路。开始的一个好资源是苹果自己的[iOS 安全指南][apple-security-guide]。

### 数据存储

如果你的应用需要存储敏感数据，比如用户名和密码、认证令牌或一些个人用户详细信息，你需要将这些数据保存在应用外部无法访问的位置。永远不要使用`NSUserDefaults`，磁盘上的其他 plist 文件或 Core Data 来存储这些数据，因为它们没有加密！在大多数这类情况下，iOS Keychain 是你的朋友。如果你不习惯直接使用 C API，你可以使用诸如[SSKeychain][sskeychain]或[UICKeyChainStore][uickeychainstore]之类的封装库。

在存储文件和密码时，确保设置正确的保护级别，并保守选择它。如果你需要在设备锁定时访问（例如，对于后台任务），使用“第一次解锁后可访问”的类型。在其他情况下，你应该要求设备解锁后才能访问数据。只有在需要敏感数据时才保留它们。

[apple-security-guide]: https://www.apple.com/business/docs/site/iOS_Security_Guide.pdf
[sskeychain]: https://github.com/soffes/sskeychain
[uickeychainstore]: https://github.com/kishikawakatsumi/UICKeyChainStore

<!-- 
### Networking

Keep any HTTP traffic to remote servers encrypted with TLS at all times. To avoid man-in-the-middle attacks that intercept your encrypted traffic, you can set up [certificate pinning][certificate-pinning]. Popular networking libraries such as [AFNetworking][afnetworking-github] and [Alamofire][alamofire-github] support this out of the box.

-->

### 网络

始终保持与远程服务器的 HTTP 流量通过 TLS 加密。为了避免拦截你加密流量的中间人攻击，你可以设置[证书固定][certificate-pinning]。诸如[AFNetworking][afnetworking-github]和[Alamofire][alamofire-github]这样的流行网络库已经支持这个功能。

[certificate-pinning]: https://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning
[afnetworking-github]: https://github.com/AFNetworking/AFNetworking
[alamofire-github]: https://github.com/Alamofire/Alamofire

<!-- 
### Logging

Take extra care to set up proper log levels before releasing your app. Production builds should never log passwords, API tokens and the like, as this can easily cause them to leak to the public. On the other hand, logging the basic control flow can help you pinpoint issues that your users are experiencing.

-->

### 日志

在发布应用程序之前，额外注意设置适当的日志级别。生产版本绝不应记录密码、API 令牌之类的信息，因为这很容易导致它们泄露给公众。另一方面，记录基本的控制流程可以帮助您精确定位用户正在经历的问题。

<!-- 
### User Interface

When using `UITextField`s for password entry, remember to set their `secureTextEntry` property to `true` to avoid showing the password in cleartext. You should also disable auto-correction for the password field, and clear the field whenever appropriate, such as when your app enters the background.

When this happens, it's also good practice to clear the Pasteboard to avoid passwords and other sensitive data from leaking. As iOS may take screenshots of your app for display in the app switcher, make sure to clear any sensitive data from the UI _before_ returning from `applicationDidEnterBackground`.

[apple-security-guide]: https://www.apple.com/business/docs/iOS_Security_Guide.pdf
[sskeychain]: https://github.com/soffes/sskeychain
[uickeychainstore]: https://github.com/kishikawakatsumi/UICKeyChainStore
[certificate-pinning]: https://possiblemobile.com/2013/03/ssl-pinning-for-increased-app-security/
[alamofire-github]: https://github.com/Alamofire/Alamofire
-->

### 用户界面

在使用 `UITextField` 输入密码时，记得将它们的 `secureTextEntry` 属性设置为 `true`，以避免以明文形式显示密码。您还应该为密码字段禁用自动更正，并在适当时清除该字段，例如当您的应用进入后台时。

发生这种情况时，清除剪贴板以避免密码和其他敏感数据泄露也是一个好习惯。由于iOS可能会为应用切换器显示而捕获您的应用屏幕截图，请确保在从 `applicationDidEnterBackground` 返回_之前_清除UI中的任何敏感数据。

[apple-security-guide]: https://www.apple.com/business/docs/iOS_Security_Guide.pdf
[sskeychain]: https://github.com/soffes/sskeychain
[uickeychainstore]: https://github.com/kishikawakatsumi/UICKeyChainStore
[certificate-pinning]: https://possiblemobile.com/2013/03/ssl-pinning-for-increased-app-security/
[alamofire-github]: https://github.com/Alamofire/Alamofire

<!-- 

## Diagnostics

### Compiler warnings

Enable as many compiler warnings as possible and treat those warnings as errors -- [it will be worth it in the long run][warnings-slides].

For Objective-C code, add these values to the _“Other Warning Flags”_ build setting:

- `-Wall` _(enables lots of additional warnings)_
- `-Wextra` _(enables more additional warnings)_

and then enable the _“Treat warnings as errors”_ build setting.

To treat warnings as errors for Swift code, add `-warnings-as-errors` to the _"Other Swift Flags"_ build setting.

[warnings-slides]: https://speakerdeck.com/hasseg/the-compiler-is-your-friend

-->

## 诊断

### 编译器警告

尽可能启用更多的编译器警告，并将这些警告视为错误——[长远来看，这是值得的][warnings-slides]。

对于Objective-C代码，在_“其它警告标志”_构建设置中添加以下值：

- `-Wall` _(启用许多附加警告)_
- `-Wextra` _(启用更多附加警告)_

然后启用 _“将警告视为错误”_ 构建设置。

对于Swift代码，将`-warnings-as-errors`添加到 _“其它Swift标志”_ 构建设置中。

[warnings-slides]: https://speakerdeck.com/hasseg/the-compiler-is-your-friend

<!-- 
### Clang Static Analyzer

The Clang compiler (which Xcode uses) has a _static analyzer_ that performs control and data flow analysis on your code and checks for lots of errors that the compiler cannot.

You can manually run the analyzer from the _Product → Analyze_ menu item in Xcode.

The analyzer can work in either “shallow” or “deep” mode. The latter is much slower but may find more issues due to cross-function control and data flow analysis.

Recommendations:

- Enable _all_ of the checks in the analyzer (by enabling all of the options in the “Static Analyzer” build setting sections).
- Enable the _“Analyze during ‘Build’”_ build setting for your release build configuration to have the analyzer run automatically during release builds. (Seriously, do this — you’re not going to remember to run it manually.)
- Set the _“Mode of Analysis for ‘Analyze’”_ build setting to _Shallow (faster)_.
- Set the _“Mode of Analysis for ‘Build’”_ build setting to _Deep_.

-->

### Clang 静态分析器

Clang 编译器（Xcode 使用的编译器）拥有一个 _静态分析器_，它执行对代码的控制流和数据流分析，并检查编译器无法检查的许多错误。

你可以从 Xcode 的 _Product → Analyze_ 菜单项手动运行分析器。

分析器可以在“浅层”或“深层”模式下工作。后者速度要慢得多，但由于进行了跨函数控制流和数据流分析，可能会发现更多问题。

建议：

- 启用分析器中的_所有_检查（通过启用“Static Analyzer”构建设置部分中的所有选项）。
- 为您的发布构建配置启用 _“Analyze during ‘Build’”_ 构建设置，以便在发布构建期间自动运行分析器。（认真做这个 —— 你不会记得手动运行它。）
- 将 _“Mode of Analysis for ‘Analyze’”_ 构建设置设置为 _Shallow (faster)_。
- 将 _“Mode of Analysis for ‘Build’”_ 构建设置设置为 _Deep_。

<!-- 
### [Faux Pas](http://fauxpasapp.com/)

Created by our very own [Ali Rantakari][ali-rantakari-twitter], Faux Pas is a fabulous static error detection tool. It analyzes your codebase and finds issues you had no idea even existed. There is no Swift support yet, but the tool also offers plenty of language-agnostic rules. Be sure to run it before shipping any iOS (or Mac) app!

_(Note: all Futurice employees get a free license to this — just ask Ali.)_

[ali-rantakari-twitter]: https://twitter.com/AliRantakari

-->

### [Faux Pas](http://fauxpasapp.com/)

由我们自己的 [Ali Rantakari][ali-rantakari-twitter] 创建，Faux Pas 是一个出色的静态错误检测工具。它分析你的代码库，并发现你甚至不知道存在的问题。虽然还不支持 Swift，但该工具也提供了大量与语言无关的规则。在发布任何 iOS（或 Mac）应用程序之前，一定要运行它！

_(注意：所有 Futurice 员工都可以免费获得此许可 —— 只需询问 Ali。)_

[ali-rantakari-twitter]: https://twitter.com/AliRantakari

<!-- 
### Debugging

When your app crashes, Xcode does not break into the debugger by default. To achieve this, add an exception breakpoint (click the "+" at the bottom of Xcode's Breakpoint Navigator) to halt execution whenever an exception is raised. In many cases, you will then see the line of code responsible for the exception. This catches any exception, even handled ones. If Xcode keeps breaking on benign exceptions in third party libraries e.g., you might be able to mitigate this by choosing _Edit Breakpoint_ and setting the _Exception_ drop-down to _Objective-C_.

For view debugging, [Reveal][reveal] and [Spark Inspector][spark-inspector] are two powerful visual inspectors that can save you hours of time, especially if you're using Auto Layout and want to locate views that are collapsed or off-screen. Xcode also has integrated [view debugger][xcode-view-debugging] which is good enough and free to use.

[reveal]: http://revealapp.com/
[spark-inspector]: http://sparkinspector.com
[xcode-view-debugging]: https://developer.apple.com/library/prerelease/content/documentation/DeveloperTools/Conceptual/debugging_with_xcode/chapters/special_debugging_workflows.html

-->

### 调试

当你的应用崩溃时，Xcode 默认不会进入调试器。要实现这一点，可以添加一个异常断点（点击 Xcode 的断点导航器底部的“+”）来在引发异常时暂停执行。在许多情况下，你将看到引起异常的代码行。这能捕获任何异常，即使是已处理的异常。如果 Xcode 不断因第三方库中的良性异常而中断，你可以通过选择 _编辑断点_ 并将 _异常_ 下拉菜单设置为 _Objective-C_ 来缓解这一问题。

对于视图调试，[Reveal][reveal] 和 [Spark Inspector][spark-inspector] 是两个强大的视觉检查工具，它们可以节省你数小时的时间，特别是如果你使用自动布局并希望定位收缩或屏幕外的视图时。Xcode 还集成了 [视图调试器][xcode-view-debugging]，这足够好且免费使用。

[reveal]: http://revealapp.com/
[spark-inspector]: http://sparkinspector.com
[xcode-view-debugging]: https://developer.apple.com/library/prerelease/content/documentation/DeveloperTools/Conceptual/debugging_with_xcode/chapters/special_debugging_workflows.html

<!-- 
### Profiling

Xcode comes with a profiling suite called Instruments. It contains a myriad of tools for profiling memory usage, CPU, network communications, graphics and much more. It's a complex beast, but one of its more straight-forward use cases is tracking down memory leaks with the Allocations instrument. Simply choose _Product_ > _Profile_ in Xcode, select the Allocations instrument, hit the Record button and filter the Allocation Summary on some useful string, like the prefix of your own app's class names. The count in the Persistent column then tells you how many instances of each object you have. Any class for which the instance count increases indiscriminately indicates a memory leak.

Pay extra attention to how and where you create expensive classes. `NSDateFormatter`, for instance, is very expensive to create and doing so in rapid succession, e.g. inside a `tableView:cellForRowAtIndexPath:` method, can really slow down your app. Instead, keep a static instance of it around for each date format that you need.
-->
### 性能分析

Xcode附带了一个名为Instruments的性能分析套件。它包含了大量用于分析内存使用、CPU、网络通信、图形等多方面的工具。这是一个复杂的工具集，但它的一个更直接的用例是使用Allocations工具追踪内存泄露。只需在Xcode中选择 _产品_ > _分析_，选择Allocations工具，点击记录按钮，并在分配摘要中使用一些有用的字符串进行过滤，比如你自己应用的类名前缀。然后，持久列中的计数会告诉你每个对象有多少个实例。任何实例计数无差别增加的类都表明存在内存泄露。

要特别注意你在哪里以及如何创建昂贵的类。例如，`NSDateFormatter`的创建成本非常高，如果在`tableView:cellForRowAtIndexPath:`方法中连续快速创建，真的会拖慢你的应用。相反，为你需要的每种日期格式保留它的静态实例。
<!-- 

## Analytics

Including some analytics framework in your app is strongly recommended, as it allows you to gain insights on how people actually use it. Does feature X add value? Is button Y too hard to find? To answer these, you can send events, timings and other measurable information to a service that aggregates and visualizes them – for instance, [Google Tag Manager][google-tag-manager]. The latter is more versatile than Google Analytics in that it inserts a data layer between app and Analytics, so that the data logic can be modified through a web service without having to update the app.

[google-tag-manager]: https://www.google.com/analytics/tag-manager/

A good practice is to create a slim helper class, e.g. `AnalyticsHelper`, that handles the translation from app-internal models and data formats (`FooModel`, `NSTimeInterval`, …) to the mostly string-based data layer:

```swift

func pushAddItemEvent(with item: Item, editMode: EditMode) {
    let editModeString = name(for: editMode)

    pushToDataLayer([
        "event": "addItem",
        "itemIdentifier": item.identifier,
        "editMode": editModeString
    ])
}

```

This has the additional advantage of allowing you to swap out the entire Analytics framework behind the scenes if needed, without the rest of the app noticing.

-->
## 分析

强烈推荐在你的应用中包含一些分析框架，因为它可以让你了解人们实际如何使用它。功能X是否增加了价值？按钮Y是否太难找到？为了回答这些问题，你可以向一个聚合和可视化这些信息的服务发送事件、计时和其他可测量的信息 - 例如，[Google Tag Manager][google-tag-manager]。后者比Google Analytics更加灵活，因为它在应用和Analytics之间插入了一个数据层，这样可以通过网络服务修改数据逻辑，而无需更新应用。

[google-tag-manager]: https://www.google.com/analytics/tag-manager/

一个好的做法是创建一个精简的助手类，例如`AnalyticsHelper`，它处理从应用内部模型和数据格式（`FooModel`，`NSTimeInterval`，…）到主要基于字符串的数据层的转换：

```swift

func pushAddItemEvent(with item: Item, editMode: EditMode) {
    let editModeString = name(for: editMode)

    pushToDataLayer([
        "event": "addItem",
        "itemIdentifier": item.identifier,
        "editMode": editModeString
    ])
}

```

这还有一个额外的优势，如果需要，允许你在幕后替换整个分析框架，而应用的其他部分不会察觉。
<!-- 
### Crash Logs

First you should make your app send crash logs onto a server somewhere so that you can access them. You can implement this manually (using [PLCrashReporter][plcrashreporter] and your own backend) but it’s recommended that you use an existing service instead — for example one of the following:

* [Fabric](https://get.fabric.io)
* [HockeyApp](http://hockeyapp.net)
* [Crittercism](https://www.crittercism.com)
* [Splunk MINTexpress](https://mint.splunk.com)
* [Instabug](https://instabug.com/)

[plcrashreporter]: https://www.plcrashreporter.org

Once you have this set up, ensure that you _save the Xcode archive (`.xcarchive`)_ of every build you release. The archive contains the built app binary and the debug symbols (`dSYM`) which you will need to symbolicate crash reports from that particular version of your app.
-->

### 崩溃日志

首先，你应该让你的应用将崩溃日志发送到某个服务器上，以便你可以访问它们。你可以手动实现这一点（使用 [PLCrashReporter][plcrashreporter] 和你自己的后端），但建议你使用现有的服务，例如以下之一：

* [Fabric](https://get.fabric.io)
* [HockeyApp](http://hockeyapp.net)
* [Crittercism](https://www.crittercism.com)
* [Splunk MINTexpress](https://mint.splunk.com)
* [Instabug](https://instabug.com/)

[plcrashreporter]: https://www.plcrashreporter.org

一旦你设置好这些，确保你 _save the Xcode archive (`.xcarchive`)_。存档包含了构建的应用二进制文件和调试符号（`dSYM`），你将需要这些来符号化来自该特定版本应用的崩溃报告。

<!-- 


## Building

This section contains an overview of this topic — please refer here for more comprehensive information:

- [iOS Developer Library: Xcode Concepts][apple-xcode-concepts]
- [Samantha Marshall: Managing Xcode][pewpew-managing-xcode]

[apple-xcode-concepts]: https://developer.apple.com/library/ios/featuredarticles/XcodeConcepts/
[pewpew-managing-xcode]: http://pewpewthespells.com/blog/managing_xcode.html

-->

## 构建

本节包含了此主题的概述 — 请参考以下资源获取更全面的信息：

- [iOS 开发者库：Xcode 概念][apple-xcode-concepts]
- [Samantha Marshall: 管理 Xcode][pewpew-managing-xcode]

[apple-xcode-concepts]: https://developer.apple.com/library/ios/featuredarticles/XcodeConcepts/
[pewpew-managing-xcode]: http://pewpewthespells.com/blog/managing_xcode.html


<!-- 
### Build Configurations

Even simple apps can be built in different ways. The most basic separation that Xcode gives you is that between _debug_ and _release_ builds. For the latter, there is a lot more optimization going on at compile time, at the expense of debugging possibilities. Apple suggests that you use the _debug_ build configuration for development, and create your App Store packages using the _release_ build configuration. This is codified in the default scheme (the dropdown next to the Play and Stop buttons in Xcode), which commands that _debug_ be used for Run and _release_ for Archive.

However, this is a bit too simple for real-world applications. You might – no, [_should!_][futurice-environments] – have different environments for testing, staging and other activities related to your service. Each might have its own base URL, log level, bundle identifier (so you can install them side-by-side), provisioning profile and so on. Therefore a simple debug/release distinction won't cut it. You can add more build configurations on the "Info" tab of your project settings in Xcode.

[futurice-environments]: http://futurice.com/blog/five-environments-you-cannot-develop-without

-->

### 构建配置

即使是简单的应用程序也可以通过不同的方式构建。Xcode为你提供的最基本区分就是 _调试（debug）_ 和 _发布（release）_ 构建之间的区别。对于后者，在编译时会进行更多的优化，但这是以牺牲调试可能性为代价的。苹果建议你在开发时使用 _调试_ 构建配置，并使用 _发布_ 构建配置来创建你的App Store包。这在默认方案中得到了体现（Xcode中播放和停止按钮旁边的下拉菜单），它命令使用 _调试_ 来运行和使用 _发布_ 来归档。

然而，对于真实世界的应用来说，这种区分有点过于简单了。你可能 —— 不，[_应该！_][futurice-environments] —— 为测试、预发布和其他与你的服务相关的活动拥有不同的环境。每个环境可能有其自己的基础URL、日志级别、包标识符（这样你就可以并排安装它们）、配置文件等等。因此，简单的调试/发布区分是不够的。你可以在Xcode的项目设置中的“信息”选项卡上添加更多的构建配置。

[futurice-environments]: http://futurice.com/blog/five-environments-you-cannot-develop-without

<!-- 
#### `xcconfig` files for build settings

Typically build settings are specified in the Xcode GUI, but you can also use _configuration settings files_ (“`.xcconfig` files”) for them. The benefits of using these are:

- You can add comments to explain things.
- You can `#include` other build settings files, which helps you avoid repeating yourself:
    - If you have some settings that apply to all build configurations, add a `Common.xcconfig` and `#include` it in all the other files.
    - If you e.g. want to have a “Debug” build configuration that enables compiler optimizations, you can just `#include "MyApp_Debug.xcconfig"` and override one of the settings.
- Conflict resolution and merging becomes easier.

Find more information about this topic in [these presentation slides][xcconfig-slides].

[xcconfig-slides]: https://speakerdeck.com/hasseg/xcode-configuration-files

-->

#### 使用 `xcconfig` 文件进行构建设置

通常，构建设置是在Xcode GUI中指定的，但你也可以使用 _配置设置文件_（“`.xcconfig` 文件”）来进行设置。使用它们的好处包括：

- 你可以添加注释来解释事情。
- 你可以使用 `#include` 引入其他构建设置文件，这有助于避免重复：
    - 如果你有一些适用于所有构建配置的设置，可以添加一个 `Common.xcconfig` 并在所有其他文件中 `#include` 它。
    - 如果你比如想要一个启用了编译器优化的“调试”构建配置，你可以仅仅 `#include "MyApp_Debug.xcconfig"` 并覆盖其中的一个设置。
- 冲突解决和合并变得更容易。

有关此主题的更多信息，请查看[这些演示幻灯片][xcconfig-slides]。

[xcconfig-slides]: https://speakerdeck.com/hasseg/xcode-configuration-files

<!-- 
### Targets

A target resides conceptually below the project level, i.e. a project can have several targets that may override its project settings. Roughly, each target corresponds to "an app" within the context of your codebase. For instance, you could have country-specific apps (built from the same codebase) for different countries' App Stores. Each of these will need development/staging/release builds, so it's better to handle those through build configurations, not targets. It's not uncommon at all for an app to only have a single target.

-->
### 目标（Targets）

在概念上，目标位于项目级别之下，即一个项目可以拥有多个可能会覆盖其项目设置的目标。大致上，每个目标对应于代码库上下文中的“一个应用”。例如，你可以有针对不同国家的App Store的国家特定应用（从同一个代码库构建）。每个应用都需要开发/预发布/发布构建，因此最好通过构建配置而不是目标来处理这些。对于一个应用来说，只有一个目标一点也不罕见。

<!-- 
### Schemes

Schemes tell Xcode what should happen when you hit the Run, Test, Profile, Analyze or Archive action. Basically, they map each of these actions to a target and a build configuration. You can also pass launch arguments, such as the language the app should run in (handy for testing your localizations!) or set some diagnostic flags for debugging.

A suggested naming convention for schemes is `MyApp (<Language>) [Environment]`:

    MyApp (English) [Development]
    MyApp (German) [Development]
    MyApp [Testing]
    MyApp [Staging]
    MyApp [App Store]

For most environments the language is not needed, as the app will probably be installed through other means than Xcode, e.g. TestFlight, and the launch argument thus be ignored anyway. In that case, the device language should be set manually to test localization.
-->

### 方案（Schemes）

方案告诉Xcode当你点击运行（Run）、测试（Test）、分析（Profile）、分析（Analyze）或归档（Archive）动作时应该发生什么。基本上，它们将这些动作映射到一个目标和一个构建配置上。你还可以传递启动参数，比如应用应该以哪种语言运行（对测试你的本地化非常方便！）或设置一些调试用的诊断标志。

对于方案的建议命名约定是 `MyApp (<Language>) [Environment]`：

    MyApp (英语) [开发]
    MyApp (德语) [开发]
    MyApp [测试]
    MyApp [预发布]
    MyApp [App Store]

对于大多数环境来说，不需要指定语言，因为应用可能会通过Xcode之外的其他方式安装，例如TestFlight，而启动参数也因此会被忽略。在这种情况下，应该手动设置设备语言以测试本地化。

<!-- 


## Deployment

Deploying software on iOS devices isn't exactly straightforward. That being said, here are some central concepts that, once understood, will help you tremendously with it.

-->

## 部署

在iOS设备上部署软件并不完全直接。话虽如此，这里有一些核心概念，一旦理解，将极大地帮助你进行部署。

<!-- 
### Signing

Whenever you want to run software on an actual device (as opposed to the simulator), you will need to sign your build with a __certificate__ issued by Apple. Each certificate is linked to a private/public keypair, the private half of which resides in your Mac's Keychain. There are two types of certificates:

* __Development certificate:__ Every developer on a team has their own, and it is generated upon request. Xcode might do this for you, but it's better not to press the magic "Fix issue" button and understand what is actually going on. This certificate is needed to deploy development builds to devices.
* __Distribution certificate:__ There can be several, but it's best to keep it to one per organization, and share its associated key through some internal channel. This certificate is needed to ship to the App Store, or your organization's internal "enterprise app store".

-->

### 签名

每当你想在实际设备上运行软件（与在模拟器上相反）时，你都需要使用苹果颁发的 __证书__ 来签署你的构建。每个证书都链接到一个私钥/公钥对，私钥部分存储在你的Mac的钥匙串中。证书有两种类型：

* __开发证书：__ 团队中的每个开发者都有自己的证书，且在请求时生成。Xcode可能会为你做这件事，但最好不要仅仅点击“修复问题”按钮而不理解实际发生了什么。这个证书是部署开发构建到设备上所需要的。
* __分发证书：__ 可能有几个，但最好每个组织保持一个，并通过某种内部渠道共享其关联的密钥。这个证书是将应用发布到App Store或你组织的内部“企业应用商店”所需要的。

<!-- 
### Provisioning

Besides certificates, there are also __provisioning profiles__, which are basically the missing link between devices and certificates. Again, there are two types to distinguish between development and distribution purposes:

* __Development provisioning profile:__ It contains a list of all devices that are authorized to install and run the software. It is also linked to one or more development certificates, one for each developer that is allowed to use the profile. The profile can be tied to a specific app or use a wildcard App ID (*). The latter is [discouraged][jared-sinclair-signing-tips], because Xcode is notoriously bad at picking the correct files for signing unless guided in the right direction. Also, certain capabilities like Push Notifications or App Groups require an explicit App ID.

* __Distribution provisioning profile:__ There are three different ways of distribution, each for a different use case. Each distribution profile is linked to a distribution certificate, and will be invalid when the certificate expires.
    * __Ad-Hoc:__ Just like development profiles, it contains a whitelist of devices the app can be installed to. This type of profile can be used for beta testing on 100 devices per year. For a smoother experience and up to 1000 distinct users, you can use Apple's newly acquired [TestFlight][testflight] service. Supertop offers a good [summary of its advantages and issues][testflight-discussion].
    * __App Store:__ This profile has no list of allowed devices, as anyone can install it through Apple's official distribution channel. This profile is required for all App Store releases.
    * __Enterprise:__ Just like App Store, there is no device whitelist, and the app can be installed by anyone with access to the enterprise's internal "app store", which can be just a website with links. This profile is available only on Enterprise accounts.

[jared-sinclair-signing-tips]: http://blog.jaredsinclair.com/post/116436789850/
[testflight]: https://developer.apple.com/testflight/
[testflight-discussion]: http://blog.supertop.co/post/108759935377/app-developer-friends-try-testflight

To sync all certificates and profiles to your machine, go to Accounts in Xcode's Preferences, add your Apple ID if needed, and double-click your team name. There is a refresh button at the bottom, but sometimes you just need to restart Xcode to make everything show up.

-->

### 配置文件

除了证书外，还有 __配置文件（provisioning profiles）__，它们基本上是设备和证书之间缺失的环节。同样地，有两种类型的配置文件，用以区分开发和分发目的：

* __开发配置文件：__ 它包含所有被授权安装和运行软件的设备的列表。它还关联到一个或多个开发证书，每个允许使用该配置文件的开发者对应一个证书。配置文件可以绑定到一个特定的应用，或使用通配符App ID（*）。后者被[不推荐使用][jared-sinclair-signing-tips]，因为Xcode在选择正确的签名文件时非常不准确，除非被引导到正确的方向。此外，某些功能如推送通知或应用组需要一个明确的App ID。

* __分发配置文件：__ 有三种不同的分发方式，每种方式适用于不同的用例。每个分发配置文件都关联到一个分发证书，并且当证书过期时会变得无效。
    * __Ad-Hoc：__ 就像开发配置文件一样，它包含了一个应用可以被安装的设备的白名单。这种类型的配置文件可以用于每年在100台设备上进行beta测试。为了获得更顺畅的体验和最多1000名独特用户，你可以使用苹果新收购的[TestFlight][testflight]服务。Supertop提供了一个关于其优点和问题的好[总结][testflight-discussion]。
    * __App Store：__ 这个配置文件没有允许的设备列表，因为任何人都可以通过苹果的官方分发渠道安装它。这个配置文件是所有App Store发布所必需的。
    * __企业：__ 就像App Store一样，没有设备白名单，任何有权访问企业内部“应用商店”的人都可以安装该应用，这个“应用商店”可以仅仅是一个带有链接的网站。这个配置文件仅在企业账户上可用。

要将所有证书和配置文件同步到你的机器上，请转到Xcode的偏好设置中的账户，如有需要添加你的Apple ID，并双击你的团队名称。底部有一个刷新按钮，但有时你只需要重启Xcode来使一切显示出来。

[jared-sinclair-signing-tips]: http://blog.jaredsinclair.com/post/116436789850/
[testflight]: https://developer.apple.com/testflight/
[testflight-discussion]: http://blog.supertop.co/post/108759935377/app-developer-friends-try-testflight


<!-- 
#### Debugging Provisioning

Sometimes you need to debug a provisioning issue. For instance, Xcode may refuse to install the build to an attached device, because the latter is not on the (development or ad-hoc) profile's device list. In those cases, you can use Craig Hockenberry's excellent [Provisioning][provisioning] plugin by browsing to `~/Library/MobileDevice/Provisioning Profiles`, selecting a `.mobileprovision` file and hitting Space to launch Finder's Quick Look feature. It will show you a wealth of information such as devices, entitlements, certificates, and the App ID.

When dealing with an existing app archive (`.ipa`), you can inspect its provisioning profile in a similar fashion: Simply rename the `*.ipa` to `*.zip`, unpack it and find the `.app` package within. From its Finder context menu, choose "Show Package Contents" to see a file called `embedded.mobileprovision` that you can examine with the above method.

[provisioning]: https://github.com/chockenberry/Provisioning

-->

#### 调试配置问题

有时，您可能需要调试配置问题。例如，Xcode 可能拒绝将构建安装到已连接的设备上，因为后者不在（开发或临时）配置文件的设备列表中。在这些情况下，您可以使用 Craig Hockenberry 出色的 [配置][provisioning] 插件，通过浏览到 `~/Library/MobileDevice/Provisioning Profiles`，选择一个 `.mobileprovision` 文件并按空格键启动 Finder 的快速查看功能。它将向您展示丰富的信息，如设备、授权、证书和 App ID。

当处理现有的应用归档（`.ipa`）时，您可以以类似的方式检查其配置文件：只需将 `*.ipa` 重命名为 `*.zip`，解包它并找到其中的 `.app` 包。从其 Finder 上下文菜单中，选择“显示包内容”来查看一个名为 `embedded.mobileprovision` 的文件，您可以使用上述方法检查它。

[provisioning]: https://github.com/chockenberry/Provisioning

<!-- 
### Uploading

[App Store Connect][appstore-connect] is Apple's portal for managing your apps on the App Store. To upload a build, Xcode requires an Apple ID that is part of the developer account used for signing. Nowadays Apple has allowed for single Apple IDs to be part of multiple App Store Connect accounts (i.e. client organizations) in both Apple's developer portal as well as App Store Connect.

After uploading the build, be patient as it can take up to an hour for it to show up under the Builds section of your app version. When it appears, you can link it to the app version and submit your app for review.

[appstore-connect]: https://appstoreconnect.apple.com
-->

### 上传

[App Store Connect][appstore-connect] 是苹果公司用于在 App Store 上管理您的应用的门户网站。要上传构建，Xcode 需要一个作为签名开发者账户一部分的 Apple ID。如今，苹果允许单个 Apple ID 成为多个 App Store Connect 账户（即客户组织）的一部分，无论是在苹果的开发者门户网站还是在 App Store Connect 中都是如此。

上传构建后，请耐心等待，因为它可能需要长达一个小时的时间才能在您应用版本的构建部分显示。当它出现后，您可以将其链接到应用版本并提交您的应用以供审核。

[appstore-connect]: https://appstoreconnect.apple.com

<!-- 


## In-App Purchases (IAP)

When validating in-app purchase receipts, remember to perform the following checks:

- __Authenticity:__ That the receipt comes from Apple
- __Integrity:__ That the receipt has not been tampered with
- __App match:__ That the app bundle ID in the receipt matches your app’s bundle identifier
- __Product match:__ That the product ID in the receipt matches your expected product identifier
- __Freshness:__ That you haven’t seen the same receipt ID before.

Whenever possible, design your IAP system to store the content for sale server-side, and provide it to the client only in exchange for a valid receipt that passes all of the above checks. This kind of a design thwarts common piracy mechanisms, and — since the validation is performed on the server — allows you to use Apple’s HTTP receipt validation service instead of interpreting the receipt `PKCS #7` / `ASN.1` format yourself.

For more information on this topic, check out the [Futurice blog: Validating in-app purchases in your iOS app][futu-blog-iap].

[futu-blog-iap]: http://futurice.com/blog/validating-in-app-purchases-in-your-ios-app
-->

## 应用内购买（IAP）

在验证应用内购买收据时，请记得执行以下检查：

- __真实性：__ 该收据来自苹果
- __完整性：__ 该收据未被篡改
- __应用匹配：__ 收据中的应用捆绑 ID 与您的应用的捆绑标识符匹配
- __产品匹配：__ 收据中的产品 ID 与您期望的产品标识符匹配
- __新鲜度：__ 您之前没有见过相同的收据 ID。

尽可能地设计您的应用内购买系统，将待售内容存储在服务器端，并仅在交换通过以上所有检查的有效收据时才向客户端提供。这种设计挫败了常见的盗版机制，并且—由于验证是在服务器上执行—允许您使用苹果的 HTTP 收据验证服务，而不是自己解释收据的 `PKCS #7` / `ASN.1` 格式。

有关此主题的更多信息，请查看 [Futurice 博客：在您的 iOS 应用中验证应用内购买][futu-blog-iap]。

[futu-blog-iap]: http://futurice.com/blog/validating-in-app-purchases-in-your-ios-app

<!-- 


## License

[Futurice][futurice] • Creative Commons Attribution 4.0 International (CC BY 4.0)

[futurice]: http://futurice.com/


## More Ideas

- Add list of suggested compiler warnings
- Ask IT about automated Jenkins build machine
- Add section on Testing
- Add "proven don'ts"

[reactivecocoa-github]: https://github.com/ReactiveCocoa/ReactiveCocoa

-->
## 许可证

[Futurice][futurice] • 知识共享署名 4.0 国际许可协议 (CC BY 4.0)

[futurice]: http://futurice.com/


## 更多想法

- 添加建议的编译器警告列表
- 向 IT 部门询问自动化的 Jenkins 构建机器
- 添加关于测试的章节
- 添加“经证实不可行”的内容

[reactivecocoa-github]: https://github.com/ReactiveCocoa/ReactiveCocoa

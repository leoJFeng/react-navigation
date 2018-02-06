# React Navigation Contributor Guide

想要参与改进React Navigation?非常欢迎您的帮助！

这里有一些方式可以帮助完善项目。

- [Bugs反馈](#Reporting-Bugs)
- [完善文档](#Improving-the-Documentation)
- [Issues反馈](#Responding-to-Issues)
- [Bug修复](#Bug-Fixes)
- [功能建议](#Suggesting-a-Feature)
- [重大的Pull Requests](#Big-Pull-Requests)

这里有一些非常有用的资源可用来开始：

- [Issue模板](#Issue-Template)
- [Pull Request模板](#Pull-Request-Template)
- [Fork仓库](#Forking-the-Repository)
- [指南代码回顾](#Code-Review-Guidelines)
- [示例app运行](#Run-the-Example-App)
- [网站运行](#Run-the-Website)
- [测试示例运行和类型检测](#Run-Tests-and-Type-Checking)

> 注意我们现在在示例里使用的是Yarn但如果你用NPM我们也同样欢迎。

## Contributing

### Reporting-Bugs

你不能指望写代码没有任何bug出现，特别是现在React Navigation还在beta版更新迭代特别快，bugs会有的。当你认为自己发现问题后可以采取的做法是:

1. 查看已存在的跟你的问题类似的issues。如果你发现有同样问题，点个赞👍 （请不要评论+1）。通读完整的评论看是否能提供更多有价值的线索。
2. 如果没有任何issues跟你的问题类似，创建一个新的issue。请确保按[issue模板](https://github.com/react-community/react-navigation/blob/master/.github/ISSUE_TEMPLATE.md)创建。

创建一个高质量的反馈是很重要的。没有这些我们可能无法修复这些bug。在理想情况下可能你会发现这其实不是bug而是自己构建有问题。立即就解决了！

### Improving-the-Documentation

任何成功的项目都需要高质量的文档，React Navigation也不例外。

文档现在是如下组织的：

- __Getting Started__: 简单介绍React Navigation的基本内容。帮助用户快速运行起来。介绍一些示例和关键的方法
- __Navigators__: 所有导航器的API文档和支持的APIs
- __Advanced Guides__: （高级）除了这些基本的外React Navigation还可以用来做什么？这里有更多探讨和示例。
- __Routers__: （高级）所有路由的API文档和如何使用/定制它们
- __Views__: （高级）所有页面的API文档和如何使用/定制它们

文档目前存在的类别和分档不是固定的。 如果您的文档资料适用于任何现有文档，请将其添加到那里。 如果为您的贡献是有必要新增文档的，请将其添加到文档索引中。

这些文档在[App.js](https://github.com/react-community/react-navigation/blob/master/website/src/App.js)中被编入索引，其中所有的页面都与标题一起声明。 要测试文档，请按照说明运行网站。

`docs`文件夹里的Markdown文档会在构建过程中被转为json文件。 要让更新的文档出现在网站上，请从`react-navigation`根文件夹运行`yarn run build-docs`来重新运行构建步骤。

### Responding to Issues

另一个帮助React navigation的好方法就是对issues的问题进行回应。也许是回答默认的问题，指出他们代码中的一个小错字，或者帮助他们重新构建。如果你对成为React Navigation的一个活跃分子感兴趣从回应issues开始吧！不仅仅是有用的而且评论还能展示了你的代码水平。

### Bug Fixes

找到问题解决它，今天一天都会好心情！就像之前所说，bug在所难免。如果你找到了，可以如下处理:

1. 检查针对该bug是否已经有pull request存在。如果存在检查一遍并留下你的评论
2. 如果没有现有的pull request，那么就指出及修复它。如果该bug相对较小那么就去修复它吧。如果这是一项影响很大改动很多代码，我们可以先讨论一下（查看big pull request部分）
3. 如果仍有issue跟该bug相关，留下评论链接到你的pull request，让别人知道已经被标记了

查看[help wanted](https://github.com/react-community/react-navigation/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) 和[good first issue tags](https://github.com/react-community/react-navigation/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)去看看哪里你可以帮助指出的！

### Suggesting a Feature

有什么是你想在React Navigation里看到的?去新建一个issue，描述你想实现和为什么要实现。有一些事你需要考虑一下：

1. 是否已经有实现方法而你想简化它？
2. 你有任何如何实现它的想法吗？你有没有做过类似的功能?

在功能讨论好之前先别提交功能的pull requests。一旦功能被讨论好确定开发，就开始创建pull request吧（跟随[big pull request](#big-pull-requests)指南。

### Big Pull Requests

项目里任何增删改文件（新功能或者修复bug），在写代码之前都要先缓一下，有几个理由这样做：

1. 重大的Pull Requests会耗费大量时间去回顾代码有时还难以理解。
2. 通常你不需要如你想象般改动这么多。

牢记于心，然后下面还有几条建议：

1. 开一条issue清晰地描述你想要实现什么和你想要实现的原因
2. 在社区里和维护者讨论清楚实现方法。提供上下文，建立边界测试，以及指出设计方式
3. 确定行动计划
4. 编写代码及提交
5. 检查PR，这虽然有点耗时，但如果你遵循了上述步骤，相信也不会耗费太多时间的

我们要这样做的原因就是为了节省大家的时间。也许功能已经实现但没写在文档里？也许功能跟该库不兼容？不管怎样，做出重大改变时先讨论一下总会节省大家的时间。

## Information

### Issue Template

在提交Issue申请前先看一下[issue模板](https://github.com/react-community/react-navigation/blob/master/.github/ISSUE_TEMPLATE.md)和遵循它。这是为了帮助每个人更好地理解你所遇到的问题，并减少来回获取必要的信息。

是的，完成issue模板需要时间和精力。 但是，这是询问高质量问题从而得到回应的唯一方法。

你愿意花1分钟时间创建一个不完整的issue报告，然后等待几个月才能得到任何回应？ 或者你愿意花20分钟时间填写一份高质量的issue报告，并附上所有必要的内容，并在几天内得到答复？ 对于愿意花时间审查您的问题的人来说，这也是一件值得尊敬的事情。

### Pull Request Template

就跟issue模板很像，[pull request 模板](https://github.com/react-community/react-navigation/blob/master/.github/PULL_REQUEST_TEMPLATE.md)会列出说明，以确保您的pull request得到及时检查并减少来回询问。 在开始编写任何代码之前，一定要仔细阅读。

### Forking the Repository

- 在Github fork [`react-navigation`](https://github.com/react-community/react-navigation) on GitHub
- 在控制台使用如下命令来本地下载并安装：

```bash
git clone https://github.com/<USERNAME>/react-navigation.git
cd react-navigation
git remote add upstream https://github.com/react-community/react-navigation.git
yarn install
```

### Code Review Guidelines

多看代码用符合之前代码基础的风格编写代码。这个项目使用ESLint和Flow来全包项目统一性。你可以通过如下命令检查你的项目：

```bash
yarn run eslint
yarn run flow-check
```

如果有任何错误出现，要么你手动修复他们或者你可以使用`yarn run format`自动修复他们。

### Run the Example App

```bash
yarn install
cd examples/NavigationPlayground
yarn install
yarn start
```

你将会展示一个二维码来扫描Expo app。你可以在这里获取[Expo](https://docs.expo.io/versions/latest/index.html) 如果你还没有获取。

All examples:

- [NavigationPlayground](https://github.com/react-community/react-navigation/tree/master/examples/NavigationPlayground)
- [ReduxExample](https://github.com/react-community/react-navigation/tree/master/examples/ReduxExample)
- [SafeAreaExample](https://github.com/react-community/react-navigation/tree/master/examples/SafeAreaExample)

Commands are the same as above for any of the example apps. If you run into any issues, please try the following to start fresh:

```bash
watchman watch-del-all
yarn start -- --reset-cache
```

### Run the Website

为了开发者模式和实时预览:

```bash
cd website
yarn install
yarn start
```

要让网站在生产模式运行并在服务端渲染：

```bash
yarn run prod
```

如果你已经对`docs`文件夹做了任何更改，你需要在网站上线前在项目根目录下运行`yarn run build-docs`。

### Run Tests and Type-Checking

React Navigation有[Jest](https://facebook.github.io/jest/)代码测试和[Flow](https://flow.org/)类型检测。要运行这些检测，在React Navigation根目录下运行如下命令（前提是已经安装了`node_modules`）。

```bash
yarn run jest
yarn run flow-check
```

我们的CI会执行这些命令以及需要在每次代码提交后都运行确保通过。

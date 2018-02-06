# Views

导航页面是获得了[`router`](/docs/api/routers)和[`navigation`](/docs/navigators/navigation-prop)属性的展示组件，而当指定了`navigation.state`时也会展示几个页面。

导航页面是受控的React组件，可以展示当前导航的状态。他们管理切换页面，动画和手势。他们也可以展示统一的导航页面譬如tabbars或headers。

## Built in Views

- [CardStack](https://github.com/react-community/react-navigation/blob/master/src/views/CardStack/CardStack.js) - 提供一个适合任何平台的堆栈
    + [Card](https://github.com/react-community/react-navigation/blob/master/src/views/CardStack/Card.js) - 用手势呈现卡页堆叠中的一张卡页
    + [Header](https://github.com/react-community/react-navigation/blob/master/src/views/Header/Header.js) - 卡页堆栈的标题视图
- [Tabs](https://github.com/react-community/react-navigation/blob/master/src/views/TabView/TabView.js) - 可配置的tab切换器/分页器
- [Drawer](https://github.com/react-community/react-navigation/blob/master/src/views/Drawer/DrawerView.js) - 从左侧滑动出现的抽屉


## [Transitioner](/docs/views/transitioner)

`Transitioner`管理页面切换的动画以及可以用来建立完全自定义的导航页面。它在`CardStack`页面里使用。[了解更多关于`Transitioner`](/docs/views/transitioner)。

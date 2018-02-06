# 导航器

导航器允许你定义自己的导航结构。导航器同时也可以让你配置通用元素譬如头部栏或者tabbar等。

在底层导航器也被解析为React组件。

## 导航器创建

`react-navigation`包含如下几个方法来帮助你创建导航器：

- [StackNavigator](/docs/navigators/stack) - 渲染一个页面同时提供页面间的跳转。当一个新页面打开时会被放置到栈顶
- [TabNavigator](/docs/navigators/tab) - 渲染一个标签栏页面让用户可以在几个页面间切换
- [DrawerNavigator](/docs/navigators/drawer) - 渲染一个抽屉页面，可以从屏幕左边滑出

## 渲染一个带有导航器的页面

导航器渲染的screens仅仅是React组件

要学习怎么创建screen，阅读相关：
- [Screen `navigation`](/docs/navigators/navigation-prop) 属性允许页面发起导航行为，譬如打开新页面
- [Screen `navigationOptions`](/docs/navigators/navigation-options) 定制导航的页面展示方式（标题，标签等）

### 顶部组件使用导航

如果你想在相同层级使用导航器，你可以使用react的[`ref`](https://facebook.github.io/react/docs/refs-and-the-dom.html#the-ref-callback-attribute)项：

```js
import { NavigationActions } from 'react-navigation';

const AppNavigator = StackNavigator(SomeAppRouteConfigs);

class App extends React.Component {
  someEvent() {
    // call navigate for AppNavigator here:
    this.navigator && this.navigator.dispatch(
      NavigationActions.navigate({ routeName: someRouteName })
    );
  }
  render() {
    return (
      <AppNavigator ref={nav => { this.navigator = nav; }} />
    );
  }
}
```
要注意这种解决方法仅仅在导航器顶层可以使用。

## 导航容器

内部的导航器即使navigation属性丢失也可以像顶层导航器一样工作。此原理是因为提供了一个隐形的导航容器，而这也是顶层的navigation属性的来源。

当渲染上述其中一个导航器时，navigation属性是可选的。当该属性丢失时，容器会自行管理自己的navigation状态。它也同时可以管理URLs，外部链接，安卓返回键处理等。

为了方便内部的导航器能拥有这项能力他们会在`scenes`使用`createNavigationContainer`创建。通常导航器需要一个`navigation`属性来实现该方法。

顶层的导航器会接受如下属性：

### `onNavigationStateChange(prevState, newState, action)`

该方法会在每次导航状态改变时调用。它接受一个旧状态，一个导航器新状态以及使得状态改变的行为（actions）。默认它会在控制台打印状态的改变。

### `uriPrefix`

App处理的URLs前缀。这会在处理[深层链接](/docs/guides/linking) 时传递路径给路由。

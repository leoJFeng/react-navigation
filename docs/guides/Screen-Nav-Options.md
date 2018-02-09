
# Screen Navigation Options

每个页面都可以在父页面里配置几个关于如何展示的属性。

#### Two Ways to specify each option

**静态配置：** 每个导航选项都可以直接配置：

```js
class MyScreen extends React.Component {
  static navigationOptions = {
    title: 'Great',
  };
  ...
```

**动态配置：**

选项也可以通过接受如下几个参数的方法来返回一个导航选项对象，这样会覆盖之前在路由里定义或在导航器里定义的导航器选项。

- `props` - 相同的属性对每个页面都可用：
  - `navigation` - 页面的[navigation](/docs/navigators/navigation-prop) 属性，它还在`navigation.state`里持有页面的路由。
  - `screenProps` - 该属性会从上面的导航器组件传递下来
  - `navigationOptions` - 默认或者旧的选项会被沿用如果新值没被指定

```js
class ProfileScreen extends React.Component {
  static navigationOptions = ({ navigation, screenProps }) => ({
    title: navigation.state.params.name + "'s Profile!",
    headerRight: <Button color={screenProps.tintColor} {...} />,
  });
  ...
```

screenProps属性会在页面渲染时传递下来。如果页面是如下配置：

```js
<SimpleApp
  screenProps={{tintColor: 'blue'}}
  // navigation={{state, dispatch}} // optionally control the app
/>
```

#### 通用Navigation配置

每个导航页的`title`属性都是通用的，它会被用作设置为每个页面的标题。

```js
class MyScreen extends React.Component {
  static navigationOptions = {
    title: 'Great',
  };
  ...
```

不像其他导航选项只能被导航页面使用，title选项可以被应用来更新浏览器标题或者 app switcher。

#### 默认Navigation选项

在页面设置`navigationOptions`是很常用的，但有时我们也会在导航器上设置该属性。

想象如下场景：
你的`TabNavigator`要展示其中一个页面，而它嵌套在顶层的`StackNavigator`里:

```
StackNavigator({
  route1: { screen: RouteOne },
  route2: { screen: MyTabNavigator },
});
```

现在`route2`为当前页，你想要改变它的头部栏颜色。这在`route1`里是很容易实现的，但它页应该在`route2`里很容易实现。这就是Default Navigation Options的作用：他们都是在导航器里简单可以设置的`navigationOptions`：

```js
const MyTabNavigator = TabNavigator({
  profile: ProfileScreen,
  ...
}, {
  navigationOptions: {
    headerTintColor: 'blue',
  },
});
```

注意你现在仍然可以在页面里设置`navigationOptions`。譬如上述的`ProfileScreen`里。从页面得到的`navigationOptions`会将导航器来的选项给改变合并。无论何时导航器和页面同时设置相同的属性，譬如`headerTintColor`，页面设置的会胜出。因此你可以通过如下步骤在`ProfileScreen`为当前页时改变颜色：

```js
class ProfileScreen extends React.Component {
  static navigationOptions = {
    headerTintColor: 'black',
  };
  ...
}
```

## Navigation Option Reference

在配置了导航器的页面可用的navigation选项

查看下一下可用的选项：
- [`drawer navigator`](/docs/navigators/drawer#Screen-Navigation-Options)
- [`stack navigator`](/docs/navigators/stack#Screen-Navigation-Options)
- [`tab navigator`](/docs/navigators/tab#Screen-Navigation-Options)

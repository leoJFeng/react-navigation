# TabNavigator

使用TabNavigator可以轻易建立拥有几个标签栏和标签路由的页面。通过[expo](https://exp.host/@react-navigation/NavigationPlayground)项目查看这些例子。

```js
class MyHomeScreen extends React.Component {
  static navigationOptions = {
    tabBarLabel: 'Home',
    // Note: By default the icon is only shown on iOS. Search the showIcon option below.
    tabBarIcon: ({ tintColor }) => (
      <Image
        source={require('./chats-icon.png')}
        style={[styles.icon, { tintColor }]}
      />
    ),
  };

  render() {
    return (
      <Button
        onPress={() => this.props.navigation.navigate('Notifications')}
        title="Go to notifications"
      />
    );
  }
}

class MyNotificationsScreen extends React.Component {
  static navigationOptions = {
    tabBarLabel: 'Notifications',
    tabBarIcon: ({ tintColor }) => (
      <Image
        source={require('./notif-icon.png')}
        style={[styles.icon, { tintColor }]}
      />
    ),
  };

  render() {
    return (
      <Button
        onPress={() => this.props.navigation.goBack()}
        title="Go back home"
      />
    );
  }
}

const styles = StyleSheet.create({
  icon: {
    width: 26,
    height: 26,
  },
});

const MyApp = TabNavigator({
  Home: {
    screen: MyHomeScreen,
  },
  Notifications: {
    screen: MyNotificationsScreen,
  },
}, {
  tabBarPosition: 'top',
  animationEnabled: true,
  tabBarOptions: {
    activeTintColor: '#e91e63',
  },
});
```

## API定义

```js
TabNavigator(RouteConfigs, TabNavigatorConfig)
```

### 路由配置

路由配置表是指路由命名到配置的映射，导航器从中知道要路由的页面。具体查看StackNavigator中的[例子](/docs/api/navigators/StackNavigator.md#routeconfigs)。

### TabNavigatorConfig

- `tabBarComponent` - 用作标签栏的组件，例如 `TabBarBottom`（这是iOS上的默认设置），`TabBarTop`（这是Android上的默认设置）
- `tabBarPosition` - 标签栏的位置，可以是`top`或`bottom`
- `swipeEnabled` - 是否允许在标签页面之间滑动
- `animationEnabled` - 标签页切换时是否展示动画
- `configureTransition` - 一个给定`currentTransitionProps`和`nextTransitionProps`的方法，返回一个配置对象，描述标签页之间的动画
- `initialLayout` - 可以传递包含初始`height`和`width`的可选对象，以防止[react-native-tab-view](https://github.com/react-native-community/react-native-tab-view#avoid-one-frame-delay) 掉帧。
- `tabBarOptions` - 配置标签栏，见下文

几个传递给路由底层修改导航逻辑的选项：

- `initialRouteName` - 第一次加载时的初始路由名称
- `order` - 路由名称数组，用来定义标签页的顺序
- `paths` - 提供路由名称到路径的映射，会覆盖routeConfigs中设置的路径
- `backBehavior` - 后退按钮是否应该使标签页切换到上一个标签页？ 如果是，则设置为`initialRoute`，否则设为`none`。 默认为`initialRoute`。


### `tabBarOptions` for `TabBarBottom` (iOS默认的tabbar)

- `activeTintColor` - 选中标签页的标签和图标颜色
- `activeBackgroundColor` - 选中标签页的背景颜色
- `inactiveTintColor` - 未选中的标签页的标签和图标颜色
- `inactiveBackgroundColor` - 未选中的标签页的背景颜色
- `showLabel` - 是否显示标签，默认为true
- `style` - 标签栏的样式
- `labelStyle` - 标签的样式
- `tabStyle` - 标签栏的样式
- `allowFontScaling` - 标签字体是否允许缩放以响应辅助功能里的文字设置，默认为true

例子:

```js
tabBarOptions: {
  activeTintColor: '#e91e63',
  labelStyle: {
    fontSize: 12,
  },
  style: {
    backgroundColor: 'blue',
  },
}
```

### `tabBarOptions` for `TabBarTop` (Android默认的tabbar)

- `activeTintColor` - 选中标签页的标签和图标颜色
- `inactiveTintColor` - 未选中的标签页的标签和图标颜色
- `showIcon` - 是否显示标签图标，默认为false
- `showLabel` - 是否显示标签，默认为true
- `upperCaseLabel` - 是否使标签字母大写，默认为true
- `pressColor` - 材质颜色（仅适用于Android版本> = 5.0）.
- `pressOpacity` - 按下标签时的透明度（iOS和Android <5.0）.
- `scrollEnabled` - 是否启用可滚动标签页.
- `tabStyle` -  标签页的样式
- `indicatorStyle` - 指示器的样式（标签底部线）
- `labelStyle` - 标签的样式
- `iconStyle` - 图标的样式
- `style` - 标签栏的样式
- `allowFontScaling` - 标签字体是否允许缩放以响应辅助功能里的文字设置，默认为true

例子:

```js
tabBarOptions: {
  labelStyle: {
    fontSize: 12,
  },
  tabStyle: {
    width: 100,    
  },
  style: {
    backgroundColor: 'blue',
  },
}
```

### Screen Navigation Options

#### `title`

可以用作`headerTitle`和`tabBarLabel`的后备的通用标题

#### `tabBarVisible`

显示或隐藏标签栏，默认为true

#### `swipeEnabled`

启用或禁用在选项卡之间滑动。默认为swipeEnabled

#### `tabBarIcon`

React元素或给定方法`{ focused: boolean, tintColor: string }`来返回一个React节点展示在标签栏上

#### `tabBarLabel`

字符串或者React元素或者给定方法`{ focused: boolean, tintColor: string }`来返回一个React节点展示在标签栏上。当该值为undefined时，scene的`title`值会被使用。要隐藏，请参阅上一节中的`tabBarOptions.showLabel`

#### `tabBarOnPress`

回调处理点击事件。其中包含：

* the `previousScene: { route, index }` 这是我们要离开的页面
* the `scene: { route, index }` 点击页面
* the `jumpToIndex` 可以为您执行导航的方法

在转换到下一个场景之前添加一个自定义逻辑（被点击的）是很有用的

### Navigator Props

由`TabNavigator(...)`创建的导航组件有如下属性

- `screenProps` - 为子页面传递额外的内容，如下：


 ```jsx
 const TabNav = TabNavigator({
   // config
 });

 <TabNav
   screenProps={/* this prop will get passed to the screen components as this.props.screenProps */}
 />
 ```

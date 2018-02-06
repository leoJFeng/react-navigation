# DrawerNavigator

使用DrawerNavigator可以很轻易的建立一个带有抽屉导航的页面。完整例子请查看我们的[expo](https://exp.host/@react-navigation/NavigationPlayground)项目

```js
class MyHomeScreen extends React.Component {
  static navigationOptions = {
    drawerLabel: 'Home',
    drawerIcon: ({ tintColor }) => (
      <Image
        source={require('./chats-icon.png')}
        style={[styles.icon, {tintColor: tintColor}]}
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
    drawerLabel: 'Notifications',
    drawerIcon: ({ tintColor }) => (
      <Image
        source={require('./notif-icon.png')}
        style={[styles.icon, {tintColor: tintColor}]}
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
    width: 24,
    height: 24,
  },
});

const MyApp = DrawerNavigator({
  Home: {
    screen: MyHomeScreen,
  },
  Notifications: {
    screen: MyNotificationsScreen,
  },
});
```

要打开和关闭抽屉，使用`DrawerOpen`和`DrawerClose`

```js
this.props.navigation.navigate('DrawerOpen'); // open drawer
this.props.navigation.navigate('DrawerClose'); // close drawer
```
如果你想要切换你的抽屉，你可以使用`DrawerToggle`。这会根据你传给抽屉的状态选择切换的页面

```js
// fires 'DrawerOpen'/'DrawerClose' accordingly
this.props.navigation.navigate('DrawerToggle');
```

### API定义：

```js
DrawerNavigator(RouteConfigs, DrawerNavigatorConfig)
```

### 路由配置

路由配置表是指路由命名到配置的映射，导航器从中知道要路由的页面。具体查看`StackNavigator`中的[例子](/docs/api/navigators/StackNavigator.md#routeconfigs)


### DrawerNavigatorConfig
- `drawerWidth` - 抽屉的宽度或返回该宽度的函数
- `drawerPosition` - 抽屉在左边还是右边。默认是`left`
- `contentComponent` - 用于呈现抽屉内容的组件，例如导航的子项。从抽屉接收`navigation`属性。默认是    `DrawerItems`。更多参见下文
- `contentOptions` - 配置抽屉内容，见下文
- `useNativeAnimations` - 启用本地动画。 默认是true
- `drawerBackgroundColor` - 抽屉背景色。 默认是白色的

几个可以传递给底层路由修改导航逻辑的选项:

- `initialRouteName` - 第一次加载时的初始路由名称
- `order` - 路由名称数组，用来定义标签页的顺序
- `paths` - 提供路由名称到路径的映射，会覆盖routeConfigs中设置的路径
- `backBehavior` - 后退按钮是否应该使标签页切换到上一个标签页？ 如果是，则设置为`initialRoute`，否则设为`none`。 默认为`initialRoute`。

### 提供定制的contentComponent

默认的抽屉组件是可以滑动的以及只包含路由配置里的链接。你可以轻易复写默认的组件来增加一个头部，尾部和其他内容在抽屉里。默认的抽屉是可滑动及支持iPhoneX安全区域。如果你要定制该内容，确保内容是在安全区域里的。

```js
import { DrawerItems, SafeAreaView } from 'react-navigation';

const CustomDrawerContentComponent = (props) => (
  <ScrollView>
    <SafeAreaView style={styles.container} forceInset={{ top: 'always', horizontal: 'never' }}>
      <DrawerItems {...props} />
    </SafeAreaView>
  </ScrollView>
);

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});
```

### `contentOptions` for `DrawerItems`

- `items` - 路由的数组，可以被修改或覆盖
- `activeItemKey` - 标识活动路由的键值
- `activeTintColor` - 选中标签的标签和图标颜色
- `activeBackgroundColor` - 选中标签的背景颜色
- `inactiveTintColor` - 非选中标签的标签和图标颜色
- `inactiveBackgroundColor` - 非选中标签的背景颜色
- `onItemPress(route)` - 当一个组件被按下时被调用的函数
- `itemsContainerStyle` - 内容区域的样式
- `itemStyle` - 单个组件的，可以包含一个图标和/或一个标签
- `labelStyle` - 复写内容部分中的文本样式，当您的标签是一个字符串
- `iconContainerStyle` - 复写View图标内容样式

#### Example:

```js
contentOptions: {
  activeTintColor: '#e91e63',
  itemsContainerStyle: {
    marginVertical: 0,
  },
  iconContainerStyle: {
    opacity: 1
  }
}
```

### Screen Navigation Options

#### `title`

可以用作`headerTitle`和`drawerLabel`的后备标题

#### `drawerLabel`

字符串，React元素或给定`{ focused: boolean, tintColor: string }`的函数用来返回一个React元素，以显示在抽屉边栏中。 未定义时，使用场景标题

#### `drawerIcon`

React元素或给定`{ focused: boolean, tintColor: string }`的函数返回一个React.Node，以显示在抽屉边栏

#### `drawerLockMode`

指定抽屉的[锁定模式](https://facebook.github.io/react-native/docs/drawerlayoutandroid.html#drawerlockmode)。这也可以通过在顶层路由器上使用`screenProps.drawerLockMode`动态更新

### Navigator Props

通过`DrawerNavigator(...)`创建的导航器有如下属性：

- `screenProps` - 传递子页面需要的额外选项，譬如:


 ```jsx
 const DrawerNav = DrawerNavigator({
   // config
 });

 <DrawerNav
   screenProps={/* this prop will get passed to the screen components and nav options as props.screenProps */}
 />
 ```

 ### Nesting `DrawerNavigation`

要记住如果你要嵌套DrawerNavigation，抽屉会显示在父导航器之下

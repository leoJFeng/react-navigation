# TabRouter

管理应用的一组tabs，处理tabs间跳转，处理返回按钮跳转到上个tab事件。

让我们看一下简单的tabs路由

```js
const MyApp = TabRouter({
  Home: { screen: HomeScreen },
  Settings: { screen: SettingsScreen },
}, {
  initialRouteName: 'Home',
})
```


### RouteConfig

Tabs路由里的每个tab都有一个路由配置：

```js
const MyApp = TabRouter({ // This is the RouteConfig:
  Home: {
    screen: HomeScreen,
    path: 'main',
  },
  Settings: {
    // This can be handy to lazily require a tab:
    getScreen: () => require('./SettingsScreen').default,
    // Note: Child navigators cannot be configured using getScreen because
    // the router will not be accessible. Navigators must be configured
    // using `screen: MyNavigator`
    path: 'settings',
  },
});
```

每个选项都有如下项：

- `path` - 指定每个tab的路径
- `screen` - 指定页面组件或子导航器
- `getScreen` - 懒加载一个页面组件（不能是导航器）


### Tab Router Config

配置选项也会传递到路由里：

- `initialRouteName` - 第一次加载时初始tab的路由名称
- `order` - 定义tabs顺序的routeNames数组
- `paths` - 提供routeName到路径配置的映射，会覆盖routeConfigs中设置的路径
- `backBehavior` - 后退按钮是否应该使tab切换到初始tab？ 如果是，则设置为`initialRoute`，否则为`none`。 默认为`initialRoute`

### Supported Actions

tabs路由器可能会响应以下导航操作。 如果可能，路由器通常会将动作处理委派给子路由器。

- Navigate - 将会跳转到与routeName匹配的tab页面
- Back - 如果当前不是首个tab，将返回到第一个tab
- SetParams - 页面派发的操作，用于更改当前路由的参数

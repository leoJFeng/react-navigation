# StackRouter

管理导航栈的逻辑，包括推入，推出，处理路径传递用来创建一个深层栈。

让我们看一个简单的栈路由：

```js
const MyApp = StackRouter({
  Home: { screen: HomeScreen },
  Profile: { screen: ProfileScreen },
}, {
  initialRouteName: 'Home',
})
```


### RouteConfig

一个基本的路由栈需要一个路由配置对象。这里有一个配置例子：

```js
const MyApp = StackRouter({ // This is the RouteConfig:
  Home: {
    screen: HomeScreen,
    path: '',
  },
  Profile: {
    screen: ProfileScreen,
    path: 'profile/:name',
  },
  Settings: {
    // This can be handy to lazily require a screen:
    getScreen: () => require('Settings').default,
    // Note: Child navigators cannot be configured using getScreen because
    // the router will not be accessible. Navigators must be configured
    // using `screen: MyNavigator`
    path: 'settings',
  },
});
```

配置里的每个选项也许都有如下项：

- `path` - 指定路径和参数用作被堆栈解析
- `screen` - 指定页面组件或者子导航器
- `getScreen` - 懒加载一个页面组件（不是导航器）


### StackConfig

配置选项同样也会传递到栈路由里。

- `initialRouteName` -  堆栈第一次加载时的默认路由的名称
- `initialRouteParams` - 初始路由的默认参数
- `paths` - 提供routeName到路径配置的映射，会覆盖routeConfigs中设置的路径

### Supported Actions

路由栈可以响应以下导航操作。 如果可能的话，路由器通常会将动作处理委派给子路由器。

- Navigate - 如果routeName匹配路由器的routeConfigs之一，将会在堆栈上推送一个新的路由
- Back - 返回（弹出）
- Reset - 清除堆栈并提供新actions用以创建全新的导航状态
- SetParams - 页面派发的操作，用于更改当前路由的参数

# Routers

路由定义了一个组件的导航状态，它允许开发者定义自己的路径和事件来处理。


## Built-In Routers

`react-navigation` 有如下几个标准路由：

- [StackRouter](/docs/routers/stack)
- [TabRouter](/docs/routers/tab)


## Using Routers

要手动创建导航器，请在组件上放置一个静态`router`。 （要快速制作带有内置组件的导航器，可以使用[内置导航器](/docs/navigators)来代替）

```js
class MyNavigator extends React.Component {
    static router = StackRouter(routes, config);
    ...
}
```

现在你可以将组件当作`screen`在另一个导航器使用。`MyNavigator`导航器的逻辑取决于`StackRouter`。


## Customizing Routers

查看[Custom Router API spec](/docs/routers/api) 来学习`StackRouter`和`TabRouter`的API。你可以根据需要复写路由的方法：

### Custom Navigation Actions

要覆盖导航行为，可以覆写`getStateForAction`中的导航状态逻辑，并手动操作`routes`和`index`。

```js
const MyApp = StackNavigator({
  Home: { screen: HomeScreen },
  Profile: { screen: ProfileScreen },
}, {
  initialRouteName: 'Home',
})

const defaultGetStateForAction = MyApp.router.getStateForAction;

MyApp.router.getStateForAction = (action, state) => {
  if (state && action.type === 'PushTwoProfiles') {
    const routes = [
      ...state.routes,
      {key: 'A', routeName: 'Profile', params: { name: action.name1 }},
      {key: 'B', routeName: 'Profile', params: { name: action.name2 }},
    ];
    return {
      ...state,
      routes,
      index: routes.length - 1,
    };
  }
  return defaultGetStateForAction(action, state);
};
```

### Blocking Navigation Actions

有时你想要阻止某些导航行为，这取决于你的路由：

```js
import { NavigationActions } from 'react-navigation'

const MyStackRouter = StackRouter({
  Home: { screen: HomeScreen },
  Profile: { screen: ProfileScreen },
}, {
  initialRouteName: 'Home',
})

const defaultGetStateForAction = MyStackRouter.router.getStateForAction;

MyStackRouter.router.getStateForAction = (action, state) => {
  if (
    state &&
    action.type === NavigationActions.BACK &&
    state.routes[state.index].params.isEditing
  ) {
    // Returning null from getStateForAction means that the action
    // has been handled/blocked, but there is not a new state
    return null;
  }
  
  return defaultGetStateForAction(action, state);
};
```


### Handling Custom URIs

也许你的app有一些独特的URI是内置路由无法处理的。你总是可以继承路由的`getActionForPathAndParams`。

```js

import { NavigationActions } from 'react-navigation'

const MyApp = StackNavigator({
  Home: { screen: HomeScreen },
  Profile: { screen: ProfileScreen },
}, {
  initialRouteName: 'Home',
})
const previousGetActionForPathAndParams = MyApp.router.getActionForPathAndParams;

Object.assign(MyApp.router, {
  getActionForPathAndParams(path, params) {
    if (
      path === 'my/custom/path' &&
      params.magic === 'yes'
    ) {
      // returns a profile navigate action for /my/custom/path?magic=yes
      return NavigationActions.navigate({
        routeName: 'Profile',
        action: NavigationActions.navigate({
          // This child action will get passed to the child router
          // ProfileScreen.router.getStateForAction to get the child
          // navigation state.
          routeName: 'Friends',
        }),
      });
    }
    return previousGetActionForPathAndParams(path, params);
  },
});
```

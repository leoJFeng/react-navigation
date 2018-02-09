# 自定义导航器

一个导航器就是任意带有[路由](/docs/routers/)的React组件。这里有一个基本的，使用了[路由的API](/docs/routers/api)来获取当前页面展示的导航器:

```js
class MyNavigator extends React.Component {
  static router = MyRouter;
  render() {
    const { state, dispatch } = this.props.navigation;
    const { routes, index } = state;

    // Figure out what to render based on the navigation state and the router:
    const Component = MyRouter.getComponentForState(state);

    // The state of the active child screen can be found at routes[index]
    let childNavigation = { dispatch, state: routes[index] };
    // If we want, we can also tinker with the dispatch function here, to limit
    // or augment our children's actions

    // Assuming our children want the convenience of calling .navigate() and so on,
    // we should call addNavigationHelpers to augment our navigation prop:
    childNavigation = addNavigationHelpers(childNavigation);

    return <Component navigation={childNavigation} />;
  }
}
```

## Navigation Props

通过导航器传递下来的`navigation`属性仅包括`state`和`dispatch`。这是导航器的当前状态和分发事件的请求。

所有导航器都是受监控的组件：他们总会展示`props.navigation.state`返回的页面，他们唯一能够改变状态的方法就是通过`props.navigation.dispatch`分发事件。

导航器可以通过[自定义路由](/docs/routers/)来改变父导航器的行为。例如导航器可以让`router.getStateForAction`返回一个`null`来阻止actions操作。或者导航器可以通过改写`router.getActionForPathAndParams`定制URI处理来输出相关的导航actions，并在`router.getStateForAction`中处理该actions。

### Navigation State

导航状态通过 `props.navigation.state`传递给导航器，它拥有如下结构

```
{
  index: 1, // identifies which route in the routes array is active
  routes: [
    {
      // Each route needs a name, which routers will use to associate each route
      // with a react component
      routeName: 'MyRouteName',

      // A unique id for this route, used to keep order in the routes array:
      key: 'myroute-123',

      // Routes can have any additional data. The included routers have `params`
      ...customRouteData,
    },
    ...moreRoutes,
  ]
}
```

### Navigation Dispatchers

一个导航可以处理各种导航事件，如打开一个URI或者返回。
dispatcher会返回`true`如果该事件成功，否则返回`false`。

## API for building custom navigators

为了帮助开发者实现自定义导航器，ReactNavigation提供下面工具：

### `createNavigator`

该工具将[导航页面](/docs/views/)和[路由](/docs/routers/)用标准形式组合在一起：

```js
const MyApp = createNavigator(MyRouter)(MyView);
```

这一切的幕后是：

```js
const MyApp = ({ navigation }) => (
  <MyView router={MyRouter} navigation={navigation} />
);
MyApp.router = MyRouter;
```

### `addNavigationHelpers`

将导航器加进`state`,`dispatch`这些属性，以及所有页面的导航方法，譬如`navigation.navigate()`和`navigation.goBack()`。这些方法很简单的帮助创建actions以及将他们发送至`dispatch`。

### `createNavigationContainer`

如果你想你的导航器能够在顶层组件应用（没有传递navigation属性），你可以使用`createNavigationContainer`。该效果是让你的导航器像顶层导航器一样即使没有navigation属性也能工作。它会管理app的状态，整合app层的导航功能，譬如处理进出的链接，Android的返回行为等。

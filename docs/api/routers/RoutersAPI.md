## Custom Router API

你可以通过如下方法建立一个对象设计你自己的路由:

```js
const MyRouter = {
  getStateForAction: (action, state) => ({}),
  getActionForPathAndParams: (path, params) => null,
  getPathAndParamsForState: (state) => null,
  getComponentForState: (state) => MyScreen,
  getComponentForRouteName: (routeName) => MyScreen,
};

// Now, you can make a navigator by putting the router on it:
class MyNavigator extends React.Component {
  static router = MyRouter;
  render() {
    ...
  }
}
```

![Routers manage the relationship between URIs, actions, and navigation state](/assets/routers-concept-map.png)


### `getStateForAction(action, state)`

定义导航状态以响应给定的操作。 当一个动作被传递到`props.navigation.dispatch`或者当任何帮助方法被调用，比如`navigation.navigate`时，这个函数将被运行。

通常这应该返回一个导航状态，具有以下形式：

```
{
  index: 1, // identifies which route in the routes array is active
  routes: [
    {
      // Each route needs a name to identify the type.
      routeName: 'MyRouteName',

      // A unique identifier for this route in the routes array:
      key: 'myroute-123',
      // (used to specify the re-ordering of routes)

      // Routes can have any data, as long as key and routeName are correct
      ...randomRouteData,
    },
    ...moreRoutes,
  ]
}
```

如果路由器在外部处理了该行为，或者想要在不改变导航状态的情况下处理该行为，则该函数将返回`null`。

### `getComponentForRouteName(routeName)`

返回子组件或者给定路由的导航器

使用路由的`getStateForAction`会返回如下一个状态：
```
{
  index: 1,
  routes: [
    { key: 'A', routeName: 'Foo' },
    { key: 'B', routeName: 'Bar' },
  ],
}
```

基于状态的路由名，当使用`router.getComponentForRouteName('Foo')` 或者`router.getComponentForRouteName('Bar')`路由有责任返回组件实际名字：

### `getComponentForState(state)`

返回当前组件深层导航状态

### `getActionForPathAndParams(path, params)`

当用户导航到改路径和提供了需要的参数时返回一个可选的导航action

### `getPathAndParamsForState(state)`

返回一个可以放进URL里的路径和参数用来链接用户返回到app相同点的时候

返回的路径和参数当传回路由的`getActionForPathAndParams`时应该形成一个action。该action一旦使用`getStateForAction`会返回一个熟悉的状态

### `getScreenOptions(navigation, screenProps)`

用于页面检索导航选项。必须提供当前页面的导航属性和选项，以及其他你的导航选项需要的属性

- `navigation` - 这是页面将使用的导航属性，其中状态是指页面的路由/状态。 使用时将触发该页面上下文中的操作。
- `screenProps` - 其他你的导航选项所要使用的属性
- `navigationOptions` -  默认是之前的一组操作选项或者由之前的配置提供

在下列示例中，也许你需要获取配置标题：
```js
// First, prepare a navigation prop for your child, or re-use one if already available.
const screenNavigation = addNavigationHelpers({
  // In this case we use navigation.state.index because we want the title for the active route.
  state: navigation.state.routes[navigation.state.index],
  dispatch: navigation.dispatch,
});
const options = this.props.router.getScreenOptions(screenNavigation, {});
const title = options.title;
```

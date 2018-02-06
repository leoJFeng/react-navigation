# Redux Integration

### Redux Integration总览:
1. 为了让你的app的导航状态在redux里处理，你可以传递你自己的`navigation`属性到导航器。

2. 一旦你传了自己的导航状态进导航器里，默认的`navigation`属性就会被销毁。你将很可能要传递[`navigation`](https://reactnavigation.org/docs/navigators/navigation-prop)属性到你想进的页面。通常[`state`](https://reactnavigation.org/docs/navigators/navigation-prop#state-The-screen's-current-stateroute)和[`dispatch`](https://reactnavigation.org/docs/navigators/navigation-prop#dispatch-Send-an-action-to-the-router)属性会被传递到导航器里。你将会在该教材里学习如何传递这些属性。因为你得销毁了默认的属性，所以如果你尝试处理一些你没传递下去操作这会导致失败。所以，如果你没传递[`dispatch`](https://reactnavigation.org/docs/navigators/navigation-prop#dispatch-Send-an-action-to-the-router)到导航器而只传了`state`状态下去的话，你就不能在你的组件里操作[`dispatch`](https://reactnavigation.org/docs/navigators/navigation-prop#dispatch-Send-an-action-to-the-router)了。 

3. `state`会被reducer处理navigation状态并返回，以及`dispatch`是redux默认的dispatch。因此你可以使用`this.props.navigation.dispatch(ACTION)`来分发常用redux操作，reducer会在发送基本分发事件后更新navigation状态，然后新的navigation状态会被传递到导航器里。

### 关于整合Redux的详细
使用redux后你的app状态将由reducer来定义管理。每个导航路由都有一个reducer,称为`getStateForAction`。下面这个小案例会展示怎样在redux项目里使用导航器：

```es6
import { addNavigationHelpers } from 'react-navigation';

const AppNavigator = StackNavigator(AppRouteConfigs);

const initialState = AppNavigator.router.getStateForAction(AppNavigator.router.getActionForPathAndParams('Login'));

const navReducer = (state = initialState, action) => {
  const nextState = AppNavigator.router.getStateForAction(action, state);

  // Simply return the original `state` if `nextState` is null or undefined.
  return nextState || state;
};

const appReducer = combineReducers({
  nav: navReducer,
  ...
});

class App extends React.Component {
  render() {
    return (
      <AppNavigator navigation={addNavigationHelpers({
        dispatch: this.props.dispatch,
        state: this.props.nav,
      })} />
    );
  }
}

const mapStateToProps = (state) => ({
  nav: state.nav
});

const AppWithNavigationState = connect(mapStateToProps)(App);

const store = createStore(appReducer);

class Root extends React.Component {
  render() {
    return (
      <Provider store={store}>
        <AppWithNavigationState />
      </Provider>
    );
  }
}
```

一旦你这样做了，你的导航状态就会存在你的redux里，你可以使用redux的dispatch方法分发导航事件了。

注意当导航器给了`navigation`属性后，它就放弃了自己管理内部状态。这意味着你现在要担起管理它状态，处理深度链接，[处理安卓返回事件](#Handling-the-Hardware-Back-Button-in-Android)等的责任了。

当你嵌套导航器时导航状态会自动传递到下一个导航器上，注意为了让子导航器接受到父导航器的状态，你应该将其定义为`screen`。

将其应用到上述的例子中，你可以像下面一样定义`AppNavigator`去嵌套一个`TabNavigator`。

```es6
const AppNavigator = StackNavigator({
  Home: { screen: MyTabNavigator },
});
```

在这个例子，一旦你将`AppNavigator`用`AppWithNavigationState` `connect` 到了`Redux`中，`MyTabNavigator`会自动的拥有一个`navigation`属性。

## Full example

这里有一些带有[redux](https://github.com/react-community/react-navigation/tree/master/examples/ReduxExample)可运行的app例子可供你自行尝试。

## Mocking tests

为了将jest test在你的 react-navigation app可以使用，你需要在`package.json`里改变jest preset。[查看这里](https://facebook.github.io/jest/docs/tutorial-react-native.html#transformignorepatterns-customization):


```json
"jest": {
  "preset": "react-native",
  "transformIgnorePatterns": [
    "node_modules/(?!(jest-)?react-native|react-navigation)"
  ]
}
```

## Handling the Hardware Back Button in Android

使用下列片段代码，你的nav组件会感知到返回按钮以及正确处理你的栈。这在安卓里是很有用的。

```es6
import React from "react";
import { BackHandler } from "react-native";
import { addNavigationHelpers, NavigationActions } from "react-navigation";

const AppNavigation = TabNavigator(
  {
    Home: { screen: HomeScreen },
    Settings: { screen: SettingScreen }
  }
);

class ReduxNavigation extends React.Component {
  componentDidMount() {
    BackHandler.addEventListener("hardwareBackPress", this.onBackPress);
  }
  componentWillUnmount() {
    BackHandler.removeEventListener("hardwareBackPress", this.onBackPress);
  }
  onBackPress = () => {
    const { dispatch, nav } = this.props;
    if (nav.index === 0) {
      return false;
    }
    dispatch(NavigationActions.back());
    return true;
  };

  render() {
    const { dispatch, nav } = this.props;
    const navigation = addNavigationHelpers({
      dispatch,
      state: nav
    });

    return <AppNavigation navigation={navigation} />;
  }
}

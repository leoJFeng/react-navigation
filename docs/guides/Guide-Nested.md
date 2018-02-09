# Nesting Navigators

手机应用中有多种类型的导航同时存在是很常见的。React Navigation里的路由和导航器都是可组合的，这允许你为自己的应用定义复杂的导航结构。

在本案的聊天应用里，我们会在第一个页面放置多个Tabs来展示最近聊天和所有联系人。

## Tab Navigator介绍

让我们在`App.js`里创造一个新的`TabNavigator`。

```js
import { TabNavigator } from "react-navigation";

class RecentChatsScreen extends React.Component {
  render() {
    return <Text>List of recent chats</Text>
  }
}

class AllContactsScreen extends React.Component {
  render() {
    return <Text>List of all contacts</Text>
  }
}

const MainScreenNavigator = TabNavigator({
  Recent: { screen: RecentChatsScreen },
  All: { screen: AllContactsScreen },
});
```

如果`MainScreenNavigator`是在导航器的最上层，那应用应该如下展示:

```phone-example
simple-tabs
```



## Nesting a Navigator in a screen

我们想让这些Tabs在app的首页里展示出来，但新的页面能够覆盖在Tabs之上。

让我们在[之前设置好](/docs/intro/)的`StackNavigator`最上层将Tabs页面添加进来。

```js
const SimpleApp = StackNavigator({
  Home: { screen: MainScreenNavigator },
  Chat: { screen: ChatScreen },
});
```

因为`MainScreenNavigator`被当作一个screen页面，所以我们可以配置`navigationOptions`。

```js
const SimpleApp = StackNavigator({
  Home: { 
    screen: MainScreenNavigator,
    navigationOptions: {
      title: 'My Chats',
    },
  },
  Chat: { screen: ChatScreen },
})
```

让我们在每个Tab页面增加一个按钮来链接对话页面：

```js
<Button
  onPress={() => this.props.navigation.navigate('Chat', { user: 'Lucy' })}
  title="Chat with Lucy"
/>
```

现在我们可以将一个导航器放在另一个导航器里了，我们可以通过`navigate`方法在不同导航器里跳转：

```phone-example
nested
```

## Nesting a Navigator in a Component
有时我们需要在组件里嵌套导航器。这在导航器仅占屏幕一部分时是很常用的。为子导航器能够链接到导航树中，我们需要从父类将`navigation`属性传递下去。

```js
const SimpleApp = StackNavigator({
  Home: { screen: NavigatorWrappingScreen },
  Chat: { screen: ChatScreen },
});
```
在本例里NavigatorWrappingScreen不是一个导航器，但它会将导航器作为自己一部分渲染出来。

如果这个导航器渲染为空白，那么请将 `<View>` 改为 `<View style={{flex: 1}}>`.

```js
class NavigatorWrappingScreen extends React.Component {
  render() {
    return (
      <View>
        <SomeComponent/>
        <MainScreenNavigator/>
      </View>
    );
  }
}
```

为了将`MainScreenNavigator`连接到导航树里，我们将它的路由分配给子组件里。这样就使得`NavigatorWrappingScreen` “导航可感知”，这会告诉父导航器将`navigation`对象传递下去。因为`NavigatorWrappingScreen`的路由已经被子导航器覆盖了，所以子导航器会收到必要的`navigation`。

```js
class NavigatorWrappingScreen extends React.Component {
  render() {
    return (
      <View>
        <SomeComponent/>
        <MainScreenNavigator navigation={this.props.navigation}/>
      </View>
    );
  }
}
NavigatorWrappingScreen.router = MainScreenNavigator.router;
```

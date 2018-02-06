
# Screen Navigation Prop

你应用里的每个页面都会收到一个`navigation`属性，它包含了以下几项:
* `navigate` - (helper) 链接其他页面
* `state` - 页面当前的状态/路由
* `setParams` - (helper) 更改路由参数
* `goBack` - (helper) 关闭当前页面并返回
* `dispatch` - 发送一个事件到路由

注意：`navigation`属性会传递到每个导航可感知组件。问题是导航的`navigation`属性可能没有帮助方法(`navigate`, `goBack`, etc),它可能只含有`state`和`dispatch`。为了使导航器的`navigation`属性能应用`navigate`方法，你应该用[action creator](navigation-actions) 来使用dispatch。

*和Redux绑定的注意事项*

> 许多人都不会正确的绑定Redux，因为他们错误的理解了navigator的顶层API里navigation属性是可选的。导航器会自己维护自己的状态如果它没收到一个navigation属性，但如果你想要绑定Redux就不应该这样做。在你的主导航器嵌套下的导航器，你应该为每个页面传递navigation属性。 这会让你的顶层navigator能够通过提供状态与每个子导航器沟通。只需要你的顶层导航器绑定了redux，因为这样所有其他的导航器都在其中了

## `navigate` - 链接到其他页面

在你的app里使用它能够跳转到别的页面。使用如下命令：

`navigate(routeName, params, action)`

- `routeName` - 注册在你app里某处的要跳转的路由名称
- `params` - 要带给目的路由的参数
- `action` - (advanced) 跑在你的子路由的可选事项，如果页面也是个导航器。查看[Actions Doc](navigation-actions)获取完整帮助

```js
class HomeScreen extends React.Component {
  render() {
    const {navigate} = this.props.navigation;

    return (
      <View>
        <Text>This is the home screen of the app</Text>
        <Button
          onPress={() => navigate('Profile', {name: 'Brent'})}
          title="Go to Brent's profile"
        />
      </View>
     )
   }
}
```

## `state` - 页面当前的状态/路由

通过`this.props.navigation.state`页面可以查看到它的路由。每一个都会返回如下对象

```js
{
  // the name of the route config in the router
  routeName: 'profile',
  //a unique identifier used to sort routes
  key: 'main0',
  //an optional object of string options for this screen
  params: { hello: 'world' }
}
```

```js
class ProfileScreen extends React.Component {
  render() {
    const {state} = this.props.navigation;
    // state.routeName === 'Profile'
    return (
      <Text>Name: {state.params.name}</Text>
    );
  }
}
```


## `setParams` - 更改路由参数

使用`setParams`允许页面更改路由参数，可以用来更改头部按钮和标题

```js
class ProfileScreen extends React.Component {
  render() {
    const {setParams} = this.props.navigation;
    return (
      <Button
        onPress={() => setParams({name: 'Lucy'})}
        title="Set title name to 'Lucy'"
      />
     )
   }
}
```

## `goBack` - 关闭当前页面并返回

提供在路由里设定的key值返回到该页面。默认goBack会关闭使用它的路由页面

*返回到一个指定页面*

思考下如下的导航栈：
```
{
  routes: [
    { routeName: 'HomeScreen', key: 'A' },
    { routeName: 'DetailScreen', key: 'B' },
    { routeName: 'DetailScreen', key: 'C' },
  ],
  index: 2
}
```

现在如果你在C页面而想要返回到A页面（退出C，和B页面）你需要指定从哪个页面返回:

```
navigation.goBack("B") // will go to screen A FROM screen B
```

如果目标是返回到任意页面，而不用指定关闭页面，使用`.goBack(null);`

```js
class HomeScreen extends React.Component {
  render() {
    const {goBack} = this.props.navigation;
    return (
      <View>
        <Button
          onPress={() => goBack()}
          title="Go back from this HomeScreen"
        />
        <Button
          onPress={() => goBack(null)}
          title="Go back from the currently active route"
        />
        <Button
          onPress={() => goBack('screen-123')}
          title="Go back from screen-123"
        />
      </View>
     )
   }
}
```

## `dispatch` - 发送给路由一个事件

使用dispatch发送任意navigation事件给路由。其他navigation方法会在页面下使用dispatch。

注意如果你想发送react-navigation事件你应该使用库里提供的action creators。

查看[Navigation Actions Docs](navigation-actions) 获取完整的可用项。

```js
import { NavigationActions } from 'react-navigation'

const navigateAction = NavigationActions.navigate({
  routeName: 'Profile',
  params: {},

  // navigate can have a nested navigate action that will be run inside the child router
  action: NavigationActions.navigate({ routeName: 'SubProfileRoute'})
})
this.props.navigation.dispatch(navigateAction)

```

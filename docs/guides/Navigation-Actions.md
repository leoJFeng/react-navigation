# Navigation Actions

所有Navigation Actions能通过`navigation.dispatch()`返回一个能发送给路由的对象。

注意如果你想发送react-navigation事件你应该使用提供的库来创建action。

支持下列操作:
* [Navigate](#Navigate) - 导航到另一个路由
* [Reset](#reset) - 用新状态替换调当前状态
* [Back](#Back) - 返回到上个状态
* [Set Params](#SetParams) - 设置参数给给定路由
* [Init](#Init) - 如果state为undefined则用此初始化状态

Action创建器定义了`toString()`来返回action类型。这可以使一些第三方redux库,譬如redux-actions 或redux-saga很轻易能够使用。

### Navigate
`Navigate` action会更新当前状态为`Navigate`的结果。

- `routeName` - *String* - Required - 要跳转的在app某处注册的路由名称
- `params` - *Object* - Optional - 传递给目的路由的参数
- `action` - *Object* - Optional - (advanced) 跑在你的子路由的可选事项，如果页面也是个导航器。查看[Actions Doc](navigation-actions)获取完整帮助。

```js
import { NavigationActions } from 'react-navigation'

const navigateAction = NavigationActions.navigate({

  routeName: 'Profile',

  params: {},

  action: NavigationActions.navigate({ routeName: 'SubProfileRoute'})
})

this.props.navigation.dispatch(navigateAction)

```


### Reset

`Reset`操作会清除所有的navigation状态以及替换为几个actions事件的结果。

- `index` - *number* - required - 当前navigation`state`里`routes`数组的当前页面的索引
- `actions` - *array* - required - 会替换掉navigation 状态的Navigation Actions数组
- `key` - *string or null* - optional -  如果设置了该值，给定key值的导航器将会被替换掉，否则则替换为根导航器

```js
import { NavigationActions } from 'react-navigation'

const resetAction = NavigationActions.reset({
  index: 0,
  actions: [
    NavigationActions.navigate({ routeName: 'Profile'})
  ]
})
this.props.navigation.dispatch(resetAction)

```
#### 怎样使用`index`参数
`index`参数用于指定当前的路由。

例如：给定一个基本的导航栈带有两个路由页面`Profile`和`Setting`。 要将状态重置为`Setting`页面但将其放置在`Profile`页面之上，您可以执行以下操作：

```js
import { NavigationActions } from 'react-navigation'

const resetAction = NavigationActions.reset({
  index: 1,
  actions: [
    NavigationActions.navigate({ routeName: 'Profile'}),
    NavigationActions.navigate({ routeName: 'Settings'})
  ]
})
this.props.navigation.dispatch(resetAction)

```

### Back

返回到之前页面并关闭当前页面。`back` 事件创建器接受如下参数：
- `key` - *string or null* - optional - 如果设置了导航器将会从给定key值的页面开始返回。如果为null导航器将会从任意位置返回

```js
import { NavigationActions } from 'react-navigation'

const backAction = NavigationActions.back({
  key: 'Profile'
})
this.props.navigation.dispatch(backAction)

```

### SetParams

当发送`SetParams`事件时，路由会产生根据该参数改变参与的路由而返回一个新的状态。

- `params` - *object* - required - 新的参数去改变已存在的路由参数
- `key` - *string* - required - 路由key值用作赋予新的参数

```js
import { NavigationActions } from 'react-navigation'

const setParamsAction = NavigationActions.setParams({
  params: { title: 'Hello' },
  key: 'screen-123',
})
this.props.navigation.dispatch(setParamsAction)

```

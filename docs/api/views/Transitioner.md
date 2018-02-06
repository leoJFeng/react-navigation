# Transitioner

`Transitioner`是一个帮助管理复杂动效组件的切换效果。它管理这动画时间，保持追踪页面的进入和退出，但它完全不知道改如何展示，因为这完全取决于开发者想要如何渲染。

底层`Transitioner`使用`CardStack`和`StackNavigator`的增强版组合实现。

`Transitioner`最有用的是获取当前导航状态。 当从那个导航状态中路由被删除时，`Transitioner`将协助这些路由的过渡动画，即使它们从导航状态不存在了也能保持在页面显示。


## Example

```jsx
class MyNavView extends Component {
  ...
  render() {
    return (
      <Transitioner
        configureTransition={this._configureTransition}
        navigation={this.props.navigation}
        render={this._render}
        onTransitionStart={this.onTransitionStart}
        onTransitionEnd={this.onTransitionEnd}
      />
    );
}
```

## Props

### `configureTransition` function

在`Transitioner.componentWillReceiveProps`上调用时，此函数允许自定义动画参数（如持续时间）。 该函数返回的值将被输入一个定时函数，默认情况下是`Animated.timing()`，作为它的配置。

#### Examples

```js
_configureTransition(transitionProps, prevTransitionProps) {
  return {
    // duration in milliseconds, default: 250
    duration: 500,
    // An easing function from `Easing`, default: Easing.inOut(Easing.ease)
    easing: Easing.bounce,
  }
}
```

注意：`duration`和`easing`仅适用于定时功能为`Animated.timing`。 我们还可以使用不同的定时功能及其相应的配置参数，如下所示：

```js
_configureTransition(transitionProps, prevTransitionProps) {
  return {
    // A timing function, default: Animated.timing.
    timing: Animated.spring,
    // Some parameters relevant to Animated.spring
    friction: 1,
    tension: 0.5,
  }
}
```

#### Flow definition

```js
  configureTransition: (
    transitionProps: NavigationTransitionProps,
    prevTransitionProps: ?NavigationTransitionProps,
  ) => NavigationTransitionSpec,
```

#### Parameters
- `transitionProps`: 从当前导航状态和属性创建的[NavigationTransitionProps](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L273)
- `prevTransitionProps`: 先前导航状态和属性创建的[NavigationTransitionProps](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L273)

#### Returns
- 一个类型为[NavigationTransitionSpec](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L316)的对象，它将作为配置提供到Animated timing 函数中


### `navigation` prop
带有`state`的对象，表示导航状态，包含`routes`和活动的路由索引`index`。 还包括`dispatch`和其他请求操作的方法。

#### Example value

```js
{
   // Index refers to the active child route in the routes array.
  index: 1,
  routes: [
    { key: 'DF2FGWGAS-12', routeName: 'ContactHome' },
    { key: 'DF2FGWGAS-13', routeName: 'ContactDetail', params: { personId: 123 } }
  ]
}
```

#### Flow definition
```js
export type NavigationState = {
  index: number,
  routes: Array<NavigationRoute>,
};
```

关于`NavigationRoute`更多信息，请查看[flow definition](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L32)

### `render` function
调用`Transitioner.render()`方法。该方法展示从`Transitioner`委托的实际渲染。在这个方法，我们可以使用像`transitionProps`和`prevTransitionProps`参数来渲染页面，创建动效和处理手势。

这里有几个`transitionProps`和`prevTransitionProps`很重要的参数对上面所述的事件很有帮助。

- `scenes: Array<NavigationScene>` - 所有可用scenes的列表
- `position: NavigationAnimatedValue` - transitioner导航状态的渐进索引
- `progress: NavigationAnimatedValue` - 表示导航状态从一个变为另一个时转换进度的值。 其数值范围从0到1

要获取NavigationTransitionProps完整的属性列表，查看[flow definition](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L273)。

#### Examples

`transitionProps.scenes`是所有可用scenes的列表。 由开发者决定如何将它们放在屏幕上。 例如，我们可以将场景渲染为一堆卡页，如下所示：

```jsx
_render(transitionProps, prevTransitionProps) {
  const scenes = transitionProps.scenes.map(scene => this._renderScene(transitionProps, scene));
  return (
    <View style={styles.stack}>
      {scenes}
    </View>
  );
}
```

然后，我们可以使用`Animated.View`来设置切换动画。 要创建必要的动画样式属性（例如不透明度），我们可以插入`transitionProps`附带的`position`和`progress`：

```jsx
_renderScene(transitionProps, scene) {
  const { position } = transitionProps;
  const { index } = scene;
  const opacity = position.interpolate({
    inputRange: [index-1, index, index+1],
    outputRange: [0, 1, 0],
  });
  // The prop `router` is populated when we call `createNavigator`.
  const Scene = this.props.router.getComponent(scene.route.routeName);
  return (
    <Animated.View style={{ opacity }}>
      { Scene }
    </Animated.View>
  )
}
```

上述代码创建了一个切换时渐变消失的动画。

有关如何创建自定义转换的综合教程，请参阅[此博客文章](http://www.reactnativediary.com/2016/12/20/navigation-experimental-custom-transition-1.html)

#### Flow definition
```js
render: (transitionProps: NavigationTransitionProps, prevTransitionProps: ?NavigationTransitionProps) => React.Node,
```

#### Parameters
- `transitionProps`: 从当前导航状态和属性创建的[NavigationTransitionProps](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L273)
- `prevTransitionProps`: 先前导航状态和属性创建的[NavigationTransitionProps](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L273)

#### Returns
- ReactElement元素，会被用作渲染Transitioner组件

### `onTransitionStart` function
当动画准备开始时调用

如果你从`onTransitionStart`返回一个promise，动画效果会在promise内容执行后再开始

#### Flow definition
```js
onTransitionStart: (transitionProps: NavigationTransitionProps, prevTransitionProps: ?NavigationTransitionProps) => (Promise | void),
```
#### Parameters
- `transitionProps`: 从当前导航状态和属性创建的[NavigationTransitionProps](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L273)
- `prevTransitionProps`: 先前导航状态和属性创建的[NavigationTransitionProps](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js#L273)

#### Returns
- `Promise` 延迟动画开始时间，或者设置none立即开始动画

### `onTransitionEnd` function
当动画完成后调用

如果你从`onTransitionEnd`返回一个promise，所有切换动画会在promise内容执行后生效。

#### Flow definition
```js
onTransitionEnd: () => void
```
#### Parameters
- none.

#### Returns
- none.

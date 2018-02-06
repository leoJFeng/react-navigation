# StackNavigator

为你的app提供页面跳转。新页面会被放置在页面栈的顶部。

默认的StackNavigator会配置为熟悉的iOS和Android风格。iOS的新页面会从右边滑进，而安卓则从底部闪出。而iOS也可以配置为modal的样式，即页面从底部滑出。

```jsx

class MyHomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
  }

  render() {
    return (
      <Button
        onPress={() => this.props.navigation.navigate('Profile', {name: 'Lucy'})}
        title="Go to Lucy's profile"
      />
    );
  }
}

const ModalStack = StackNavigator({
  Home: {
    screen: MyHomeScreen,
  },
  Profile: {
    path: 'people/:name',
    screen: MyProfileScreen,
  },
});
```

## API定义

```js
StackNavigator(RouteConfigs, StackNavigatorConfig)
```

### 路由配置

路由配置表是指路由命名到配置的映射，导航器从中知道要路由的页面。

```js
StackNavigator({

  // For each screen that you can navigate to, create a new entry like this:
  Profile: {

    // `ProfileScreen` is a React component that will be the main content of the screen.
    screen: ProfileScreen,
    // When `ProfileScreen` is loaded by the StackNavigator, it will be given a `navigation` prop.

    // Optional: When deep linking or using react-navigation in a web app, this path is used:
    path: 'people/:name',
    // The action and route params are extracted from the path.

    // Optional: Override the `navigationOptions` for the screen
    navigationOptions: ({navigation}) => ({
      title: `${navigation.state.params.name}'s Profile'`,
    }),
  },

  ...MyOtherRoutes,
});
```

### StackNavigatorConfig

路由选项：

- `initialRouteName` - 为栈设置默认的页面。一定要是路由配置表里的其中一个
- `initialRouteParams` - 默认的路由参数
- `navigationOptions` - 默认的导航选项
- `paths` - 设置在路由配置里的路径映射

视觉选项：

- `mode` - 设置页面的过度动画和展示方式：
  - `card` - iOS和Android的标准展示方式。此为默认配置
  - `modal` - 让页面像iOS modal模式从底部滑出。该项只在iOS生效，Android无效

- `headerMode` - 定义顶部栏应该如何展示：
  - `float` - 渲染一个保持在顶部不跟随页面滑动的顶部栏。这是iOS的常用模式
  - `screen` - 每个页面附着一个顶部栏，跟随页面出现和小时。这是Android的常用模式
  - `none` - 不渲染任何顶部栏
- `cardStyle` - 用此属性重写默认的栈页面样式
- `transitionConfig` - 该方法会返回一个默认页面动画效果的对象（请参考[type definitions](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js)里的TransitionConfig）该方法需要传递下面参数：
	- `transitionProps` - 新页面的过渡动画属性
	- `prevTransitionProps` - 旧页面的过渡动画属性
	- `isModal` - 指定页面是否modal
- `onTransitionStart` - 页面动画开始时调用该方法
- `onTransitionEnd` - 页面动画结束时调用该方法


### Screen Navigation Options

#### `title`

可用作headerTitle的后备标题。此外也可以用作`tabBarLabel`（如果嵌套在`TabNavigator`）或者`drawerLabel`（如果嵌套在`DrawerNavigator`）的后备。

#### `header`

React元素或者一个返回React元素的需要传入`HeaderProps`属性的方法，用作展示标题栏。设置`null`时隐藏标题栏。


#### `headerTitle`

用作顶部栏的字符串，React元素或者React 组件。scene里默认是`title`。当组件被使用时，它会收到`allowFontScaling`，`style`，和`children`属性。title字符串会传递给`children`。

#### `headerTitleAllowFontScaling`

顶部栏标题字体是否允许缩放。默认为true

#### `headerBackTitle`

iOS上返回键文字或者设置`null`不显示。默认是上个页面的标题`headerTitle`。

#### `headerTruncatedBackTitle`

当`headerBackTitle`不管用时返回按钮的文字。默认是`"Back"`

#### `headerRight`

顶部栏的右边React元素。

#### `headerLeft`

顶部栏的左边React元素。当该组件应用时，会收到一些属性诸如：`onPress`,`title`,`titleStyle`等等。具体查看`Header.js`获取完整列表。

#### `headerStyle`

顶部栏样式

#### `headerTitleStyle`

顶部栏标题样式

#### `headerBackTitleStyle`

返回键标题样式

#### `headerTintColor`

顶部栏色调

#### `headerPressColorAndroid`

Android顶部栏点击颜色

#### `gesturesEnabled`

使用手势返回页面。默认iOS为true Android为false

#### `gestureResponseDistance`

复写从屏幕边缘滑动的距离以识别手势。它需要以下几个属性:

- `horizontal` - *number* - 水平滑动数值，默认是25
- `vertical` - *number* - 竖直方向滑动距离，默认为135

#### `gestureDirection`

手势滑动返回页面方向。Default为正常手势inverted为右往左方向

### Navigator Props

由StackNavigator创建的导航条有如下属性：

- `screenProps` - 传递额外的项给子页面。譬如:


 ```jsx
 const SomeStack = StackNavigator({
   // config
 });

 <SomeStack
   screenProps={/* this prop will get passed to the screen components as this.props.screenProps */}
 />
 ```

### 例子

查看 [SimpleStack.js](https://github.com/react-community/react-navigation/tree/master/examples/NavigationPlayground/js/SimpleStack.js) 和 [ModalStack.js](https://github.com/react-community/react-navigation/tree/master/examples/NavigationPlayground/js/ModalStack.js) 这些你可以在本地作为[NavigationPlayground](https://github.com/react-community/react-navigation/tree/master/examples/NavigationPlayground)应用跑起来的例子。

你可以通过我们的[expo](https://exp.host/@react-navigation/NavigationPlayground)项目直接在你手机上查看这些例子。

#### Modal StackNavigator with Custom Screen Transitions

 ```js
const ModalNavigator = StackNavigator(
  {
    Main: { screen: Main },
    Login: { screen: Login },
  },
  {
    headerMode: 'none',
    mode: 'modal',
    navigationOptions: {
      gesturesEnabled: false,
    },
    transitionConfig: () => ({
      transitionSpec: {
        duration: 300,
        easing: Easing.out(Easing.poly(4)),
        timing: Animated.timing,
      },
      screenInterpolator: sceneProps => {
        const { layout, position, scene } = sceneProps;
        const { index } = scene;

        const height = layout.initHeight;
        const translateY = position.interpolate({
          inputRange: [index - 1, index, index + 1],
          outputRange: [height, 0, 0],
        });

        const opacity = position.interpolate({
          inputRange: [index - 1, index - 0.99, index],
          outputRange: [0, 1, 1],
        });

        return { opacity, transform: [{ translateY }] };
      },
    }),
  }
);
 ```

Header 切换效果也可以在`transitionConfig`里的`headerLeftInterpolator`和`headerTitleInterpolator`以及`headerRightInterpolator`配置。

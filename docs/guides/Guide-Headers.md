# 配置顶部栏

顶部栏只在StackNavigator可用。

在之前的例子里，我们在应用里创建了个StackNavigator来展示多个页面。


当导航到一个新页面时，我们可以通过`navigation`方法为新路由指定一些参数。在这个案例里，我们想提供聊天页面里的人名。

```js
this.props.navigation.navigate('Chat', { user:  'Lucy' });
```
`user`参数能在 `chat`页面访问到。

```js
class ChatScreen extends React.Component {
  render() {
    const { params } = this.props.navigation.state;
    return <Text>Chat with {params.user}</Text>;
  }
}
```

### 配置顶部标题

下一步，顶部标题也可以通过参数在页面配置

```js
class ChatScreen extends React.Component {
  static navigationOptions = ({ navigation }) => ({
    title: `Chat with ${navigation.state.params.user}`,
  });
  ...
}
```

```phone-example
basic-header
```


### 增加一个右按钮

接着我们在navigation option里增加一个[`header`](/docs/navigators/navigation-options#Stack-Navigation-Options)，这允许我们增加一个右按钮：

```js
static navigationOptions = {
  headerRight: <Button title="Info" />,
  ...
```

```phone-example
header-button
```

navigation options定义时可以带有[`navigation`](/docs/navigators/navigation-prop)参数。让我们渲染一个基于路由参数更改的右按钮，当按钮被点击时调用`navigation.setParams`方法。

```js
static navigationOptions = ({ navigation }) => {
  const { state, setParams } = navigation;
  const isInfo = state.params.mode === 'info';
  const { user } = state.params;
  return {
    title: isInfo ? `${user}'s Contact Info` : `Chat with ${state.params.user}`,
    headerRight: (
      <Button
        title={isInfo ? 'Done' : `${user}'s info`}
        onPress={() => setParams({ mode: isInfo ? 'none' : 'info' })}
      />
    ),
  };
};
```

现在顶部栏可以跟route和state值互动了。

```phone-example
header-interaction
```

### 顶部栏和页面组件互动

有时顶部栏需要获取页面组件属性或者state值。

譬如我们想创建一个顶部带有保存按钮的编辑联系人信息页面。我们想在按保存按钮后用`ActivityIndicator`将按钮替换调。

```js
class EditInfoScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    const { params = {} } = navigation.state;
    let headerRight = (
      <Button
        title="Save"
        onPress={params.handleSave ? params.handleSave : () => null}
      />
    );
    if (params.isSaving) {
      headerRight = <ActivityIndicator />;
    }
    return { headerRight };
  };

  state = {
    nickname: 'Lucy jacuzzi'
  }

  _handleSave = () => {
    // Update state, show ActivityIndicator
    this.props.navigation.setParams({ isSaving: true });
    
    // Fictional function to save information in a store somewhere
    saveInfo().then(() => {
      this.props.navigation.setParams({ isSaving: false});
    })
  }

  componentDidMount() {
    // We can only set the function after the component has been initialized
    this.props.navigation.setParams({ handleSave: this._handleSave });
  }

  render() {
    return (
      <TextInput
        onChangeText={(nickname) => this.setState({ nickname })}
        placeholder={'Nickname'}
        value={this.state.nickname}
      />
    );
  }
}
```
**注意**：因为`handleSave`参数只有在页面加载完后才设置，而不是在`navigationOptions`应用时立即生效，所以我们开始先为`Button`的`handleSave`设为为空方法以便加载页面时不会卡顿。

要看剩余的顶部栏设置请看[navigation options 文档](/docs/navigators/navigation-options#Stack-Navigation-Options)。

除了使用`setParams`你也可以考虑使用[MobX](https://github.com/mobxjs/mobx)或者[Redux](https://github.com/reactjs/redux)等状态管理库。当导航到一个新页面时传递一个包含新页面所需要的内容的对象，或者是你在修改数据，请求网络等操作后想回调的方法这样你的页面组件或者静态`navbarOptions`模块都能访问到改对象。当使用这种方法时，请记得考虑深度链接，在只有javascript基元作为属性传递时效果最好。当如果要考虑深度链接时，你可以使用[higher order component (HOC)](https://reactjs.org/docs/higher-order-components.html)来传递你页面想要的元素。

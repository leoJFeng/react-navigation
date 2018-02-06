# Hello Mobile Navigation

让我们来用React Navigation建立一个Android iOS通用的仿聊天应用吧。

## 开始安装

首先请确保你已经[正确配置使用React Native](http://facebook.github.io/react-native/docs/getting-started.html)。然后创建一个新的项目以及添加`react-navigation`。

```sh
# Create a new React Native App
react-native init SimpleApp
cd SimpleApp

# Install the latest version of react-navigation from npm
npm install --save react-navigation

# Run the new app
react-native run-android
# or:
react-native run-ios
```

如果你要使用`create-react-native-app`来代替`react-native init`，那么：

```sh
# Create a new React Native App
create-react-native-app SimpleApp
cd SimpleApp

# Install the latest version of react-navigation from npm
npm install --save react-navigation

# Run the new app
npm start

# This will start a development server for you and print a QR code in your terminal.
```

确认你的项目已经成功在iOS和Android上运行起来，如下:

```phone-example
bare-project
```

我们想要共享iOS和Android代码所以让我们删掉`index.js`内容(如果是0.49版本以前就是`index.ios.js`和`index.android.js`)而用`import './App';`方式替换调。然后我们创建一个新的文件命名为`App.js`来实现。(如果你已经成功执行了`create-react-native-app`)

## Stack Navigator介绍：

我们的app将使用`StackNavigator`因为一般来说我们会希望拥有卡片栈式的切换效果，即新页面会被放置栈顶以及返回时优先删除栈顶页面。下面我们来实现一个页面：

```js
import React from 'react';
import {
  AppRegistry,
  Text,
} from 'react-native';
import { StackNavigator } from 'react-navigation';

class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    return <Text>Hello, Navigation!</Text>;
  }
}

export const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
});

AppRegistry.registerComponent('SimpleApp', () => SimpleApp);
```

如果你已经执行了`create-react-native-app`，那么上面建的`App.js`页面将修改为：

```js
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import { StackNavigator } from 'react-navigation';

class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome'
  };
  render() {
    return <Text>Hello, Navigation!</Text>;
  }
}

const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen }
});

export default class App extends React.Component {
  render() {
    return <SimpleApp />;
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center'
  }
});

```

页面的标题在[static `navigationOptions`](/docs/navigators/navigation-options)配置。此处可配置许多导航器内页面的各种展现方式。

现在iPhone和Android的app都已经展示相同的页面：

```phone-example
first-screen
```

## 增加一个新页面：

现在我们在`App.js`文件里增加个新页面`ChatScreen`，定义在`HomeScreen`之下。

```js
// ...

class HomeScreen extends React.Component {
    //...
}

class ChatScreen extends React.Component {
  static navigationOptions = {
    title: 'Chat with Lucy',
  };
  render() {
    return (
      <View>
        <Text>Chat with Lucy</Text>
      </View>
    );
  }
}

```

我们可以在`HomeScreen`增加个按钮组件来连接`ChatScreen`，我们需要给`navigate`方法(从页面[navigation属性](/docs/navigators/navigation-prop)获得)提供我们想要跳转页面的`routeName`，本例里是`Chat`。


```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <View>
        <Text>Hello, Chat App!</Text>
        <Button
          onPress={() => navigate('Chat')}
          title="Chat with Lucy"
        />
      </View>
    );
  }
}
```

(不要忘了从react-native引入View和Button。* `import { AppRegistry, Text, View, Button } from 'react-native';`)

但这仍然不会工作，直到我们告诉`StackNavigator`新`Chat`页面的存在，如下:

```js
export const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
  Chat: { screen: ChatScreen },
});
```

现在你可以导航到你的新页面，然后返回。

```phone-example
first-navigation
```

## 传递参数

在`ChatScreen`页面写死名字不够理想，更常用的是我们传入名字属性来展示，让我们开始吧。

除了在navigate方法里指定`routeName`之外，我们还可以传递新页面所需要的参数。首先我们编辑`HomeScreen`组件来传递一个`user`参数到路由。

```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <View>
        <Text>Hello, Chat App!</Text>
        <Button
          onPress={() => navigate('Chat', { user: 'Lucy' })}
          title="Chat with Lucy"
        />
      </View>
    );
  }
}
```

然后我们可以编辑`ChatScreen`组件来展示我们通过路由传递的`user`参数：

```js
class ChatScreen extends React.Component {
  // Nav options can be defined as a function of the screen's props:
  static navigationOptions = ({ navigation }) => ({
    title: `Chat with ${navigation.state.params.user}`,
  });
  render() {
    // The screen's current route is passed in to `props.navigation.state`:
    const { params } = this.props.navigation.state;
    return (
      <View>
        <Text>Chat with {params.user}</Text>
      </View>
    );
  }
}
```

现在当你跳转到`Chat`页面时可以看到新页面名字了。尝试修改`user`参数看看会发生什么吧！

```phone-example
first-navigation
```

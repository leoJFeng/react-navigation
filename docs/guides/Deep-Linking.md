# Deep Linking

在本教程中我们会建一个app处理外部链接。让我们以[开始教程里的SimpleApp](/docs/intro)开始吧。

在这个例子中，我们想要一个类似`mychat://chat/Taylor` 的链接来打开我们的app以及直接跳转到Taylor的聊天页面里。

## Configuration

之前我们是这样定义一个导航的：

```js
const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
  Chat: { screen: ChatScreen },
});
```
我们想像`chat/Taylor`可以链接到"Chat"页面还带有`user`参数。让我们重新定义我们的Chat页面增加一个`path`来告诉路由如何将路径匹配，以及参数具体定义。路径会被指定为`chat/：user`。

```js
const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
  Chat: {
    screen: ChatScreen,
    path: 'chat/:user',
  },
});
```


### URI Prefix

下一步我们来配置导航页面来提取URI路径。

```js
const SimpleApp = StackNavigator({...});

// on Android, the URI prefix typically contains a host in addition to scheme
const prefix = Platform.OS == 'android' ? 'mychat://mychat/' : 'mychat://';

const MainApp = () => <SimpleApp uriPrefix={prefix} />;
```

## iOS

让我们在原生iOS里配置一下来打开`mychat://`的URI scheme。

In `SimpleApp/ios/SimpleApp/AppDelegate.m`:

```
// Add the header at the top of the file:
#import <React/RCTLinkingManager.h>

// Add this above the `@end`:
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
  return [RCTLinkingManager application:application openURL:url
                      sourceApplication:sourceApplication annotation:annotation];
}
```

在Xcode里，打开`SimpleApp/ios/SimpleApp.xcodeproj`项目。选择边栏跳转到info页面。滚动到URL Types增加一项，设置`identifier`和`url scheme`为你想要的链接。

![Xcode project info URL types with mychat added](/assets/xcode-linking.png)

现在你可以运行Xcode项目，或者用如下命令重新构建:

```sh
react-native run-ios
```

为了测试模拟器里的URI，运行如下命令：

```
xcrun simctl openurl booted mychat://chat/Taylor
```

为了在真机上测试URI，打开Safari输入`mychat://chat/Taylor`。

## Android

为了配置Android外部链接，你可以在manifest创建一个新的intent。

在`SimpleApp/android/app/src/main/AndroidManifest.xml`增加新的`View`类型的`intent-filter`到`MainActivity`里:

```
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="mychat"
          android:host="mychat" />
</intent-filter>
```

现在重新构建项目：

```sh
react-native run-android
```

为了测试Android处理intent，运行如下代码:

```
adb shell am start -W -a android.intent.action.VIEW -d "mychat://mychat/chat/Taylor" com.simpleapp
```

```phone-example
linking
```

# Quick Start Guide

想要应用React Navigation，首先你要做的就是通过npm包安装react-navigation。

### 使用NPM安装

```
npm install --save react-navigation
```

### 使用Yarn安装

```
yarn add react-navigation
```

要使用React Navigation 首先你要创建一个新的导航。React Navigation提供了三种默认导航：

- `StackNavigator` - 让你的app页面相互跳转，每一个新页面都会出现在栈的顶层
- `TabNavigator` - 创建带有Tabs的导航页面
- `DrawerNavigator` - 创建带有抽屉的导航页面

## 创建一个StackNavigator

StackNavigator是最常用的一种导航器，下面我们将提供一个简单例子创建一个StackNavigator来介绍学习。

```javascript
import { StackNavigator } from 'react-navigation';

const RootNavigator = StackNavigator({

});

export default RootNavigator;
```

我们可以为StackNavigator增加页面，每一个key值代表了一个页面。

```javascript
import React from 'react';
import { View, Text } from 'react-native';
import { StackNavigator } from 'react-navigation';

const HomeScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
  </View>
);

const DetailsScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Details Screen</Text>
  </View>
);

const RootNavigator = StackNavigator({
  Home: {
    screen: HomeScreen,
  },
  Details: {
    screen: DetailsScreen,
  },
});

export default RootNavigator;
```

现在为导航条增加个标题吧

```javascript
...

const RootNavigator = StackNavigator({
  Home: {
    screen: HomeScreen,
    navigationOptions: {
      headerTitle: 'Home',
    },
  },
  Details: {
    screen: DetailsScreen,
    navigationOptions: {
      headerTitle: 'Details',
    },
  },
});

export default RootNavigator;
```

最后我们要让home页面有能力跳转到detail页面。当你为一个组件注册一个导航时，组件将会拥有一个navigation属性，而这个navigation属性能让我们跳转到不同的页面。

为了能从home页面跳转到detail页面，我们将使用navigation.navigate方式，如下：

```javascript
...
import { View, Text, Button } from 'react-native';

const HomeScreen = ({ navigation }) => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
    <Button
      onPress={() => navigation.navigate('Details')}
      title="Go to details"
    />
  </View>
);

...
```

成功了！这就是StackNavigator和React Navigation的基本使用方法。下面我们将提供以上例子的完整代码:

<div class="snack" data-snack-id="HJlnU0XTb" data-snack-platform="ios" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#fafafa;border:1px solid rgba(0,0,0,.16);border-radius:4px;height:505px;width:100%"></div>

## 创建一个TabNavigator

要使用TabNavigator，首先引入和创建一个RootTabs组件。

```javascript
import { TabNavigator } from 'react-navigation';

const RootTabs = TabNavigator({

});

export default RootTabs;
```

接着我们需要创建一些页面以及将它们添加到我们的TabNavigator里。

```javascript
import React from 'react';
import { View, Text } from 'react-native';
import { TabNavigator } from 'react-navigation';

const HomeScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
  </View>
);

const ProfileScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Profile Screen</Text>
  </View>
);

const RootTabs = TabNavigator({
  Home: {
    screen: HomeScreen,
  },
  Profile: {
    screen: ProfileScreen,
  },
});

export default RootTabs;
```

好了！现在我们来为每个Tabbar子项添加一些标签和图标。

> 我们将在下面示例使用 [`react-native-vector-icons`](https://github.com/oblador/react-native-vector-icons) 库，如果你没有为你项目安装此库请添加之。

```javascript
...
import Ionicons from 'react-native-vector-icons/Ionicons';

...

const RootTabs = TabNavigator({
  Home: {
    screen: HomeScreen,
    navigationOptions: {
      tabBarLabel: 'Home',
      tabBarIcon: ({ tintColor, focused }) => (
        <Ionicons
          name={focused ? 'ios-home' : 'ios-home-outline'}
          size={26}
          style={{ color: tintColor }}
        />
      ),
    },
  },
  Profile: {
    screen: ProfileScreen,
    navigationOptions: {
      tabBarLabel: 'Profile',
      tabBarIcon: ({ tintColor, focused }) => (
        <Ionicons
          name={focused ? 'ios-person' : 'ios-person-outline'}
          size={26}
          style={{ color: tintColor }}
        />
      ),
    },
  },
});

export default RootTabs;
```
这里要确保`tabBarLabel`是一致的（在嵌套的导航器时尤其重要）而这将会设置一个`tabBarIcon`。这个图标在iOS设备上会以tab bar的默认方式展现，而在Android上会以标准设计方式展示。

下面你可以浏览完整的代码:

<div class="snack" data-snack-id="BJZ2GVVpb" data-snack-platform="ios" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#fafafa;border:1px solid rgba(0,0,0,.16);border-radius:4px;height:505px;width:100%"></div>

## 创建一个Drawernavigator:

要开始使用Drawernavigator首先要引入和创建一个新的RootDrawer组件。

```javascript
import { DrawerNavigator } from 'react-navigation';

const RootDrawer = DrawerNavigator({

});

export default RootDrawer;
```

接着我们需要创建一些页面以及将它们添加到我们的`DrawerNavigator`里。

```javascript
import React from 'react';
import { View, Text } from 'react-native';
import { DrawerNavigator } from 'react-navigation';

const HomeScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
  </View>
);

const ProfileScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Profile Screen</Text>
  </View>
);

const RootDrawer = DrawerNavigator({
  Home: {
    screen: HomeScreen,
  },
  Profile: {
    screen: ProfileScreen,
  },
});

export default RootDrawer;
```

好了！现在我们来为每个Drawer子项添加一些标签和图标。

> 我们将在下面示例使用`react-native-vector-icons`库，如果你没有为你项目安装此库请添加之。

```javascript
...
import Ionicons from 'react-native-vector-icons/Ionicons';

...

const RootDrawer = DrawerNavigator({
  Home: {
    screen: HomeScreen,
    navigationOptions: {
      drawerLabel: 'Home',
      drawerIcon: ({ tintColor, focused }) => (
        <Ionicons
          name={focused ? 'ios-home' : 'ios-home-outline'}
          size={20}
          style={{ color: tintColor }}
        />
      ),
    },
  },
  Profile: {
    screen: ProfileScreen,
    navigationOptions: {
      drawerLabel: 'Profile',
      drawerIcon: ({ tintColor, focused }) => (
        <Ionicons
          name={focused ? 'ios-person' : 'ios-person-outline'}
          size={20}
          style={{ color: tintColor }}
        />
      ),
    },
  },
});

export default RootDrawer;
```

要打开抽屉（drawer）你可以在页面从左往右滑动。你也可以通过我们即将加在home页面的`navigation.navigate('DrawerToggle')`方式来打开抽屉页面。请确保你已经正确从`react-native`引用`Button`。

```javascript
...

const HomeScreen = ({ navigation }) => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
    <Button
      onPress={() => navigation.navigate('DrawerToggle')}
      title="Open Drawer"
    />
  </View>
);

...
```

下面你可以浏览完整的代码:

<div class="snack" data-snack-id="rk90N44a-" data-snack-platform="ios" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#fafafa;border:1px solid rgba(0,0,0,.16);border-radius:4px;height:505px;width:100%"></div>

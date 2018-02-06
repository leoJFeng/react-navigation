
# withNavigation

[`withNavigation`](/src/views/withNavigation.js) 是一个高度定制的组件用来传递`navigation`属性到被包裹的组件中。当你不能直接传递`navigation`属性时，或者不想传递下去以防子组件深度嵌套时，它是很有用的

## Example

```js
import { Button } 'react-native';
import { withNavigation } from 'react-navigation';

const MyComponent = ({ to, navigation }) => (
    <Button title={`navigate to ${to}`} onPress={() => navigation.navigate(to)} />
);

const MyComponentWithNavigation = withNavigation(MyComponent);


// or use decorators:

@withNavigation
export default class MainScreen extends Component {
  ...
}
```

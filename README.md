[![Build Status](https://travis-ci.org/KeKs0r/mqtt-react.svg?branch=master)](https://travis-ci.org/KeKs0r/mqtt-react)

# mqtt-react-native
React Container for [mqttjs/MQTT.js](https://github.com/mqttjs/MQTT.js)
modified for React Native 
<!--
### Installation
```
npm i -S mqtt-react-native
```
-->

## Demo
There is a very minimalistic Demo-App: [mqtt-react-demo](https://github.com/KeKs0r/mqtt-react-demo)

### Usage
Currently, mqtt-react-native exports two enhancers.
Similarly to react-redux, you'll have to first wrap a root component with a
```Connector``` which will initialize the mqtt instance and then subscribe to
data by using ```subscribe```.

#### Root component
The only property for the connector is the connection information for [mqtt.Client#connect](https://github.com/mqttjs/MQTT.js#connect)

**Example Root component:**
```JavaScript
import { Connector } from 'mqtt-react-native';
import App from './components/App';

export default () => (
  <Connector mqttProps="ws://test.mosca.io/">
    <App />
  </Connector>
);
```

#### Subscribe 
**Example Subscribed component:**
```JavaScript
import { subscribe } from 'mqtt-react-native';

// Messages are passed on the "data" prop
const MessageList = (props) => (
  <View>
    {props.data.map( message => <Text>{message}</Text> )}
  </View>
);

// simple subscription to messages on the "@test/demo" topic
export default subscribe({
  topic: '@demo/test'
})(MessageList)
```


**Example Posting Messages**

MQTT Client is passed on to subscribed component and can be used to publish messages via
[mqtt.Client#publish](https://github.com/mqttjs/MQTT.js#publish)

```JavaScript
import React from 'react';
import { subscribe } from 'mqtt-react-native';

export class PostMessage extends React.Component {
    
  sendMessage(e) {
      e.preventDefault();
      //MQTT client is passed on
      const { mqtt } = this.props;
      mqtt.publish('@demo/test', 'My Message');
  }  
  
  render() {
    return (
      <Button onClick={this.sendMessage.bind(this)}>
        Send Message
      </Button>
    );
  }
}

export default subscribe({
  topic: '@demo/test'
})(PostMessage)
```

**Advanced Susbcription / Integration with Redux:**

It is possible to provide a function that handles received messages. 
By default the function adds the message to the data prop, but it can be used to dispatch actions to a redux store.
```JavaScript
import { subscribe } from 'mqtt-react-react';
import store from './store';


const customDispatch = function(topic, message, packet) {
    store.dispatch(topic, message);
}


export default subscribe({
  topic: '@demo/test',
  dispatch: customDispatch
})
```

## Credits
Sponsored by <a href="http://nearform.com">nearForm</a>

### Contributing

Pull Requests are very welcome!

If you find any issues, please report them via [Github Issues](https://github.com/KeKs0r/mqtt-react/issues)!

### Contributors
- Marc Höffl [@KeKs0r](https://github.com/KeKs0r)

### License
(MIT)
Sure! Here's an example of how to create a fully capable GUI for Asterisk using JavaScript and the React library:

1. Set up a new React application using create-react-app or a similar tool.

2. Create a new component for the Asterisk GUI. Here's an example of how to create the component:

```javascript
import React, { useState, useEffect } from 'react';
import WebSocketClient from './WebSocketClient';

function AsteriskGUI() {

  const [connected, setConnected] = useState(false);
  const [socket, setSocket] = useState(null);

  useEffect(() => {
    return () => {
      if (socket) {
        socket.disconnect();
      }
    };
  }, [socket]);

  const connect = () => {
    const newSocket = new WebSocketClient('ws://localhost:8080/ws');
    newSocket.connect();
    setSocket(newSocket);
    setConnected(true);
  };

  const disconnect = () => {
    if (socket) {
      socket.disconnect();
      setSocket(null);
      setConnected(false);
    }
  };

  return (
    <div>
      <h1>Asterisk GUI</h1>
      <p>{connected ? 'Connected to AMI' : 'Not connected'}</p>
      <button onClick={connect}>Connect to AMI</button>
      <button onClick={disconnect} disabled={!connected}>Disconnect from AMI</button>
    </div>
  );

}

export default AsteriskGUI;
```

3. Create a WebSocket client to handle the WebSocket connection to the Asterisk server. Here's an example of how to create the client:

```javascript
class WebSocketClient {

  constructor(serverUri) {
    this.socket = new WebSocket(serverUri);
    this.socket.onopen = this.onOpen.bind(this);
    this.socket.onclose = this.onClose.bind(this);
    this.socket.onmessage = this.onMessage.bind(this);
    this.socket.onerror = this.onError.bind(this);
  }

  connect() {
    this.socket.connect();
  }

  disconnect() {
    this.socket.close();
  }

  onOpen(event) {
    console.log('WebSocket connection opened');
  }

  onClose(event) {
    console.log('WebSocket connection closed');
  }

  onMessage(event) {
    console.log('Received message: ' + event.data);
  }

  onError(event) {
    console.error('WebSocket error', event);
  }

}

export default WebSocketClient;
```

4. Render the AsteriskGUI component in the main App component or another appropriate component in your application.

```javascript
import React from 'react';
import AsteriskGUI from './AsteriskGUI';

function App() {
  return (
    <div>
      <AsteriskGUI />
    </div>
  );
}

export default App;
```

5. Build and run the application to see the GUI. When you click "Connect to AMI", the WebSocket connection will be established with the server, and when you click "Disconnect from AMI", the connection will be closed. You can modify the WebSocket client to send actual Asterisk commands and receive responses from the server.

I hope this helps you get started with creating a fully capable GUI for Asterisk in JavaScript using React or another library of your choice!

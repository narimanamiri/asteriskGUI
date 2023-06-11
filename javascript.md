Sure! Here's an example of how you can create a GUI for Asterisk using JavaScript and the Vue.js framework:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Asterisk GUI</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  </head>
  <body>
    <div id="app">
      <h1>Asterisk GUI</h1>
      <div>
        <p>Status: {{ status }}</p>
        <button v-if="!connected" @click="connect">Connect to AMI</button>
        <button v-if="connected" @click="disconnect">Disconnect from AMI</button>
      </div>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            connected: false,
            status: 'Not connected',
            ami_username: 'AMI_USERNAME',
            ami_password: 'AMI_PASSWORD',
            ami_host: 'AMI_HOST',
            ami_port: 'AMI_PORT',
          }
        },
        methods: {
          connect() {
            axios.post('/connect', {
              username: this.ami_username,
              password: this.ami_password,
              host: this.ami_host,
              port: this.ami_port,
            })
            .then(response => {
              if (response.data.status === 'connected') {
                this.connected = true;
                this.status = 'Connected to AMI';
              }
            })
            .catch(error => {
              console.log(error);
            });
          },
          disconnect() {
            axios.post('/disconnect')
            .then(response => {
              if (response.data.status === 'disconnected') {
                this.connected = false;
                this.status = 'Not connected';
              }
            })
            .catch(error => {
              console.log(error);
            });
          },
        }
      });
      app.mount('#app');
    </script>
  </body>
</html>
```

Here's how the script works:

1. The HTML file includes the Vue.js library and the Axios library for making HTTP requests.
2. The script creates a Vue app with data properties for the AMI connection settings, the connection status, and a boolean value indicating whether the app is connected to the AMI.
3. The script includes two methods: `connect()` and `disconnect()`, which make HTTP POST requests to the server to connect to or disconnect from the AMI.
4. The HTML file includes a button to connect to the AMI if the app is not connected, and a button to disconnect from the AMI if the app is connected.
5. When the Connect to AMI button is clicked, the `connect()` method is called, which sends an HTTP POST request to the server with the AMI connection settings. If the connection is successful, the `connected` property is set to `true` and the `status` property is updated to indicate that the app is connected to the AMI.
6. When the Disconnect from AMI button is clicked, the `disconnect()` method is called, which sends an HTTP POST request to the server to disconnect from the AMI. If the disconnection is successful, the `connected` property is set to `false` and the `status` property is updated to indicate that the app is not connected to the AMI.
7. The Vue app is mounted to the `#app` element in the HTML file.

To complete the GUI, you need to create a server-side script that handles the HTTP requests and connects to/disconnects from the AMI. You can use a server-side language like Node.js with the `ami-client` library to handle the AMI connections. Here's an example of how you can create a simple server-side script using Node.js:

```javascript
const AsteriskManager = require('asterisk-manager');
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
const port = 3000;

// Set up Asterisk Manager Interface (AMI) connection
const ami_username = 'AMI_USERNAME';
const ami_password = 'AMI_PASSWORD';
const ami_host = 'AMI_HOST';
const ami_port = 'AMI_PORT';
const ami_manager = new AsteriskManager(ami_port, ami_host, ami_username, ami_password, true);

// Use body-parser middleware
app.use(bodyParser.json());

// Connect to AMI
app.post('/connect', (req, res) => {
  const { username, password, host, port } = req.body;
  ami_manager.connect(port, host, username, password, true)
    .then(() => {
      res.json({ status: 'connected' });
    })
    .catch(error => {
      console.log(error);
      res.status(500).json({ error });
    });
});

// Disconnectfrom AMI
app.post('/disconnect', (req, res) => {
  ami_manager.disconnect()
    .then(() => {
      res.json({ status: 'disconnected' });
    })
    .catch(error => {
      console.log(error);
      res.status(500).json({ error });
    });
});

// Start server
app.listen(port, () => {
  console.log(`Server listening on port ${port}`);
});
```

Here's how the server-side script works:

1. The script imports the `asterisk-manager`, `express`, and `body-parser` libraries.
2. The script sets up an AMI connection to Asterisk using the `asterisk-manager` library.
3. The script sets up an Express app and uses the `body-parser` middleware to parse JSON data in the request body.
4. The script includes two routes: `/connect` and `/disconnect`, which handle HTTP POST requests to connect to or disconnect from the AMI.
5. When a Connect to AMI request is received, the script uses the AMI connection settings from the request body to connect to the AMI using the `connect()` method of the `AsteriskManager` class. If the connection is successful, the server sends a JSON response with a `status` property set to `connected`.
6. When a Disconnect from AMI request is received, the script disconnects from the AMI using the `disconnect()` method of the `AsteriskManager` class. If the disconnection is successful, the server sends a JSON response with a `status` property set to `disconnected`.
7. The server listens on port 3000 for incoming requests.

To run the application, save both scripts to separate files (`index.html` for the client-side script and `server.js` for the server-side script) in the same directory. Open a terminal and navigate to the directory, then run the server script using the command `node server.js`. Open a web browser and navigate to `http://localhost:3000` to use the GUI.

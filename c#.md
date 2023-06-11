Sure! Here's an example of how to create a GUI for Asterisk using C# and the Windows Forms library:

1. Create a new Windows Forms application in Visual Studio.

2. Design the main form for the Asterisk GUI. Here's an example of how to design the form using the Visual Studio designer:

![Asterisk GUI form example](https://i.imgur.com/3J6fzNz.png)

3. Add event handlers to the connect and disconnect buttons to handle the Asterisk connection. Here's an example of how to add the event handlers:

```csharp
using System;
using System.Windows.Forms;

namespace AsteriskGUI {
    public partial class MainForm : Form {

        private bool connected = false;
        private WebSocketClient socket = null;

        public MainForm() {
            InitializeComponent();
        }

        private void connectButton_Click(object sender, EventArgs e) {
            socket = new WebSocketClient("ws://localhost:8080/ws");
            socket.Connect();
            statusLabel.Text = "Connected to AMI";
            connected = true;
        }

        private void disconnectButton_Click(object sender, EventArgs e) {
            socket.Disconnect();
            statusLabel.Text = "Not connected";
            connected = false;
        }

    }
}
```

4. Create a WebSocket client to handle the WebSocket connection to the Asterisk server. Here's an example of how to create the client:

```csharp
using System;
using WebSocketSharp;

namespace AsteriskGUI {
    class WebSocketClient {

        private WebSocket socket;

        public WebSocketClient(string serverUri) {
            socket = new WebSocket(serverUri);
            socket.OnOpen += OnOpen;
            socket.OnClose += OnClose;
            socket.OnMessage += OnMessage;
            socket.OnError += OnError;
        }

        public void Connect() {
            socket.Connect();
        }

        public void Disconnect() {
            socket.Close();
        }

        private void OnOpen(object sender, EventArgs e) {
            Console.WriteLine("WebSocket connection opened");
        }

        private void OnClose(object sender, CloseEventArgs e) {
            Console.WriteLine("WebSocket connection closed");
        }

        private void OnMessage(object sender, MessageEventArgs e) {
            Console.WriteLine("Received message: " + e.Data);
        }

        private void OnError(object sender, ErrorEventArgs e) {
            Console.WriteLine("WebSocket error: " + e.Message);
        }

    }
}
```

5. Build and run the application to see the GUI. When you click "Connect to AMI", the WebSocket connection will be established with the server, and when you click "Disconnect from AMI", the connection will be closed. You can modify the WebSocket client to send actual Asterisk commands and receive responses from the server.

I hope this helps you get started with creating a GUI for Asterisk in C# using Windows Forms or another GUI library of your choice!

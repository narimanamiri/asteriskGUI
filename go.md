Sure! Here's an example of how to create a GUI for Asterisk using Go:

1. Choose a GUI library that you want to use with Go. Some popular options include web frameworks like Echo, Gin, and Beego, as well as desktop application frameworks like Go-GTK and Go-Qt. For this example, we'll use the web framework Echo.

2. Install Echo using the following command:
```sh
go get github.com/labstack/echo/v4
```

3. Create a new Echo server and define a route for the Asterisk GUI page. Here's an example of how to create the server and route:

```go
package main

import (
	"net/http"

	"github.com/labstack/echo/v4"
)

func main() {
	e := echo.New()

	e.GET("/", func(c echo.Context) error {
		return c.File("gui.html")
	})

	e.Logger.Fatal(e.Start(":8080"))
}
```

4. Create a new HTML file called "gui.html" in the same directory as the Go file. Add the following code to create the Asterisk GUI:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Asterisk GUI</title>
	<style>
		body {
			font-family: Arial, sans-serif;
			margin: 0;
			padding: 0;
		}

		#app {
			margin: 20px;
		}

		button {
			padding: 10px;
			margin: 10px;
			cursor: pointer;
			background-color: #4CAF50;
			color: white;
			border: none;
			border-radius: 5px;
		}
	</style>
</head>
<body>
	<div id="app">
		<h1>Asterisk GUI</h1>
		<div>
			<p>Status: <span id="status">Not connected</span></p>
			<button id="connectButton" onclick="connect()">Connect to AMI</button>
			<button id="disconnectButton" onclick="disconnect()" style="display:none">Disconnect from AMI</button>
		</div>
	</div>

	<script>
		var connected = false;
		var socket = null;

		function connect() {
			socket = new WebSocket("ws://localhost:8080/ws");

			socket.onopen = function(event) {
				document.querySelector("#status").innerText = "Connected to AMI";
				document.querySelector("#connectButton").style.display = "none";
				document.querySelector("#disconnectButton").style.display = "block";
				connected = true;
			};

			socket.onmessage = function(event) {
				var response = event.data;
				var responseBox = document.querySelector("#response");
				responseBox.innerText = response;
			};
		}

		function disconnect() {
			socket.close();

			document.querySelector("#status").innerText = "Not connected";
			document.querySelector("#disconnectButton").style.display = "none";
			document.querySelector("#connectButton").style.display = "block";
			connected = false;
		}
	</script>
</body>
</html>
```

5. Add a new route to the Echo server to handle WebSocket connections. Here's an example of how to do it:

```go
package main

import (
	"net/http"

	"github.com/labstack/echo/v4"
	"github.com/labstack/echo/v4/middleware"
	"github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
	ReadBufferSize:  1024,
	WriteBufferSize: 1024,
}

func main() {
	e := echo.New()

	e.Use(middleware.Logger())
	e.Use(middleware.Recover())

	e.GET("/", func(c echo.Context) error {
		return c.File("gui.html")
	})

	e.GET("/ws", func(c echo.Context) error {
		conn, err := upgrader.Upgrade(c.Response(), c.Request(), nil)
		if err != nil {
			return err
		}

		for {
			messageType, message, err := conn.ReadMessage()
			if err != nil {
				return err
			}
			if messageType == websocket.TextMessage {
				response := []byte("Response from Asterisk")
				if err := conn.WriteMessage(messageType, response); err != nil {
					return err
				}
			}
		}
	})

	e.Logger.Fatal(e.Start(":8080"))
}
```

6. Build and run the application to see the GUI. When you click "Connect to AMI", the WebSocket connection will be established with the server, and when you click "Disconnect from AMI", the connection will be closed. You can modify the WebSocket handler to send actual Asterisk commands and receive responses from the server.

I hope this helps you get started with creating a GUI for Asterisk in Go using Echo or another web framework of your choice!

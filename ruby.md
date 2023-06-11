Sure! Here's an example of how to create a GUI for Asterisk using Ruby:

1. Choose a GUI library that you want to use with Ruby. Some popular options include web frameworks like Rails and Sinatra, as well as desktop application frameworks like Qt and Shoes. For this example, we'll use the web framework Sinatra.

2. Install Sinatra using the following command:
```sh
gem install sinatra
```

3. Create a new Sinatra application and define a route for the Asterisk GUI page. Here's an example of how to create the application and route:

```ruby
require 'sinatra'

get '/' do
  send_file File.join(settings.public_folder, 'gui.html')
end
```

4. Create a new HTML file called "gui.html" in the same directory as the Ruby file. Add the following code to create the Asterisk GUI:

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
			socket = new WebSocket("ws://localhost:4567/ws");

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

5. Add a new route to the Sinatra application to handle WebSocket connections. Here's an example of how to do it:

```ruby
require 'sinatra'
require 'sinatra-websocket'

get '/' do
  send_file File.join(settings.public_folder, 'gui.html')
end

get '/ws' do
  if request.websocket?
    request.websocket do |ws|
      # Initialize the WebSocket connection here
    end
  end
end
```

6. Build and run the application to see the GUI. When you click "Connect to AMI", the WebSocket connection will be established with the server, and when you click "Disconnect from AMI", the connection will be closed. You can modify the WebSocket handler to send actual Asterisk commands and receive responses from the server.

I hope this helps you get started with creating a GUI for Asterisk in Ruby using Sinatra or another web framework of your choice!

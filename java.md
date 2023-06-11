Sure! Here's an example of how to create a GUI for Asterisk using Java:

1. Choose a GUI library that you want to use with Java. Some popular options include JavaFX, Swing, and AWT. For this example, we'll use JavaFX.

2. Create a new JavaFX application and define a stage for the Asterisk GUI. Here's an example of how to create the application and stage:

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class AsteriskGUI extends Application {

    private boolean connected = false;
    private WebSocketClient socket = null;
    private Label statusLabel;

    @Override
    public void start(Stage primaryStage) throws Exception {
        primaryStage.setTitle("Asterisk GUI");

        VBox root = new VBox();
        root.setSpacing(10);

        Label titleLabel = new Label("Asterisk GUI");
        titleLabel.setStyle("-fx-font-size: 24px;");

        statusLabel = new Label("Not connected");
        statusLabel.setStyle("-fx-font-size: 18px;");

        Button connectButton = new Button("Connect to AMI");
        connectButton.setOnAction(event -> {
            connect();
        });

        Button disconnectButton = new Button("Disconnect from AMI");
        disconnectButton.setOnAction(event -> {
            disconnect();
        });
        disconnectButton.setDisable(true);

        root.getChildren().addAll(titleLabel, statusLabel, connectButton, disconnectButton);

        primaryStage.setScene(new Scene(root, 400, 300));
        primaryStage.show();
    }

    private void connect() {
        socket = new WebSocketClient("ws://localhost:8080/ws");
        socket.connect();
        statusLabel.setText("Connected to AMI");
        connected = true;
    }

    private void disconnect() {
        socket.disconnect();
        statusLabel.setText("Not connected");
        connected = false;
    }

    public static void main(String[] args) {
        launch(args);
    }

}
```

3. Create a new WebSocket client to handle the WebSocket connection to the Asterisk server. Here's an example of how to create the client:

```java
import java.net.URI;
import java.net.URISyntaxException;
import org.java_websocket.client.WebSocketClient;
import org.java_websocket.handshake.ServerHandshake;

public class WebSocketClient extends WebSocketClient {

    public WebSocketClient(String serverURI) {
        super(getServerURI(serverURI));
    }

    @Override
    public void onOpen(ServerHandshake handshake) {
        System.out.println("WebSocket connection opened");
    }

    @Override
    public void onClose(int code, String reason, boolean remote) {
        System.out.println("WebSocket connection closed");
    }

    @Override
    public void onMessage(String message) {
        System.out.println("Received message: " + message);
    }

    @Override
    public void onError(Exception ex) {
        System.err.println("WebSocket error: " + ex.getMessage());
    }

    private static URI getServerURI(String serverURI) {
        try {
            return new URI(serverURI);
        } catch (URISyntaxException ex) {
            throw new IllegalArgumentException("Invalid server URI: " + serverURI, ex);
        }
    }

}
```

4. Build and run the application to see the GUI. When you click "Connect to AMI", the WebSocket connection will be established with the server, and when you click "Disconnect from AMI", the connection will be closed. You can modify the WebSocket client to send actual Asterisk commands and receive responses from the server.

I hope this helps you get started with creating a GUI for Asterisk in Java using JavaFX or another GUI library of your choice!

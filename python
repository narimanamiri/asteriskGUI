an example of how you can create a GUI for Asterisk using Python and the PyQt5 library:

```python
import asterisk.manager
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QPushButton, QVBoxLayout, QWidget

# Set up Asterisk Manager Interface (AMI) connection
ami_username = 'AMI_USERNAME'
ami_password = 'AMI_PASSWORD'
ami_host = 'AMI_HOST'
ami_port = 'AMI_PORT'

# Set up AMI manager
ami_manager = asterisk.manager.Manager()
ami_manager.connect(ami_host, int(ami_port))
ami_manager.login(ami_username, ami_password)

# Set up GUI
class AsteriskGUI(QMainWindow):
    def __init__(self):
        super().__init__()

        # Set up window title and size
        self.setWindowTitle('Asterisk GUI')
        self.setGeometry(100, 100, 300, 150)

        # Add label for status
        self.status_label = QLabel('Not connected')
        self.status_label.setAlignment(Qt.AlignCenter)

        # Add button to connect to AMI
        self.connect_button = QPushButton('Connect to AMI')
        self.connect_button.clicked.connect(self.connect_to_ami)

        # Add button to disconnect from AMI
        self.disconnect_button = QPushButton('Disconnect from AMI')
        self.disconnect_button.clicked.connect(self.disconnect_from_ami)
        self.disconnect_button.setDisabled(True)

        # Add widgets to layout
        layout = QVBoxLayout()
        layout.addWidget(self.status_label)
        layout.addWidget(self.connect_button)
        layout.addWidget(self.disconnect_button)

        # Set up central widget
        central_widget = QWidget()
        central_widget.setLayout(layout)
        self.setCentralWidget(central_widget)

    # Function to connect to AMI
    def connect_to_ami(self):
        ami_manager.login(ami_username, ami_password)
        if ami_manager.connected():
            self.status_label.setText('Connected to AMI')
            self.connect_button.setDisabled(True)
            self.disconnect_button.setDisabled(False)

    # Function to disconnect from AMI
    def disconnect_from_ami(self):
        ami_manager.logoff()
        ami_manager.close()
        if not ami_manager.connected():
            self.status_label.setText('Disconnected from AMI')
            self.connect_button.setDisabled(False)
            self.disconnect_button.setDisabled(True)

# Run GUI
if __name__ == '__main__':
    app = QApplication([])
    gui = AsteriskGUI()
    gui.show()
    app.exec_()
```

Here's how the script works:

1. The script sets up an AMI connection to Asterisk using the `asterisk.manager.Manager` class from the `python-asterisk` library.
2. The script creates a GUI using the `QMainWindow` class from the PyQt5 library.
3. The GUI includes a label for the status of the AMI connection and two buttons: one to connect to the AMI and one to disconnect from the AMI.
4. The `connect_to_ami()` function is called when the Connect to AMI button is clicked. This function logs in to the AMI and updates the status label to indicate that the connection is successful.
5. The `disconnect_from_ami()` function is called when the Disconnect from AMI button is clicked. This function logs off from the AMI, closes the connection, and updates the status label to indicate that the connection is closed.
6. The `AsteriskGUI` class sets up the main window, including the layout and widgets.
7. The GUI is run using the `exec_()` method of the `QApplication` class.

Note that this is a basic example of a GUI for Asterisk using PyQt5. You can modify the script to add more features, such as buttons to initiate and end calls, display call logs, and so on, using the PyQt5 documentation.

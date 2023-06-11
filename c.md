Sure! Here's an example of how to create a GUI for Asterisk using C:

1. Choose a GUI library that you want to use with C. Some popular options include GTK+, Qt, and FLTK. For this example, we'll use GTK+.

2. Create a new GTK+ application and define a window for the Asterisk GUI. Here's an example of how to create the application and window:

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    GtkWidget *window;
    GtkWidget *titleLabel;
    GtkWidget *statusLabel;
    GtkWidget *connectButton;
    GtkWidget *disconnectButton;

    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Asterisk GUI");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);

    titleLabel = gtk_label_new("Asterisk GUI");
    gtk_widget_set_halign(titleLabel, GTK_ALIGN_CENTER);
    gtk_widget_set_valign(titleLabel, GTK_ALIGN_CENTER);
    gtk_widget_set_margin_top(titleLabel, 20);
    gtk_widget_set_margin_bottom(titleLabel, 20);
    gtk_widget_set_margin_start(titleLabel, 20);
    gtk_widget_set_margin_end(titleLabel, 20);
    gtk_widget_set_name(titleLabel, "title-label");
    gtk_container_add(GTK_CONTAINER(window), titleLabel);

    statusLabel = gtk_label_new("Not connected");
    gtk_widget_set_halign(statusLabel, GTK_ALIGN_CENTER);
    gtk_widget_set_valign(statusLabel, GTK_ALIGN_CENTER);
    gtk_widget_set_margin_bottom(statusLabel, 10);
    gtk_widget_set_margin_start(statusLabel, 20);
    gtk_widget_set_margin_end(statusLabel, 20);
    gtk_container_add(GTK_CONTAINER(window), statusLabel);

    connectButton = gtk_button_new_with_label("Connect to AMI");
    gtk_widget_set_halign(connectButton, GTK_ALIGN_CENTER);
    gtk_widget_set_valign(connectButton, GTK_ALIGN_CENTER);
    gtk_widget_set_margin_bottom(connectButton, 10);
    gtk_widget_set_margin_start(connectButton, 20);
    gtk_widget_set_margin_end(connectButton, 20);
    gtk_container_add(GTK_CONTAINER(window), connectButton);

    disconnectButton = gtk_button_new_with_label("Disconnect from AMI");
    gtk_widget_set_halign(disconnectButton, GTK_ALIGN_CENTER);
    gtk_widget_set_valign(disconnectButton, GTK_ALIGN_CENTER);
    gtk_widget_set_margin_bottom(disconnectButton, 10);
    gtk_widget_set_margin_start(disconnectButton, 20);
    gtk_widget_set_margin_end(disconnectButton, 20);
    gtk_container_add(GTK_CONTAINER(window), disconnectButton);

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}
```

3. Add signal handlers to the connect and disconnect buttons to handle the Asterisk connection. Here's an example of how to add the signal handlers:

```c
#include <gtk/gtk.h>

static void connect_to_ami(GtkWidget *widget, gpointer data) {
    // Connect to Asterisk using AMI
}

static void disconnect_from_ami(GtkWidget *widget, gpointer data) {
    // Disconnect from Asterisk using AMI
}

int main(int argc, char *argv[]) {
    GtkWidget *window;
    GtkWidget *titleLabel;
    GtkWidget *statusLabel;
    GtkWidget *connectButton;
    GtkWidget *disconnectButton;

    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Asterisk GUI");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);

    titleLabel = gtk_label_new("Asterisk GUI");
    // ...
    gtk_container_add(GTK_CONTAINER(window), titleLabel);

    statusLabel = gtk_label_new("Not connected");
    // ...
    gtk_container_add(GTK_CONTAINER(window), statusLabel);

    connectButton = gtk_button_new_with_label("Connect to AMI");
    g_signal_connect(connectButton, "clicked", G_CALLBACK(connect_to_ami), NULL);
    // ...
    gtk_container_add(GTK_CONTAINER(window), connectButton);

    disconnectButton = gtk_button_new_with_label("Disconnect from AMI");
    g_signal_connect(disconnectButton, "clicked", G_CALLBACK(disconnect_from_ami), NULL);
    // ...
    gtk_container_add(GTK_CONTAINER(window), disconnectButton);

    // ...

    gtk_main();

    return 0;
}
```

4. Build and run the application to see the GUI. When you click "Connect to AMI", the Asterisk connection will be established, and when you click "Disconnect from AMI", the connection will be closed. You can modify the signal handlers to send actual Asterisk commands and receive responses from the server.

I hope this helps you get started with creating a GUI for Asterisk in C using GTK+ or another GUI library of your choice!

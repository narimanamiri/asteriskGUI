Here's an example of how you can create a GUI for Asterisk using ASP.NET:

1. Open Visual Studio and create a new ASP.NET project.

2. In the Solution Explorer, right-click on the project name and select "Add" -> "New Item".

3. In the "Add New Item" dialog box, select "Web Form" and give it a name such as "AsteriskGUI.aspx".

4. In the "AsteriskGUI.aspx" file, add the following markup code to create the Asterisk GUI:
```html
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="AsteriskGUI.aspx.cs" Inherits="YourNamespace.AsteriskGUI" %>

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
    <form id="form1" runat="server">
        <div id="app">
            <h1>Asterisk GUI</h1>
            <div>
                <p>Status: <%= status %></p>
                <asp:Button ID="connectButton" runat="server" Text="Connect to AMI" OnClick="ConnectButton_Click" Visible="<%# !connected %>" />
                <asp:Button ID="disconnectButton" runat="server" Text="Disconnect from AMI" OnClick="DisconnectButton_Click" Visible="<%# connected %>" />
            </div>
        </div>
    </form>

    <script>
        var connected = <%= connected.ToString().ToLower() %>;
        var status = '<%= status %>';

        var connectButton = document.getElementById('connectButton');
        var disconnectButton = document.getElementById('disconnectButton');

        connectButton.addEventListener('click', function () {
            window.location.href = '/connect';
        });

        disconnectButton.addEventListener('click', function () {
            window.location.href = '/disconnect';
        });

        if (connected) {
            status = 'Connected to AMI';
        } else {
            status = 'Not connected';
        }

        document.querySelector('#app p').innerText = 'Status: ' + status;
    </script>
</body>
</html>
```

5. In the code-behind file `AsteriskGUI.aspx.cs`, add the following C# code to handle the AMI connection requests:
```csharp
using System;
using AsterNET.Manager;
using AsterNET.Manager.Action;
using AsterNET.Manager.Response;

namespace YourNamespace
{
    public partial class AsteriskGUI : System.Web.UI.Page
    {
        private ManagerConnection ami;

        protected bool connected
        {
            get { return Session["ami"] != null && ((ManagerConnection)Session["ami"]).IsConnected(); }
        }

        protected string status
        {
            get
            {
                if (connected)
                {
                    return "Connected to AMI";
                }
                else
                {
                    return "Not connected";
                }
            }
        }

        protected void ConnectButton_Click(object sender, EventArgs e)
        {
            string amiUsername = "AMI_USERNAME";
            string amiPassword = "AMI_PASSWORD";
            string amiHost = "AMI_HOST";
            int amiPort = 5038;

            ami = new ManagerConnection(amiHost, amiPort, amiUsername, amiPassword);

            try
            {
                ami.Login();
                Session["ami"] = ami;
            }
            catch (Exception ex)
            {
                Response.Write("Failed to connect to AMI: " + ex.Message);
            }
        }

        protected void DisconnectButton_Click(object sender, EventArgs e)
        {
            if (Session["ami"] != null)
            {
                ami = (ManagerConnection)Session["ami"];

                if (ami.IsConnected())
                {
                    ami.Logoff();
                    Session["ami"] = null;
                }
            }
        }
    }
}
```

6. Add the following lines to the `web.config` file to allow for the AMI connection requests:
```xml
<configuration>
  <system.web>
    <compilation debug="true" targetFramework="4.7.2" />
    <httpRuntime targetFramework="4.7.2" />
    <pages>
      <controls>
        <add tagPrefix="asp" namespace="System.Web.UI" assembly="System.Web.Extensions" />
      </controls>
    </pages>
  </system.web>
  <system.webServer>
    <modules runAllManagedModulesForAllRequests="true" />
  </system.webServer>
</configuration>
```

7. Finally, add the following code to the `Global.asax.cs` file to enable session state:
```csharp
using System.Web.SessionState;

namespace YourNamespace
{
    public class Global : System.Web.HttpApplication
    {
        protected void Application_Start(object sender, EventArgs e)
        {
            // ...
        }

        protected void Session_Start(object sender, EventArgs e)
        {
            SessionStateModule session = new SessionStateModule();
            session.Init(new HttpApplication());
        }
    }
}
```

That's it! You should now be able to run the ASP.NET application and navigate to the Asterisk GUI page to connect and disconnect from the AMI.

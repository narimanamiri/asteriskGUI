 Here's an example of how you can create a GUI for Asterisk using Laravel PHP framework:

1. First, create a new Laravel project by running the following command in your terminal:
```
laravel new asterisk-gui
```

2. Next, create a route to display the Asterisk GUI page. In `routes/web.php`, add the following route definition:
```php
Route::get('/', 'AsteriskController@index');
```

3. Create a controller called `AsteriskController` by running the following command in your terminal:
```
php artisan make:controller AsteriskController
```

4. In `AsteriskController`, create a method called `index` that returns the Asterisk GUI view:
```php
public function index()
{
    return view('asterisk.index');
}
```

5. Create a new view called `index.blade.php` in `resources/views/asterisk/` directory. This view will contain the HTML for the Asterisk GUI. Here's an example of what the view might look like:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Asterisk GUI</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="{{ asset('css/app.css') }}">
    <script src="{{ asset('js/app.js') }}" defer></script>
</head>
<body>
    <div id="app">
        <h1>Asterisk GUI</h1>
        <div>
            <p>Status: @{{ status }}</p>
            <button v-if="!connected" @click="connect">Connect to AMI</button>
            <button v-if="connected" @click="disconnect">Disconnect from AMI</button>
        </div>
    </div>
</body>
</html>
```

6. Add the following code to the `resources/js/app.js` file to create the Vue.js component that will control the Asterisk GUI:
```javascript
require('./bootstrap');

import Vue from 'vue';
import axios from 'axios';

const app = new Vue({
    el: '#app',
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
```

7. Run the following command in your terminal to compile the JavaScript code:
```
npm run dev
```

8. Add the following code to the `app/Http/Controllers/AsteriskController.php` file to create the server-side functions that will handle the AMI connection requests:
```php
use Asterisk\Client\AsteriskClient;

public function connect(Request $request)
{
    $username = $request->input('username');
    $password = $request->input('password');
    $host = $request->input('host');
    $port = $request->input('port');

    $client = new AsteriskClient($host, $port, $username, $password);
    $client->connect();

    if ($client->isConnected()) {
        session(['ami_client' => $client]);
        return response()->json(['status' => 'connected']);
    } else {
        return response()->json(['error' => 'Failed to connect to AMI'], 500);
    }
}

public function disconnect()
{
    $client = session('ami_client');

    if ($client && $client->isConnected()) {
        $client->disconnect();
        session()->forget('ami_client');
    }

    return response()->json(['status' => 'disconnected']);
}
```

9. Finally, add the following lines to the `routes/web.php` file to define the routes that will handle the AMI connections:
```php
Route::post('/connect', 'AsteriskController@connect');
Route::post('/disconnect', 'AsteriskController@disconnect');
```

That's it! You should now be able to run the Laravel application using the following command:
```
php artisan serve
```

Navigate tohttp://localhost:8000/ in your web browser to see the Asterisk GUI.

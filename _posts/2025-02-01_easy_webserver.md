---
title: "Easy Ways to Run a Simple Web Server"
date: 2025-02-01
---

# Easy Ways to Run a Simple Web Server

If you are on Windows like me and you did not want to run IIS or IIS Express just to serve some static content, you might want to consider the following no-fuss, no-configuration way to start a simple web server. 

### 1. Using Node.js and http-server

- **Install http-server**: 
  ```bash
    npm install -g http-server
    http-server
  ```

### 2. Using Python
- Using Python's built-in HTTP server if you have Python installed.
- You can run a simple server with one command:
  ```bash
    python -m http.server 8000
  ```

### 3. Visual Studio Code with Live Server Extension

- **Download and Install Visual Studio Code**: [VS Code](https://code.visualstudio.com/)
- **Install Live Server Extension**: By Ritwick Dey from the [Extensions marketplace](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer).
- **Run Live Server**: Right-click on your HTML file and select "Open with Live Server."

### 4. Web Server for Chrome

- **Install the Extension**: [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb)
- **Open the Extension**: Select the folder you want to serve.
- **Start the Server**: Click "Start" to run the server.

### 5. Using Node.js and the http module
- Save this code in a file named server.js.
- Run it with `node server.js`.
```js
const http = require('http');
const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

### 6. Using PowerShell
- Change $folder to the directory you want to serve and $port to the port you want to use.
- Save this file to simple_webserver.ps1 and run it.
```powershell
$port = 8080
$folder = "C:\Path\To\Your\Folder"
Add-Type -TypeDefinition @"
using System.Net;
using System.IO;
public class WebServer {
    public static void Start(int port, string folder) {
        HttpListener listener = new HttpListener();
        listener.Prefixes.Add("http://+:{port}/");
        listener.Start();
        while (true) {
            HttpListenerContext context = listener.GetContext();
            string file = Path.Combine(folder, context.Request.Url.LocalPath.TrimStart('/'));
            context.Response.StatusCode = File.Exists(file) ? 200 : 404;
            using (Stream output = context.Response.OutputStream)
            using (FileStream fs = File.OpenRead(file)) {
                fs.CopyTo(output);
            }
        }
    }
}
"@
[WebServer]::Start($port, $folder)
```

### 7. Using Docker with an Existing Image
- Granted these are a little bit memory intensive but I am including them here because it is easy to remove them once you are done.

- Using nginx:
```bash
docker run --name my-nginx -v /path/to/your/site:/usr/share/nginx/html:ro -d -p 8080:80 nginx
```

- Using httpd (Apache):
```bash
docker run --name my-apache -v /path/to/your/site:/usr/local/apache2/htdocs:ro -d -p 8080:80 httpd
```

Hope you find one of these ways useful!
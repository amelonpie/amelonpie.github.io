#### How to create a HTTPS server
Creating a HTTPS server is as simple as creating a HTTP server.
If the server key is created by RSA, use 4096 bit. Or recommended 256 bit ECDSA.
```python
#!/usr/bin/env python3

import http.server, ssl
httpd = http.server.HTTPServer(server_address, http.server.SimpleHTTPRequestHandler)
httpd.socket = ssl.wrap_socket(httpd.socket,
                               server_side=True,
                               certfile='serverCert.crt',
                               keyfile='serverKey.key',
                               ssl_version=ssl.PROTOCOL_TLS)
httpd.serve_forever()
```

#### Define customized HTTP method
The SimpleHTTPRequestHandler only implement do_GET() and do_HEAD() method. They can be overrided.
If the client uses HTTP to upload a file (such as CUROPT_UPLOAD by curl), then the server need to define the do_PUT method to respond to the HTTP PUT method.
```python
class CustomHTTPRequestHandler(http.server.SimpleHTTPRequestHandler):
    # This is a override do_GET to just send response
    def do_GET(self):
        self.send_response(200, "Connected")
        self.end_headers()
        replyBody = 'This is my reply body \n'
        # wfile: contains the output stream for writing a response back to the client.
        self.wfile.write(replyBody.encode('utf-8'))
    
    # This do_PUT will only print the contents of the received file in the server console.
    # if want to save file to server disk, see ref. Need to define the filename itself.
    def do_PUT(self):
      contentLength = int(self.headers['Content-Length'])
      # rfile: an io.BufferedIOBase input stream, ready to read from the start of the optional input data.
      contents = self.rfile.read(contentLength)
      print(contents)
      self.send_response(201, "Created")
      self.end_headers()

Handler = CustomHTTPRequestHandler
httpd = http.server.HTTPServer(server_address, Handler)
# ... the below are the same
```

#### Ref
https://docs.python.org/3.8/library/http.server.html#http.server.SimpleHTTPRequestHandler
https://www.youtube.com/watch?v=T4Df5_cojAs
https://gist.github.com/fabiand/5628006
https://stackoverflow.com/questions/13146064/simple-python-webserver-to-save-file

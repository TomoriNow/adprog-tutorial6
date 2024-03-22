# Tutorial 6 Reflections
## Name: Muhammad Sean Arsha Galant
## NPM: 2206822963

## Commit 1 Reflection notes

The Rust code in main.rs sets up a simple TCP server that listens on the local address 127.0.0.1 on port 7878. It first imports necessary modules from the standard library, including those for handling input-output operations (io) and networking (net).

The main function initializes a TCP listener bound to the specified address and port. It then enters a loop to handle incoming connections. For each incoming connection, it calls the handle_connection function, passing the connected stream.

The handle_connection function in the is responsible for processing incoming TCP streams, specifically in the context of HTTP requests. Upon receiving a mutable reference to a TCP stream, it creates a BufReader to efficiently read from the stream. Using the lines method, it reads the incoming data line by line, unwrapping each line to handle potential errors. It utilizes take_while to collect lines until an empty line is encountered, indicating the end of the HTTP request. The collected lines are stored in a vector named http_request. Finally, the contents of this vector are printed using println!, allowing observation of the HTTP request content.

## Commit 2 Reflection notes

In this updated handle_connection function, which is part of a simple TCP server, the incoming TCP stream is processed to handle HTTP requests. Initially, a BufReader is created to efficiently read from the stream. The HTTP request is then read line by line, unwrapping each line to handle any potential errors, and collecting them into a vector named http_request, similar to the previous version.

However, unlike the previous version, this updated function proceeds to generate an HTTP response. It first creates a status_line indicating the response status as "HTTP/1.1 200 OK". Then, it reads the contents of a file named "hello.html" using fs::read_to_string, assuming this file contains the content to be served in the response. The length of the contents is calculated to set the "Content-Length" header.

A response string is constructed using the format! macro, including the status line, content length, and the actual contents of the file. This response is then written to the TCP stream using write_all, converting the response string to bytes using as_bytes().

Overall, this updated handle_connection function not only processes incoming HTTP requests but also generates and sends an HTTP response containing the content of a specified file.

![Commit 2 screen capture](/assets/images/commit2.png)
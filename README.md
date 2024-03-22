# Tutorial 6 Reflections
## Name: Muhammad Sean Arsha Galant
## NPM: 2206822963

## Commit 1 Reflection notes

The Rust code in main.rs sets up a simple TCP server that listens on the local address 127.0.0.1 on port 7878. It first imports necessary modules from the standard library, including those for handling input-output operations (io) and networking (net).

The main function initializes a TCP listener bound to the specified address and port. It then enters a loop to handle incoming connections. For each incoming connection, it calls the handle_connection function, passing the connected stream.

The handle_connection function in the is responsible for processing incoming TCP streams, specifically in the context of HTTP requests. Upon receiving a mutable reference to a TCP stream, it creates a BufReader to efficiently read from the stream. Using the lines method, it reads the incoming data line by line, unwrapping each line to handle potential errors. It utilizes take_while to collect lines until an empty line is encountered, indicating the end of the HTTP request. The collected lines are stored in a vector named http_request. Finally, the contents of this vector are printed using println!, allowing observation of the HTTP request content.



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

## Commit 3 Reflection notes

In the third commit, the handle_connection function has been updated and refactored once more so that it could process unrecognized HTTP requests (when the page is not found it returns a 404 error) where the incoming TCP stream is processed to handle HTTP requests. Initially, a BufReader is created to efficiently read from the stream. Then, the first line of the HTTP request is extracted using buf_reader.lines().next().unwrap().unwrap(), which retrieves the first line of the request or panics if there's an error or no line is available.

Based on the extracted request_line, a tuple containing the status_line and filename for the response is determined. If the request_line corresponds to a GET request for the root ("/") path in HTTP/1.1, indicating a request for the "hello.html" file, the status_line is set to "HTTP/1.1 200 OK" and the filename to "hello.html". Otherwise, if the requested path does not match the root, indicating a not-found scenario, the status_line is set to "HTTP/1.1 404 NOT FOUND" and the filename to "404.html".

The content of the file specified by filename is then read using fs::read_to_string, assuming it contains the content to be served in the response. The length of the contents is calculated to set the "Content-Length" header.

Next, a response string is constructed using the format! macro, incorporating the status_line, content length, and the actual contents of the file. This response is then written to the TCP stream using write_all, converting the response string to bytes using as_bytes(). 

In summary, the updated handle_connection function processes incoming HTTP requests, determines the appropriate response based on the requested path, serves the content of corresponding files, and sends the response back over the TCP stream.

![Commit 3 screen capture](/assets/images/commit3.png)

## Commit 4 Reflection notes

The handle_connection function is updated once more, except this time it is used to simulate a slow request. A match statement is used to determine the appropriate status_line and filename based on the content of the request_line. If the request line corresponds to a GET request for the root ("/") path in HTTP/1.1, indicating a request for the "hello.html" file, the status_line is set to "HTTP/1.1 200 OK", and the filename to "hello.html". Additionally, if the request line corresponds to a GET request for the "/sleep" path in HTTP/1.1, indicating a request to simulate a delay, a 10-second delay is introduced using thread::sleep(Duration::from_secs(10)). After the delay, the status_line remains "HTTP/1.1 200 OK", and the filename remains "hello.html". If the requested path does not match either the root or the "/sleep" path, indicating a not-found scenario, the status_line is set to "HTTP/1.1 404 NOT FOUND", and the filename to "404.html".

## Commit 5 Reflection notes

In this last milestone we are told to use a Multithread server using Threadpool. It was required that we upadated the main() function in main.rs and create a new file lib.rs in order to explore multithreading for our server since it previously ran single-threaded.

In main.rs, The updated main function sets up a TCP listener on the local address 127.0.0.1 on port 7878 using TcpListener::bind(), and a thread pool with a capacity of 4 threads using ThreadPool::new(4). It then enters a loop to handle incoming connections using listener.incoming(). For each incoming connection, it extracts the stream, unwraps it, and then submits a closure to the thread pool using pool.execute(). This closure invokes the handle_connection function, passing the stream as an argument. This setup allows handling multiple connections concurrently by distributing the workload across multiple threads in the thread pool.

The code in lib.rs imports necessary modules and defines the main function, which sets up a TCP listener similar to the previous explanation. It also imports ThreadPool from the hello module. Inside the main function, it creates a thread pool with a capacity of 4 threads. Then, it enters a loop to handle incoming connections similar to the main function in the first code snippet. For each incoming connection, it unwraps the stream and submits a closure to the thread pool. This closure invokes the handle_connection function, which processes the incoming TCP stream. Depending on the request line, it may introduce a delay for specific requests, reads the content of the requested file, generates an appropriate HTTP response, and writes the response back to the TCP stream. This setup allows handling multiple connections concurrently with asynchronous processing using the thread pool.
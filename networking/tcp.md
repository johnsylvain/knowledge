# tcp/ip

> Transmission control protocol

It dictates how information should be packaged (in packets), sent, and received.

## common tcp/ip protocols

* **HTTP**: between a web client and web server
* **HTTPS**: secure version of http
* **FTP**: for sending data direction from one computer to another

## a C program

### client

```c
#include <stdio.h>
#include <stdlib.h>

#include <sys/types.h>
#include <sys/socket.h>

#include <netinet/in.h>
#include <unistd.h>

int main() {
  int network_socket;
  network_socket = socket(AF_INET, SOCK_STREAM, 0);

  // specify an address for the socket
  struct sockaddr_in server_address;
  server_address.sin_family = AF_INET;
  server_address.sin_port = htons(9002);
  server_address.sin_addr.s_addr = INADDR_ANY;

  int connection_status = connect(
    network_socket,
    (struct sockaddr *) &server_address,
    sizeof(server_address)
  );
  
  // check for error with the connection
  if (connection_status == -1) {
    printf("There was an error making a connection to the socket");
  }

  // recieve data from the server
  char server_response[256];
  recv(network_socket, &server_response, sizeof(server_response), 0);

  // print out the server's response
  printf("Ther server sent the data. %s", server_response);

  // and the close the socket
  close(network_socket);

  return 0;
}
```

### server

```c
#include <stdio.h>
#include <stdlib.h>

#include <sys/types.h>
#include <sys/socket.h>

#include <netinet/in.h>
#include <unistd.h>

int main() {
  char server_message[256] = "You have reached the server!";

  int server_socket;
  server_socket = socket(AF_INET, SOCK_STREAM, 0);

  struct sockaddr_in server_address;
  server_address.sin_family = AF_INET;
  server_address.sin_port = htons(9002);
  server_address.sin_addr.s_addr = INADDR_ANY;

  bind(
    server_socket,
    (struct sockaddr*) &server_address,
    sizeof(server_address)
  );

  listen(server_socket, 5);

  int client_socket;
  client_socket = accept(server_socket, NULL, NULL);

  send(client_socket, server_message, sizeof(server_message), 0);

  close(server_socket);

  return 0;
}
```

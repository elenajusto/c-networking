# C-Networking
Try to learn networking by making web server in C.

### Important functions
```sh
getaddrinfo()
```

```sh
memset()
```

### Layers in Networking
- Network Access -> Physical layer
- Internet -> Sending/Routing of data packets
- Host to Host -> Network programming, protocols, connections
- Process/Application -> Presented to user

Programming occurs in the Host to Host Layer.

### Internet Protocol (IP)
- IPv4
- 32 bit address
- 0.0.0.0 to 255.255.255.255

- IPv6
- 128 bit address
- 3*10^38 addresses

- Code is usually specific to IPv4 or IPv6. However you can code for both using a "dual stack configuration".

### IP Protocl Suite
- TCP (Transmission Control Protocol)
- Streaming
- Data sent in sequence

- UDP (User Datagram Protocl)
- Connectionless

- TCP or UDP must be set when writing networking code.

### Network Adresses and Ports
IP Addresses reserved for local networks tend to fall into the following bins:
- 10.0.0.0 to 10.255.255.255 (the ten's)
- 172.16.0.0 to 172.31.255.255 (the 172's)
- 192.168.0.0 to 192.168.255.255 (the 192.168's)

Local router assigns each device connected to it an address from one of the above ranges -> Device's "public address".

Devices can have both an IPv4 and IPv6 address.

Subnet Masks:
- IPv4 have a /
-> 192.168.0.0/16
- IPv6 have a %
-> fe80::8cc1%9

Ports
- 16 bits wise
- Endpoint for a device that allows communication
- Port 1 to Port 1023 -> System ports
- Port 1024 to 49151 -> Registered ports
- Port 49152 to 65535 -> Dynamic, Private or Ephemeral

View list of ports with
```sh
cd ~/etc

less services
```

### Client/Server roles
Running a program on a network enters the realm of "Distributed Applications"

Server: Sends data to computer over the internet

Client: Web browser accepts data and displays webpage

### Socket Programming
- A socket is a communications endpoint that works like a file handler
- Linux can use standard I/O functions for socket communications
- Similar to file handlers, you close socket when done

- Windows uses Winsock which works differently to Linux and Mac
- Network code is not compatible between operating systems

- TCP and UDP have different ways of being written
- Clients and servers have different ways of being written

#### Writing a TCP client
1. Get host information `getaddrinfo()`
2. Create a socker `socket()`
3. Connect socket with the host `connect()`
4. Use send/recieve functions `send()` and `recv`
5. Close the socket `close()`

#### Writing a TCP server
1. Configure the server's address, port and protocol `getaddrinfo()`
2. Create communications endpoint `socker()`
3. Bind the socket to address and port `bind()`
4. Listen for incoming connections `listen()`
5. Establish link to client `accept()`
6. Exchange data with client `send()` and `recv()`
7. Close endpoint `close()`

#### Writing a UDP client
1. `getaddrinfo()`
2. `socket()`
3. `sendto()` and `recvfrom()` - Notice there is no need to establish a connction. UDP is a connectionless protocol.
4. `close()`

#### Writing a UDP server
1. `getaddrinfo()`
2. `socket()`
3. `bind()`
4. `recvfrom()` and `sendto()`
5. `close()`

### Role of the `getaddrinfo()` function
- Used to setup and configure a connection
- Used to setup servers and clients
- Compatible with TCP and UDP

```C
int getaddrinfo(const char *hostname, const char *servname, const struct addrinfo *hints, struct addrinfo **res);
```

Where:
`*hostname` is a string representing the network host, name or IP address
- String such as `yahoo.com`
- IPv4 or IPv6 address such as `127.0.0.1`
- If zero or null, local system is used `0` or `NULL`

`*servname` string representing the service or port
- Is a service
- Port number such as `21`
- Srevice name such as `FTP`
- Stored in `/etc/services` file

`*hints` a structure that configures the connection
- Init to `0` before members are assigned to prevent garbage memory items from coming in
- Members are set to configure host or client

`**res` a structure that stores the results
- Filled with information about the connection
- `freeaddrinfo()` releases memory used by `*res` structure

#### The `struct addrinfo` structure
```C
struct addrinfo {
    int ai_flags;             /* input flags */
    int ai_family;            /* socket protocol family */
    int ai_socketype;         /* socket type */
    int ai_protocol;          /* protocl for socket */
    socketlen_t ai_addrlen;   /* socket address length */
    struct sockaddr *ai_addr; /* socket address */
    char *ai_canonname;       /* service name */
    struct addrinfo *ai_next; /* next item in list */
}
```
Computer Networking
===================

Why study computer networking in a distributed systems class?

1. A distributed system relies on a network for communication. The performance of the network has a direct effect on the performance of the application.

2. The Internet is a distributed system and the underlying principles and algorithms are applicable to building application-layer distributed systems.

# The Internet

The Internet is a "network of networks". It is a hierarchical system that must take messages and route them from one host to another. 

Inside of the Internet are a series of routers and switches that forward messages in a best-effort manner. The core of the Internet makes no guarnatees about whether a message will actually arrive at its destination. The Internet also uses packet switching. Each packet sent into the network is routed independently, so messages sent from one host to another can arrive out of order.

# Layered Model

If you’ve previously taken a networking class you may have studied the OSI (Open Systems Interconnection) model. The ISO (International Organization for Standardization) developed this seven-layer model that 

> characterizes and standardizes the communication functions of a telecommunication or computing system without regard to their underlying internal structure and technology. - Wikipedia

![http://www.tech-faq.com/wp-content/uploads/2009/01/osimodel.png](http://www.tech-faq.com/wp-content/uploads/2009/01/osimodel.png)

Like with object-oriented code design, or the service oriented architecture model, a layered protocol design provides a modular way to describe the functions required of a network and how each layer will communicate, and then allows different implementations of each layer. A new transport protocol could be deployed in the network stack without affecting the routing protocol used, for example. 

![https://upload.wikimedia.org/wikipedia/commons/thumb/c/c4/IP_stack_connections.svg/490px-IP_stack_connections.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c4/IP_stack_connections.svg/490px-IP_stack_connections.svg.png)

The Internet protocol stack, on the other hand, essentially folds the functionality of the Presentation and Session layers into the Application.

![https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/UDP_encapsulation.svg/800px-UDP_encapsulation.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/UDP_encapsulation.svg/800px-UDP_encapsulation.svg.png)

When a message is sent, each layer will process the message, perhaps break it up into smaller pieces, and add its own header before passing it to the layer below. The transport layer takes an application-layer message and adds a transport-layer header to create a segment. The network layer takes a transport-layer segment and adds a network-layer header to produce a network-layer datagram. The link layer takes the datagram and adds a link-layer header to produce a link-layer frame. As the data traverses the network, each layer will strip off the appropriate header and pass the resulting message to the layer above. The routers in the core of the internet only go as far as the network layer to determine the next hop for the packet. 


# Application Layer

The application developer writes code that executes at the application layer. A single host may run multiple applications and an application-layer protocol defines the communication between these processes communicating across the network. HTTP is one example of an application-layer protocol. SMTP and FTP are two other examples.

The application layer communicates with the transport layer via a socket. When the transport layer has a message to deliver to the application layer it determines which socket to use based on the port number specified in the message. When an application creates a socket it determines which transport layer protocol to use.

## HTTP

The HyperText Transfer Protocol was originally used for communication between web clients and servers. Today, it is used in many ways beyond just simple web browsing, for example as the communication mechanism between backend microservices.

Recall that for most web requests the client will request a web page, then request several additional resources that appear on the page, for example images. HTTP 1.0 required the client to open a new connection for every request. A big change in HTTP 1.1 was that it allowed persistent connections, so the same connection could be reused to make multiple requests. Because of the way that TCP operates, this can result in significant performance gains.

HTTP/2 is the latest iteration of HTTP. As noted on [Akami's website](https://http2.akamai.com/), there are several differences that improve performance in the latest version.

> Multiplexing and concurrency: Several requests can be sent in rapid succession on the same TCP connection, and responses can be received out of order - eliminating the need for multiple connections between the client and the server
>
Stream dependencies: the client can indicate to the server which of the resources are more important
than the others
>
Header compression: HTTP header size is drastically reduced
>
Server push: The server can send resources the client has not yet requested

## WebSockets

The traditional client-server web model assumes that the client initiates the connection and the server is stateless. That isn't to say that the server doesn't keep state about clients that may be retrieved based upon a cookie or token, however there is no state associated with the specific request. This becomes a limiting factor in many applications, for example a home automation system.

WebSockets are a new(ish) protocol that allow a client and server to maintain a persistent connection over a long period of time. The client uses HTTP to make the initial connection to the server, but specifies a header that indicates it would like to upgrade to websockets. If the server is able, it will then upgrade the socket and maintain a long-running TCP connection with the client. The benefit of this model is that the server may push content to the client, at the cost of maintaining state about all open connections on the server side.

# Transport Layer

The transport layer maintains an end-to-end connection between two hosts. The two transport-layer protocols most used in the Internet are TCP and UDP. The Transmission Control Protocol is used for any application that requires reliability from the transport layer. The User Datagram Protocol is unreliable and provides little functionality over raw IP. It is used, however, for applications such as real-time streaming where resending old packets is not helpful for the application. It may also be used if the application developer wants to provide a custom reliability mechanism.

## TCP 

TCP is a connection-oriented, reliable transport protocol. Among other things, TCP provides reliability, flow control, and congestion control.

![https://fthmb.tqn.com/O8I6Dt9t5xVrw3UOVnbBUPdKjGs=/768x0/filters:no_upscale()/about/tcp-header-56a1adc85f9b58b7d0c1a24f.png](https://fthmb.tqn.com/O8I6Dt9t5xVrw3UOVnbBUPdKjGs=/768x0/filters:no_upscale()/about/tcp-header-56a1adc85f9b58b7d0c1a24f.png) 

### Reliability

TCP provides reliable message delivery by using acknowledgements, timeouts and retransmissions. When TCP sends a message it sets a timer to indicate the message is outstanding. If the timer reaches a timeout interval before an acknowledgement is received for the message then the message is resent. TCP keeps an estimate of the expected round trip time (RTT), the time for a message to reach the receiver and for the receiver's ACK to reach the sender, and uses that estimate to set the timeout interval. 

### Flow Control

The receive window is a value provided by the receiver to indicate how much data it is able to receive and process. This prevents the sender from overwhelming the receiver.

### Congestion Control


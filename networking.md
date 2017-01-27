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

The application developer writes code that executes at the application layer. A single host may run multiple applications and an application-layer protocol defines the communication between these processes communicating across the networking. HTTP is one example of an application-layer protocol. SMTP and FTP are two other examples.

The application layer communicates with the transport layer via a socket. When the transport layer has a message to deliver to the application layer it determines which socket to use based on the port number specified in the message.

## HTTP

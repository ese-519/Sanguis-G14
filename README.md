# Sanguis-G14

# Utilizing middleware for information sharing in IoT applications

Internet of things comprise of two elements, “Internet” and “Things” where the “Internet” comprises of a global, scalable network infrastructure that enables device communication and “Things” comprises a heterogeneous set of physical devices/sensors that produce data at the user end. Internet of Things middleware is the software that serves as an interface between components of the IoT, making communication possible among elements that would not otherwise be capable. Our project involves development of a microfluidic medical device which, like most of the IoT based medical devices, requires services that can be used to send data over the internet for remote monitoring in a secure and reliable manner. We use the message-oriented middleware service MQTT to implement machine to machine communication in our application.

# What is MQTT?

MQTT (formerly the MQ Telemetry Transport) is a lightweight protocol that’s primarily designed for connecting power-constrained devices over low-bandwidth networks with support for multiple levels of Quality of Service as well. MQTT uses the publish and subscribe pattern to connect interested parties with each other. It does it by decoupling the sender (publisher) with the receiver (subscriber). The publisher sends a message to a central topic to the broker which has multiple subscribers waiting to receive the message. The publishers and subscribers are autonomous, which means that they do not need to know the presence of each other. Let us look into the various elements of a typical MQTT connection.

Client : Any publisher or subscriber that connects to the centralized broker over a network is considered to be the client. It’s important to note that there are servers and clients in MQTT. Both publishers and subscribers are called as clients since they connect to the centralized service. Clients often connect to the broker through libraries and SDKs. There are over a dozen libraries available for C, C++, Go, Java, C#, PHP, Python, Node.js, and Arduino.

Broker : The broker is the software that receives all the messages from the publishing clients and sends them to the subscribing clients. There are a lot of commercial implementations of the MQTT broker available online. Some of the popular ones are HiveMQ, CloudMQTT and AWS IoT.

Topic : A topic in MQTT is an endpoint to that the clients connect. It acts as the central distribution hub for publishing and subscribing messages. In MQTT, a topic is a well-known location for the publisher and subscriber. It’s created on the fly when either of the clients establish the connection with the broker. Topics are simple, hierarchical strings, encoded in UTF-8, delimited by a forward slash. For example, building1/room1/temperature and building1/room1/humidity are valid topic names.

Connection : MQTT can be utilized by clients based on TCP/IP. The standard port exposed by brokers is 1883, which is not a secure port. Those brokers who support TLS/SSL typically use port 8883. For secure communication, the clients and the broker rely on digital certificates. AWS IoT is one of the secure implementations of MQTT, which requires the clients to use the X.509 certificates.

# How to use MQTT in your IoT application?

MQTT can be used as means to send information from your device to other devices or portals such as a web page, where your data can be viewed. In order to do so, the following stepts have to be undertaken:
1) Setup an MQTT broker 
2) Setup a publishing client
3) Setup a subscribing client

In the upcoming sections, we would be expalining the above steps with respect to our application where the publishing client is our end device which is running on Raspberry Pi while the subscribing client is a webserver and an android application that is parsing the received data.

# 1) Setting up the MQTT broker

HiveMQ, CloudMQTT and AWS IoT are some of the popular brokers that are availbale for users for free. In our application we used the "CloudMQTT cute cat" version. It can be setup by following these steps:

> Go to https://www.cloudmqtt.com/

> Create an account and login into your account

> In order to use the broker, it is necessary to first create an instance. Create an instance

> Upon succesful creation, you will see all the instance details which constitutes the broker server address, the websocket port number, SSL port number etc

> In the particular instance, you have the option to create different users and different topics for the users. This can be done by going to the "Users and ACL" tab.

> Creating users with a username and password allows a secure connection that is based on authentication.Make sure to give the user both read and write access if both data writing and reading is happening based on the same credentials.

> The broker is now set! Note down the server address, the port number and the user details for further use.

# 2) Setting up the Publisher on Raspberry Pi using Python

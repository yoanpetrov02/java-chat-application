# Instant messaging chat application

## Table of contents

* [Installation Guide](#installation)

## Overview

An instant messaging chat application, written in Java 16 and JavaFX 19.\
Users can connect to the server, see who is online and send messages to each other.\
\
![281410014-eb41db2c-305f-484a-a8df-582ea1e84c99](https://github.com/yoanpetrov02/java-chat-application/assets/87146784/8c377978-3d53-4804-b135-d30eee9b2c2b)

<a name="installation"></a>
## Installation guide

### Windows

Navigate to a directory where you want to install the system. Open cmd and cd to that directory, for example:

```
cd C:\test\java-chat-application
```
Then, clone the repository:

```
git clone https://github.com/yoanpetrov02/java-chat-application.git
```

Afterwards, use the provided maven wrapper if you don't have Maven installed to build the project:

```
mvnw clean install
```

If you have Maven **3.9.2** installed, you can instead just use the mvn command:

```
mvn clean install
```

After building the project, open it with an IDE and launch the server application by navigating to the `ChatServerRunner` class in the `Chat-Server` module.\
Upon launching the server, you will be asked to provide a hostname and a port:\
\
![image](https://github.com/yoanpetrov02/java-chat-application/assets/87146784/16518b1e-137f-4593-9b77-0b9bb9c6d157)\
\
To run it locally, just type `localhost` in the Hostname field. Choose a free port and if everything is ok, the application will start. You will see this window:\
\
![image](https://github.com/yoanpetrov02/java-chat-application/assets/87146784/ed7df621-a48d-446e-ad70-64a21af2d6af)\
\
Here, you can start/stop the server and see any messages that appear.\
\
Whenever the server is running, clients can connect. Launch a client by navigating to the `ChatClientRunner` class in the `Chat-Client` module.\
Upon launching it, you will be asked to provide a hostname, a port and your username:\
\
![image](https://github.com/yoanpetrov02/java-chat-application/assets/87146784/d9d50fe3-d36a-4ba6-84a6-8a345d791ee1)\
\
In order to be able to connect to a running server, the hostname and the port need to match.\
Whenever the main app launches, you will see the following window:\
\
![image](https://github.com/yoanpetrov02/java-chat-application/assets/87146784/413ade61-8706-4551-9f27-74c8ddc7b518)\
\
You can see the online and offline users. Whenever you select a user, a chat box opens and you can send messages to the selected user.\
\
\
This explains the main usage of the application and how to set it up. Alternative ways to launch the project includes:

- Navigating to the `target` folder of the `Chat-Server` and `Chat-Client` modules after building the project, and executing the following commands, respectively:
```
java -jar Chat-Server-1.0-SNAPSHOT-jar-with-dependencies.jar
```
```
java -jar Chat-Client-1.0-SNAPSHOT-jar-with-dependencies.jar
```
Note that you can rename those jars to whatever you want, they are named this way by Maven when it builds them.

- Downloading the pre-built jars from the Releases section and running them the same way:
```
java -jar Server.jar
```
```
java -jar Client.jar
```


## How it works

### Technologies

The application uses JavaFX for the desktop UI along with its MVC support.\
The used architecture is Client-Server, as it allows the needed functionality to be implemented in the easiest way. For example:

- A user sends a message to another user. Internally, the message is actually sent to the server, which sends it to the receiver. The server acts as a mediator between the clients for every message they send.

### Front end

The user interface was written purely in FXML using SceneBuilder 20.0.0.

- The application supports multiple interface languages, which is achieved using Java's `ResourceBundle`s.
- The currently supported languages are Bulgarian and English.
- The front end part of the application communicates with the back end parts through the controllers (MVC pattern).
- Both the server and the client sides have their own respective GUI application.

### Back end

#### Client-Server communication

The server is implemented using Java TCP sockets. The communication between it and the clients happens through those sockets. Whenever a client sends a message, they follow a format, which allows the server to parse the message and act accordingly. For example, whenever the client wants to send a message to another client, the client application sends a message of the following type:
```
userMessage [recepientName] [currentTime] [messageContent]
```

#### Storing users and chat histories

- The server stores the registered users in `registeredusers.dat`. This is a file, where a list of users is serialized each time a user registers.
- The chat histories are stored in files for each unique pair of users. Whenever a chat history is requested, the history is read from the file and sent over the network.
- The server logs activity in log files for debugging purposes. These can be found in `logs/server/latest.log`.


## Conclusion

There are many ways to improve this application, especially the back end:

- Improve text history loading, because if two users have a really long chat history, it can cause out of memory errors.
- No need for using the hashCode of a `ChatPair` to name the file uniquely, we can just take the two names, sort them so they always appear in the same order and use those two concatenated to name the file which contains the chat history. This makes debugging easier as the developer can just look up the exact history they need easily, just by reading the name of the file.
- Communication between the clients and the server can happen using serialization/deserialization of objects through object streams, which eliminates problems that occur with the current pattern-oriented messaging and many other smaller design enhancements.

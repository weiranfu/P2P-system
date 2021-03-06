# Peer-to-Peer with Centralized Index (P2P-CI) System

> A P2P-CI system in which peers can download RFC files from another active peer with TCP connection.

Internet protocol standards are defined in documents called “Requests for Comments” (RFCs).

There is a centralized server which keeps information about active peers and maintains an index of RFCs available at each active peer.

When peers join the P2P-CI system, it will register itself at the server and provide information about RFCs that it makes available to other peers.

When a peer wishes to download a specific RFC, it will get a list of other peers who have that RFC by requiring the central server. 

The TCP connection between a peer and central server is long-lived, and the TCP connection between peers is short-lived.

## Features

*  Join/Leave P2P System
*  Add RFCs to central server
*  Look up a RFC from central server
*  List all RFCs in P2P System
*  Download a RFC from other peer

## Build from source

### Prerequisites

#### Install Java & Maven

* JDK 8+
* Maven version 3.x

### Download the source

`git clone https://github.com/weiranfu/P2P-system.git`

`cd P2P-system/`

### Build the source

`mvn clean install`

## How to run

Run Server:

`$java -cp target/P2PSystem-1.0-SNAPSHOT.jar P2PSystem.P2PSystemServer`

Run Client:

`$java -cp target/P2PSystem-1.0-SNAPSHOT.jar P2PSystem.P2PSystemClient`

## RFC files

* Local RFCs are stored under `/localRFCs/`
* RFCs downloaded from other peers are stored under `/downloadedRFCs/`
* The format of RFC name is `rfcxxx.txt`

## Protocols

The protocols used in P2P-CI system is a simplified version of HTTP protocol.

* Protocol among peers:
   
   query:
   ```
      method <sp> RFC number <sp> version <cr> <lf> 
      header field name <sp> value <cr> <lf>
      header field name <sp> value <cr> <lf>
      ...
      <cr> <lf>
   ```
   response:
   ```
      version <sp> status code <sp> phrase <cr> <lf> 
      header field name <sp> value <cr> <lf>
      header field name <sp> value <cr> <lf>
      ...
      <cr> <lf> 
      data
      <cr> <lf>
   ```
* Protocol between server and peer:
   
    query:
    ```
      method <sp> RFC number <sp> version <cr> <lf> 
      header field name <sp> value <cr> <lf>
      header field name <sp> value <cr> <lf>
      ...
      <cr> <lf>
    ```
    response:
    ```
      version <sp> status code <sp> phrase <cr> <lf>
      <cr> <lf>
      RFC number <sp> RFC title <sp> hostname <sp> upload port number<cr><lf> 
      RFC number <sp> RFC title <sp> hostname <sp> upload port number<cr><lf>
      ...
      <cr><lf>
    ```
   
  > Four methods are supported\
  > • ADD, to add a locally available RFC to the server’s index\
  > • LOOKUP, to find peers that have the specified RFC\
  > • LIST, to request the whole index of RFCs from the server\
  > • GET, to download a specific RFC from other peer

  > Four status codes are supported\
  > • 200 OK\
  > • 400 Bad Request\
  > • 404 Not Found\
  > • 505 P2P-CI Version Not Supported
 
## License

MIT
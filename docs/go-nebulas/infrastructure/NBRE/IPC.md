# IPC
IPC is the messenger for NEB and NBRE interaction.

### Features
IPC adopts shared memoty to communicate between NEB and NBRE to improve performance.
There are two sub-threads, a server and a client, inside IPC. The server listens for the NEB request, and the client listens for the NBRE result. Also, there is communication interaction between the two threads.

### Framework
The framework of IPC is shown as below.

![](https://github.com/nebulasio/nebdocs/blob/master/docs/resources/NBRE-IPC.png)

1. NEB calls a function, and the server receives the request and sends it to the client.

2. The client sends the request to NBRE.

3. NBRE runs the corresponding program and returns the result to the client, the client sends the result to the server.

4. The server returns the result to the NEB.
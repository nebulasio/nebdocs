NBRE Design Doc
===============

NBRE (Nebulas Runtiome Environment) is the Nebulas chain execution environment.
Its framework is shown as follows.

.. |image0| image:: ../../resources/NBRE-Overview.png

When an external interface function and algorithm need to be called, the NEB submits an execution request to the NBRE.
Then, the NBRE executes the corresponding program and returns the result to the NEB, and finally the NEB returns the result to the outside.

The specfic execution process is shown in the above figure, and the corresponding details are as follows:

1. The NEB submits an execution request to the NBRE and transmits it to the IPC server.

2. The IPC passes the execution request to the ir warden via the client.

3. ir warden parses the request, including the name and version, and passes the parsed information to the ir version manger.

4. The ir version queries the corresponding program in the NBRE database.

5. If there exits the corresponding program in the NBRE database, the NBRE database returns the programs to the ir version manger; if not, it returns no corresponding program queried.

6. If the ir version manger does not query the corresponding program in the NBRE database, it will continue to query the program in the NEB database.

7. ir version manger passes the corresponding program to the jit driver.

8. The jit driver parses the corresponding program and passes it to the jit engine.

9. jit engine executes the program and returns the result to the jit driver.

10. The jit driver returns the result to the ir version manger.

11. 12-13, the result is returned to NEB through ir warden and IPC, and finally returned to the outside.
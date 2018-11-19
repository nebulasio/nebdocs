JIT
===
JIT is a concurrent virtual machine based on LLVM, which runs ir programs providing algorithms and interfaces for NBRE.
It is the key of the dynamic update for NBRE.

Features
--------

**Dynamic update**

The dynamic update in NBRE contains two respects:
- NBRE's own dynamic update
- NBRE's new feature interfaces

NBRE's updates are performed by adding algorithms and interface programs to the database. When a new function is updated or called, the corresponding program will be loaded into the JIT in the database.

**Concurrent virtual machine**

To improve performace, JIT is implemented based on a concurrent virtual machine mechanism.
When one interface is called, the JIT first queries whether the corresponding program has been loaded.
If the programs is loaded, sets its execution count to be 1800; otherwise, loads the program from database and sets its execution count to be 1801.
Then runs the corresponding progrm.
At regular intervals, the JIT decrements the corresponding count of each loaded function by one and releases the program with a count when its count less than zero.

Framework
---------
The JIT framework is shown as below.

.. image:: ../../../resources/NBRE-JIT.png

1. One interface is requested from outside.
2. JIT queries the corresponding function program from the database.
3. JIT loads the corresponding program.
4. Runs the program.
5. Returns the result.
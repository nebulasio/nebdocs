# NBRE框架
NBRE（Nebulas Runtime Environment）是星云链执行环境，其总框架如下图所示：
![](https://github.com/nebulasio/nebdocs/blob/zh-CN/docs/resources/NBRE-Overview.png)
当外部需要调用一个接口功能和算法时，NEB向NBRE提出执行申请，NBRE执行对应程序并返回结果给NEB，最后NEB返回结果给外部。
具体执行流程如图中标号所示，对应详细内容如下：
1. NEB向NBRE提出执行请求，传递给IPC的server；
2. IPC将执行请求通过client传递给ir warden；
3. ir warden将请求进行解析，查看执行程序的名称、版本等信息，将解析后的信息传递给ir版本管理器；
4. ir版本管理器在nbre的数据库中查询对应程序；
5. 如果数据中有对应程序，则返回给ir版本管理器；如果没有，则返回没有查询到对应程序；
6. 如果ir版本管理器没有在nbre数据中查询到对应程序，其将会在neb数据库中继续查询并读取对应数据库；
7. ir版本管理器将对应程序传递给jit driver；
8. jit driver将对应程序进行解析，并传递给jit engine；
9. jit engine运行程序，并返回结果给jit driver；
10. jit driver将结果返回给ir版本管理器；
11. 12-13,结果通过ir warden和IPC返回给NEB，最后返回给外部。

# 一. C/S架构


```cpp
/*

     * Subscribe a list of messages so that the messages can be received

     *      when the service broadcast a message.

     * The method doesn't block and is asynchronous.

     *

     *   service                                  client

     *     |<------------subscribe(list)------------|

     *     |---------broadcast(event1)------------->|

     *     |                            /-----------|

     *     |                       onBroadcast()    |

     *     |                            \---------->|

     *     |---------broadcast(event2)------------->|

     *     |                            /-----------|

     *     |                       onBroadcast()    |

     *     |                            \---------->|

     *

     * @iparam list: list of messages to be subscribed

     */
```

## Client 

接收函数：onBroadcast

发送函数：send,transact


## Server

发送函数：broadcast
>服务端的发生机制是广播，服务端会把数据发给所有连接的客户端，客户端根据订阅机制判断是不是自己模块的数据，如果是则接受

接收函数：onTransact



![[Pasted image 20231010100044.png]]


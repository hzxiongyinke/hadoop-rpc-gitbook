# RPC调用完整流程时序图

```mermaid
sequenceDiagram
    participant Client as 客户端应用
    participant Stub as 客户端存根
    participant Serializer as 序列化器
    participant Network as 网络传输层
    participant Dispatcher as 服务端分发器
    participant Deserializer as 反序列化器
    participant Service as 服务端实现
    
    Note over Client, Service: RPC调用完整流程
    
    Client->>Stub: 1. 调用本地方法
    Note right of Client: 像调用本地函数一样
    
    Stub->>Serializer: 2. 序列化请求参数
    Note right of Stub: 方法名、参数类型、参数值
    
    Serializer->>Network: 3. 发送网络请求
    Note right of Serializer: 转换为字节流
    
    Network-->>Network: 4. 网络传输
    Note over Network: TCP/UDP协议传输<br/>可能出现延迟、丢包等问题
    
    Network->>Dispatcher: 5. 接收请求
    Note left of Dispatcher: 服务端接收字节流
    
    Dispatcher->>Deserializer: 6. 反序列化请求
    Note left of Dispatcher: 解析方法名和参数
    
    Deserializer->>Service: 7. 调用实际服务
    Note left of Deserializer: 根据方法名路由到具体实现
    
    Service->>Service: 8. 执行业务逻辑
    Note over Service: 处理实际业务请求
    
    Service->>Deserializer: 9. 返回执行结果
    Note right of Service: 业务处理完成
    
    Deserializer->>Dispatcher: 10. 序列化响应
    Note right of Deserializer: 将结果转换为字节流
    
    Dispatcher->>Network: 11. 发送响应
    Note right of Dispatcher: 通过网络返回结果
    
    Network-->>Network: 12. 网络传输
    Note over Network: 响应数据传输
    
    Network->>Serializer: 13. 接收响应
    Note left of Serializer: 客户端接收响应
    
    Serializer->>Stub: 14. 反序列化响应
    Note left of Serializer: 恢复为对象形式
    
    Stub->>Client: 15. 返回结果
    Note left of Stub: 就像本地函数返回一样
    
    Note over Client, Service: 整个过程对客户端透明
```

## 图表说明

这个时序图展示了一个完整的RPC调用过程，从客户端发起调用到服务端返回结果的全部步骤：

### 关键步骤解析

1. **客户端调用** - 应用程序调用看似本地的方法
2. **参数序列化** - 将方法调用转换为可传输的格式
3. **网络传输** - 通过网络协议发送请求
4. **服务端处理** - 接收、解析并执行实际业务逻辑
5. **结果返回** - 将执行结果通过相同路径返回客户端

### 透明性体现

整个过程对客户端应用是透明的，开发者只需要像调用本地函数一样使用RPC服务，底层的网络通信、序列化等复杂性都被RPC框架屏蔽了。

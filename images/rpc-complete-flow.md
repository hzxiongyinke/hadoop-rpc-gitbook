# RPC调用完整流程时序图

```mermaid
sequenceDiagram
    participant Client as 客户端应用
    participant Proxy as RPC代理
    participant RpcEngine as RpcEngine
    participant ClientClass as Client类
    participant Connection as Connection
    participant Listener as Listener线程
    participant Reader as Reader线程
    participant CallQueue as CallQueueManager
    participant Handler as Handler线程
    participant Service as 服务实现
    participant Responder as Responder线程
    
    Note over Client, Responder: RPC调用完整生命周期
    
    %% 1. 代理创建阶段
    rect rgb(230, 240, 255)
        Note over Client, RpcEngine: 1. 代理创建阶段
        Client->>RpcEngine: getProxy(protocol, address, conf)
        RpcEngine->>Proxy: 创建动态代理对象
        Proxy-->>Client: 返回代理实例
    end
    
    %% 2. 方法调用阶段
    rect rgb(240, 255, 240)
        Note over Client, Connection: 2. 方法调用发起阶段
        Client->>Proxy: 调用业务方法
        Proxy->>RpcEngine: invoke(method, args)
        RpcEngine->>ClientClass: call(rpcRequest)
        ClientClass->>ClientClass: 创建Call对象
        ClientClass->>Connection: getConnection(connectionId)
        
        alt 连接不存在
            Connection->>Connection: 建立新连接
            Connection->>Connection: SASL认证
        end
    end
    
    %% 3. 请求发送阶段
    rect rgb(255, 245, 230)
        Note over ClientClass, Connection: 3. 请求发送阶段
        ClientClass->>Connection: sendRpcRequest(call)
        Connection->>Connection: 序列化请求头
        Connection->>Connection: 序列化请求体
        Connection->>Connection: 通过Socket发送
    end
    
    %% 4. 服务端接收阶段
    rect rgb(255, 230, 240)
        Note over Listener, Reader: 4. 服务端接收阶段
        Connection-->>Listener: 网络数据到达
        Listener->>Listener: accept()新连接
        Listener->>Reader: 分发连接到Reader
        Reader->>Reader: 从Socket读取数据
        Reader->>Reader: 解析RPC请求头
        Reader->>Reader: 读取完整请求体
    end
    
    %% 5. 请求队列阶段
    rect rgb(240, 230, 255)
        Note over Reader, Handler: 5. 请求队列阶段
        Reader->>CallQueue: 创建Call对象并入队
        CallQueue->>CallQueue: 根据优先级调度
        Handler->>CallQueue: 从队列取出Call
    end
    
    %% 6. 业务处理阶段
    rect rgb(230, 255, 230)
        Note over Handler, Service: 6. 业务处理阶段
        Handler->>Handler: 反序列化请求参数
        Handler->>Service: 调用具体业务方法
        Service->>Service: 执行业务逻辑
        Service-->>Handler: 返回处理结果
        Handler->>Handler: 序列化响应结果
    end
    
    %% 7. 响应发送阶段
    rect rgb(255, 240, 230)
        Note over Handler, Responder: 7. 响应发送阶段
        Handler->>Responder: 准备响应数据
        Responder->>Responder: 管理写操作队列
        Responder->>Responder: 通过Socket发送响应
    end
    
    %% 8. 客户端接收阶段
    rect rgb(230, 245, 255)
        Note over Connection, Client: 8. 客户端接收阶段
        Responder-->>Connection: 网络响应数据
        Connection->>Connection: 读取响应头
        Connection->>Connection: 读取响应体
        Connection->>Connection: 反序列化响应
        Connection->>ClientClass: 完成CompletableFuture
        ClientClass->>Proxy: 返回结果
        Proxy-->>Client: 返回业务结果
    end
    
    %% 异常处理流程
    rect rgb(255, 230, 230)
        Note over ClientClass, Handler: 异常处理机制
        alt 网络异常
            Connection->>ClientClass: 抛出IOException
            ClientClass->>ClientClass: 重试机制
        else 业务异常
            Service->>Handler: 抛出业务异常
            Handler->>Responder: 序列化异常信息
        else 超时异常
            ClientClass->>ClientClass: 超时检测
            ClientClass->>Connection: 取消请求
        end
    end
    
    %% 异步处理流程（可选）
    rect rgb(240, 255, 255)
        Note over Handler, Responder: 异步处理优化（Router Federation）
        Handler->>Handler: Async-Handler处理
        Handler->>Connection: 放入connection.calls
        Connection->>Connection: 异步处理响应
        Connection->>Responder: Async-Responder处理
        
        alt 需要重试
            Responder->>Connection: 重新添加到calls
        else 处理完成
            Responder->>Responder: 放入ResponseQueue
        end
    end
```

## 流程关键节点说明

### 🚀 **代理创建阶段**
- **RpcEngine.getProxy()**: 创建动态代理，封装网络通信细节
- **协议绑定**: 将业务接口与RPC传输协议绑定
- **配置初始化**: 设置连接参数、超时时间、重试策略

### 📤 **请求发送阶段**
- **Call对象创建**: 包含唯一ID、重试计数、CompletableFuture
- **连接管理**: 复用现有连接或建立新连接
- **序列化处理**: 将Java对象转换为网络传输格式
- **SASL认证**: 安全认证和授权检查

### 🔄 **服务端处理阶段**
- **多线程架构**: Listener→Reader→Handler→Responder的流水线处理
- **队列调度**: 支持优先级、公平性等多种调度策略
- **业务调用**: 通过反射机制调用具体的服务实现
- **资源管理**: 连接池、线程池的智能管理

### 📥 **响应处理阶段**
- **异步响应**: CompletableFuture支持非阻塞调用
- **错误处理**: 网络异常、业务异常、超时异常的分类处理
- **性能监控**: 详细的调用指标和性能数据收集

### ⚡ **异步优化机制**
- **Async-Handler**: 非阻塞的请求处理
- **nameservice隔离**: 多租户环境下的资源隔离
- **智能重试**: 基于异常类型的重试策略
- **响应队列**: 高效的响应管理机制

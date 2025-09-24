```mermaid
sequenceDiagram
    participant Client as 客户端
    participant Socket as Socket连接
    participant Server as 服务端
    participant SASL as SASL认证器
    participant UGI as UserGroupInformation
    participant KDC as Kerberos KDC

    Note over Client, KDC: Hadoop RPC协议握手与认证流程

    %% 1. 连接建立阶段
    Note over Client, Server: === 1. TCP连接建立阶段 ===
    Client->>Socket: 创建Socket连接
    Socket->>Server: TCP三次握手
    Server-->>Socket: 连接建立成功
    Socket-->>Client: 连接就绪

    %% 2. 连接头部交换
    Note over Client, Server: === 2. 连接头部交换阶段 ===
    Client->>Client: 构造连接头部
    Note right of Client: 魔数"hrpc" + 版本号 + 服务类别 + 认证协议
    Client->>Server: 发送连接头部 (7字节)
    Server->>Server: 验证魔数和版本

    alt 版本不兼容
        Server-->>Client: FATAL_VERSION_MISMATCH
        Client->>Client: 抛出VersionMismatch异常
    else 版本兼容
        Server->>Server: 接受连接头部
    end

    %% 3. 认证协议协商
    Note over Client, Server: === 3. 认证协议协商阶段 ===
    Client->>UGI: 检查用户认证信息
    UGI-->>Client: 返回认证类型和凭据

    alt 启用安全认证
        Client->>Client: 选择SASL认证协议
        Note right of Client: 根据配置选择GSSAPI/DIGEST-MD5等
    else 简单认证
        Client->>Client: 选择SIMPLE认证协议
        Note right of Client: 仅传输用户名，无加密
    end

    Client->>Server: 发送认证协议选择
    Server->>Server: 验证协议支持性

    %% 4. SASL认证流程 (安全模式)
    Note over Client, KDC: === 4. SASL认证流程 (安全模式) ===

    alt Kerberos认证
        Client->>UGI: 获取Kerberos票据
        UGI->>KDC: 请求服务票据 (TGS)
        KDC-->>UGI: 返回服务票据
        UGI-->>Client: 提供Kerberos凭据

        Client->>SASL: 初始化SASL客户端
        SASL->>SASL: 创建GSSAPI机制

        loop SASL握手循环
            Client->>SASL: 生成认证令牌
            SASL-->>Client: 返回认证数据
            Client->>Server: 发送SASL认证数据
            Server->>Server: 验证认证数据
            Server-->>Client: 返回挑战或确认

            alt 认证成功
                Server->>Server: 建立安全上下文
                break 认证完成
            else 需要继续认证
                Note over Client, Server: 继续SASL握手
            end
        end

    else Token认证
        Client->>UGI: 获取Delegation Token
        UGI-->>Client: 返回Token凭据
        Client->>SASL: 使用DIGEST-MD5机制
        Client->>Server: 发送Token认证信息
        Server->>Server: 验证Token有效性
        Server-->>Client: 认证结果
    end

    %% 5. 连接上下文建立
    Note over Client, Server: === 5. 连接上下文建立阶段 ===
    Client->>Client: 构造连接上下文
    Note right of Client: 用户信息 + 协议名称 + 认证方法
    Client->>Server: 发送连接上下文 (writeConnectionContext)
    Server->>Server: 解析连接上下文
    Server->>Server: 注册连接到ConnectionManager
    Server->>Server: 分配Reader线程
    Server-->>Client: 连接上下文确认

    %% 6. 连接就绪状态
    Note over Client, Server: === 6. 连接就绪状态 ===
    Client->>Client: 连接状态设为ACTIVE
    Client->>Client: 启动响应读取线程
    Server->>Server: 连接加入活跃连接池
    Server->>Server: 开始监听RPC请求

    Note over Client, Server: 连接建立完成，可以进行RPC调用

    %% 7. 错误处理机制
    Note over Client, Server: === 7. 错误处理与重试机制 ===

    alt 连接超时
        Client->>Client: ConnectTimeoutException
        Client->>Client: 更新远程地址
        Client->>Client: 根据RetryPolicy重试
    else 认证失败
        Server-->>Client: 认证错误响应
        Client->>Client: 处理认证异常
        alt Kerberos重放攻击
            Client->>Client: 退避等待
            Client->>Client: 重新获取票据
        end
    else SASL连接失败
        Client->>Client: 特殊SASL错误处理
        Client->>Client: 可能涉及退避和重试
    end

    %% 8. 连接维护机制
    Note over Client, Server: === 8. 连接维护机制 ===

    loop 连接保活
        Client->>Server: 发送Ping消息
        Server-->>Client: Pong响应
        Note over Client, Server: 防止连接超时
    end

    alt 连接空闲超时
        Server->>Server: 检测连接空闲时间
        Server->>Server: 关闭空闲连接
        Server->>Client: 发送连接关闭通知
    end

    alt 客户端主动关闭
        Client->>Client: 调用close()方法
        Client->>Server: 发送连接关闭请求
        Server->>Server: 清理连接资源
        Server-->>Client: 关闭确认
    end
```

# 超时设置

BFE中有一些涉及超时的配置，包括：

+ BFE和客户端间通信的超时
+ BFE和后端实例间通信的超时

超时的设置，对于业务流量的处理非常重要。

## 客户端和BFE间通信的超时

### 定义

BFE和客户端间通信的超时包括：

+ **读用户请求超时**：从连接建立开始，到完整读取到来自客户端请求的HTTP Header
+ **读用户Body超时**: 从完成读取HTTP Header，到完成读取Body
+ **写响应超时**: 从发送Response开始，到将Response完全发送给客户端
+ **与用户长连接超时**：从上一个请求结束，到完成读取下一个请求的HTTP Header

![timeout between BFE and client](./timeout_BFE_client.png)

### 配置方法

+ **读用户请求超时**

  在[/conf/bfe.conf](https://github.com/bfenetworks/bfe/tree/master/conf)中统一配置，单位为秒。这个配置只能在程序重新启动时生效，无法热加载。

```
[Server]
...
# read timeout, in seconds
ClientReadTimeout = 60
...
```

+ **读用户Body超时**

  在[/conf/server_data_conf/cluster_conf.data](https://github.com/bfenetworks/bfe/blob/master/conf/server_data_conf/cluster_conf.data)中，针对各集群（Cluster）独立配置，单位为毫秒。

```
{
    "Version": "init version",
    "Config": {
   		"cluster_example": {
    		...
    	    "ClusterBasic": {
                "TimeoutReadClient": 30000,
                ...
            }
    	 }
    }
}
```

+ **写响应超时**

  在[/conf/server_data_conf/cluster_conf.data](https://github.com/bfenetworks/bfe/blob/master/conf/server_data_conf/cluster_conf.data)中，针对各集群（Cluster）独立配置，单位为毫秒。

```
{
    "Version": "init version",
    "Config": {
   		"cluster_example": {
    		...
    	    "ClusterBasic": {
                "TimeoutWriteClient": 60000,
                ...
            }
    	 }
    }
}
```

+ **与用户长连接超时**

  在[/conf/server_data_conf/cluster_conf.data](https://github.com/bfenetworks/bfe/blob/master/conf/server_data_conf/cluster_conf.data)中，针对各集群（Cluster）独立配置，单位为毫秒。

```
{
    "Version": "init version",
    "Config": {
   		"cluster_example": {
    		...
    	    "ClusterBasic": {
                "TimeoutReadClientAgain": 30000,
                ...
            }
    	 }
    }
}
```

## BFE和后端实例间通信的超时

### 定义

BFE和后端实例间通信的超时包括：

+ **连接后端超时**：从向后端发起建立连接，到建立连接完成
+ **读后端响应头部超时**：从BFE开始发送请求，到完成接收Response Header

![timeout between BFE and backend](./timeout_BFE_backend.png)

### 配置方法

+ **连接后端超时**

  在[/conf/server_data_conf/cluster_conf.data](https://github.com/bfenetworks/bfe/blob/master/conf/server_data_conf/cluster_conf.data)中，针对各集群（Cluster）独立配置，单位为毫秒。

```
{
    "Version": "init version",
    "Config": {
   		"cluster_example": {
    		...
    	    "BackendConf": {
                "TimeoutConnSrv": 2000,
                ...
            }
    	 }
    }
}
```

+ **读后端响应头部超时**

  在[/conf/server_data_conf/cluster_conf.data](https://github.com/bfenetworks/bfe/blob/master/conf/server_data_conf/cluster_conf.data)中，针对各集群（Cluster）独立配置，单位为毫秒。

```
{
    "Version": "init version",
    "Config": {
   		"cluster_example": {
    		...
    	    "BackendConf": {
                "TimeoutResponseHeader": 50000,
                ...
            }
    	 }
    }
}
```



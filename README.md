# skynet-cocos-creator-client
本项目是基于[cocos-creator-puremvc](https://github.com/ikerlin/cocos-creator-puremvc) 为基础 搭建的一个cocos creator 
可以与[skynet](https://github.com/cloudwu/skynet)通信的 基础架构项目。

### 技术栈
* typescript Version 3.0.1
* cocos-creator Version 2.0.1
* [puremvc](https://github.com/tekool/puremvc-typescript-standard-framework/tree/master/bin)
* [protobufjs](https://github.com/dcodeIO/protobuf.js)

### 安装
使用`npm install`


### script目录结构
* controller
    * bootstrap 需要初始化的MVC
    * commands 游戏内Command，增加Command基本都应该在这里
    * StartupCommand.ts 启动Command
* lib
    * puremvc
    * state-machine状态机
    * behavior3行为树
* model
    * GameProxy.ts ws数据代理
    * HttpProxy.ts http数据代理
    * GameProxy.ts 游戏数据代理示例
* view
    * component 视图组件
    * StartViewMediator.ts Start场景中介
* AppFacade.ts puremvc入口
* Start.ts 开始场景启动脚本，AppFacade在这里初始化

### 注意点
1. Command与Proxy、Mediator  
    Command管理应用程序的 Business Logic（业务逻辑），要协调Model 与视图状态。

    Model 通过使用 Proxy 来保证数据的完整性、一致性 。Proxy 集中程序的Domain Logic（域逻辑），并对外公布操作数据对象的API。它封装了所有对数据模型的操作，不管数据是客户端还是服务器端的。

    Mediator 和Proxy 可以提供一些操作接口让Command 调用来管理View Component 和Data Object ，同时对 Command隐藏具体操作的细节。

2. 一般一个Mediator（handleNotification方法）处理的Notification应该在4、5个之内。  
    还要注意的是，Mediator的职责应该要细分。如果处理的Notification很多，则意味着Mediator需要被拆分，在拆分后的子模块的Mediator里处理要比全部放在一起更好。

3. 应该避免Mediator与Proxy 直接交互。  
    通常应该遵从了这个规则，但实际上项目Mediator中不可避免需要获取Proxy数据，如果每次都通过一个Notification去获取数据，然后返回数据给Mediator，这样无形中增加了通信次数、带反馈数据的通信加重通信负担。所以可以适当是的在Mediator中facade.retrieveProxy获取Proxy然后拿到数据，而且从proxy直接拿数据，可以保证拿到最新数据。 
 4. 与skynet交互是 先http验证然后拿到 ws 相关所需的信息 再以 rpcId(大端4字节) + body 形式的数据结构

### 参考文档
* [PureMVC（AS3）剖析：实例](http://www.cnblogs.com/skynet/archive/2013/01/29/2881244.html)
* [i18n](http://docs.cocos.com/creator/manual/zh/advanced-topics/i18n.html)

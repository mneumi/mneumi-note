## IOC

### 背景

假设有一个订单查询服务（OrderService）和一个用户服务（UserService）

为了提供服务，还有数据库服务（DBService）和配置（ServerConfig）

如果使用传统的OOP开发，则实现伪代码如下

```typescript
import ServerConfig from "./serverConfig.ts"
import DBService from "./dbService.ts"

class OrderService {
    serverConfig: ServerConfig
    dbService: DBService
    
    constructor() {
        serverConfig = new ServerConfig()
        dbService = new DBService()
    }
    
    // 具体的业务代码
    // ...
}

class UserService {
    serverConfig: ServerConfig
    dbService: DBService
    
    constructor() {
        serverConfig = new ServerConfig()
        dbService = new DBService()
    }
    
    // 具体的业务代码
    // ...
}
```

### 存在的问题

传统的OOP开发模式，存在下面的问题

1. 每次都需要实例化ServerConfig和DBService，实例化的过程可能需要读取配置或者通过网络获取资源，这导致实例化比较麻烦和困难
2. 没有必要每次都实例化ServerConfig和DBService，实际的开发中应该只创建一个实例就好了（单例模式）
3. 创建函数修改起来很麻烦，如果一个类在程序中多处使用，则如果其构造函数参数发生更改，则需要修改大量的地方
4. 需要手动维护实例之间的依赖关系，程序规模大了就难以维护
5. 类的实例销毁很麻烦，为了释放实例拥有的资源，需要保证每个实例都按照顺序被销毁

### IOC的概念

为了解决传统OOP存在的问题，就需要由一个专门的东西来负责创建维护类的实例，这就是IOC这种开发模式

IOC全称是：Inversion of Control，即控制反转

* 控制：指的是控制类的实例化
* 反转：本来类的实例化应该由开发者来创建，但为了解决传统OOP面临的问题，则需要由IOC容器来进行实例化，即进行了反转

因为IOC又称为依赖注入（DI，Dependency Injection）



## AOP

### AOP概念

AOP的全称：Aspect Oriented Programming，即面向切面编程

AOP的功能：把系统分解为不同的关注点（切面），将代码注入到切面中，从而无侵入地增强类或函数的功能，将业务代码和系统功能解藕

### AOP使用实例

权限检验、日志记录是非常常见的功能，很多的功能中都需要集成这些功能

#### 传统开发模式

```typescript
import auth from "./auth.ts"
import log from "./log.ts"

function addOrder(user: User) {
    auth()
    log()
    
    // 业务代码
    ...
}
```

### 使用AOP模式

```ts
import authDecorator from "./auth.ts"
import logDecorator from "./log.ts"

@authDecorator()
@logDecorator()
function addOrder(user: User) {
    // 业务代码
    ...
}
```




## 软件架构模式的演进
![](https://upload-images.jianshu.io/upload_images/6271376-a9250d25647f182b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 微服务代码模型
![](https://upload-images.jianshu.io/upload_images/6271376-105c87964865686c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Interface（用户接口层）：主要存放用户接口层与前端交互、展示数据相关的代码。用来处理用户发送的Restful请求，解析用户输入的配置文件，并将数据传递给Application层
- Application（应用层）：主要存放应用层服务组合和编排相关的代码。应用服务和事件等代码会放在这一层目录里。
- Domain（领域层）：主要存放领域层核心业务逻辑相关的代码。聚合以及聚合内的实体、方法、领域服务和事件等代码会放在这一层目录里。
- Infrastructure（基础层）：主要存放基础资源服务相关的代码，为其他层提供通用技术能力、三方软件包、数据库服务、配置和基础资源服务的代码都会放在这一层目录里。

### 1、用户接口层
![](https://upload-images.jianshu.io/upload_images/6271376-6a30cac73468415e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Assembler：实现 DTO 与领域对象之间的相互转换和数据交换。一般来说 Assembler 与 DTO 总是一同出现。
- Dto：它是数据传输的载体，内部不存在任何业务逻辑，我们可以通过 DTO 把内部的领域对象与外界隔离。
- Facade：提供较粗粒度的调用接口，将用户请求委派给一个或多个应用服务进行处理

### 2、应用层
![](https://upload-images.jianshu.io/upload_images/6271376-86f7ff9a92417589.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Event（事件）：这层目录主要存放事件相关的代码。它包括两个子目录：publish 和 subscribe。前者主要存放事件发布相关代码，后者主要存放事件订阅相关代码（事件处理相关的核心业务逻辑在领域层实现）。
- Service（应用服务）：这层的服务是应用服务。应用服务会对多个领域服务或外部应用服务进行封装、编排和组合，对外提供粗粒度的服务。应用服务主要实现服务组合编排，是一段独立的业务逻辑。

### 3、领域层
![](https://upload-images.jianshu.io/upload_images/6271376-3736ce29d8b85537.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Aggregate（聚合）：它是聚合软件包的根目录，可以根据实际项目的聚合名称命名，比如权限聚合。在聚合内定义聚合根、实体和值对象以及领域服务之间的关系和边界。聚合内实现高内聚的业务逻辑，它的代码可以独立拆分为微服务。
- Entity（实体）：存放聚合根、实体、值对象以及工厂模式（Factory）相关代码。实体类采用充血模型，同一实体相关的业务逻辑都在实体类代码中实现。跨实体的业务逻辑代码在领域服务中实现。
- Event（事件）：存放事件实体以及与事件活动相关的业务逻辑代码。
- Service（领域服务）：一个领域服务是多个实体组合出来的一段业务逻辑。领域服务封装多个实体或方法后向上层提供应用服务调用。
- Repository（仓储）：存放所在聚合的查询或持久化代码，通常包括仓储接口和仓储实现方法。为了方便聚合的拆分和组合，我们设定了一个原则：一个聚合对应一个仓储。

### 4、基础层
Infrastructure 的代码目录结构有：config 和 util 两个子目录。

Config主要存放配置相关代码；Util主要存放平台、开发框架、消息、数据库、缓存、文件、总线、网关、三方类库、通用算法等基础代码，你可以为不同的资源类别建立不同的子目录。


![](https://upload-images.jianshu.io/upload_images/6271376-91870ffef81dca2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 关于微服务拆分和设计
原则上一个领域模型就可以设计为一个微服务，但由于领域建模时只考虑了业务因素，没有考虑微服务落地的技术、团队以及运行环境等非业务因素，因此在微服务拆分与设计时，我们不能简单地将领域模型作为拆分微服务的唯一标准，它只是作为微服务拆分的一个重要依据。

微服务的设计还需要考虑服务的粒度、分层、边界划分、依赖关系和集成关系。除了考虑业务职责单一外，我们还需要考虑将敏态与稳态业务的分离、非功能性需求（如弹性伸缩要求、安全性等要求）、团队组织和沟通效率、软件包大小以及技术异构等非业务因素。


## DDD与微服务的关系
DDD是一种架构设计方法，微服务是一种架构风格，两者从本质上都是为了追求高响应力，而从业务视角去分离应用系统建设复杂度的手段。两者都强调从业务出发，其核心要义是强调根据业务发展，合理划分领域边界，持续调整现有架构，优化现有代码，以保持架构和代码的生命力，也就是我们常说的演进式架构。

DDD主要关注：从业务领域视角划分领域边界，构建通用语言进行高效沟通，通过业务抽象，建立领域模型，维持业务和代码的逻辑一致性。

微服务主要关注：运行时的进程间通信、容错和故障隔离，实现去中心化数据管理和去中心化服务治理，关注微服务的独立开发、测试、构建和部署。

**DDD不仅可以用于微服务设计，还可以很好地应用于企业中台的设计。**

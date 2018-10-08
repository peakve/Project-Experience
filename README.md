# Project-Experience
# 微服务容易超时？
> - 设置超时时间
> - 改异步调用、结果通知？


# 开发环境调试相互影响
> - 分布式rpc调用，不知道调到哪个节点了，不是本地的
> - 一个人要活跃在多个系统中，不仅仅是调式自己单一系统的接口、或服务

# 新增服务商信息时，开通账号失败
> - rpc事务统一
> - 分布式事务一致性，不一致事务补偿但太复杂
> - 调用多次不同的微服务，有失败导致不一致

# 变更生效时间
> - 不同上游生效时间规则不一致，如何确定
> - 内部如何获取实时的结果
> - 一天中多次变更又如何？？


# 上游通知同一交易结果多次
> - 是否幂等
> - 交易成功会触发多次及时清算

# 应用打开文件句柄超限
- 打开文件数不停的增加，不能回收
- 是否是nginx的open-file-cache设置问题
- 是应用中使用极光推送，每次推送都创建了实例


# mysql 中slow log中commit事务超慢
> - 采用mysql 集群Galera Cluster，是同步事务
> - 每次都是系统级别的中断错误
> - 去掉采用TIDB方案？同步数据太慢，国内PingCAP团队，已加入腾讯


# 上下游太多，审核太乱
> - 审核路线的主流程确定
> - 不同的上下游只是路线中一个分支
> - 不要有在一个面上的复杂性，要把复杂性遏制在一个点上


# 数据量大，无法导出excel
> - 查询数据分页
> - 分多个文件存储，2w一个文件
> - 目录压缩包下载
> - 返回DeferredResult、callable对象，异步处理还是超时
> - 超时受限于nginx，不能单独设置更久的时间，会影响其他请求，开销大，不释放
> - 前端无感。提供2个接口，前端先提交导出请求，后续查询。根据后端生成的结果再下载。
> - 注意控制前端查询次数、频率



# 批量商户审核，商户号重复
> - 多线程actor调用
> - 商户信息初始化事务太大
> - 代码生成的商户号入库时违反数据唯一性约束
> - 分布式系统没有锁，redis、memcached、zookeeper 临时节点


# 分布式系统，日志难追踪查看
> - rpc请求链从入口带参数traceId
> - 日志收集分析。。。elastic search，kaffka


# 交易结果状态，无顺序消息，导致交易结果记录有误
> - 消息框架。。。redis自研（消息平衡接受处理），rocketmq（感觉会漏消息），rabbitMQ（定时延时有问题，重量级协议多，并发不行，有后台界面），zeroMQ(持久化会丢失)，Beanstalkd（后起之秀）。。
> - 有序消息，比对等待延后处理


# 同步会阻塞，http请求如何异步
> - springmvc 自带DeferredResult，servlet3.0异步处理支持，callback接口
> - 内部redis保存
> - 消息通知返回
> - hession http返回


# 交易笔数多，消息随之很多，此台应用接受消息处理太慢
> - 消息框架不靠谱
> - 每个应用根据自身处理能力接受消息
> - 过载保护，生产能力和消费能力的配对
> - 重要、无关紧要服务，大小服务分开
> - 只需要去看一个请求在队列里待的平均时间是否可以接受，是一个上涨趋势还是一个下降趋势


# 不同上游的交易相互影响
> - 同一系统中，需要拆分
> - 使用同一线程池，分上游分配不同的线程池，actor模式

# mysql deadlock
> - 莫名其妙找不到问题？多个线程争夺资源
> - 锁的原理是什么，基于索引项来锁
> -  show engine innodb status--》LATEST DETECTED DEADLOCK
> - insert & update xxx in(select xxx) 


# 查询统计慢怎么办
> - 看看slow_log
> - explain分析sql级别，consts 、ref、range、index
> - 不要有表关联，互联网模式单表结构
> - 建立索引
> - sql语句优化
> - 一次统计包含多个查询，异步统计-->同步输出
> - 保证微信公众号5s出结果


# nginx 代理https后，应用redirect https变成http
> - jquery ajax mixs reqeust
> - 分别配置一下 Nginx 和 Tomcat
> - X-Forwarded-Proto  $scheme
> - 代码动态配置


# 如何应对商户申请过程中，由不同系统中的多角色审核，带来的流程流转？
> - 业务逻辑，过程逻辑怎么分开
> - 工作流很重，引入很庞大，不符合具体业务，还得二次开发
> - 工作流程、状态机？解决什么问题，每个角色只关心自己的处理，不用关心整个流程。
> - 有序、有状态
> - 商户审核流程不易抽象通用过程
> - 如果使用工作流，不同角色不同的系统，如何定义工作流程，又如何查询各个角色的当前任务


# 何时需要代理？
> - 反向代理nginx，正向代理squid,vanish
> - 对方限制了访问IP，内部是分布式，有多个出口
> - 内部部署网络限制，不能访问外网
> - 快速封装https，强制转换http
> - 代理使用java的proxy，还是本地host配置


# 对账单下载
> - 下载的定时时间
> - 请求超时控制
> - 重复下载，记录下载路径
> - 打开文件流，解析获取200条对账一次
> - 对完账标记状态，不一致单独记录
> 












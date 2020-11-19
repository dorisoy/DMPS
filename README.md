# DMPS

分布式管理系统， 包括日志收集，消息订阅，SOA,集群管理，性能数据监控
配置文件 Event.Bindings.config</br>

```xml
<eventProvider>
	<queues>
		<queue id="DMPHubEvent" class="DMPSystem.Core.EventBus.Utilities.QueueContext,DMPSystem.Core.EventBus">
			<property name="appRuleFile" ref="rule"/>
			<property name="dataContextPool" value="event_sample">
				<map name="RabbitMq">
					<property value="guest:guest@127.0.0.1::test"/>
				</map>
				<map name="MsMq"></map>
				<map name="PushEvent"></map>
			</property>
			<property name="connectTimeout" value="120"/>
			<property name="CunsumerNum" value="5"/>
			<property name="UseRetryNum" value="5"/>
		</queue>
	</queues>
</eventProvider>
```

push publisher:

```csharp
EventContainer.GetInstances<IEventPublisher>("PushEvent").Publish(new ChanageStateEvent() { UserID = manager.UserID });
```

rabbitmq publisher:</br>

```csharp
EventContainer.GetInstances<IEventPublisher>("DMPHubEvent.RabbitMq").Publish(new ChanageStateEvent() { UserID = manager.UserID });
```
 
UseRateLimit 每分钟消息消费数限定在多少之内</br>
UseRetryNum 消息消费失败后重试次数，每次间隔1分钟</br>
增加注册消费者模块注册的方法，这样就不需要依赖Autofac</br>
 EventContainer.RegisterConsumeModule(params Type[] types)
 

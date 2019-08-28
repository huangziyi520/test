# Spring

一 初识Spring

....xml配置文件

```xml
configuration metadata
```

这是寻找域名代码的东西

xml配置文件注释：

```xml
配置map数据类型
<property>
    <map>
        <entry key="AA" value-ref="某个bean的id"></entry>
        <entry key="BB" value-ref="某个bean的id"></entry>
    </map>
</property>

配置list数据类型
<property>
	<ref bean="bean的id">
	<ref bean="bean的id">
	<bean class="">
		<constructor-arg value=""></constructor-arg>
		<constructor-arg value="" type="double"></constructor-arg>
	</bean>
</property>

```


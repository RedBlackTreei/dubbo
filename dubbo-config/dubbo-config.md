对外配置接口，以ServiceConfig和ReferenceConfig为中心，可以直接new配置类，也可以通过spring解析配置生成配置类。

- provider配置
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans.xsd  
        http://code.alibabatech.com/schema/dubbo  
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd  
        ">
    <!-- 具体的实现bean -->
    <bean id="testApiImpl" class="com.changjiang.test.TestApiImpl" />
    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="xixi_provider" />
    <!-- 使用zookeeper注册中心暴露服务地址 -->
    <dubbo:registry address="zookeeper://127.0.0.1:2181" />
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />
    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.changjiang.test.TestApi" ref="testApiImpl" />

</beans> 
```

- consumer配置
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans.xsd  
        http://code.alibabatech.com/schema/dubbo  
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd  
        ">
    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="hehe_consumer" />
    <!-- 使用zookeeper注册中心暴露服务地址 -->
    <!-- <dubbo:registry address="multicast://224.5.6.7:1234" /> -->
    <dubbo:registry address="zookeeper://127.0.0.1:2181" />
    <!-- 生成远程服务代理，可以像使用本地bean一样使用demoService -->
    <dubbo:reference id="testApiImpl" interface="com.changjiang.test.TestApi" />
</beans>
```
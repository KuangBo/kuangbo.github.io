---
layout:     post
title:      Redis
subtitle:   SpringData与Redis的集成
date:       2020-11-11
author:     kuangbo
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - Redis
    - SpringData
---

# Redis

dokcer配置Redis“一主一从” `docker-compose.yaml`

```yaml
version: '3.7'
services:
  redis-0:
    image: redis
    container_name: redis-master
    restart: always
    command: redis-server /etc/redis/redis.conf --requirepass redis --appendonly yes
    ports:
      - 6379:6379
      - 16379:16379
    volumes:
      - ./redis/redis-6379/data:/data
      - ./redis/redis-6379/conf/redis.conf:/etc/redis/redis.conf
    environment:
      - TZ=Asia/Shanghai
  redis-1:
    image: redis
    container_name: redis-1
    restart: always
    command: redis-server /etc/redis/redis.conf --requirepass redis --appendonly yes
    ports:
      - 6380:6380
      - 16380:16380
    volumes:
      - ./redis/redis-6380/data:/data
      - ./redis/redis-6380/conf/redis.conf:/etc/redis/redis.conf
    environment:
      - TZ=Asia/Shanghai
  redis-2:
    image: redis
    container_name: redis-2
    restart: always
    command: redis-server /etc/redis/redis.conf --requirepass redis --appendonly yes
    ports:
      - 6381:6381
      - 16381:16381
    volumes:
      - ./redis/redis-6381/data:/data
      - ./redis/redis-6381/conf/redis.conf:/etc/redis/redis.conf
    environment:
      - TZ=Asia/Shanghai
  redis-3:
    image: redis
    container_name: redis-3
    restart: always
    command: redis-server /etc/redis/redis.conf --requirepass redis --appendonly yes
    ports:
      - 6382:6382
      - 16382:16382
    volumes:
      - ./redis/redis-6382/data:/data
      - ./redis/redis-6382/conf/redis.conf:/etc/redis/redis.conf
    environment:
      - TZ=Asia/Shanghai
  redis-4:
    image: redis
    container_name: redis-4
    restart: always
    command: redis-server /etc/redis/redis.conf --requirepass redis --appendonly yes
    ports:
      - 6383:6383
      - 16383:16383
    volumes:
      - ./redis/redis-6383/data:/data
      - ./redis/redis-6383/conf/redis.conf:/etc/redis/redis.conf
    environment:
      - TZ=Asia/Shanghai
  redis-5:
    image: redis
    container_name: redis-5
    restart: always
    command: redis-server /etc/redis/redis.conf --requirepass redis --appendonly yes
    ports:
      - 6384:6384
      - 16384:16384
    volumes:
      - ./redis/redis-6384/data:/data
      - ./redis/redis-6384/conf/redis.conf:/etc/redis/redis.conf
    environment:
      - TZ=Asia/Shanghai
```

docker执行容器命令：`sudo docker exec -it redis-master bash`

redis自动分配主从关系：`redis-cli --cluster create 202.120.87.165:6379 202.120.87.165:6380 202.120.87.165:6381 202.120.87.165:6382 202.120.87.165:6383 202.120.87.165:6384 --cluster-replicas 1 -a redis`

`redis-cli --cluster create 202.120.87.165:6379 202.120.87.165:6380 202.120.87.165:6381 202.120.87.165:6382 202.120.87.165:6383 202.120.87.165:6384 202.120.87.165:6385 202.120.87.165:6386 202.120.87.165:6387 --cluster-replicas 2 -a redis`

## SpringData与Redis集成

### SpringData简介

使用SpringData可以轻松实现各个数据库中对象信息保存处理。

Redis本身支持的数据类型：基本数据、Hash数据，在整个Redis里面只有这两类数据支持普通的数据转换功能。

- 基本数据：JSON结构数据
- HashSet数据：Hash数据本身就可以描述出各个属性的内容![SpringData-Redis-1](https://kuangbo.github.io/img/SpringData-Redis-1.png)

绝大多数将选择第一种方式：

- 操作比较直观；
- 只进行一次数据的保存。

如果真的使用基本数据类型进行对象的保存，实际上有两种方案：对象序列化（速度快）、JSON处理（速度慢）。利用SpringData技术可以轻松的实现转换。SpringData的转换原则如下：![SpringData-Redis-2](https://kuangbo.github.io/img/SpringData-Redis-2.png)

SpringData版本

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.data/spring-data-redis -->
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>2.3.5.RELEASE</version>
</dependency>
```

### SpringData配置

1、配置好所有的Spring、Redis开发包

2、pool.xml文件如下

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.kuangbo.jedis</groupId>
    <artifactId>Jedis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>Jedis</name>
    <!-- FIXME change it to the project's website -->
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
        <!-- 依赖版本 -->
        <jdk.version>1.8</jdk.version>
        <encoding.charset>UTF-8</encoding.charset>
        <junit.version>4.11</junit.version>
        <jedis.version>3.3.0</jedis.version>
        <slf4j.version>1.7.30</slf4j.version>
        <!-- 配置Spring开发包的相关版本属性 -->
        <spring.version>5.2.10.RELEASE</spring.version>
        <spring-data.version>2.3.5.RELEASE</spring-data.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>${jedis.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>${slf4j.version}</version>
            <scope>compile</scope>
        </dependency>
        <!-- SpringData-Redis开发包 -->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
            <version>${spring-data.version}</version>
        </dependency>
        <!-- 以下是Spring开发包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--        <dependency>-->
        <!--            <groupId>org.springframework</groupId>-->
        <!--            <artifactId>spring-web</artifactId>-->
        <!--            <version>${spring.version}</version>-->
        <!--        </dependency>-->
        <!--        <dependency>-->
        <!--            <groupId>org.springframework</groupId>-->
        <!--            <artifactId>spring-webmvc</artifactId>-->
        <!--            <version>${spring.version}</version>-->
        <!--        </dependency>-->
    </dependencies>

    <build>
        <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
                <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.1.0</version>
                </plugin>
                <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.0</version>
                    <configuration>
                        <source>${jdk.version}</source>
                        <target>${jdk.version}</target>
                        <encoding>${encoding.charset}</encoding>
                    </configuration>
                </plugin>
                <plugin>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.22.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-jar-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-install-plugin</artifactId>
                    <version>2.5.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-deploy-plugin</artifactId>
                    <version>2.8.2</version>
                </plugin>
                <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
                <plugin>
                    <artifactId>maven-site-plugin</artifactId>
                    <version>3.7.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-project-info-reports-plugin</artifactId>
                    <version>3.0.0</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

3、建立`src/main/profiles/dev`目录，建立`config/redis.properties`配置文件

```properties
# Redis的连接主机地址
redis.host=202.120.87.165
# Redis连接端口号
redis.port=6379
# Redis的认证信息
redis.password=redis
# Redis连接的超时时间
redis.timeout=2000
# 设置最大可用连接数
redis.pool.maxTotal=100
# 设置最大空闲连接数
redis.pool.maxIdle=20
# 设置最大等待时间
redis.pool.maxWaitMillis=2000
# 是否返回可用连接
redis.pool.testOnBorrow=true
```

4、`spring-redis.xml`配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 如果要进行Redis处理，用户不应该取关注具体的序列化与反序列化操作，这都应该交给SpringData处理 -->
    <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <property name="connectionFactory" ref="connectionFactory"/>   <!-- 定义Redis连接工厂 -->
        <property name="keySerializer"> <!-- 定义序列化Key的程序处理类 -->
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </property>
        <property name="valueSerializer">   <!-- 处理value数据的操作 -->
            <!-- 明确表示如果要进行value数据保存，保存的对象一定要使用JDK提供的序列化处理类 -->
            <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer"/>
        </property>
        <property name="hashKeySerializer">
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </property>
        <property name="hashValueSerializer">   <!-- 处理hash数据的保存 -->
            <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer"/>
        </property>
    </bean>

    <!-- 首先进行Jedis连接池的相关配置 -->
    <bean id="jeidsPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxTotal" value="${redis.pool.maxTotal}"/>  <!-- 最大可用连接数 -->
        <property name="maxIdle" value="${redis.pool.maxIdle}"/>    <!-- 最大空闲连接数 -->
        <property name="maxWaitMillis" value="${redis.pool.maxWaitMillis}"/>    <!-- 最大等待时间 -->
        <property name="testOnBorrow" value="${redis.pool.testOnBorrow}"/>  <!-- 取得可用连接 -->
    </bean>
    <!-- 进行ConnectionFactory的配置 -->
    <bean id="connectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <property name="poolConfig" ref="jeidsPoolConfig"/> <!-- 引入连接池的配置项 -->
        <property name="hostName" value="${redis.host}"/>   <!-- Redis的连接地址 -->
        <property name="password" value="${redis.password}"/>   <!-- 认证信息 -->
        <property name="port" value="${redis.port}"/>   <!-- 认证端口 -->
        <property name="timeout" value="${redis.timeout}"/> <!-- 连接超时时间 -->
    </bean>
</beans>
```

5、定义`config/spring-common.xml`文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="cn.kuangbo"/>
    <!-- 导入所需要的资源配置文件 -->
    <context:property-placeholder location="classpath:config/*.properties"/>
    <import resource="classpath:spring/spring-redis.xml"/>
</beans>
```

### SpringData序列化与反序列化

直接使用SpringTest进行项目测试，可以自动注入资源，主要使用的是redisTemplate资源。

1、建立一个VO类

```java
package cn.kuangbo.springdata.vo;

import java.util.Date;

/**
 * Created with IntelliJ IDEA.
 * User: kuangbo
 * Date: 2020/11/11
 * Time: 下午2:01
 */
public class Member {
    private String mid;
    private String name;
    private Integer age;
    private Date Birthday;
    private Double salary;
}
```

2、编写测试程序实现SpringData数据处理

```java
package cn.kuangbo.jedis;

import cn.kuangbo.springdata.vo.Member;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;
import java.util.Date;

/**
 * Created with IntelliJ IDEA.
 * User: kuangbo
 * Date: 2020/11/11
 * Time: 下午2:00
 */
@ContextConfiguration(locations = {"classpath:spring/spring-common.xml"})
@RunWith(SpringJUnit4ClassRunner.class) // 使用Junit进行测试
public class TestMember {
    @Resource
    private RedisTemplate<String, Object> redisTemplate;    // 序列化操作模板

    @Test
    public void testSave() {    // 数据保存
        Member vo = new Member();
        vo.setMid("uid");
        vo.setAge(18);
        vo.setBirthday(new Date());
        vo.setName("username");
        vo.setSalary(500.0);
        redisTemplate.opsForValue().set("mem-1", vo);
    }

    @Test
    public void testLoad() {
        Object obj = this.redisTemplate.opsForValue().get("mem-1");
        System.out.println(obj);
    }
}
```

### SpringData与哨兵访问

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 如果要进行Redis处理，用户不应该取关注具体的序列化与反序列化操作，这都应该交给SpringData处理 -->
    <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <property name="connectionFactory" ref="connectionFactory"/>   <!-- 定义Redis连接工厂 -->
        <property name="keySerializer"> <!-- 定义序列化Key的程序处理类 -->
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </property>
        <property name="valueSerializer">   <!-- 处理value数据的操作 -->
            <!-- 明确表示如果要进行value数据保存，保存的对象一定要使用JDK提供的序列化处理类 -->
            <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer"/>
        </property>
        <property name="hashKeySerializer">
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </property>
        <property name="hashValueSerializer">   <!-- 处理hash数据的保存 -->
            <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer"/>
        </property>
    </bean>

    <!-- 进行所有哨兵地址的配置 -->
    <bean id="redisSentinelConfiguration" class="org.springframework.data.redis.connection.RedisSentinelConfiguration">
        <property name="master">    <!-- 配置master的节点名称 -->
            <bean class="org.springframework.data.redis.connection.RedisNode">
                <!-- 通过配置文件读取出master的名称进行配置 -->
                <property name="name" value="${redis.sentinel.master.name}"/>
            </bean>
        </property>
        <!-- 配置所有哨兵的连接地址信息 -->
        <property name="sentinels">
            <set>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg name="host" value="${redis.sentinel-1.host}"/>
                    <constructor-arg name="port" value="${redis.sentinel-1.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg name="host" value="${redis.sentinel-2.host}"/>
                    <constructor-arg name="port" value="${redis.sentinel-2.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg name="host" value="${redis.sentinel-3.host}"/>
                    <constructor-arg name="port" value="${redis.sentinel-3.port}"/>
                </bean>
            </set>
        </property>
    </bean>

    <!-- 首先进行Jedis连接池的相关配置 -->
    <bean id="jeidsPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxTotal" value="${redis.pool.maxTotal}"/>  <!-- 最大可用连接数 -->
        <property name="maxIdle" value="${redis.pool.maxIdle}"/>    <!-- 最大空闲连接数 -->
        <property name="maxWaitMillis" value="${redis.pool.maxWaitMillis}"/>    <!-- 最大等待时间 -->
    </bean>
    <!-- 进行ConnectionFactory的配置 -->
    <bean id="connectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <constructor-arg name="sentinelConfig" ref="redisSentinelConfiguration"/>
        <property name="poolConfig" ref="jeidsPoolConfig"/> <!-- 引入连接池的配置项 -->
        <property name="password" value="${redis.password}"/>   <!-- 认证信息 -->
    </bean>
</beans>
```

### Spring访问RedisCluster

1、`redis.properties`配置文件

```properties
# 追加RedisCluster中的所有主机信息
redis.cluster.node-1.host=202.120.87.165
redis.cluster.node-2.host=202.120.87.165
redis.cluster.node-3.host=202.120.87.165
redis.cluster.node-4.host=202.120.87.165
redis.cluster.node-5.host=202.120.87.165
redis.cluster.node-6.host=202.120.87.165
redis.cluster.node-7.host=202.120.87.165
redis.cluster.node-8.host=202.120.87.165
redis.cluster.node-9.host=202.120.87.165
# 追加RedisCluster中的所有主机端口号
redis.cluster.node-1.port=6379
redis.cluster.node-2.port=6380
redis.cluster.node-3.port=6381
redis.cluster.node-4.port=6382
redis.cluster.node-5.port=6383
redis.cluster.node-6.port=6384
redis.cluster.node-7.port=6385
redis.cluster.node-8.port=6386
redis.cluster.node-9.port=6387

# Redis的认证信息
redis.password=redis
# Redis连接的超时时间
redis.timeout=2000
# 设置最大可用连接数
redis.pool.maxTotal=100
# 设置最大空闲连接数
redis.pool.maxIdle=20
# 设置最大等待时间
redis.pool.maxWaitMillis=2000
# 是否返回可用连接
redis.pool.testOnBorrow=true

# 定义一个限制redirect操作属性，如果该属性的内容过长，则设置可能出现问题
redis.clsuter.max-redirect=2
```

2、`spring-redis.xml`配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 如果要进行Redis处理，用户不应该取关注具体的序列化与反序列化操作，这都应该交给SpringData处理 -->
    <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <property name="connectionFactory" ref="connectionFactory"/>   <!-- 定义Redis连接工厂 -->
        <property name="keySerializer"> <!-- 定义序列化Key的程序处理类 -->
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </property>
        <property name="valueSerializer">   <!-- 处理value数据的操作 -->
            <!-- 明确表示如果要进行value数据保存，保存的对象一定要使用JDK提供的序列化处理类 -->
            <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer"/>
        </property>
        <property name="hashKeySerializer">
            <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
        </property>
        <property name="hashValueSerializer">   <!-- 处理hash数据的保存 -->
            <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer"/>
        </property>
    </bean>

    <!-- 定义RedisCluster的配置 -->
    <bean id="clusterConfiguration" class="org.springframework.data.redis.connection.RedisClusterConfiguration">
        <property name="maxRedirects" value="${redis.clsuter.max-redirect}"/>
        <!-- 配置出集群中的所有节点 -->
        <property name="clusterNodes">
            <list>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-1.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-1.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-2.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-2.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-3.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-3.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-4.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-4.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-5.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-5.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-6.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-6.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-7.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-7.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-8.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-8.port}"/>
                </bean>
                <bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${redis.cluster.node-9.host}"/>
                    <constructor-arg index="1" value="${redis.cluster.node-9.port}"/>
                </bean>
            </list>
        </property>
    </bean>

    <!-- 首先进行Jedis连接池的相关配置 -->
    <bean id="jeidsPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxTotal" value="${redis.pool.maxTotal}"/>  <!-- 最大可用连接数 -->
        <property name="maxIdle" value="${redis.pool.maxIdle}"/>    <!-- 最大空闲连接数 -->
        <property name="maxWaitMillis" value="${redis.pool.maxWaitMillis}"/>    <!-- 最大等待时间 -->
        <!--        <property name="testOnBorrow" value="${redis.pool.testOnBorrow}"/>  &lt;!&ndash; 取得可用连接 &ndash;&gt;-->
    </bean>
    <!-- 进行ConnectionFactory的配置 -->
    <bean id="connectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <constructor-arg name="clusterConfig" ref="clusterConfiguration"/>
        <property name="poolConfig" ref="jeidsPoolConfig"/> <!-- 引入连接池的配置项 -->
        <property name="password" value="${redis.password}"/>   <!-- 认证信息 -->
    </bean>
</beans>
```

3、测试程序

```java
package cn.kuangbo.jedis;

import cn.kuangbo.springdata.vo.Member;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;
import java.util.Date;

/**
 * Created with IntelliJ IDEA.
 * User: kuangbo
 * Date: 2020/11/11
 * Time: 下午2:00
 */
@ContextConfiguration(locations = {"classpath:spring/spring-common.xml"})
@RunWith(SpringJUnit4ClassRunner.class) // 使用Junit进行测试
public class TestMember {
    @Resource
    private RedisTemplate<String, Object> redisTemplate;    // 序列化操作模板

    @Test
    public void testSave() {    // 数据保存
        Member vo = new Member();
        vo.setMid("uid");
        vo.setAge(18);
        vo.setBirthday(new Date());
        vo.setName("username");
        vo.setSalary(500.0);
        this.redisTemplate.opsForValue().set("mem-1", vo);
    }

    @Test
    public void testMultiSave() {    // 数据保存
        for (int x = 0; x < 1000; x++) {
            Member vo = new Member();
            vo.setMid("uid");
            vo.setAge(18);
            vo.setBirthday(new Date());
            vo.setName("username");
            vo.setSalary(500.0);
            this.redisTemplate.opsForValue().set("mem-" + x, vo);
        }
    }

    @Test
    public void testLoad() {
        Object obj = this.redisTemplate.opsForValue().get("mem-1");
        System.out.println(obj);
    }
}
```


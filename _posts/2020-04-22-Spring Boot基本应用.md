---
layout:     post
title:      Spring Boot基本应用
subtitle:   
date:       2020-04-22
author:     tideseng
header-img: 
header-mask: 
catalog: true
tags:
    - springboot
---

## 简介

### 概念

Spring Boot是基于Spring提供开箱即用的应用框架，即服务于Spring框架的框架，服务范围是简化配置(Spring是简化企业级应用开发的轻量级框架，通过IOC和AOP，用POJO实现EJB的功能)

### 特点

- 开箱即用，没有代码生成，也不需要XML配置
- 起步依赖，通过传递依赖提供了默认的功能
- 自动配置，尽可能自动配置Spring和第三方库
- 提供了常用功能，如嵌入式Tomcat服务器、指标，运行状况检查和外部化配置

### 约定优于配置的体现

- 使用maven工程，默认以jar包方式打包、默认resource目录放配置文件和资源文件
- 起步依赖，spring-boot-starter-web中提供mvc相关依赖和内置tomcat
- 默认application配置文件
- 默认通过spring.profiles.active决定运行的配置文件
- EnableAutoConfiguration默认对于依赖的starter进行自动装配

## 快速入门

### 创建工程

创建一个无骨架的maven工程即可（亦可快速创建，但需要选择版本）

### 添加起步依赖

```xml
<!--所有Spring Boot应用的父工程，锁定依赖、插件版本等功能-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.4.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
    <!--Web功能的起步依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

### 编写启动类

```java
// Spring Boot启动类注解
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        // 根据相关参数启动Spring Boot
        SpringApplication.run(Application.class);
    }

}
```

### 编写Controller

```java
//@Controller
@RestController // RestController注解中包含了ResponseBody注解
public class ControllerTest {

//    @ResponseBody
    @RequestMapping("/test")
    public Object test(){
        return "佳欢";
    }

}
```

## 技术封装

### AutoConfiguration

### Starter

### Actuator

### SpringBoot CLI

## 原理分析

### 起步依赖

#### spring-boot-starter-parent 

- 定义了资源文件信息

  ```xml
  <resources>
      <resource>
          <filtering>true</filtering>
          <directory>${basedir}/src/main/resources</directory>
          <includes>
              <!-- 在资源文件定义的属性会覆盖自动配置的值 -->
              <include>**/application*.yml</include>
              <include>**/application*.yaml</include>
              <include>**/application*.properties</include>
          </includes>
      </resource>
      <resource>
          <directory>${basedir}/src/main/resources</directory>
          <excludes>
              <exclude>**/application*.yml</exclude>
              <exclude>**/application*.yaml</exclude>
              <exclude>**/application*.properties</exclude>
          </excludes>
      </resource>
  </resources>
  ```

- 引入了父工程spring-boot-dependencies

  ```xml
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>2.1.4.RELEASE</version>
      <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
  ```

- 父工程中锁定了版本

  ```xml
  <dependencyManagement>
      <dependencies>
      	<dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
              <version>2.1.4.RELEASE</version>
          </dependency>
      	...
      </dependencies>
  </dependencyManagement>
  
  <build>
  	<pluginManagement>
  		<plugins>
  			<plugin>
  				<groupId>org.springframework.boot</groupId>
  				<artifactId>spring-boot-maven-plugin</artifactId>
  				<version>2.1.4.RELEASE</version>
  			</plugin>
  			...
  		</plugins>
  	</pluginManagement>
  </build>
  ```

#### spring-boot-starter-web

- 引入web开发的依赖

  ```xml
  <parent>
  	<groupId>org.springframework.boot</groupId>
  	<artifactId>spring-boot-starters</artifactId>
  	<version>2.1.4.RELEASE</version>
  </parent>
  
  <dependencies>
  	<dependency>
  		<groupId>org.springframework.boot</groupId>
  		<artifactId>spring-boot-starter</artifactId>
  		<version>2.1.4.RELEASE</version>
  		<scope>compile</scope>
  	</dependency>
  	<dependency>
  		<groupId>org.springframework.boot</groupId>
  		<artifactId>spring-boot-starter-json</artifactId>
  		<version>2.1.4.RELEASE</version>
  		<scope>compile</scope>
  	</dependency>
  	<dependency>
  		<groupId>org.springframework.boot</groupId>
  		<artifactId>spring-boot-starter-tomcat</artifactId>
  		<version>2.1.4.RELEASE</version>
  		<scope>compile</scope>
  	</dependency>
  	<dependency>
  		<groupId>org.hibernate.validator</groupId>
  		<artifactId>hibernate-validator</artifactId>
  		<version>6.0.16.Final</version>
  		<scope>compile</scope>
  	</dependency>
  	<dependency>
  		<groupId>org.springframework</groupId>
  		<artifactId>spring-web</artifactId>
  		<version>5.1.6.RELEASE</version>
  		<scope>compile</scope>
  	</dependency>
  	<dependency>
  		<groupId>org.springframework</groupId>
  		<artifactId>spring-webmvc</artifactId>
  		<version>5.1.6.RELEASE</version>
  		<scope>compile</scope>
  	</dependency>
  </dependencies>
  ```

### 自动配置

#### @SpringBootApplication

##### @SpringBootConfiguration注解

  ```java
@Configuration // 注解方式声明配置类
public @interface SpringBootConfiguration {
}
  ```

##### @ComponentScan注解

  ```java
// 扫描该包及其子包的相关Component注解类，放入IOC容器
@ComponentScan(
    excludeFilters = {@Filter(
        type = FilterType.CUSTOM,
        classes = {TypeExcludeFilter.class}
    ), @Filter(
        type = FilterType.CUSTOM,
        classes = {AutoConfigurationExcludeFilter.class}
    )}
)
  ```

##### @EnableAutoConfiguration注解

  开启自动配置功能，将符合条件的@Configuration配置加载到IOC容器中（动态注入）

  简化bean的注入逻辑，从原始的xml注入-->注解注入-->自动注入

  ```java
// @Import注解可以配置三种不同的class
// 基于普通的bean或带有@Configuration的bean进行直接注入
// 实现ImportSelector接口进行动态注入
// 实现ImportBeanDefinitionRegistrar接口进行动态注入
// Registrar实现了ImportBeanDefinitionRegistrar接口进行动态注入
@AutoConfigurationPackage // @Import({Registrar.class})，Registrar实现了ImportBeanDefinitionRegistrar接口进行动态注入
// AutoConfigurationImportSelector实现了ImportSelector接口进行动态注入
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";
    Class<?>[] exclude() default {};
    String[] excludeName() default {};
}
  ```

  ```java
// DeferredImportSelector接口继承自ImportSelector接口
public class AutoConfigurationImportSelector implements DeferredImportSelector, BeanClassLoaderAware, ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered {
    // @Enable*注解的工作原理:ImportSelector接口的selectImports方法返回的数组（类的全类名）都会被纳入到spring容器中
    @Override
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return NO_IMPORTS;
        } else {
            // 先加载classpath/META-INF/spring-autoconfigure-metadata.properties文件
            // 条件注解，后期用于条件过滤
            AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader.loadMetadata(this.beanClassLoader);
            // 调用getAutoConfigurationEntry方法
            AutoConfigurationImportSelector.AutoConfigurationEntry autoConfigurationEntry = this.getAutoConfigurationEntry(autoConfigurationMetadata, annotationMetadata);
            return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
        }
    }

    protected AutoConfigurationImportSelector.AutoConfigurationEntry getAutoConfigurationEntry(AutoConfigurationMetadata autoConfigurationMetadata, AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return EMPTY_ENTRY;
        } else {
            AnnotationAttributes attributes = this.getAttributes(annotationMetadata);
            // 1.调用getCandidateConfigurations方法，获取所有要注入的bean
            List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
            // 2.对重复的bean进行去重（当依赖的其它jar包中存在/META-INF/spring.factories文件时bean可能重复，SPI扩展点）
            configurations = this.removeDuplicates(configurations);
            // 3.指定过滤，过滤在SpringBootApplication中的类
            Set<String> exclusions = this.getExclusions(annotationMetadata, attributes);
            this.checkExcludedClasses(configurations, exclusions);
            configurations.removeAll(exclusions);
            // 4.条件过滤，过滤spring-autoconfigure-metadata.properties文件中的条件，减少没必要的加载
            configurations = this.filter(configurations, autoConfigurationMetadata);
            this.fireAutoConfigurationImportEvents(configurations, exclusions);
            return new AutoConfigurationImportSelector.AutoConfigurationEntry(configurations, exclusions);
        }
    }

    protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
        // SpringFactoriesLoader工具类，SPI扩展点
        // 从classpath/META-INF/spring.factories文件中根据key来加载对应的类到IOC容器中
        List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
        Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
        return configurations;
    }
}
  ```

  ```properties
# spring.factories文件中得配置信息
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
...,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\...
  ```

  ```java
// 属性配置类ServerProperties类
@EnableConfigurationProperties({ServerProperties.class})
@Import({ServletWebServerFactoryAutoConfiguration.BeanPostProcessorsRegistrar.class, EmbeddedTomcat.class, EmbeddedJetty.class, EmbeddedUndertow.class})
public class ServletWebServerFactoryAutoConfiguration {...}
  ```

  ```java
// 配置文件和实体属性间的映射，在META-INF\spring-configuration-metadata.json文件中也能找到该属性
@ConfigurationProperties(prefix = "server",ignoreUnknownFields = true)
public class ServerProperties {
    private Integer port;
    private InetAddress address;
    ...
    get/set...
}
  ```

## 配置文件

### 作用

SpringBoot基于约定，很多配置都有默认值

SpringBoot默认会从application.properties/application.yml配置文件替换默认配置值

### yml配置

```yml
server:
	port: 8888
	servlet:
		context-path: /demo  
```

### 配置文件与配置配的映射

- @Value属性注解

  属性上添加注解，属性不需要提供set方法

- @ConfigurationProperties类注解

  类上添加疏解，且属性必须提供set方法

  > 建议加上spring-boot-configuration-processor依赖
  >
  > 可参考自动配置时的ServletWebServerFactoryAutoConfiguration类和ServerProperties类

  

## 技术整合

### 整合Mybatis

- 起步依赖

  ```xml
  <dependency>
      <groupId>org.mybatis.spring.boot</groupId>
      <artifactId>mybatis-spring-boot-starter</artifactId>
      <version>1.1.1</version>
  </dependency>
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
  </dependency>
  ```

- 配置文件

  ```properties
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8&useSSL=false&autoReconnect=true&serverTimezone=Asia/Shanghai
  spring.datasource.username=root
  spring.datasource.password=root
  
  #pojo别名扫描包
  mybatis.type-aliases-package=com.itheima.domain
  #加载Mybatis映射文件
  mybatis.mapper-locations=classpath:mapper/*Mapper.xml
  ```

- 接口类

  ```java
  @Mapper
  public interface UserMapper {
  	public List<User> queryUserList();
  }
  ```

- 映射文件

  ```xml
  <?xml version="1.0" encoding="utf-8" ?>
  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
  <mapper namespace="com.itheima.mapper.UserMapper">
      <select id="queryUserList" resultType="user">
          select * from user
      </select>
  </mapper>
  ```

### 整合Spring Data JPA

- 起步依赖

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
  </dependency>
  ```

- 配置文件

  ```properties
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8&useSSL=false&autoReconnect=true&serverTimezone=Asia/Shanghai
  spring.datasource.username=root
  spring.datasource.password=root
  
  #JPA Configuration:
  spring.jpa.database=MySQL
  spring.jpa.show-sql=true
  spring.jpa.generate-ddl=true
  spring.jpa.hibernate.ddl-auto=update
  spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy
  ```

- 实体类

  ```java
  @Entity
  public class User {
      // 主键
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;
      private String username;
      private String password;
      private String name;
      //省略setter和getter方法... ...
  }
  ```

- 接口类

  ```java
  public interface UserRepository extends JpaRepository<User,Long>{
      public List<User> findAll();
  }
  ```

  

### 整合Junit

- 起步依赖

  ```xml
  <dependency>
  	<groupId>org.springframework.boot</groupId>
  	<artifactId>spring-boot-starter-test</artifactId>
  	<scope>test</scope>
  </dependency>
  ```

- 测试类

  ```
  // SpringRunner继承自SpringJUnit4ClassRunner
  @RunWith(SpringRunner.class)
  @SpringBootTest(classes = Application.class)
  public class MapperTest {
  }
  ```
  
- 启动类

  ```java
  @SpringBootApplication
  public class Application {
      public static void main(String[] args) {
          SpringApplication.run(Application.class);
      }
  }
  ```

Spring整合Junit

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {
}

@Configuration
@ComponentScan("com.tideseng.annotation") // <context:component-scan base-package="com.tideseng"></context:component-scan>
@PropertySource("jdbc.properties") // <context:property-placeholder location="classpath:jdbc.properties"/>
@Import({JdbcConfig.class, TransactionConfig.class})
@EnableAspectJAutoProxy // <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
@EnableTransactionManagement // <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
public class SpringConfiguration {
}

public class JdbcConfig {

    @Value("${jdbc.driverClass}")
    private String driver;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name="dataSource")
    public DataSource createDataSource(){
        DriverManagerDataSource ds = new DriverManagerDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(username);
        ds.setPassword(password);
        return ds;
    }

    @Bean(name="jdbcTemplate")
    public JdbcTemplate createJdbcTemplate(DataSource dataSource){
        return new JdbcTemplate(dataSource);
    }
}
```

### 整合Reids

- 起步依赖

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
  </dependency>
  ```

- 配置文件

  ```properties
  spring.redis.host=127.0.0.1
  spring.redis.port=6379
  ```

- 工具类

  ```java
  @Slf4j
  public class RedisServiceImpl implements RedisService {
  	
  	@Autowired
  	private RedisTemplate<String, String> redisTemplate;
  	
  	 /**
       * 设置缓存
       * */
  	@Override
      public <T> boolean set(KeyPrefix prefix, String key, T value) {
           String str = beanToString(value);
           if(StringUtils.isEmpty(str)) 
               throw new PlatformException("redis_value_error", "缓存存储值错误");
           if(prefix.getExpireSeconds() <= 0) 
          	 this.redisTemplate.opsForValue().set(prefix.getrRealPrefix(key), str);
           else 
          	 this.redisTemplate.opsForValue().set(prefix.getrRealPrefix(key), str, prefix.getExpireSeconds(), TimeUnit.SECONDS);
           log.info("给{}设置缓存", prefix.getrRealPrefix(key));
           return true;
      }
  	
  	/**
  	 * 不存在则设置缓存
  	 */
  	@Override
  	public <T> boolean setIfAbsent(KeyPrefix prefix, String key, T value) {
  		Boolean hasKey = this.redisTemplate.hasKey(prefix.getrRealPrefix(key));
  		if(!hasKey) return this.set(prefix, key, value);
  		return false;
  	}
  	
      /**
       * 获取缓存
       * */
  	@Override
  	public <T> T get(KeyPrefix prefix, String key, Class<T> clazz) {
  		String str = this.redisTemplate.opsForValue().get(prefix.getrRealPrefix(key));
  		log.info("获取{}的缓存", prefix.getrRealPrefix(key));
  		return stringToBean(str, clazz);
  	}
  	@Override
  	public <T> T get(KeyPrefix prefix, String key, TypeReference<T> typeRef) {
  		String str = this.redisTemplate.opsForValue().get(prefix.getrRealPrefix(key));
  		log.info("获取{}的缓存", prefix.getrRealPrefix(key));
  		return stringToBean(str, typeRef);
  	}
  	
      /**
       * 判断是否存在
       * */
  	@Override
      public <T> boolean exist(KeyPrefix prefix, String key) {
          return  this.redisTemplate.hasKey(prefix.getrRealPrefix(key));
      }
  	
  	 /**
       * 删除缓存
       * */
  	@Override
  	public void delete(KeyPrefix prefix, String key) {
  		log.info("删除{}的缓存", prefix.getrRealPrefix(key));
  		this.redisTemplate.delete(prefix.getrRealPrefix(key));
  	}
      
      /**
       * 增加值，key不存在时默认为0且永不过期
       * */
  	@Override
      public <T> Long increase(KeyPrefix prefix, String key) {
  		log.info("增加{}的缓存", prefix.getrRealPrefix(key));
  		return this.redisTemplate.opsForValue().increment(prefix.getrRealPrefix(key), 1);
      }
      
      /**
       * 减少值，key不存在时默认为0且永不过期
       * */
  	@Override
      public <T> Long decrease(KeyPrefix prefix, String key) {
  		log.info("减少{}的缓存", prefix.getrRealPrefix(key));
          return  this.redisTemplate.opsForValue().increment(prefix.getrRealPrefix(key), -1);
      }
  	
  	/**
  	 * 设置过期时间
  	 */
  	@Override
  	public Boolean expire(KeyPrefix prefix, String key, long timeout) {
  		return this.redisTemplate.expire(prefix.getrRealPrefix(key), timeout, TimeUnit.SECONDS);
  	}
  	@Override
  	public Boolean expire(KeyPrefix prefix, String key, Date date) {
  		return this.redisTemplate.expireAt(prefix.getrRealPrefix(key), date);
  	}
      
  	/**
  	 * 获取剩余过期时间
  	 */
  	@Override
  	public Long getExpire(KeyPrefix prefix, String key) {
  		return this.redisTemplate.getExpire(prefix.getrRealPrefix(key), TimeUnit.SECONDS);
  	}
  	
      private <T> String beanToString(T value) {
          if(value == null) 
              return null;
          Class<?> clazz = value.getClass();
          if(clazz == int.class || clazz == Integer.class) 
               return ""+value;
          else if(clazz == String.class) 
               return (String)value;
          else if(clazz == long.class || clazz == Long.class) 
              return ""+value;
          else 
              return JsonUtils.serialize(value);
      }
      
      @SuppressWarnings("unchecked")
      private <T> T stringToBean(String str, Class<T> clazz) {
          if(StringUtils.isEmpty(str) || clazz == null) 
               return null;
          if(clazz == int.class || clazz == Integer.class) 
               return (T)Integer.valueOf(str);
          else if(clazz == String.class) 
               return (T)str;
          else if(clazz == long.class || clazz == Long.class) 
              return  (T)Long.valueOf(str);
          else 
              return JsonUtils.deserialize(str, clazz);
      }
      
      private <T> T stringToBean(String str, TypeReference<T> typeRef) {
      	if(StringUtils.isEmpty(str) || typeRef == null) 
              return null;
      	return JsonUtils.deserialize(str, typeRef);
      }
      
  }
  ```

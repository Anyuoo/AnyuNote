## 微服务（gradle+springcloud +alibaba）

### **1. 使用gradle创建微服务或者多模块项目的步骤**

#### 1.0 建一个空的gradle项目,修改build.gradle配置

- buildscript 构建脚本最先执行
- allprojects  所有项目的统一配置，包括版本号
- subprojects  子项目的所有配置，依赖
- 多模块项目需要配置主程序入口

```groovy
//先执行构建脚本
buildscript {
    //引入gradle依赖配置文件
    apply from: "config.gradle"
    ext {
        springBootVersion = '2.3.2.RELEASE'
        springCloudVersion = 'Hoxton.SR8'
        springCloudAlibabaVersion = '2.2.3.RELEASE'
    }
    repositories {
        mavenLocal()
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}
//所有项目配置
allprojects {
    group 'com.anyu'
    version '1.0.0'
    apply plugin: 'java'
    apply plugin: 'idea' //idea 插件编译
    // 指定JDK版本
    sourceCompatibility = 11
    targetCompatibility = 11
    //指定编码格式
    tasks.withType(JavaCompile) {
        options.encoding = "UTF-8"
    }

    repositories {
        mavenLocal()
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        mavenCentral()
    }
}

//子项目配置
subprojects {
    //dependency-management 插件
    apply plugin: 'io.spring.dependency-management'
    apply plugin: 'org.springframework.boot'  //使用springboot插件

    // java编译的时候缺省状态下会因为中文字符而失败
    [compileJava, compileTestJava, javadoc]*.options*.encoding = 'UTF-8'
    dependencyManagement {
        imports {
            //spring bom helps us to declare dependencies without specifying version numbers.
            mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
            mavenBom "org.springframework.boot:spring-boot-dependencies:${springBootVersion}"
            mavenBom "com.alibaba.cloud:spring-cloud-alibaba-dependencies:${springCloudAlibabaVersion}"
        }
    }
    //所有子模块共有依赖
    dependencies {
        //依赖
        def deps = rootProject.ext.dependencies
        implementation fileTree(dir: 'libs', include: ['*.jar'])
        implementation 'org.springframework.boot:spring-boot-starter-validation'
        implementation 'org.springframework.boot:spring-boot-starter-web'
        annotationProcessor "org.springframework.boot:spring-boot-configuration-processor"
        compileOnly deps.lombok
        annotationProcessor deps.lombok
        testImplementation 'org.springframework.boot:spring-boot-starter-test'
    }
    jar {
        manifest.attributes provider: 'gradle'
    }
}

```



#### **1.1 创建子模块**

1. 删除除src、build.gradle 之外的所以有文件，build.gradle只保留dependencies

2. 子模块引入子模块，`implenmention project(":projectName")`

3. 被引入的子模块，作为jar，在build.gradle文件中添加 

   ```groovy
   //是生成jar包，不生成可执行jar包
   jar.enabled = true
   bootJar.enabled = false
   ```

#### 1.2 springcloud、springboot对应版本

[版本依赖]:https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E

![](C:\Users\Administrator\Desktop\note\微服务（gradle+springcloud +alibaba）\捕获.PNG)

#### 1.3 nacos 使用

- 程序入口加原生的`@EnableDiscovery`
- yaml

```yaml
server:
  port: 9060
  servlet:
    application-display-name: video-service
spring:
  application:
    name: video-service-provider
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848 #nacos 服务地址
        username: nacos
        password: nacos
```

#### 1.4 单个应用起多个实例

![](C:\Users\Administrator\Desktop\note\微服务（gradle+springcloud +alibaba）\多实例启动配置.PNG)

#### 1.5 openFeign 使用

> ```groovy
> 依赖 ："open_feign"     : 'org.springframework.cloud:spring-cloud-starter-openfeign',
> ```

1. 在客户端是创建OpenFeign 的请求接口层，

   ```java
   //如果springboot设置了server.context-path 则要指定path属性
   //name 属性 为application.name
   @FeignClient(name = "cache-service",path = "/cache") 
   public interface CacheFeignClient {
       @GetMapping("test")
        int test();
   }
   ```

### 2.0 Sentinel 使用

> ```java
> 'com.alibaba.cloud:spring-cloud-starter-alibaba-sentinel',//依赖
> 'org.springframework.boot:spring-boot-starter-actuator' //springboot 监测
> ```

```yaml
management:
  endpoints:
    web:
      exposure:
        include: '*' #sentinel 配置 暴露所有
```

**访问项目：localhost://xxxx/xx/actuator/sentinel,可知道**

1. `https://github.com/alibaba/Sentinel/releases` **下载Alibaba的sentinel 的dashbord图形界面**
2. 运行 `java -jar 该jar包 -Dserver.port = 9100`，监控端口8080
3. 访问localhost://8080 ,密码 账户都是 sentinel

```yaml
  #sentinel dashboard 地址
    sentinel:
      transport:
        dashboard: localhost:8080
```

#### 2.1 熔断机制

<img src="C:\Users\Administrator\Desktop\note\微服务（gradle+springcloud +alibaba）\熔断机制.PNG" style="zoom:75%;" />

- 慢调用

  ![](C:\Users\Administrator\Desktop\note\微服务（gradle+springcloud +alibaba）\慢调用.PNG)

- 异常比例

  ![](C:\Users\Administrator\Desktop\note\微服务（gradle+springcloud +alibaba）\异常比例.PNG)

- 异常数

  ![](C:\Users\Administrator\Desktop\note\微服务（gradle+springcloud +alibaba）\异常数.PNG)

#### 2.2 Sentinel 对OpenFeign限流

​	yaml配置中加`fein.sentinel.enabeled = true`

#### 2..3 gateway 使用





1. 引入gateway依赖、nacos依赖；排除web依赖。

2. 配置路由规则

   ```yaml
       gateway: #
         discovery:
           locator:
             enabled: false #智能路由
         routes: #手动配置节点
           - id: article-service-route #节点唯一id【数组形式】
             uri: lb://article-service #获取nacos，article-service上的可用节点
             predicates:
               - Path=/article/** #将该路径的的URL转发给article-service节点 
               #9000/artice/test => article-service/article/test
             filters:
               - StripPrefix = 1 #忽略掉第几层前缀
           - id: cache-service-route
             uri: lb://cache-service
             predicates:
               - Path=/cache/**
   ```

### 3.0 微服务认证方案

![](C:\Users\Administrator\Desktop\note\微服务（gradle+springcloud +alibaba）\微服务认证方案.PNG)

3.1 JWT(JAVA Web Json)

- https://github.com/jwtk/jjwt github的地址
- gradle 依赖
 ```groovy
dependencies {
    compile 'io.jsonwebtoken:jjwt-api:0.11.2'
    runtime 'io.jsonwebtoken:jjwt-impl:0.11.2',
            // Uncomment the next line if you want to use RSASSA-PSS (PS256, PS384, PS512) algorithms:
            //'org.bouncycastle:bcprov-jdk15on:1.60',
            'io.jsonwebtoken:jjwt-jackson:0.11.2' // or 'io.jsonwebtoken:jjwt-gson:0.11.2' for gson
}
 ```

- 


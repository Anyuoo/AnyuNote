## Mybatis-Plus

###  搭建环境

1. **导入依赖**

   ```xml
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus</artifactId>
        <version>3.3.0</version>
   </dependency>
   <!--springboot-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.3.2</version>
   </dependency>
   ```

2. **创建数据库导入数据**

   ```mysql
   DROP TABLE IF EXISTS user;
   
   CREATE TABLE user
   (
   	id BIGINT(20) NOT NULL COMMENT '主键ID',
   	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
   	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
   	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
   	PRIMARY KEY (id)
   );
   
   DELETE FROM user;
   
   INSERT INTO user (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

3. **springboot 数据源配置**

   ```yml
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       #serverTimezone=GMT%2B8 MySQL8配置时区
       url: jdbc:mysql://localhost:3306/meta?useUnicode=true&characterEncoding=utf-
       8&useSSL=false&serverTimezone=GMT%2B8
       username: root
       password: 123456
       type: org.apache.ibatis.datasource.pooled.PooledDataSource
   ```

4. **Mapper接口实现BaseMapper**

   ```java
   public interface UserMapper extends BaseMapper<User> {
   }
   //开启mapper组件扫描
   @MapperScan("pers.anyu.mybatisplus.mapper")
   ```

5. **直接使用**

   ```java
   List<User> users = userMapper.selectList(null);
   users.forEach(System.out::println);
   ```

6. **日志配置**

   ```yaml
   mybatis-plus:
     configuration:
       log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
   ```

### insert

> insert

```java
//自动生成id
@Test
void insert() {
    User user = new User();
    user.setName("zhangsan");
    user.setAge(11);
    user.setEmail("282@qq.com");
    userMapper.insert(user);
}
```

> 数据库插入的id的默认值为：全局唯一id

### **主键生成策略**

```java
public enum IdType {
    AUTO(0), //自增，数据库需要设自增
    NONE(1), //未设置主键
    INPUT(2), //手动输入
    ASSIGN_ID(3),
    ASSIGN_UUID(4),
    /** @deprecated */
    @Deprecated
    ID_WORKER(3), //全局唯一id
    /** @deprecated */
    @Deprecated
    ID_WORKER_STR(3),
    /** @deprecated */
    @Deprecated
    UUID(4);
}
```

- **snowflake雪花算法**
- **数据库自增id**

###   update -时间自动填充



1. **数据库需要有更新时间，和创建时间字段**

2. **使用mp的@TableFields**

```java
//fill属性的枚举
public enum FieldFill {
    DEFAULT, //默认不处理
    INSERT, //插入时填充字段
    UPDATE, //更新时填充字段
    INSERT_UPDATE; //插入和更新时填充字段
    private FieldFill() {
    }
}

```

3. **编写配置类**

   ```java
   @Slf4j
   @Component
   public class MyMetaObjectHandler implements MetaObjectHandler {
   
       @Override
       public void insertFill(MetaObject metaObject) {
           log.info("start insert fill ....");
           this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now()); // 起始版本 3.3.0(推荐使用)
           this.fillStrategy(metaObject, "createTime", LocalDateTime.now()); // 也可以使用(3.3.0 该方法有bug请升级到之后的版本如`3.3.1.8-SNAPSHOT`)
           /* 上面选其一使用,下面的已过时(注意 strictInsertFill 有多个方法,详细查看源码) */
           //this.setFieldValByName("operator", "Jerry", metaObject);
           //this.setInsertFieldValByName("operator", "Jerry", metaObject);
       }
   
       @Override
       public void updateFill(MetaObject metaObject) {
           log.info("start update fill ....");
           this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now()); // 起始版本 3.3.0(推荐使用)
           this.fillStrategy(metaObject, "updateTime", LocalDateTime.now()); // 也可以使用(3.3.0 该方法有bug请升级到之后的版本如`3.3.1.8-SNAPSHOT`)
           /* 上面选其一使用,下面的已过时(注意 strictUpdateFill 有多个方法,详细查看源码) */
           //this.setFieldValByName("operator", "Tom", metaObject);
           //this.setUpdateFieldValByName("operator", "Tom", metaObject);
       }
   }
   ```

   ### 分页插件

   ```java
   //Spring boot方式
   @EnableTransactionManagement
   @Configuration
   @MapperScan("com.baomidou.cloud.service.*.mapper*")
   public class MybatisPlusConfig {
   
       @Bean
       public PaginationInterceptor paginationInterceptor() {
           PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
           // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
           // paginationInterceptor.setOverflow(false);
           // 设置最大单页限制数量，默认 500 条，-1 不受限制
           // paginationInterceptor.setLimit(500);
           // 开启 count 的 join 优化,只针对部分 left join
           paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
           return paginationInterceptor;
       }
   }
   
   
   public class MybatisPlusConfig {
       @Bean
       public PaginationInterceptor paginationInterceptor() {
           return  new PaginationInterceptor();
       }
   }
   //第一页，每页5条数据
   //参数一：当前页
   //参数二：页面大小
   Page<User> page = new Page(1,5);
   userMaper.selectPage(page,null);
   page.getRecoder();
   ```

   ### 逻辑删除

   ```yaml
   mybatis-plus:
     global-config:
       db-config:
         logic-delete-field: flag  #全局逻辑删除字段值 3.3.0开始支持，详情看下面。
         logic-delete-value: 1 # 逻辑已删除值(默认为 1)
         logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
   ```

   ```java
   @TableLogic
   private Integer deleted;
   ```

   
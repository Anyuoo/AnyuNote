# Community项目

## 后端

### 1.0 问题和解决方案

#### 1.1 引入mybatis-plus 所涉及的方案

1. 数据库使用自带的id 生成器，``IdType.ASSIGN_ID`

2. service层设计，直接使用mybatis-plus自带的I`Service<T>`接口

   ```java
   public interface UserService extends IService<User> {
   }
   //
   @Service
   public class UserServiceImpl extends ServiceImpl<UserMapper,User> implements UserService {
        public User getUserById(Long id) {
           return this.getById(id);
       }
   }
   ```

3. ==3.3.2 版本使用失败，无法生成时间（版本升级后优化）==

   ```java
   @Override
   public void insertFill(MetaObject metaObject) {
       log.info("start insert fill ....");
           this.strictInsertFill(metaObject, "createTime", LocalDateTime.class,
                                 LocalDateTime.now()); // 起始版本 3.3.0(推荐使用)
           this.fillStrategy(metaObject, "createTime", LocalDateTime.now()); // 也可以使用(3.3.0 该方法有bug请升级到之后的版本如`3.3.1.8-SNAPSHOT`)
           /* 上面选其一使用,下面的已过时(注意 strictInsertFill 有多个方法,详细查看源码) */
           //this.setFieldValByName("operator", "Jerry", metaObject);
           //this.setInsertFieldValByName("operator", "Jerry", metaObject);
       }
   ```

   









## 前端
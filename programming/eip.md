## 项目

项目是一个微服务架构，包括小程序、认证授权、缓存、表单、cas管理、日志、消息队列、



### 1、controller

#####  1.1. **controller 类都继承于BaseApiController 与BaseWebController,其都继承与BaseController**

>BaseController 封装了web 中的resqponse ,request 数据获取，植入获取用户、缓存、用户适配器等，常用的web方法

1. **api  json数据交互的controller，都继承与BaseApiController**

   ![](C:\Users\Administrator\Desktop\note\eip\BaseApiConctroller继承.PNG)

   ```java
   /**
    * @Description API接口基类，所有返回json格式数据应该继承该类
    * @Author peter
    * @Date 2019/1/31 10:02 AM
    */
   @ResponseBody //返回的信息json 格式 controller层 可以直接使用 @Controller，无需加@RestController
   public abstract class BaseApiController extends BaseController implements SerializationViews, ILocalizer {
   
       //jr 方法对controller层的结果进行统一格式封装
       protected <T> JsonResult<T> jr(){
           return jr(GlobalConstants.SUCCESS, "");
       }
       //... jr 方法的重载
   }
   ```

2. **视图跳转的controller，都继承于BaseWebController**

   ![](C:\Users\Administrator\Desktop\note\eip\BaseWebController继承.PNG)

   ```java
   **
    * @Description web页面controller超类，所有跳转页面应该继承该类
    * @Author peter
    * @Date 2019/1/31 10:03 AM
    */
   public abstract class BaseWebController extends BaseController {
   
       //view方法对controller页面跳转及数据进行统一封装
   	protected ModelAndView view() {
   		return new ModelAndView();
   	}
       //... view 方法的重载
   }
   ```

   

### warn

![](C:\Users\Administrator\Desktop\note\eip\error1.PNG)
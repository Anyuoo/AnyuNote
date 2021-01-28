# Graphql

## 1. graphql-spring-boot-starter   graphql-java-tools

### graphql-java-tools

[graphql-java-tools](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fgraphql-java-kickstart%2Fgraphql-java-tools)能够从GraphQL的模式定义*.graphqls文件构建出对应的Java的POJO类型对象（graphql-java-tools将读取classpath下所有以*.graphqls为后缀名的文件，创建GraphQLSchema对象。），同时为我们屏蔽了graphql-java的底层细节，它本身依赖 graphql-java。

### graphql-spring-boot-starter

[graphql-spring-boot-starter](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Foembedler%2Fgraphql-spring-boot)是辅助SpringBoot接入GraphQL的库，它本身依赖graphql-java和[graphql-java-servlet](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fgraphql-java-kickstart%2Fgraphql-java-servlet)（将GraphQL服务发布为通过HTTP可访问的Web服务，封装了一个GraphQLServlet接收GraphQL请求，并提供[Servlet Listeners](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fgraphql-java-kickstart%2Fgraphql-java-servlet)功能）

```xml
<dependency>
  <groupId>com.graphql-java-kickstart</groupId>
  <artifactId>graphql-spring-boot-starter</artifactId>
  <version>7.0.1</version>
</dependency>
<dependency>
  <groupId>com.graphql-java-kickstart</groupId>
  <artifactId>graphiql-spring-boot-starter</artifactId>
  <version>7.0.1</version>
</dependency>
<dependency>
  <groupId>com.graphql-java-kickstart</groupId>
  <artifactId>voyager-spring-boot-starter</artifactId>
  <version>7.0.1</version>
</dependency>
<dependency>
  <groupId>com.graphql-java-kickstart</groupId>
  <artifactId>graphql-spring-boot-starter-test</artifactId>
  <version>7.0.1</version>
  <scope>test</scope>
</dependency>

```

```JAVA
class BookResolver implements GraphQLResolver<Book> /* This class is a resolver for the Book "Data Class" */ {

    private AuthorRepository authorRepository;

    public BookResolver(AuthorRepository authorRepository) {
        this.authorRepository = authorRepository;
    }

    public Author author(Book book) {
        return authorRepository.findById(book.getAuthorId());
    }
}
```

```groovy
dependencies {
    compile 'com.graphql-java-kickstart:graphql-spring-boot-starter:7.0.1'
    compile 'com.graphql-java-kickstart:graphiql-spring-boot-starter:7.0.1'
    compile 'com.graphql-java-kickstart:voyager-spring-boot-starter:7.0.1'
    testCompile 'com.graphql-java-kickstart:graphql-spring-boot-starter-test:7.0.1'
}
```

## 2. gradle

`gradlew` 绑定一个gradle版本

`daemon` 

> groovy 运行在jvm的脚本语言，强类型

```groovy
List l = []
Map m = [a: 1]
class A{
	int a
    void printA(){
        println(a)
    }
}

//
new A(a: 1).printA()
//GROOVY: MOP 反射进行调用
def a = new A(a: 1)
invokeMethod(a,"printa",[])

//闭包
def closure = {it + 1}
closure()

//方法的调用可以省略括号
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation('org.springframework.boot:spring-boot-starter-test') {
        exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
    }

}

dependencies ({
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation('org.springframework.boot:spring-boot-starter-test') {
        exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
    }
})

println "hello world"

//语法糖 
apply([plugin: 'java'])
//map 去中括号
apply(plugin: 'java')
//方法不引起歧义，去掉
apply plugin: 'java'
```



## 3. resolver

```java
GraphQLQueryResolver//查询
GraphQLMutationResolver//修改
GraphQLResolver<T>// 针对某个字段
```

## 4.0 scalar

`https://github.com/graphql-java/graphql-java-extended-scalars`



## 5.0 validation

> 支持springboot的validation

```java
//字段加验证规则
@NoNull
private String name;
//方法参数加验证,类上加注解
@Validated
class E{
   public void exp(@Valid String name){
       
   }
}

```


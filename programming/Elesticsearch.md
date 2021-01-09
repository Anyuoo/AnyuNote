```yaml
title: Elesticsearch
categories:
  -  Elesticsearch
    tags:
  -  Elesticsearch 
    date: 2020-05-22 14:46:35
```



## Elesticsearch

#### elasticsearch安装配置

1. > elasticsearch的目录下config/elasticsearch.yaml

   ```yaml
   # Use a descriptive name for your cluster:
   #
   cluster.name: application
   # Path to directory where to store the data (separate multiple locations by comma):
   path.data: E:/Data/Elasticsearch/data
   #
   # Path to log files:
   #
   path.logs: E:/Data/Elasticsearch/logs
   
   #解决跨域问题 elasticsearch-head
   http.cors.enabled: true
   http.cors.allow-origin: "*"
   ```

   

2. > elaseticsearch-head安装配置（**elasticsearch可视化界面**）

   ```shell
   git clone git://github.com/mobz/elasticsearch-head.git
   cd elasticsearch-head
   npm install
   npm run start
   ```

3. > 安装kibana

   

#### elaseticsearch核心概念

1. 索引
2. 字段类型（mapping）
3. 文档（documents）
4. 分片

![](C:\Users\Administrator\Desktop\note\Elesticsearch\1.PNG)

#### ik分词器

> 两种分词算法

```yaml
ik_smart : 最少切分
ik_max_word: 最细腻度划分，穷尽词库可能
```

>ik分词器增加自己的配置

```xml
编写自定义词典 
ext_dict 格式：utf-8
添加到配置
<entry key="ext_dict"></entry>
```



#### Rest风格

> 说明

<img src="C:\Users\Administrator\Desktop\note\Elesticsearch\2.PNG" style="zoom:75%;" />

> 使用

```json
PUT /索引名/类型/文档id
{请求体}
```

<img src="C:\Users\Administrator\Desktop\note\Elesticsearch\3.PNG" style="zoom:100%;" />

自动建立索引，数据也添加成功了

**1.指定字段类型，创建规则**

![](C:\Users\Administrator\Desktop\note\Elesticsearch\4.PNG)

**2.通过GET得到具体信息**

```json
GET /索引名
```

**3.POST修改文档（推荐）**

```json
POST /索引/类型/文档id/_update
{"doc":{
	"name": "value"
}}
```

![](C:\Users\Administrator\Desktop\note\Elesticsearch\5.PNG)

**4.删除**

```json
DELETE /索引/类型/文档id  根据路径删除索引或文档
```

#### 文档操作

> 简单搜索

```json
GET /索引/类型/_search?q=key:valuehttp://localhost:5601
```

![](C:\Users\Administrator\Desktop\note\Elesticsearch\6.PNG)

```yaml
_score: 权重得分，搜索数据的匹配程度
max_score: 搜索最高权重得分
hit: 索引和文档信息，查询总数，所有的信息
_source: 索引存储的信息
```

> 复杂查询（推荐）

```json
GET /索引/类型/_search
{
	//参数体
	"query":{
        //匹配条件
		"match":{
			"name":"李四"
		}
        //查询结果过滤
        "_source": ["name","age"]
    	//排序
    	"sort": [{
    		"age": {
    			"order": "desc"
			}
		}]
		//分页 limit offset,limit
		"from": 0,
		"size": 10
	}
}
```

```json
GET /索引/类型/_search
{
	"query":{
		"boolean": {
            //must_not must：精确匹配(and 交集) ;should: 选择性匹配(or 并集) 
			"must" :[{
				"match":{
					"name": "张三"
				},
				"math":{
					"age": 14
				}
			}]
		}
	}
}
```

```json
GET /索引/类型/_search
{
	"query":{
		"boolean": { 
			"must" :[{
				"match":{
					"name": "张三"
				}
			}],
			//过滤 age在3~45 (gt: 大于 gte：大于等于 lt：小于)
 			"filter":{
				"range":{
					"age":{
						gte: 3,
						lte: 45
					}
				}
			}
		}
	}
}
```

> 精确查询

- term ，查询是直接通过**倒排索引**查询指定的词条进行精确查找
- match，会使用分词器，先分析文档，然后通过分析文档查询

```yaml
keyword： 字段不会被分词器解析 可以使用倒排索引
```

> 高亮查询

```json
GET /索引/类型/_search
{
	"query":{
		"math":{
			"name": "李四"
		}
		"highlight": {
        	//自定义高亮规则
        	"pre_tags": "<p class='key' style='color:red'>",
        	"post_tags": "</p>"
        	//高亮的字段
			"fields": {
				"name":{}
			}
		}
	}
}
```

#### Springboot 集成elasticsearch

1. ##### 	编写配置类 

```Java
@Configuration
public class ElasticsearchConfig {
    @Bean("restHighLevelClient")
    public RestHighLevelClient getRestHighLevelClient(){
        RestHighLevelClient client = new RestHighLevelClient(
                RestClient.builder(
                    //127.0.0.1本地IP，端口为默认，协议为http
                        new HttpHost("127.0.0.1", 9200, "http")));
         return client; 
    }
}
```

2. ##### spring boot自动配置

```Java
 @Configuration(
        proxyBeanMethods = false
    )
    @ConditionalOnClass({RestHighLevelClient.class})
    static class RestHighLevelClientConfiguration {
        RestHighLevelClientConfiguration() {
        }

        @Bean
        @ConditionalOnMissingBean
        RestHighLevelClient elasticsearchRestHighLevelClient(RestClientBuilder restClientBuilder) {
            return new RestHighLevelClient(restClientBuilder);
        }

        @Bean
        @ConditionalOnMissingBean
        RestClient elasticsearchRestClient(RestClientBuilder builder, ObjectProvider<RestHighLevelClient> restHighLevelClient) {
            RestHighLevelClient client = (RestHighLevelClient)restHighLevelClient.getIfUnique();
            return client != null ? client.getLowLevelClient() : builder.build();
        }
    }
```

3. ##### api测试

> 创建索引

```java
//创建索引请求
CreateIndexRequest request = new CreateIndexRequest("test");
//客户端执行indices请求，执行后获得响应
CreateIndexResponse createIndexResponse =
    restHighLevelClient.indices().create(request, RequestOptions.DEFAULT);
System.out.println(createIndexResponse);
```

> 索引是否存在

```java
GetIndexRequest indexRequest = new GetIndexRequest("test");
boolean exists = restHighLevelClient.indices().exists(indexRequest,RequestOptions.DEFAULT);	
```

> 删除索引

```java
DeleteIndexRequest request = new DeleteIndexRequest("test");
		AcknowledgedResponse delete = restHighLevelClient.indices().delete(request, RequestOptions.DEFAULT);
```

> 插入文档

```Java
//创建对象
User user = new User("张三", 14);
//创建请求
IndexRequest indexRequest = new IndexRequest("test");
indexRequest.id("1");
indexRequest.timeout(TimeValue.timeValueSeconds(1));
//将对象转为json数据
indexRequest.source(JSON.toJSON(user), XContentType.JSON);
//客户端发送请求，并响应结果
IndexResponse index = restHighLevelClient.index(indexRequest, RequestOptions.DEFAULT);
```

> 判断文档是否存在

```java
GetRequest request = new GetRequest("test", "1");
//不获取_source的上下文
request.fetchSourceContext(new FetchSourceContext(false));
boolean exists = restHighLevelClient.exists(request, RequestOptions.DEFAULT);
```

> 查询文档

```java
GetRequest request = new GetRequest("test", "1");
GetResponse documentFields = restHighLevelClient.get(request, RequestOptions.DEFAULT);
```

> 更新文档

```java
UpdateRequest updateRequest = new UpdateRequest("test","1");
User user = new User("张三", 45);
updateRequest.doc(JSON.toJSON(user),XContentType.JSON);
updateRequest.timeout(TimeValue.timeValueSeconds(1));
UpdateResponse update = restHighLevelClient.update(updateRequest, RequestOptions.DEFAULT);
```

> 查询文档

```Java
SearchRequest searchRequest = new SearchRequest("test");
//构建搜索条件
SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
//可以通过QueryBuilders工具类构造查询条件 TermQueryBuilder 精确查询 HighlightBuilder 高亮查询条件
MatchQueryBuilder queryBuilder = QueryBuilders.matchQuery("name","张三");
sourceBuilder.query(queryBuilder);
//设置分页
sourceBuilder.from(0);
sourceBuilder.size(10);
searchRequest.source(sourceBuilder);
SearchResponse search = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
System.out.println(JSON.toJSONString(search.getHits()));
```


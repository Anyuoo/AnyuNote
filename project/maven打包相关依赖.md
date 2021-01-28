```xml
<!--此插件生成的origin-前缀的jar是原始jar，不带以来jar的。-->

<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>util.Microseer</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.hisense.main.Main</mainClass>   //主程序入口类
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

> 打包的jar要运行起来需要设置主类，否则java -jar跑不起来，报“no main manifest attribute”。需要使用maven-jar-plugin插件，配置主程序入口类：

> 两种方式除了配置pom.xml之外，还需要在maven工程中配置Run Configurations。shade插件命令行为：shade:shade -f pom.xml，assembly插件命令行为：assembly:assembly -f pom.xml。双击运行该配置后得到对应的包。
1. 调整 Maven 依赖，不再依赖 spring-boot-starter-web，转而依赖最基础的 spring-boot-starter 
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
</dependencies>
```
2. 此时按照原先的方式启动 SpringBootApplication 会发现启动加载完之后会立即退出，这时需要做点工作让主线程阻塞让程序不退出 
```java
@SpringBootApplication
public class SampleApplication implements CommandLineRunner {
    public static void main(String[] args) throws Exception {
        SpringApplication.run(SampleApplication.class, args);
    }
    @Override
    public void run(String... args) throws Exception {
        Thread.currentThread().join();
    }
}
```
> SpringBoot 应用程序在启动后，会遍历 CommandLineRunner 接口的实例并运行它们的 run 方法。也可以利用 @Order 注解（或者实现Order接口）来规定所有 CommandLineRunner 实例的运行顺序

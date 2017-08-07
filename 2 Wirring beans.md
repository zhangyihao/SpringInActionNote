Spring提供了三种方式装配bean：
- XML中显示配置
- Java代码中显示配置
- 自动装配

# 自动装配 #
自动装配的含义为：Spring自动发现一个bean，并适配这个bean依赖的其它bean，然后将依赖的bean自动注入到该bean中。
因此，Spring从两个角度实现自动装配：
- 组件扫描（component scanning）:Spring会自动发现应用上下文中所创建的bean。
- 自动装配（autowiring）：Spring自动满足bean之间的依赖。

## 配置bean ##
使用`@Component`注解声明一个bean，表示这是一个可以被Spring实例化并管理的bean对象。
每个被Spring管理的bean都有一个ID，默认ID为类名，其中类名中第一个字母小写，如类名为TestUtil，那么默认ID为testUtil。如果需要给bean指定ID,需要将名称当作参数传给`@Component`注解，例如给TestUtil指定ID为testutil,代码如下所示：
```java
@Component("testutil")
public class TestUtil {
}
```

## 配置自动扫描 ##
Spring的自动扫描默认是关闭的，需要显示配置一下。有两种配置方式：
1. 使用`@ComponentScan`注解。
2. 在XML中配置`<context:component-scan>`标签。

### @ComponentScan注解 ###
这个注解没有配置任何属性时，默认会以配置这个注解的bean所在的包作为基础包（base package）进行扫描，扫描范围为基础包及其子包下的所有配置`@Component`注解的bean。
如果想指定扫描的包，可以在@Component的value属性中指定声明的包名称，如下所示：
```java
@ComponentScan("com.test")
public calss TestConfig {
}
```
如果想更加清晰的表名你所设置的是基础包，可以通过basePackages属性进行配置：
```java
@ComponentScan(basePackages="com.test")
public calss TestConfig {
}
```
如果需要指定多个基础包，只需要将basePackages属性设置为数组即可：
```java
@ComponentScan(basePackages={"com.test","com.test1"})
public calss TestConfig {
}
```
如上所示，配置基础包时用的是String类型的值，这是类型不安全的，可以使用直接指定类或接口的方式替代，如下所示：
```java
@ComponentScan(basePackageClasses={TestConfig.class,Test1Config.class})
public calss TestConfig {
}
```

## 配置自动装配 ##
使用`@Autowired`注解，表示一个bean的属性需要自动装配。
`@Autowired`注解可以用在类的构造方法、setter方法或者其它任意的方法上。Spring会为方法的参数注入相对应的bean，如果有且只有一个bean适配到，那么这个bean会被注入进去，否则抛出异常。为了避免异常，可以设置Autowired注解的required属性为false。
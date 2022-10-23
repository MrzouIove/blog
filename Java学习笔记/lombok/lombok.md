j

#  **一、Lombok的简介**

> 是一个在Java开发过程中用注解的方式，简化了 JavaBean 的编写，避免了冗余和样板式代码而出现的插件，让编写的类更加简洁。
>
> 以@Data为例
>
> 在写实体类时，经常需要先定义变量



在写实体类时，经常需要先定义变量

```java

private int rid;
private String rname;

## 手写或者自动生成，get、set、ToString方法等等操作
public int getRid() {
return rid;
}
public void setRid(int rid) {
this.rid = rid;
}
public String getRname() {
return rname;
}
public void setRname(String rname) {
this.rname = rname;
}
```

```java
private int rid;
private String rname;

## 手写或者自动生成，get、set、ToString方法等等操作
public int getRid() {
return rid;
}
public void setRid(int rid) {
this.rid = rid;
}
public String getRname() {
return rname;
}
public void setRname(String rname) {
this.rname = rname;
}
```

而通过使用Lombok则可以大大减少人工操作的方面，只使用@Data 注解即可

```java
import lombok.Data;
@Data 
public class Role { 
	private int rid;
    private String rname; 
    private String level;
    }
```



Lombok注解的简介

#  **二、 Lombok的安装**

> 先在idea中安装Lombok插件
>
> File —> Settings —> Plugins —> Browse repositories —> 搜索lombok
>
> 或者在项目pom.xml中添加相关依赖

```xml
<dependency> 
    <groupId>org.projectlombok
    </groupId> <artifactId>lombok</artifactId>
     <version>1.16.10</version> 
 </dependency>
```


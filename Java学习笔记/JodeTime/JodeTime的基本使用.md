# Jode-Time 



## **Jode-Time 介绍**

> 任何企业应用程序都需要处理时间问题。应用程序需要知道当前的时间点和下一个时间点，有时它们还必须计算这两个
> 时间点之间的路径。使用 JDK 完成这项任务将非常痛苦和繁琐。
> 既然无法摆脱时间，为何不设法简化时间处理？现在来看看 Joda Time，一个面向 Java™ 平台的易于
> 使用的开源时间/日期库。正如您在本文中了解的那样，JodaTime轻松化解了处理日期和时间的痛苦和繁琐。
>
> Joda-Time 令时间和日期值变得易于管理、操作和理解。事实上，易于使用是 Joda 的主要设计目标。其他目标包括可扩展性、完整的特性集以及对多种日历系统的支持。
> 并且 Joda 与 JDK 是百分之百可互操作的，因此您无需替换所有 Java 代码，只需要替换执行日期/时间计算的那部分代码。
> Joda-Time提供了一组Java类包用于处理包括ISO8601标准在内的date和time。可以利用它把JDK Date和Calendar类完全替换掉，而且仍然能够提供很好的集成。

## 使用JodeTime

```xml
<!--日期时间工具-->
<dependency>
    <groupId>joda-time</groupId>
    <artifactId>joda-time</artifactId>
    <version>${jodatime.version}</version>
</dependency>
```



## 创建某一时刻

> 考虑创建一个用时间表示的某个随意的时刻 — 比如，2000 年 1 月 1 日 0 时 0 分。

```java
// 使用calendar创建
Calendar calendar = Calendar.getInstance();
calendar.set(2000, Calendar.JANUARY, 1, 0, 0, 0);

// 使用jodeTime创建
DateTime dateTime = new DateTime(2000, 1, 1, 0, 0, 0, 0);
```



## 格式化

```java
// 使用calendar格式化
Calendar calendar = Calendar.getInstance();
calendar.set(2000, Calendar.JANUARY, 1, 0, 0, 0);

SimpleDateFormat sdf =
new SimpleDateFormat("E MM/dd/yyyy HH:mm:ss.SSS");

string s = sdf.format(calendar.getTime())
    
//使用jodeTime格式化
DateTime dateTime = new DateTime(2000, 1, 1, 0, 0, 0, 0);
String s = dateTime.toString("yyyy/MM/dd")
```

## 日期比较

```java
DateTime d1 = new DateTime("2012-02-01"); 
DateTime d2 = new DateTime("2012-05-01"); 
//和系统时间比 
boolean b1 = d1.isAfterNow(); 
boolean b2 = d1.isBeforeNow(); 
boolean b3 = d1.isEqualNow();    

//和其他日期比 
 boolean f1 = d1.isAfter(d2); 
 boolean f2 = d1.isBefore(d2); 
 boolean f3 = d1.isEqual(d2); 
```


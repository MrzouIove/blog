# Mybatis的笔记



## Mybatis-plus的快速入门

### 1  导入xml文件

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.2</version>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

### 2  编写配置

```yaml
# DataSource Config
spring:
    datasource:
        #使用默认的数据源 com.mysql.cj.jdbc.Driver
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://127.0.0.1:3306/mybatis-plus?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
        username: root
        password: root
```

### 3 编写Mapper包和Pojo包

```java
/**
* Mapper包中注解,并在引导类中加载进入
* @MapperScan("包的地址")
*/
@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```



```java
package mybatisplusdemo.demo.pojo;

import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId   //标示主键
    private Long id;
    private String name;
    private Integer age;
    private String email;
}

```

### 4 测试

```java
package mybatisplusdemo.demo.pojo;

import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId   //标示主键
    private Long id;
    private String name;
    private Integer age;
    private String email;
}

```

## CRUD的基本操作

### 1  添加操作

```java
// 插入一条记录  成功返回1 
int insert(T entity);
```

```java
 @Test
    void saveOneUser(){
        int i=0;
        User user =new User();
        user.setAge(18);
        user.setName("张大方");
        user.setEmail("1546922461@123.com");
        user.setId(6l);
        try {
            i = userMapper.insert(user);
        }catch (Exception e){
            e.printStackTrace();
        }
        System.out.println(i);
    }
```

> ## **主键生成策略**
>
> ***分布式系统唯一id生成***

> ### 雪花算法
>
> SnowFlake 算法，是 Twitter 开源的分布式 id 生成算法。其核心思想就是：使用一个 64 bit 的 long 型的数字作为全局唯一 id。在分布式系统中的应用十分广泛，且ID 引入了时间戳，基本上保持自增的。
>
> 这 64 个 bit 中，其中 1 个 bit 是不用的，然后用其中的 41 bit 作为毫秒数，用 10 bit 作为工作机器 id，12 bit 作为序列号。
>
> 解决雪花算法：自己注解加上自增
>
> @TableId（type =IdType.AUTO）



### 2  更新操作

```java
// 根据 whereEntity 条件，更新记录
int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);
// 根据 ID 修改
int updateById(@Param(Constants.ENTITY) T entity);
```

```java
/**
     * 更新用户
     */
    @Test
    void UpdateOneUserById(){
        int i=0;
        User user =new User();
        user.setAge(38);
        user.setName("邹小方");
        user.setEmail("154685977@123.com");
        user.setId(3l);
        userMapper.updateById(user);
    }
```

### 3 删除操作

```java
    @Test
    void DeleteOneUserById(){
        /**
         * 根据主键删除  Long
         */
        int i = userMapper.deleteById(1l);
        if(i == 1){
            System.out.println("delete success！");
        }
    }

    @Test
    void DeleteOneUserByIdEntity(){
        /**
         * 根据实体类删除  Long
         */
        int i=0;
        User user =new User();
        user.setAge(38);
        user.setName("邹小方");
        user.setEmail("154685977@123.com");
        user.setId(3l);
        i = userMapper.deleteById(user);
        if(i == 1){
            System.out.println("delete success！");
        }
    }
```

### 4 查询操作

```java
T selectById(Serializable id)：使用场景为通过主键查询，只要该主键类型实现了Serialzable接口即可。
```

```java
  /**
     * 根据主鍵查找
     */
    @Test
    void FindOneUserById(){
        User user = userMapper.selectById(1l);
        System.out.println(user);
    }
```

```java
List selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList)：
    使用场景为通过主键的集合去批量查询，前提主键的类型实现了Serializable接口。
```

```java
    /**
     * 根据主鍵集合批量查找
     */
    @Test
    void FindOneUserByIds(){
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1l,2l,3l));

        users.stream().forEach((user)-> System.out.println(user));
//        users.stream().forEach(System.out::println);
    }

```

```java
List selectByMap(@Param(Constants.COLUMN_MAP) Map<String,Object> columnMap)：
    使用场景为传入一个Map集合，key为表字段，value为表字段值。
```

​    
​    

## 自动插入值

### 修改表中的字段

### 修改pojo中的代码

```java

    @TableField(value = "create_time",fill = FieldFill.INSERT )
    private Date createTime;

    @TableField(value = "update_time",fill = FieldFill.UPDATE)
    private Date updateTime;
```

### 配置handler

```java
package mybatisplusdemo.demo.handler;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class MyMateObjectHandler implements MetaObjectHandler {
    //mp实现添加方法时执行改方法
    @Override
    public void insertFill(MetaObject metaObject) {
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
    //mp更新该方法时执行
    @Override
    public void updateFill(MetaObject metaObject) {
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
}

```

## 乐观锁

> 为了解决问题的一种方法  --->   解决丢失更新问题
>
> - 取出数据时，获取当前的version
> - 更新时，带上这个version
> - 执行更新时，set version = new Version where version = oldVersion
> - 如果version不对，就更新失败

### 在mybatis-plus中使用乐观锁

1. 添加version字段

   ```sql
   alter table 'user' add colum 'version' int;
   ```

   

2. 实体类添加version字段并添加注解

   ```java
       @Version
       private Integer version;
   ```

   

3. 元对象处理器添加version的插件

   ```java
   /**
    * mp的配置类
    */
   @Configuration
   @MapperScan("mybatisplusdemo.demo.mapper")
   public class MybatisConfig {
    /**
        * 乐观锁插件
        * @return
        */
       @Bean
       public MybatisPlusInterceptor mybatisPlusInterceptor() {
           MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
           //乐观锁插件
           interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
           return interceptor;
       }
   }
   ```

   

## 悲观锁

> 串行，在一个读取数据时，其他用户不可对该数据操作

## 分页查询

### 配置分页插件

```java
@Configuration
@MapperScan("mybatisplusdemo.demo.mapper")
public class MybatisConfig {
    /**
     * 乐观锁插件
     * @return
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        //1 创建MybatisPlusInterceptor拦截器对象
        MybatisPlusInterceptor mpInterceptor=new MybatisPlusInterceptor();
        //2 添加乐观锁拦截器
        mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        //3 添加分页插件
        mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpInterceptor;
    }

}
```



### 通过selectPage实现分页

```java
 /**
     * 分页
     */
    @Test
    void FindPage(){
        // 把每一页的数据写固定
        final int PageSize = 3;
        // 1.创建page对象
        // 2.调用mp的分页查询方法
        // 3.把所有的分页数据封装到page对象中
        // 4.通过page对象获取分页数据
        Page<User> page =new Page<>(1,PageSize);  //第一个参数是当前页，第二个参数是每页的记录数
        userMapper.selectPage(page,null);
        System.out.println(page.getCurrent());   //获取当前页
        System.out.println(page.getSize());      //获取每一页的记录数
        System.out.println(page.getTotal());     //获取总记录数
        page.getRecords().stream().forEach(System.out::println);   //获取当前页的list集合
    }
```

## 删除（逻辑删除与物理删除）

### 逻辑删除

#### 默认

> mybatis-plus.global-config.db-config.logic-delete-value = 1
>
> mybatis-plus.global-config.db-config.logic-not-delete-value = 0

#### 步骤

1. 添加Default字段

   ```java
   
       @TableLogic  //逻辑删除的注解
       @TableField(fill = FieldFill.INSERT)
       private Integer deleted;
   ```

   

2. 添加逻辑删除的配件（3.X)版本以上不需要加

   ```yaml
   
   mybatis-plus:
       # 打印日志
       configuration:
           log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
   
       global-config:
           db-config:
               logic-not-delete-value: 0   # 默认没有删除是0
               logic-delete-field: 1       # 删除是1
   ```

3. 测试

   ```java
    /**
        * 逻辑删除
        */
       @Test
       void DeleteOneUserByLogin(){
           int i = userMapper.deleteById(2l);
           System.out.println(i);
   
       }
   ```

#### 配置完逻辑删除后实现（物理删除）

> 写SQL语句实现

### 物理删除

> 在数据库中完全删除

#### 通过id删除（单个删除）

```java
  @Test
    void DeleteOneUserById(){
        /**
         * 根据主键删除  Long
         */
        int i = userMapper.deleteById(1l);
        if(i == 1){
            System.out.println("delete success！");
        }
    }

    @Test
    void DeleteOneUserByIdEntity(){
        /**
         * 根据实体类删除  Long
         */
        int i=0;
        User user =new User();
        user.setAge(38);
        user.setName("邹小方");
        user.setEmail("154685977@123.com");
        user.setId(3l);
        i = userMapper.deleteById(user);
        if(i == 1){
            System.out.println("delete success！");
        }
    }
```

#### 批量删除

```java
  /**
     * 批量删除
     */
    @Test
    void DeleteOneUserByIdEntities(){
        int i = userMapper.deleteBatchIds(Arrays.asList(1l, 2l, 3l));
        if (i == 1){
            System.out.println("delete success!!!");
        }
    }
```

## MybatisPlus性能分析插件

> 三种环境：
>
> - dev :开发环境
> - test：测试环境
> - prod：生产环境

### 1.在MybatisConfig中导入性能分析插件

### 2.在yaml中配置

## Wapper查询

> Wrapper：条件查询的父类
>
> QueryWrapper：使用此类构建条件查询

### 使用方法

- 构造wrapper类
- 条件查询的参数

### Like(j模糊查询)

```java
  @Test
    void FindWrapperLike(){
        //1.构造wrapper查询器
        QueryWrapper<User> queryWrapper=new QueryWrapper<>();
        //2.构造wrapper的查询条件
        //查询类中name包含向的而且age小于40
        queryWrapper.like("name","向").lt("age" ,40);
        Page<User> page =new Page<>(1,PageSize);  //第一个参数是当前页，第二个参数是每页的记录数
        userMapper.selectPage(page, queryWrapper);
        List<User> records = page.getRecords();
        records.stream().forEach(System.out::println);
    }
```

### Eq(相等)

```java
 @Test
    void FindWrapperEq(){
        //1.构造wrapper查询器
        QueryWrapper<User> queryWrapper=new QueryWrapper<>();
        //2.构造wrapper的查询条件
        //查询类中name包含向的而且age小于40
        queryWrapper.eq("name","张三");
        Page<User> page =new Page<>(1,PageSize);  //第一个参数是当前页，第二个参数是每页的记录数
        userMapper.selectPage(page, queryWrapper);
        List<User> records = page.getRecords();
        records.stream().forEach(System.out::println);

    }
```

### Ne(不等于)

```java
 @Test
    void FindWrapperNe(){
        //1.构造wrapper查询器
        QueryWrapper<User> queryWrapper=new QueryWrapper<>();
        //2.构造wrapper的查询条件
        //查询类中name包含向的而且age小于40
        queryWrapper.ne("name","向小婷");
        Page<User> page =new Page<>(1,PageSize);  //第一个参数是当前页，第二个参数是每页的记录数
        userMapper.selectPage(page, queryWrapper);
        List<User> records = page.getRecords();
        records.stream().forEach(System.out::println);

    }
```

### ge,gt,le,lt(大于&&小于)between

```java
 //小于
    @Test
    void FindWrapperLt(){
        //1.构造wrapper查询器
        QueryWrapper<User> queryWrapper=new QueryWrapper<>();
        //2.构造wrapper的查询条件
        //查询类中name包含向的而且age小于40
        queryWrapper.lt("age","22");
        List<User> users = userMapper.selectList(queryWrapper);
        users.stream().forEach(System.out::println);

    }
    // between
    @Test
    void FindWrapperBetween(){
        //1.构造wrapper查询器
        QueryWrapper<User> queryWrapper=new QueryWrapper<>();
        //2.构造wrapper的查询条件
        //查询类中name包含向的而且age小于40
        queryWrapper.between("age",20,30);
        List<User> users = userMapper.selectList(queryWrapper);
        users.stream().forEach(System.out::println);

    }
```

### 升序与降序

```java
  @Test
    void FindWrapperBetween(){
        //1.构造wrapper查询器
        QueryWrapper<User> queryWrapper=new QueryWrapper<>();
        //2.构造wrapper的查询条件
        //查询类中name包含向的而且age小于40
        // 通过id降序
        queryWrapper.between("age",20,30).orderByDesc("id");
        List<User> users = userMapper.selectList(queryWrapper);
        users.stream().forEach(System.out::println);

    }
```

## 自定义SQL

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
    @Select("select * from user")
    List<User> findAll();
}

```



## 注意：(MybatisPlus3.4前后的区别)

### **Mybatisplus的3.4版本前后的区别描述**


> ### **MybatisPlusInterceptor**
>
> 从Mybatis Plus 3.4.0版本开始，不再使用旧版本的PaginationInterceptor ，而是使用MybatisPlusInterceptor。
>
> MybatisPlusInterceptor是一系列的实现InnerInterceptor的拦截器链，也可以理解为一个集合。可以包括如下的一些拦截器
>
> - 自动分页: PaginationInnerInterceptor（最常用）
>
> - 多租户: TenantLineInnerInterceptor
>
> - 动态表名: DynamicTableNameInnerInterceptor
>
> - 乐观锁: OptimisticLockerInnerInterceptor
>
> - sql性能规范: IllegalSQLInnerInterceptor
>
> - 防止全表更新与删除: BlockAttackInnerInterceptor
>



### **Mybatis Plus 3.4.0版本之前**


>
>   
>
>   ```java
>   @Configuration
>   @MapperScan(basePackages = {"com.guli.**.mapper"})
>   public class MybatisPlusConfig {
>   
>       @Bean
>       public PaginationInterceptor paginationInterceptor() {
>           PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
>           // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
>           // paginationInterceptor.setOverflow(false);
>           // 设置最大单页限制数量，默认 500 条，-1 不受限制
>           // paginationInterceptor.setLimit(500);
>           // 开启 count 的 join 优化,只针对部分 left join
>           paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
>           return paginationInterceptor;
>       }
>   }
>   ```



### **Mybatis Plus 3.4.0版本之后**

>
>    * ```java
>      @Configuration
>      @MapperScan("mybatisplusdemo.demo.mapper")
>      public class MybatisConfig {
>          /**
>      
>         * 乐观锁插件
>            @return
>                */
>               @Bean
>               public MybatisPlusInterceptor mybatisPlusInterceptor(){
>           //1 创建MybatisPlusInterceptor拦截器对象
>           MybatisPlusInterceptor mpInterceptor=new MybatisPlusInterceptor();
>           //2 添加乐观锁拦截器
>           mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
>           //3 添加分页插件
>           mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
>           return mpInterceptor;
>               }
>      }
>      ```



## MybatisPlus代码生成器

> 使用代码生成器，生成相关代码，在实际开发中使用较多



```xml
        <!-- velocity 模板引擎, Mybatis Plus 代码生成器需要 -->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
        </dependency>
```

### CodeGenertor代码

```java
package com.guli;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import org.junit.Test;

/**
 * @author
 * @since 2018/12/13
 */
public class CodeGenerator {

    @Test
    public void run() {

        // 1、创建代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 2、全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        // 最好些绝对路径
        gc.setOutputDir("E:\\zouzilu\\Java_code\\guli\\service\\service_edu" + "/src/main/java");
        gc.setAuthor("testjava");
        gc.setOpen(false); //生成后是否打开资源管理器
        gc.setFileOverride(false); //重新生成时文件是否覆盖
        gc.setServiceName("%sService");	//去掉Service接口的首字母I
        gc.setIdType(IdType.ID_WORKER_STR); //主键策略  ID_WORKER Long
        gc.setDateType(DateType.ONLY_DATE);//定义生成的实体类中日期类型
        gc.setSwagger2(true);//开启Swagger2模式

        mpg.setGlobalConfig(gc);

        // 3、数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/guli");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("root");
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);

        // 4、包配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("eduservice"); //模块名
        pc.setParent("com.guli");
        pc.setController("controller");
        pc.setEntity("entity");
        pc.setService("service");
        pc.setMapper("mapper");
        mpg.setPackageInfo(pc);

        // 5、策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setInclude("edu_teacher");
        strategy.setNaming(NamingStrategy.underline_to_camel);//数据库表映射到实体的命名策略
        strategy.setTablePrefix(pc.getModuleName() + "_"); //生成实体时去掉表前缀
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);//数据库表字段映射到实体的命名策略
        strategy.setEntityLombokModel(true); // lombok 模型 @Accessors(chain = true) setter链式操作
        strategy.setRestControllerStyle(true); //restful api风格控制器
        strategy.setControllerMappingHyphenStyle(true); //url中驼峰转连字符
        mpg.setStrategy(strategy);

        // 6、执行
        mpg.execute();
    }
}

```

### 出错 

> The server time zone value '?й???????' is unrecognized or represents more than one time zone

> 方案1、在项目代码-数据库连接URL后，加上 ?serverTimezone=UTC（注意大小写必须一致）

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/guli?serverTimezone=UTC
```

> 方案2、在mysql中设置时区，默认为SYSTEM（推荐）
> set global time_zone=’+8:00’

```sql
mysql> set global time_zone='+8:00';
Query OK, 0 rows affected (0.01 sec)

```


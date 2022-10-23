# Jpa

---

> JPA （Java Persistence API）Java持久化API。是一套Sun公司Java官方制定的ORM 方案,是规范，是标准 ，sun公司自己并没有实现
> ORM（Object Relational Mapping）对象关系映射。




##  ORM 
> 在操作数据库之前，先把数据表与实体类关联起来。然后通过实体类的对象操作（增删改查）数据库表，这个就是ORM的行为！
> 
> 所以：ORM是一个实现使用对象操作数据库的设计思想！！！
> 通过这句话，我们知道JPA的作用就是通过对象操作数据库的，不用编写sql语句。

## Jpa的入门步骤

### 配置步骤说明

#### 第一步：导入包

```xml
<dependencies>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>4.3.6.Final</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.40</version>
        </dependency>
    </dependencies>
```



#### 第二步：创建一个总配置文件(或者在springboot中配置yaml)
```properties
    spring:
      datasource:
        #使用默认的数据源 com.mysql.cj.jdbc.Driver
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://127.0.0.1:3306/university_changjian
        username: root
        password: root
      jpa:
        hibernate:
          ##自动更新
          ddl-auto: update
        show-sql: true
```

#### 第三步：编写实体类
```java

@Date
@Entity //告诉JPA这是一个实体类（和数据表映射的类）
@Table(name = "tbl_user") //@Table来指定和哪个数据表对应;如果省略默认表名就是user；
public class User {

    @Id //这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)//自增主键
    private Integer id;

    @Column(name = "last_name",length = 50) //这是和数据表对应的一个列
    private String lastName;
    @Column //省略默认列名就是属性名
    private String email;

```
#### 第四步：编写一个Dao接口来操作实体类对应的数据表（Repository）

```java
//继承JpaRepository来完成对数据库的操作
//JpaRepository<实体，实体的主键>
public interface UserRepository extends JpaRepository<User,Integer> {
}

```

### 自己写sql

> 下面是一些常用的
> 
> @Query(value=" 这里就是查询语句")
> @Query支持hql和原生sql两种方式，默认是hql ，hql就是语句中用的是实体名字和实体属性，原生sql用的表名字和表字段,
> Hql
> 要想查询全部字段可以用 sellect 实体名 这里省略了value ，参数使用了占位置符 ?1 代表第一个参数 ?2代表第二个


```java
public interface UserRepository extends JpaRepository<User, Long> {
 
  @Query("select u from User u where u.emailAddress = ?1")
  User findByEmailAddress(String emailAddress);
}

//如果是更新或者删除操作，方法上面要加@Modifying      默认开启的事务只是可读的，更新操作加入@Modifying 就会关闭可读
    @Modifying
    @Transactional
    @Query("update CardConfig  set cardStatus=?1 where  id in ?2")
    void updateCardStatus( Integer status,List<Integer> listIds);

// @Param 代替参数占位符，  hql或者sql里就用  :firstname替换   方法里的参数顺序可以打乱
 @Query("select u from User u where u.firstname = :firstname or u.lastname = :lastname")
  User findByLastnameOrFirstname(@Param("lastname") String lastname,
                                 @Param("firstname") String firstname);

//返回字段 组成新的entity返回 类名必须是全写的
@Query(value="select new com.hikvision.metro.modules.repository.entity.CameraIndexs(c.preOneCameraIndexcode, c.preTwoCameraIndexcode, c.backOneCameraIndexcode) from StationDeviceConfig c")
    List<CameraIndexs> getAllCameraIndexs();

```
### SpringDataJpa的规范
```java
 符合SpringDataJpa的dao层接口规范
       JpaRepository<操作的实体类类型，实体类中主键属性的类型>
            *  封装了基本CRUD操作
            

                public void testBaseQuery() throws Exception {
                            User user=new User();
                            userRepository.findAll();
                            userRepository.findOne(1l);
                            userRepository.save(user);
                            userRepository.delete(user);
                            userRepository.count();
                            userRepository.exists(1l);
                        }
        
```

#### 自定义查询
        JpaSpecificationExecutor<操作的实体类类型>
     .  封装了复杂查询（分页）
     .  模糊查询
     .  多条件查询
     
      自定义查询条件的步骤：
         *      1.实现Specification接口（提供泛型：查询的对象类型）
         *      2.实现toPredicate方法（构造查询条件）
         *      3.需要借助方法参数中的两个参数（
         *          root：获取需要查询的对象属性
         *          CriteriaBuilder：构造查询条件的，内部封装了很多的查询条件（模糊匹配，精准匹配）
         *       ）
   例子：         
 ```java
    /**
     *
     *  案例：根据客户名称查询，查询客户名为邹的客户(将name改为“邹”)
     *          查询条件
     *              1.查询方式
     *                  cb对象
     *              2.比较的属性名称
     *                  root对象
     *
     */
    public void ServiceImpl(String name) {
    Specification<Customer> spec = new Specification<Customer>() {
        @Override
        public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
            //1.获取比较的属性
            Path<Object> custName = root.get("custId");
            //2.构造查询条件  ：    select * from cst_customer where cust_name = '邹'
            /**
             * 第一个参数：需要比较的属性（path对象）
             * 第二个参数：当前需要比较的取值
             */
            Predicate predicate = cb.equal(custName, name);//进行精准的匹配  （比较的属性，比较的属性的取值）
            return predicate;
        }
    };
    Customer customer = customerDao.findOne(spec);
    System.out.println(customer);
 ```
多条件查询

```java
    /**
     * 多条件查询
     *      案例：根据客户名（传智播客）和客户所属行业查询（it教育）
     *
     */
        @Test
        public void testSpec1() {
        /**
         *  root:获取属性
         *      客户名
         *      所属行业
         *  cb：构造查询
         *      1.构造客户名的精准匹配查询
         *      2.构造所属行业的精准匹配查询
         *      3.将以上两个查询联系起来
         */
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                Path<Object> custName = root.get("custName");//客户名
                Path<Object> custIndustry = root.get("custIndustry");//所属行业
                //构造查询
                //1.构造客户名的精准匹配查询
                Predicate p1 = cb.equal(custName, "传智播客");//第一个参数，path（属性），第二个参数，属性的取值
                //2..构造所属行业的精准匹配查询
                Predicate p2 = cb.equal(custIndustry, "it教育");
                //3.将多个查询条件组合到一起：组合（满足条件一并且满足条件二：与关系，满足条件一或满足条件二即可：或关系）
                Predicate and = cb.and(p1, p2);//以与的形式拼接多个查询条件
                // cb.or();//以或的形式拼接多个查询条件
                return and;
            }
        };
        Customer customer = customerDao.findOne(spec);
        System.out.println(customer);
    }
```

模糊查询
```    java
     /**
     * 案例：完成根据客户名称的模糊匹配，返回客户列表
     *      客户名称以 ’传智播客‘ 开头
     *
     * equal ：直接的到path对象（属性），然后进行比较即可
     * gt，lt,ge,le,like : 得到path对象，根据path指定比较的参数类型，再去进行比较
     *      指定参数类型：path.as(类型的字节码对象)
     */
    @Test
    public void testSpec3() {
        //构造查询条件
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                //查询属性：客户名
                Path<Object> custName = root.get("custName");
                //查询方式：模糊匹配
                Predicate like = cb.like(custName.as(String.class), "传智播客%");
                return like;
            }
        };
        List<Customer> list = customerDao.findAll(spec);
        for (Customer customer : list) {
            System.out.println(customer);
        }
        //添加排序
        //创建排序对象,需要调用构造方法实例化sort对象
        //第一个参数：排序的顺序（倒序，正序）
        //   Sort.Direction.DESC:倒序
        //   Sort.Direction.ASC ： 升序
        //第二个参数：排序的属性名称
        Sort sort = new Sort(Sort.Direction.DESC,"custId");
        List<Customer> list = customerDao.findAll(spec, sort);
        for (Customer customer : list) {
            System.out.println(customer);
        }
    }
```
分页查询
```java
  /**
     * 分页查询
     *      Specification: 查询条件
     *      Pageable：分页参数
     *          分页参数：查询的页码，每页查询的条数
     *          findAll(Specification,Pageable)：带有条件的分页
     *          findAll(Pageable)：没有条件的分页
     *  返回：Page（springDataJpa为我们封装好的pageBean对象，数据列表，共条数）
     */
    @Test
    public void testSpec4() {

        Specification spec = null;
        //PageRequest对象是Pageable接口的实现类
        /**
         * 创建PageRequest的过程中，需要调用他的构造方法传入两个参数
         *      第一个参数：当前查询的页数（从0开始）
         *      第二个参数：每页查询的数量
         */
        Pageable pageable = new PageRequest(0,2);
        //分页查询
        Page<Customer> page = customerDao.findAll(null, pageable);
        System.out.println(page.getContent()); //得到数据集合列表
        System.out.println(page.getTotalElements());//得到总条数
        System.out.println(page.getTotalPages());//得到总页数
    }
}

```
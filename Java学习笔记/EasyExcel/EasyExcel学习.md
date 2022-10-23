# Easy Excel

## Excel导入导出的应用场景

> 1.数据导入：减轻录入的工作量
>
> 2.数据导出：统计信息归档
>
> 3.数据传输：异构系统之间数据传输

## Easy Excel简单使用

### Easy Excel特点

> Java领域解析，生成Excel比较有名的框架有Apache poi,jxl等，但他们都存在一个严重的问题就是非常的耗内存，如果你的系统并发量不大的话可能还行，但是一旦并发上来后一定会OOM或者JVM频繁的full gc.
>
> EasyExcel是阿里巴巴开源的一个excel处理框架，以使用简单，节省内存著称，EasyExcel能大大减少占用内存的主要原因是在解析Excel时没有将文件数据一次性全部加载到内存中，而是从磁盘上一行行读取数据，逐个解析。
>
> EasyExcel采用一行一行的解析模式，并将一行的解析结果以观察者的模式通知处理（AnalysisEventListener）。

### 1.导入maven依赖，主要还需要poi的依赖，并且版本要对应上

```xml
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>easyexcel</artifactId>
        <version>2.1.1</version>
    </dependency>
     <!--poi-->
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>${poi.version}</version>
    </dependency>
    <!--xlsx-->
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>${poi.version}</version>
    </dependency>
</dependencies>
```

### 2.执行并生成xlsm文件

#### 编写entity对象类

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class DemoData {
        //设置excel表头名称
        @ExcelProperty(value = "学生编号",index = 0)
        private Integer sno;
        @ExcelProperty(value = "学生姓名",index = 1)
        private String sname;

}
```

#### 使用EasyExcel的write方法生成xlsx文件

```java
public class TestEasyExcelWrite {

    public static void main(String[] args) {

        //实现excel写的操作
        //1 设置写入文件夹地址和excel文件名称
        String filename = "D:\\soft_package\\BindCode\\test.xlsx";
       // 2 调用easyexcel里面的方法实现写操作
       // write方法两个参数：第一个参数文件路径名称，第二个参数实体类class
        EasyExcel.write(filename,DemoData.class).sheet("学生列表").doWrite(getData());

    }

    //创建方法返回list集合
    private static List<DemoData> getData() {
        List<DemoData> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            DemoData data = new DemoData();
            data.setSno(i);
            data.setSname("lucy"+i);
            list.add(data);
        }
        list.add(new DemoData(1,"张三"));
        return list;
    }
}
```

![image-20221022203016972](E:\Typora\data\img\image-20221022203016972.png)

### 4.读取Easy Excle

#### 编写entity对象类

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class DemoData {
        //设置excel表头名称
        @ExcelProperty(value = "学生编号",index = 0)
        private Integer sno;
        @ExcelProperty(value = "学生姓名",index = 1)
        private String sname;
    
}
```

#### 创建一个监听excel文件读取

```java
public class TestEasyListener extends AnalysisEventListener<DemoData> {
    // 一行一行的读取数据
    @Override
    public void invoke(DemoData demoData, AnalysisContext analysisContext) {
        System.out.println("***"+demoData);
    }
    // 读取标头的方法
    public void invokeHead(Map<Integer, CellData> headMap, AnalysisContext context) {
        System.out.println("标头"+headMap);
    }
    //读取完成之后执行
    @Override
    public void doAfterAllAnalysed(AnalysisContext analysisContext) {

    }
}
```

#### 最终方法调用

```java
    public static void main(String[] args) {
        //设置读取路径
        String filename = "D:\\soft_package\\BindCode\\test.xlsx";
        EasyExcel.read(filename,DemoData.class,new TestEasyListener()).sheet().doRead();
    }

```


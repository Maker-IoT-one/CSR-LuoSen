# 一、案例引出

案例需求：获取员工数据，返回统一响应结果，展示到前端页面上
## 1.导入dom4j的依赖，用于解析XML文件
因为数据是存放在标签文本中，因此需要用到这个工具类
需要再maven里面导入坐标依赖
``` xml
<!-- 解析XML -->  
<dependency>  
    <groupId>org.dom4j</groupId>  
    <artifactId>dom4j</artifactId>  
    <version>2.1.3</version>  
</dependency>
```
## 2.导入工具类包，方便解析
我们要是有个类，可以用方法直接调用dom4j的方法，放在utils目录下
``` java
package com.example.project1.utils;  
  
import org.dom4j.Document;  
import org.dom4j.Element;  
import org.dom4j.io.SAXReader;  
import java.io.File;  
import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.util.ArrayList;  
import java.util.List;  
  
public class XmlParserUtils {  
  
    public static <T> List<T> parse(String file , Class<T> targetClass)  {  
        ArrayList<T> list = new ArrayList<T>(); //封装解析出来的数据  
        try {  
            //1.获取一个解析器对象  
            SAXReader saxReader = new SAXReader();  
            //2.利用解析器把xml文件加载到内存中,并返回一个文档对象  
            Document document = saxReader.read(new File(file));  
            //3.获取到根标签  
            Element rootElement = document.getRootElement();  
            //4.通过根标签来获取 user 标签  
            List<Element> elements = rootElement.elements("emp");  
  
            //5.遍历集合,得到每一个 user 标签  
            for (Element element : elements) {  
                //获取 name 属性  
                String name = element.element("name").getText();  
                //获取 age 属性  
                String age = element.element("age").getText();  
                //获取 image 属性  
                String image = element.element("image").getText();  
                //获取 gender 属性  
                String gender = element.element("gender").getText();  
                //获取 job 属性  
                String job = element.element("job").getText();  
  
                //组装数据  
                Constructor<T> constructor = targetClass.getDeclaredConstructor(String.class, Integer.class, String.class, String.class, String.class);  
                constructor.setAccessible(true);  
                T object = constructor.newInstance(name, Integer.parseInt(age), image, gender, job);  
  
                list.add(object);  
            }  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
        return list;  
    }  
  
}
```
## 3.导入接收的实体类
解析xml文件后获取的数据用什么接收，我们用一个实体类接收就OK
``` java
package com.example.project1.pojo;  
  
public class Emp {  
    private String name;  
    private Integer age;  
    private String image;  
    private String gender;  
    private String job;  
  
    public Emp() {  
    }  
  
    public Emp(String name, Integer age, String image, String gender, String job) {  
        this.name = name;  
        this.age = age;  
        this.image = image;  
        this.gender = gender;  
        this.job = job;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public Integer getAge() {  
        return age;  
    }  
  
    public void setAge(Integer age) {  
        this.age = age;  
    }  
  
    public String getImage() {  
        return image;  
    }  
  
    public void setImage(String image) {  
        this.image = image;  
    }  
  
    public String getGender() {  
        return gender;  
    }  
  
    public void setGender(String gender) {  
        this.gender = gender;  
    }  
  
    public String getJob() {  
        return job;  
    }  
  
    public void setJob(String job) {  
        this.job = job;  
    }  
  
    @Override  
    public String toString() {  
        return "Emp{" +  
                "name='" + name + '\'' +  
                ", age=" + age +  
                ", image='" + image + '\'' +  
                ", gender='" + gender + '\'' +  
                ", job='" + job + '\'' +  
                '}';  
    }  
}
```
## 5.xml文件
那解析的xml文件就是下面这样,放在resource目录下
``` xml
<?xml version="1.0" encoding="UTF-8" ?>  
<emps>  
    <emp>        <name>金毛狮王</name>  
        <age>55</age>  
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/1.jpg</image>  
        <!-- 1: 男, 2: 女 -->  
        <gender>1</gender>  
        <!-- 1: 讲师, 2: 班主任 , 3: 就业指导 -->  
        <job>1</job>  
    </emp>  
    <emp>        <name>白眉鹰王</name>  
        <age>65</age>  
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/2.jpg</image>  
        <gender>1</gender>  
        <job>1</job>  
    </emp>  
    <emp>        <name>青翼蝠王</name>  
        <age>45</age>  
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/3.jpg</image>  
        <gender>1</gender>  
        <job>2</job>  
    </emp>  
    <emp>        <name>紫衫龙王</name>  
        <age>38</age>  
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/4.jpg</image>  
        <gender>2</gender>  
        <job>3</job>  
    </emp></emps>
```
## 6.准备响应实体类
之前说为了响应统一，需要统一的工具类，来接收
``` java
package com.example.project1.pojo;  
  
/**  
 * 统一响应结果封装类  
 */  
public class Result {  
    private Integer code ;//1 成功 , 0 失败  
    private String msg; //提示信息  
    private Object data; //数据 data  
    public Result() {  
    }  
    public Result(Integer code, String msg, Object data) {  
        this.code = code;  
        this.msg = msg;  
        this.data = data;  
    }  
    public Integer getCode() {  
        return code;  
    }  
    public void setCode(Integer code) {  
        this.code = code;  
    }  
    public String getMsg() {  
        return msg;  
    }  
    public void setMsg(String msg) {  
        this.msg = msg;  
    }  
    public Object getData() {  
        return data;  
    }  
    public void setData(Object data) {  
        this.data = data;  
    }  
  
    public static Result success(Object data){  
        return new Result(1, "success", data);  
    }  
    public static Result success(){  
        return new Result(1, "success", null);  
    }  
    public static Result error(String msg){  
        return new Result(0, msg, null);  
    }  
  
    @Override  
    public String toString() {  
        return "Result{" +  
                "code=" + code +  
                ", msg='" + msg + '\'' +  
                ", data=" + data +  
                '}';  
    }  
}
```
## 7.编写接口
这用了反射和集合的知识
分三步进行
接下开进行postman调用即可看到结果
``` java
package com.example.project1.controller;  
  
  
import com.example.project1.pojo.Emp;  
import com.example.project1.pojo.Result;  
import com.example.project1.utils.XmlParserUtils;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
import java.util.List;  
import java.util.Random;  
  
@RestController  
  
public class EmpController {  
    @RequestMapping("/listEmp")  
    public Result list(){  
        //1.加载并且解析emp.xml  
        String file=this.getClass().getClassLoader().getResource("emp.xml").getFile();  
        System.out.printf(file);  
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);  
  
        //2,对数据进行转换  
        empList.stream().forEach(emp -> {  
            //处理gender  
            String gender = emp.getGender();  
            if(gender.equals("1")){  
                emp.setGender("男");  
  
            }else if(gender.equals("2")){  
                emp.setGender("女");  
            }  
  
            //处理job  
            String job = emp.getJob();  
            if(job.equals("1")){  
                emp.setJob("讲师");  
  
            }else if(job.equals("2")){  
                emp.setJob("班主任");  
            }else if(job.equals("3")){  
                emp.setJob("就业指导");  
            }  
        });  
  
        //响应数据  
        return Result.success(empList);  
    }  
}
```
# 二、三层架构
接口程序，所有代码都写在一个文件中了，所以复用性和拓展性差，而且难以维护
刚刚我们的代码分为三步骤：数据访问，逻辑处理，接收请求和响应数据三个步骤
![](img/11.png)
## 1.Controller控制层
接收请求，响应数据。
接收前端发送的请求，对请求进行处理，并响应数据
## 2.Service业务逻辑层
逻辑处理，处理具体业务的逻辑
## 3.Dao数据访问层
也叫持久层，负责数据访问操作，包括增删改查
# 三、三层架构代码初步实现
 整个逻辑：前端发起请求，调用控制层，然后到业务处理，再到数据访问层，然后再返回
## 1.Dao层实现
 因为这个案例只是实现了对xml文件的解析，那如果是其他的呢，那我们可以面向接口进行实现。就是接口+多态
 接口代码：
 我们创建一个接口，里面有个抽象方法需要重写
 ```java
 package com.example.project1.dao;  
  
import com.example.project1.pojo.Emp;  
  
import java.util.List;  
  
public interface EmpDao {  
    //获取员工数据列表  
    public List<Emp> listEmp();  
}
```
创建实现类:
将刚才的逻辑直接复制过来，并且返回获取的集合
```java
package com.example.project1.dao.impl;  
  
import com.example.project1.dao.EmpDao;  
import com.example.project1.pojo.Emp;  
import com.example.project1.utils.XmlParserUtils;  
  
import java.util.List;  
  
public class EmpDaoA implements EmpDao {  
    @Override  
    public List<Emp> listEmp() {  
        //1.加载并且解析emp.xml  
        String file=this.getClass().getClassLoader().getResource("emp.xml").getFile();  
        System.out.printf(file);  
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);  
        return empList;  
    }  
}
```
## 2.Service实现
逻辑层也和数据层的结构相像
接口：
```java
package com.example.project1.service;  
  
import com.example.project1.pojo.Emp;  
  
import java.util.List;  
  
public interface EmpService {  
    public List<Emp> listEmp();  
}
```
实现类：
实现类需要先把数据层的接口实现，获取里面的集合
最后继续放回已经处理好的逻辑
```java
package com.example.project1.service.impl;  
  
  
import com.example.project1.dao.EmpDao;  
import com.example.project1.dao.impl.EmpDaoA;  
import com.example.project1.pojo.Emp;  
import com.example.project1.service.EmpService;  
  
import java.util.List;  
  
public class EmpServiceA implements EmpService {  
    private EmpDao empDao=new EmpDaoA();  
    @Override  
    public List<Emp> listEmp() {  
        List<Emp> empList = empDao.listEmp();  
        //2,对数据进行转换  
        empList.stream().forEach(emp -> {  
            //处理gender  
            String gender = emp.getGender();  
            if(gender.equals("1")){  
                emp.setGender("男");  
  
            }else if(gender.equals("2")){  
                emp.setGender("女");  
            }  
  
            //处理job  
            String job = emp.getJob();  
            if(job.equals("1")){  
                emp.setJob("讲师");  
  
            }else if(job.equals("2")){  
                emp.setJob("班主任");  
            }else if(job.equals("3")){  
                emp.setJob("就业指导");  
            }  
        });  
        return empList;  
    }  
}
```
## 3.Controller实现
同样接口获取到逻辑层处理完的数据，再进行响应
```java
package com.example.project1.controller;  
  
  
import com.example.project1.pojo.Emp;  
import com.example.project1.pojo.Result;  
import com.example.project1.service.EmpService;  
import com.example.project1.service.impl.EmpServiceA;  
import com.example.project1.utils.XmlParserUtils;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
import java.util.List;  
import java.util.Random;  
  
@RestController  
  
public class EmpController {  
    private EmpService empService=new EmpServiceA();  
    @RequestMapping("/listEmp")  
    public Result list(){  
        List<Emp> empList = empService.listEmp();  
  
        //响应数据  
        return Result.success(empList);  
    }  
}  
//@RestController  
  
//public class EmpController {  
//    @RequestMapping("/listEmp")  
//    public Result list(){  
//        //1.加载并且解析emp.xml  
//        String file=this.getClass().getClassLoader().getResource("emp.xml").getFile();  
//        System.out.printf(file);  
//        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);  
//  
//        //2,对数据进行转换  
//        empList.stream().forEach(emp -> {  
//            //处理gender  
//            String gender = emp.getGender();  
//            if(gender.equals("1")){  
//                emp.setGender("男");  
//  
//            }else if(gender.equals("2")){  
//                emp.setGender("女");  
//            }  
//  
//            //处理job  
//            String job = emp.getJob();  
//            if(job.equals("1")){  
//                emp.setJob("讲师");  
//  
//            }else if(job.equals("2")){  
//                emp.setJob("班主任");  
//            }else if(job.equals("3")){  
//                emp.setJob("就业指导");  
//            }  
//        });  
//  
//        //响应数据  
//        return Result.success(empList);  
//    }  
//}
```
# 四、分层和解耦
但是发现一个问题没有，虽然接口不用改了，但是每次的实现类的名字都需要重新改，有新的数据层，那还需要改，那么怎么办呢，有没有办法不用new就可以直接使用对应的类的方法呢
那就是分层解耦
## 1.内聚和耦合
### 内聚
软件中各个功能模块内部的功能联系——就如逻辑层里只写逻辑处理代码，就是内聚高
### 耦合
衡量软件各个层/模块之间的依赖，关联的程度——上面controller层调用service层，service层调用dao层，这就代表耦合
### 程序设计原则
高内聚，低耦合
## 2.分层解耦
可以看到service调用dao层，每次都要进行new，为了解耦，就不要new了
```java
public class EmpServiceA implements EmpService {  
    private EmpDao empDao=new EmpDaoA();  
    @Override  
    public List<Emp> listEmp() {  
        List<Emp> empList = empDao.listEmp();  
      //...............
    }  
}
```

去掉new
```java
public class EmpServiceA implements EmpService {  
    private EmpDao empDao;  
    @Override  
    public List<Emp> listEmp() {  
        List<Emp> empList = empDao.listEmp();  
      //...............
    }  
}
```
那么去掉new之后，我该怎么获得对象呢。我们可以把要new的对象提前放在一个容器中，在使用时候再从容器中拿到
问题1：怎么把对象放在容器中
问题2：怎么使用容器中的类
### 控制反转
IOC:Spring框架的核心：对象的创建控制权由程序自身转移到外部容器，思想叫做控制反转
### 依赖注入
DI：容器为应用程序提供运行时，所以来的资源叫做依赖注入
### Bean对象
IOC容器中创建，管理的对象，称为bean
## 3.分层解耦实现
## 控制反转实现
使用@Component将接口的实现类放入容器,直接再类上面加
```java
import java.util.List;  
@Component  
public class EmpDaoA implements EmpDao {  
    @Override  
    public List<Emp> listEmp() {  
        //1.加载并且解析emp.xml  
        String file=this.getClass().getClassLoader().getResource("emp.xml").getFile();  
        System.out.printf(file);  
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);  
        return empList;  
    }  
}

```
## 3.依赖注入实现
再对应实现接口上加入@Autowired
就自动从IOC容器中寻找加入
```java
@RestController  
  
public class EmpController {  
    @Autowired  
    private EmpService empService;  
    @RequestMapping("/listEmp")  
    public Result list(){  
        List<Emp> empList = empService.listEmp();  
  
        //响应数据  
        return Result.success(empList);  
    }  
}
```
## 4.IOC详解
再实现IOC的时候，我们使用后@Component
当然在不同层次里面也可以使用不同注解
![](img/22.png)
因为控制层中的@RestController包括@Controller所有不用再次增加
注意：
+ 声明bean的时候，可以在直接中通过value书写指定bean名字，没有指定默认为类名首字母小写
+ 以上是个注解都可以声明bean，在springboot开发中，控制器bean只能用@Controller
+ IOC扫描默认在启动类所在的包内，当然也可以使用@ComponentScan扫描bean类
## 5.DI详解
注入的时候使用@Autowired，但是如果多个实现类都加入到容器
解决方案：
### @Primary
可以在注解上再加上@Primary代表更加优先
```java
@Primary
@Service
```
### @Qualifier
通过类型指定，也是加上bean名称
```
Autowired
@Qualifier("empServiceA")
```
### @Resource
指定bean名称
```
@Resource("empServiceA")
```
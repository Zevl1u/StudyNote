# Java学习笔记

- JDK = JRE + 开发工具集
- JRE = JVM + Java SE标准类库
- 环境变量：执行命令时候，若不在命令所在目录，则自动搜索的路径
- JAVA_HOME：JDK的bin路径，Tomcat也要用这个变量
- Java区分大小写  Windows不区分
- 二进制：0b开头  八进制：0开头  十六进制：0X开头
- sout打印出来都是十进制
- %取余符号和被取余数一样
- 一个Java源文件中只能有一个类被public修饰且必须和源文件同名
- byte a=127  byte b= 1  sout a+b  编译错误or运行错误？自动转换为int型
- 定义long或者float要以大小写L和F结尾
- short  byte  char之间(包括和自己)运算之后都是int型  short a = 1  a =a + 1     //报错a + 1为int型 不能赋值给short
- Java底层用的是补码储存数据
- 逻辑或运算符 第一个表达式若为ture 则不执行第二个表达式
- 逻辑x两边都会执行判断  短路x在执行了第一个表达式之后若能确定结果则不会执行第二个表达式    逻辑x与位运算符的区别就是表达式两边的值
- k = m ^ n ，则m = k ^ n = (m ^ n) ^ n
- switch结构中表达式只能是以下6种之一：byte,short,char,int,string,枚举类型
- 找质数  for循环不用到n-1   到n^(1/2)即可
- 嵌套循环可以用标签的break或者continue来跳出
- 数组元素初始值   整型:0   浮点型:0.0    char类型:ASCII码0或者"\u0000"(不是"0",打印出来效果是空格但不是空格space)    布尔类型:false    引用类型:null 
- 多维数组使用静态初始化  同维中长度可以不容(如二维数组用静态初始化  列长可以不同)
- 动态初始化  第一维必须有数 后面可以留空(如 String [][] array = new String[3][] 并不会报错)
- 类型推断:int[] array = {1,2,3}
- 生成一个[a,b]之间的整数:                       (int)( Math.random( ) * ( b - a + 1 ) + a )   其中random函数返回一个[ 0 , 1 )之间的double类型小数
- 成员属性有默认值  局部变量没有
- void方法可以用return提前结束方法
- 一个java文件中可以定义多个类，但是只能又一个类是public修饰且同时还得是文件名
- 方法，局部变量在栈内存；对象在堆内存；未使用到的类，方法等的字节码在方法区
- 类被定义时候默认自带无参构造器；**但是一旦定义了有参构造器，默认的无参构造器就没了**，需要自己定义

### 字符串/ArrayList

- 以""方式给出的字符串对象只会在**堆内存的常量池**中存储一份；而通过**构造器new出来**和**运算（比如+）得到**的对象，是一个新对象，引用不同（即使内容相同），放置在**堆内存**中
- s1 = "abc"; s2 = "a" + "b" + "c"; s1 == s2 为true；因为s2在编译阶段已经确定，编译器会自动优化成"abc"
- JDK 1.7之后，构造器泛型后面的类型申明可以不写

### OOP进阶

- static{}：静态代码块，一般用于初始化资源，随着类加载而加载，自动触发，只执行一次，；{}：构造代码块，一般初始化资源用，调用构造器时会执行，且在构造器之前执行
- 饿汉单例模式：构造器私有，对象公有，类名.静态成员变量访问；懒汉单例模式：构造器私有，静态成员变量也私有，用一个方法返回静态的成员变量（if判断，为null创建新对象，否则返回静态成员变量）
- 1子类不能继承父类的构造器2可以继承父类私有成员，但是不能直接访问3可以访问静态成员，但不是继承
- 访问遵循就近原则
- 方法重写override：方法声明相同。方法声明包括方法名称和形参列表
- 父类私有方法子类不能重写
- 子类重写父类方法时候，访问权限必须大于或者等于父类（访问权限：private(本类) < default(本类和所在包其他类) < protected(本类和所在包其他类以及其他包下子类) < public )
- 不能重写父类静态方法（相当于根本没有继承到对象里 只存在于方法区字节码 所以不能重写）
- 子类的全部构造器默认会先访问父类的无参数构造器然后再执行自己（因为子类初始化时候，会用到父类中的数据，所以要先初始化父类） 即子类构造器第一行会默认调用super()
- this()可以调用本类其他兄弟构造器
- this()和super()都必须在构造器的第一行，因此不能同时使用：因为必须要先调用父类构造器super()初始化父类的变量，如果此时再this()调用兄弟构造器，因为兄弟构造器第一行是super()，就会重复2遍调用父类构造器

### 包，权限修饰符

- final：修饰类：不能被继承(工具类)；  修饰方法：不能被重写；  修饰变量：第一次赋值之后就不能修改
- final修饰变量基本类型：数据值不能变；修饰引用类型：引用值不能变，但是指向对象的内容可以变
- public static final修饰的变量为常量，在编译时候会优化，替换成字面值
- public enum classname{}：枚举类，一种特殊的类（此处enum和class差不多），**用于标志和分类（无法包含具体信息）**，继承自枚举类型java.lang.Enum，都是final修饰不可被继承，构造器都是私有的
- 可以通过反编译命令查看枚举类代码
- Java字节码的反编译命令：javap xxx.class
- 通过将枚举类作为形参，然后 枚举类.枚举 参数来调用某个枚举参数   优点：可读性好，入参严谨，代码优雅

### 抽象类

- abstract，可以修饰类，成员方法
- 抽象发放只有方法签名，无方法体
- 定义了抽象方法就必须声明为抽象类
- 继承的子类一定得实现父类的抽象方法，否则就也得声明称抽象类
- 抽象类无法创建对象
- 模板方法模式：定义通用结构和抽象方法并且在通用结构中调用抽象方法，然后由子类实现抽象方法，提高了代码的复用性

### 接口

- JDK8之前只能写抽象方法和常量，因为是一种规范思想，所以默认都是public的，因此方法和变量的修饰符public abstract可以省略不写

- 一个类实现一个接口必须实现所有方法否则要定义为抽象类

- 类与类只能单继承；类可实现多个接口；接口与接口可以多继承：整合多个接口成为一个接口

- JDK8之后，允许接口直接定义带有方法体的方法：
  
  1. 默认方法：类似普通实例方法，必须用default修饰，默认自带public修饰，用实现类的对象调用   
  2. 静态方法：默认public修饰，必须static修饰，必须用接口名来调用
  3. 私有方法：类似私有实例方法，JDK9之后才有，必须private修饰，只能被本类中其他的默认方法或者私有方法访问

- 注意事项
  
  1. 接口不能实例化
  2. 子类继承了父类，又实现了接口，父类和接口有同名方法，默认使用父类的
  3. 一个类实现了多个接口，多个接口有同名的默认方法，不冲突，编译器会要求重写默认方法
  4. 接口可以多继承，但是如果多个接口中存在规范冲突则不能多继承（比如俩接口 一个int f();  一个void f()；）就会有问题

### 多态

- 同类型对象，执行同一个行为，表现出不同行为特征

- 多态中成员访问特点：多态侧重行为多态
  
  - 方法调用：编译看左边，运行看右边
  
  - 变量调用：编译和运行都看左边
    
    ```java
    public class Main {
        public static void main(String[] args) {
            Animal a = new Dog();
            Animal b = new Cat();
            a.run();  // dog run
            b.run();  // cat run
            System.out.println(a.name);  // animal
        }
    }
    
    abstract class Animal{
        public String name = "animal";
        abstract void run();
    }
    
    class Dog extends Animal{
        public String name = "dog";
        @Override
        void run() {
            System.out.println("dog run");
        }
    }
    
    class Cat extends Animal{
        public String name = "cat";
        @Override
        void run() {
            System.out.println("cat run");
        }
    }
    ```

- 多态形式下，右边对象可以实现解耦，便于扩展维护

- 定义方式时候，使用父类型作为参数，可以接受一切子类对象，体现出了多态的扩展性与便利

- 多态下无法访问子类独有的方法

- 类型转换：
  
  - 自动（子类到父类）
  - 强制（父类到子类）

### 内部类

- 一个类内部还需要一个完整的结构描述，则可使用内部类

- 内部类可以方便访问外部类的成员，包括私有成员

- 内部类分类：
  
  - 静态内部类：Outer.Inner in = new Outer.Inner()
  
  - 成员内部类：Outer.Inner in = new Outer().new Inner(); 也叫实例内部类；JDK16之前不支持在这里面写静态成员；成员内部类访问外部类的实例成员用Outer.this.xxx访问
  
  - 局部内部类：JDK15之前不支持在这里面写静态成员
  
  - **匿名内部类**：本质上是一个没有名字的局部内部类，定义在方法中，代码块中等；方便创建子类对象，简化代码（比如在GUI中为按钮绑定监听事件）
    
    ```java
    Employee e = new Employee(){  // 此处假设Employee是个抽象类或者接口
        public void work(){
        }
    };
    e.work();
    ```
    
    匿名内部类写出来就会产生生一个对象，相当于是**当前new的类型的子类型**；都会产生class字节码文件

### Object和常用API等

- toString()：默认输出引用值，重写输出对象内容
- o.equals()：默认比较地址，重写比较对象内容
- Objects.equals()：更安全，方法里面还是会调用传入对象的equals方法
- StringBuilder：一个可变字符串类，可以看作是一个对象容器，可以提高字符串操作效率：String底层使用StringBuilder实现，使用“+”连接字符串时候会重复产生很多StringBuilder对象，效率较低
- Math，System都是工具类，不能创建对象
  - System.exit(0)：终止虚拟机，0是正常终止，其他数是异常终止
- BigDecimal：禁止使用构造方法BigDecimal(double)方式把double值转化为BigDecimal对象，因为BigDecimal(double)存在精度损失风险。正确做法是用入参为String的构造方法，或者使用BigDecimal的valueOf方法（这个方法里面调用了入参为String的构造方法）
- BigDecimal一定是精度运算，如果出现10.0/3.0这样的无限小数会报错，可以调用带rounding的方法：(10.0).divide(3.0, scale保留小数位数, RoundingMode舍入模式)

### 时间日期，包装类，正则表达式，操作数组，Lambda

- Data类，通过构造器传入毫秒值，或者通过对象.setTime(毫秒值)来创建日期对象

- Simpledataforamt：格式化事件形式和将字符串事件形式解析成日期对象：年y月M日d时H分m秒s星期几EEE上下午a
  
  ```java
  SimepleDataFormat sdf = new SimpleDataFormat("yyyy-MM-dd HH:mm:ss");
  String rs1 = sdf.format(data);
  String rs2 = sdf.format(time);
  Date d = sdf.parse("2021-08-04 11:11:11");
  ```

- Calendar：代表了系统刺客日期对应的日历对象，是一个抽象类，不能直接创建对象

- JDK8开始新增时间API，更加方便，为不可变对象

- 包装类：基本类型的引用类型

- 正则表达式：
  
  - 字符串类提供了匹配正则表达式的API：public boolean matches (String regex)
  - 在字符串方法中使用：public StringreplaceAll(String regex, String newStr)  按照正则白哦大师内容进行替换
  - public String[] split(String regex)：按照正则表达式匹配的内容进行分割字符串，返回一个字符串数组

- Arrays类
  
  - 排序：public static void sort(类[] a)
  
  - 比较器（默认只支持引用类型）：public statice <T> void sort(类型[] a, Comparator < ? super T > c)
    
    ```java
    Arrays.sort(Integer[] a, new Comparator<Integer>(){
        @Override
        public int compare(Integer o1, Integer o2){
            // 如果o1 > o2 返回正整数
            // 如果o1 < o2 返回负整数
            // 如果o1 = o2 返回0
            return o1 - o2;
        }
    });
    ```

- Lambda表达式：
  
  - 只能简化**函数式接口**（有且仅有一个抽象方法的接口）的匿名内部类写法
  
  - 通过加上注解 @FunctionalInterface , 则被注解的接口一定得是函数式接口
    
    ```java
    public class Main {
        public static void main(String[] args) {
    //        forSum o = new forSum() {
    //            @Override
    //            public int sum(int a, int b) {
    //                return a + b;
    //            }
    //        };
    
    //        forSum o = (int a, int b) -> {
    //            return a + b;
    //        };
    
    //        forSum o = (a,b) ->{
    //            return a + b;
    //        };
            forSum o = (a, b) -> a + b;
            System.out.println(o.sum(3, 4));
        }
    }
    
    @FunctionalInterface
    interface forSum{
        int sum(int a, int b);
    }
    ```
  
  - 参数类型可以省略不写，即从 (int a, int b) ->{} 变成(a, b) -> {}
  
  - 如果只有一个参数，甚至连括号都能不写
  
  - 如果Lambda表达式方法体只有一行代码，可以省略大括号不写，同时要省略分号。如果这行代码是return语句，必须省略return和”;“

### 集合

- 只能存储**引用类型**

- Collection（单列元素）和Map（双列元素）

- Collection（接口）
  
  - List（接口）：元素有序，可重复，有索引
    - ArrayList（实现类）
    - LinkedList（实现类）
  - Set（接口）：元素无序，不重复，无索引
    - HashSet（实现类）
      - LinkedHashSet（实现类）：有序，不重复，无索引
    - TreeSet（实现类）：按照大小默认升序排序，不重复，无索引
    - shift + f6 重命名变量

- remove方法默认只删除第一个

- toArray()返回的是object数组（因为有办法绕过泛型注入不同类型数据）

- 迭代器：Iterator，是集合的专用遍历方式
  
  1. 通过Iterator<E> iterator() 方法获取迭代器对象
  2. hasNext() 方法询问当前位置是否有元素存在，粗壮乃返回ture
  3. 获取当前位置元素，同时将迭代器对象移向下一个位置，注意防止取出越界

- 增强for（foreach）：idea里直接list.for就有了
  
  ```java
  Collection<String> list = new ArrayList<>();
  for(String str: list){
      ...
  }
  ```
  
  - 数组/集合都可用
  - JDK5之后出现，内部原理是一个迭代器
  - 实现了Iterable接口的类才可以使用迭代器和foreach，Clollection接口已经实现了Iterable接口

- 实现了Iterable接口的类有foreach
  
  ```java
  list.forEach(new Consumer<String>(){
      @Override
      public void accept(String s){
          sout(s);
      }
  });
  可以简化成：
  list.forEach(s->{
      sout(s);
  });
  ```

- 迭代删除列表元素的时候，不要用列表本身的remove方法，用迭代器的方法，不然会报错
  
  - 迭代器和foreach里直接用集合方法删除元素可能会出问题（并发修改异常）
  
  - 用迭代器方法删元素，或用for循环可以避免
  
  ```java
  ArrayList<String> list = new ArrayList<>();
          Iterator<String> it = list.iterator();
          while(it.hasNext()){
              String ele = it.next();
              if("abc".equals(ele)){
  //                list.remove("abc");
                  it.remove();
              }
          }
  ```

### 泛型

- 编译阶段约束操作的数据类型

- 定义类（方法）同时定义泛型的类（方法）就是泛型类（方法），用于编译阶段指定数据类型（即把类型当变量）
  
  ```java
  class MyArrayList<T>{
      private ArrayList<T> list = new ArrayList<>();
  
      public static void main(String[] args) {
          String[] name = {"Tim", "Kat", "Zen"};
          show(name);
          Integer[] ages = {10, 20, 30};
          show(ages);
      }
      public void add(T t){
          list.add(t);
      }
      public static <K> void show(K k){
  
      }
  }
  ```

- 泛型接口：让实现类选择当前功能需要操作的数据类型
  
  ```java
  public interface Data<E>{
      void add(E e);
       ...
  }
  ```

- 注意：BMW类和BENZ类都继承自Car类，但是`ArrayList<BMW>`和`ArrayList<BENZ>`与`ArrayList<Car>`没有关系

- 泛型上下限
  
  - `? extends Car`：？必须是Car或者其子类   泛型上限
  
  - `? super Car`：？必须是Car或者其父类   泛型下限

- `ArrayList<? extends Car> cars`即可完美解决

### 集合与map

- HashSet底层采用哈希表存储数据。哈希表实现：
  
  - 在JDK8之前用数组+链表，使用头插法
  
  - JDK8之后开始采用数组+链表+红黑树，使用尾插法，链表长度超过8时候，转换成红黑树
  
  - 根据哈希值和数组长度激素按存入位置，为null直接存入，不为null调用equals方法比较，所以如果希望set集合认为两个内容一样的对象是重复的，则需要重写对象的hashCode()和equals()方法
  
  - 数组默认大小16，加载因子默认0.75，数组一旦存满到16*0.75就自动扩容至原来的两倍

- TreeSet
  
  - 对于数值类型，默认按照大小升序排序
  
  - 对于字符串，默认按照首字符编号升序
  
  - 自定义类型，自己定义排序规则
    
    - 自定义类实现Comparable接口重写里面的compareTo方法
    
    - TreeSet有参构造器，设置Comparator接口对应的比较器对象
    
    - 都有的话，优先构造器里的

### 可变参数

- 形参中可以接受多个数据

- 格式：数据类型...参数名称
  
  ```java
  public static void sum(int...nums){
      //方法内部就是数组
  }
  ```

- 一个形参列表中只能由一个可变参数

- 且必须放在最后

### Map

- Map是一个接口，实现类有
  
  - HashMap
  
  - HashTable

- 键无序，不重复，无索引

- ctrl+alt+v：补全变量声明

- lambda表达式遍历map
  
  ```java
  maps.forEach(new BiConsumer<String, Integer>(){
      @Override
      public void accept(String key, Integer value){
          sout(key + "--->" +value);
      }
  });
  ```

- HashMap和HashSet底层原理一模一样，都是哈希表结构，只是HashMap的每个元素包含两个值。**实际上Set系列集合的底层就是Map实现，只是Set集合中的元素只要键数据，不要值数据而已。**

##### 不可变集合

- 在List、Set、Map接口中的of方法，可以创建一个不可变集合。

### Stream流

- Java8中，得益于Lambda带来的函数式编程，引入了一个全新的Steam流概念。

- 用于简化集合和数组操作的API

- 一个例子
  
  ```java
  public static void main(String[] args) {
          List<String> names = new ArrayList<>();
          Collections.addAll(names, "张三丰", "张无忌", "周芷若", "赵敏", "张强");
          System.out.println(names);
  
          //1.找出姓张的放到辛几何
          List<String> zhang = new ArrayList<>();
          for (String name : names) {
              if(name.startsWith("张")){
                  zhang.add(name);
              }
          }
          System.out.println(zhang);
  
          //2.张里面名字长度为3的
          List<String> three = new ArrayList<>();
          for (String s : zhang) {
              if(s.length() == 3){
                  three.add(s);
              }
          }
          System.out.println(three);
  
          //3.用stream流方式进行处理
          names.stream().filter(s -> s.startsWith("张")).filter(s -> s.length() == 3).forEach(s -> System.out.println(s));
          //源码
          names.stream().filter(new Predicate<String>(){
              @Override
              public boolean test(String s){
                  return false;
              }
          })
          // 简写
          names.forEach(System.out::println);
      }
  ```

- stream流的三类方法
  
  - 获取stream流：创建一条流水线，并把数据放到流水线上准备操作
    
    - Collections接口钟默认方法stream生成流
    
    - 数组的stream方法/of方法
  
  - 中间方法：流水线上的操作，可以链式调用
  
  - 终结方法：一个stream流只能有一个终结方法，是流水线的最后一个操作

- 处理后的流通过collect方法可以收集成list
  
  ```java
  Stream<String> abc = names.stream().filter(s -> s.startsWith("张"));
          List<String> l = abc.collect(Collectors.toList());
  ```

- 流只能被收集一次，之后就关闭了

- 比较复杂，要用时候再查

### 异常

- Throwable
  
  - Error：系统级别的问题、jvm退出、代码无法控制
  
  - Exception
    
    - RuntimeException及其子类：运行时候异常
    
    - 其他：编译时异常

- 自定义异常
  
  - 编译时异常：继承Exception
  
  - 运行时异常：继承RuntimeException

### 日志框架logback

- 可以将日志信息写入到文件中或者数据库中，不需要修改源代码，比较灵活，性能较好

- Logback日志框架，目前比较好的日志实现框架，实现的Simple Logging Facade for Java(slf4j)规范接口，比log4j好

- 主要分为三个技术模块
  
  - logback-core：核心
  
  - logback-classic：log4j的改良版本，同时实现了slf4j的API
  
  - logback-access模块与Tomcat和Jetty等Servlet容器集成，以提供HTTP访问日志功能

- 入门
  
  - 下载好对应jar包，在项目里新建一个目录把包放进去，然后右键Add as Library添加到项目依赖库中
  
  - 将logback 的核心配置文件logback.xml拷贝到src目录下
  
  - 在代码中获取日志对象
  
  ```java
  public static final Loger LOGGER = LoggerFactory.getLogger("Test.class");
  ```
  
  - xml配置文件中的append标签可以设置输出位置和日志信息的详细格式
  
  - 设置输出的日志级别：只输出级别不低于设定级别的日志信息
    
    - TRACE<DEBUG<INFO<WARN<ERROR
    
    - 默认时debug，忽略大小写
    
    - 在\<root level\>标签设置

##### File、方法递归、IO流

- File类可以定位文件：进行删除，获取文本本身信息，但是不能读写文件内容

- 相对路径：以工程为起始
  
  ```java
  // 获取java虚拟机运行时候环境，执行某个可执行程序
  Runtime r = Runtime.getRuntime();
  r.exec(file.getAbsolutePath());
  ```

- 字符集编码
  
  ```java
  "abc我爱你".getBytes() //返回一个byte[],参数可选字符集
  new String(byte[] bytes)// 通过bytep[]构造一个字符串
  ```

- io流
  
  - 字节
    
    - 输入：InputStream（抽象类），实现类是FileInputStream
    
    - 输出：OutputStream：输出之后要flush()
  
  - 字符
    
    - 输入：Reader
    
    - 输出：Writer

- try catch finally：在finally语句块关闭资源

- finally之前用return都没用，如果在finally里有return，则一定执行finally里的return
  
  ```java
  try(
                  // 这里只能放资源（实现了AutoCloseable/Closeable接口的类）
                  InputStream is = new FileInputStream("abc.txt");
                  OutputStream os = new FileOutputStream("abc_copy.txt");
                  ){
              //...
          }catch (Exception e){
              //...
          }
  ```

- 对象序列化：把内存中的Java对象存到持久化到磁盘里

- transient关键字修饰的成员变量：不参与序列化serialVersionUID为序列化版本号：也就是更新了新的东西，磁盘里旧的版本对象就不能用了
  
  ```java
  private staticc final long serialVersionUID = 1;
  private transient String name;
  ```

- PrintStream和PrintWriter区别：
  
  - 打印数据功能上一样，都使用方便，性能搞笑
  
  - PrintStream继承自字节输出流OutputStream，支持写字节数据的方法
  
  - PrintWriter继承自字符输出流Writer，支持写字符数据
  
  - 实际上System.out.println中的out就是一个打印流，在静态代码块中初始化；System.setOut(ps)设置成自己的打印流

- Properties：一个属性文件，可以把自己对象中的键值对存入到一个属性文件中

### 线程

- 创建方法

  - 继承Thread类

  - 实现Runnable接口，创建任务类，然后交给一个线程类执行

  - 前两种方式无法返回线程执行结果；于是另一种方式：JDK5.0提供了Callable和FutureTask来解决

    - 定义一个任务类，实现Callable接口，重写call方法，封装要做的事
    - 用FutureTask吧Callable对象封装成线程任务对象
    - 把线程任务对象交给Thread处理
    - 调用Thread的start方法启动线程
    - 执行完毕后，通过FutureTask的get方法获取任务执行的结果

    ```java
    import java.util.concurrent.Callable;
    import java.util.concurrent.FutureTask;
    
    public class Main {
        public static void main(String[] args) {
            System.out.println("Hello world!");
            Callable c = new MyCall(100);
            FutureTask<String> f = new FutureTask<String>(c);
            new Thread(f).start();
    
            String s = null;
            try {
                s = f.get();
            } catch (Exception e) {
                e.printStackTrace();
            }
            System.out.println(s);
    
        }
    
    }
    
    class MyCall implements Callable<String> {
        private int n;
    
        public MyCall(int n) {
            this.n = n;
        }
    
        @Override
        public String call() throws Exception {
            int sum = 0;
            for (int i = 0; i < n; i++) {
                sum += i;
                Thread.sleep(50);
            }
            return "result" + sum;
        }
    }
    ```


- 一些线程常用方法，setName()，getName()，Thread.currentThread()返回当前执行线程对象；Thread.sleep()让当前线程休眠指定毫秒数

- 线程安全

  - 同步代码块：把出现线程安全的核心代码上锁，执行完毕之后自动解锁

    - ```java
      synchronized(同步锁对象){
      	// 核心代码
      }
      ```

    - 理论上锁对象只要对于当前同时执行的线程来说是同一个对象即可

    - ctrl+alt+T可以将一段代码surround with

    - 锁对象不能随便给，有可能会影响其他线程执行；建议使用共享资源作为锁对象，

      - 实例方法可以使用this作为锁对象
      - 静态方法可以用类字节码（类名.class）对象作为锁对象
    
  - 同步方法

    - 核心：出现线程安全问题的核心方法上锁；每次之恩那个有一个线程进入，执行完毕之后自动解锁
    - 格式：修饰符 synchronized 返回值类型 方法名(形参){同步代码}
    - 底层原理：隐式锁，锁的范围是整个方法代码
    - 如果方法是实例方法：**同步方法默认用this作为锁对象**，但是代码要高度面向对象。（锁可以跨方法）
    - 如果发方法是静态方法：同步方法默认用类名.class作为锁对象

  - Lock锁

    - 为了更清晰表达如何加锁和释放锁，JDK5之后提供了一个新的锁对象Lock
    - Lock实现提供 比使用synchronized方法和语句更加广泛的锁定操作
    - Lock是接口不能实例化，采用实现类ReentrantLock来构建Lock锁对象
    - 可以由人为控制上锁和释放锁
    - 例如，对于一个账户对象可以在其成员变量添加一个final修饰的锁对象，然后在需要同步的代码块前后lock()和unlock()

- 线程通信


  - 在Object类里有

    - wait()：让当前所在线程等待并释放所占锁，知道另一个线程调用notify方法
    - notify()：唤醒正在等待锁的某个线程
    - notifyAll()方法：唤醒正在等待锁的所有线程
    - 注意

      - 一定要用当前同步锁对象进行调用，因为只有当前锁对象知道持有锁的线程
      - 先notify唤醒其他线程，然后再wait；因为wait之后的代码就不执行了

- 线程池


  - 一个可以复用线程的技术：创建线程开销过大，影响系统性能

  - 线程池接口：ExecutorService，实现类：ThreadPoolExecutor，使用它直接创建（推荐）


    - ThreadPoolExecutor构造器
    
    - corePoolSize：线程池**核心线程**数
    
    - maximumPoolSize：线程池可支持的最大线程数（核心线程+临时线程）
    
    - keepAliveTime：临时线程的空闲存货时间，不能小于0
    
    - workQueue：指定任务队列
    
    - threadFactory：指定用哪个线程工厂创建线程
    
    - handler：指定线程忙，任务满的时候，新任务来了怎么办


​      

    - 临时线程什么时候创建？新任务提交时候发现核心线程都在忙，**任务队列也满了**，并且还可以创建临时线程，此时才会创建临时线程。
    
    - 什么时候会开始拒绝任务？核心线程和临时线程都在忙，任务队列也满了，新任务过来时候才会开始任务拒绝。

  - 或者使用Executors线程池工具类调用方法返回不同特点的线程池对象（有可能有OOM错误，大型互联网场景不适合）

  - 线程池不会自己关闭

  - 常用方法


    - void execute(Runnable command)
    - Future<T> submit(Callable<T> task)
    - void shutdown()：等任务执行完毕后关闭线程池
    - List<Runnable> shuntdownNow()：立即关闭，停止正在执行的任务，并返回队列中未执行的任务

  - 定时器：控制任务延时调用，或者周期调用的技术


    - 实现方式1：Timer
    
      ```java
      import java.util.Timer;
      import java.util.TimerTask;
      
      public class Main {
          public static void main(String[] args) {
              System.out.println("Hello world!");
              Timer t = new Timer();
              t.schedule(new TimerTask() {
                  @Override
                  public void run() {
                      System.out.println(Thread.currentThread().getName());
                  }
              }, 3000, 2000);
          }
      }
      ```


      - 问题：1.**Timer是单线程**，处理多个任务按照顺序执行，存在延时与设置定时器的时间有出入。2.可能因为其中某个任务的一场是的Timer线程死掉，从而影响后续任务执行。
    
    - 实现方式2：ScheduledExecutorSerive


      - 并发的，弥补Timer缺陷，内部为线程池：通过工具类Executors.newScheduledThreadPool()方法创建
      - pool.schedule()方法执行

### 网络编程


- IP地址操作类：InetAddress

- 端口号：标识一个正在计算机设备上运行的进程（程序），16位二进制，0~65535


  - 周知端口：0~1023，被预先定义的知名应用占用，如HTTP是80；FTP是21
  - 注册端口：1024~49151，分配给用户进程或者某些应用程序，如Tomcat是8080；MySQL是3306
  - 动态端口：40152~65535，动态分配

- UDP


  - DatagramPacket数据包对象

  - DatagramPacket数据管道（端）

  - 一个简单的UDP传输信息例子

    ```java
    // 服务器端，需要先启动，会执行至receive语句后等待
    import java.net.DatagramPacket;
    import java.net.DatagramSocket;
    
    public class UDPServer {
        public static void main(String[] args) throws Exception {
            System.out.println("Server start...");
            // 创建接收端
            DatagramSocket socket = new DatagramSocket(8888);
            // 创捷接收数据包
            byte[] bytes = new byte[1024 * 64];
            DatagramPacket packet = new DatagramPacket(bytes, bytes.length);
            // 等待接收数据
            socket.receive(packet);
            // 取出数据
            System.out.println(new String(bytes, 0, packet.getLength()));
            // 获取发送方的ip和端口
            String ip = packet.getSocketAddress().toString();
            int port = packet.getPort();
            System.out.println("ip:" + ip + ",port:" + port);
            socket.close();
        }
    }
    
    ```

    ```java
    // 用户端
    import java.net.DatagramPacket;
    import java.net.DatagramSocket;
    import java.net.Inet4Address;
    
    public class Client {
        public static void main(String[] args) throws Exception {
            // 创建发送端
            DatagramSocket socket = new DatagramSocket();
            // 创建数据包封装对象
            byte[] bytes = "Information...".getBytes();
            DatagramPacket packet = new DatagramPacket(bytes, bytes.length, Inet4Address.getLocalHost(), 8888);
            // 发送数据包
            socket.send(packet);
            // 关闭管道
            socket.close();
        }
    }
    ```

  - UDP广播：ip为255.255.255.255，然后加端口号就可以了

  - TCP


    - 使用java.net.Socket类实现通信，底层即是用了TCP协议
    
    - 例子
    
      ```java
      // TCPClient
      import java.io.OutputStream;
      import java.io.PrintStream;
      import java.net.Socket;
      import java.util.Scanner;
      
      public class TCPClient {
          public static void main(String[] args) throws Exception {
              Socket socket = new Socket("127.0.0.1", 7777);
              OutputStream os = socket.getOutputStream();
              PrintStream ps = new PrintStream(os);
              Scanner s = new Scanner(System.in);
              String msg;
              while (true) {
                  System.out.print("Send:");
                  msg = s.nextLine();
                  if ("exit".equals(msg)) {
                      System.out.println("Bye");
                      socket.close();
                      break;
                  } else {
                      // 一定要用println而不是print，print没有换行符，服务器会认为还有数据没接收
                      ps.println(msg);
                      ps.flush();
                  }
              }
          }
      }
      
      ```
    
      ```java
      // TCPServer
      import java.net.ServerSocket;
      import java.net.Socket;
      import java.util.concurrent.ArrayBlockingQueue;
      import java.util.concurrent.ExecutorService;
      import java.util.concurrent.ThreadPoolExecutor;
      import java.util.concurrent.TimeUnit;
      
      public class TCPServer {
          private static ExecutorService pool = new ThreadPoolExecutor(3, 5,
                  6, TimeUnit.SECONDS, new ArrayBlockingQueue<>(2),
                  new ThreadPoolExecutor.AbortPolicy());
      
          public static void main(String[] args) throws Exception {
              System.out.println("Server start...");
              ServerSocket serverSocket = new ServerSocket(7777);
              while (true) {
                  Socket socket = serverSocket.accept();
                  ServerReaderRunnable task = new ServerReaderRunnable(socket);
                  pool.execute(task);
              }
          }
      }
      
      ```
    
      ```java
      // Thread
      import java.io.BufferedReader;
      import java.io.IOException;
      import java.io.InputStream;
      import java.io.InputStreamReader;
      import java.net.Socket;
      
      public class ServerReaderRunnable implements Runnable {
          private Socket socket;
      
          public ServerReaderRunnable(Socket socket) {
              this.socket = socket;
          }
      
          @Override
          public void run() {
              try {
                  InputStream is = socket.getInputStream();
                  BufferedReader br = new BufferedReader(new InputStreamReader(is));
                  String msg;
                  while ((msg = br.readLine()) != null) {
                      System.out.println(socket.getRemoteSocketAddress() + ":" + msg);
                  }
              } catch (IOException e) {
                  System.out.println(socket.getRemoteSocketAddress() + " offline");
              }
          }
      }
      
      ```
    
    - 对于BS架构，只要在浏览器中访问ip+端口，就可以访问到对应socket；socket只要按html格式写入文本即可在浏览器上解析


### 单元测试

- Java程序最小功能单元是方法，单元测试就是针对Java方法的测试，检查正确性。
- 目前自己写main方法测试缺点

  - 只有一个main方法，某一个方法测试失败，其他方法会受到影响
  - 无法得到测试报告，需要自己去观察是否成功
  - 无法实现自动化测试

- JUnit优点
  - 可以灵活选择执行哪一些测试方法，可以一键执行所有测试方法
  - 可以生成测试报告
  - 某方法失败不影响其他方法执行

### 快速入门

1. 将JUnit包导入项目
2. 编写测试方法：该测试方法必须是公共的、无参数的、无返回值的、非静态的方法
3. 在测试方法上使用@Test注解：标注该方法是一个测试方法

### 一个实例

```Java
package forTest;

import org.junit.Assert;
import org.junit.BeforeClass;
import org.junit.Test;

public class TestUserService {

    @BeforeClass
    public static void init() {
        System.out.println("init...");
    }

    @Test
    public void testLogin() {
        UserService userService = new UserService();
        String msg = userService.login("admin", "123");
        Assert.assertEquals("something wrong...", "success", msg);
    }

    @Test
    public void testshow() {
        new UserService().show();
    }
}
```

### 相关注解

1. `@Test`：测试方法
2. `@Before`（JUnit5里是`@BeforeEach`）：修饰实例方法，该方法会在每个测试方法执行之前执行一次
3. `@After`（JUnit5里是`@AfterEach`）：修饰实例方法，该方法会在每个测试方法执行之后执行一次
4. `@BeforeClass`（JUnit5里是`@BeforeAll`）：用来修饰静态方法，该方法会在所有测试方法执行之前执行一次（初始化资源）
5. `@AfterClass`（JUnit5里是`@AfterEach`）：用来修饰静态方法，该方法会在所有测试方法执行之后执行一次（释放资源）

# 反射

- 反射指对于任何一个class类，在运行时候都看可以得到这个类的全部成分。

- 如，运行时候获取类的构造器对象Constructor；成员变量对象Field；成员方法对象Method

- 这种运行时候动态获取类信息以及调用类中成分的能力称为Java的反射机制

- 需要先获取编译后的Class类对象，然后就可以获得类的全部成分：HelloWorld.java ->  javac -> HelloWorld.class

  `Class c = HelloWorld.class`：类本身对象

## 流程

1. 获取Class类对象

   - 源码阶段：Class类中的静态方法`forName(String className)`
   - Class对象阶段：`类名.class`，如`Person.class`
   - 运行时阶段：`对象.getClass()`

   注意：一个类Class对象只会有一个，因为编译后只产生一个Class类文件

2. 获取构造器对象

   - `Constructor<?>[] getConstructors()`：返回public构造器对象的数组
   - `Constructor<?>[] getDeclaredConstructors()`：返回所有构造器对象的数组
   - `Constructor<T> getConstructor(Class<?>... parameterTypes)`：返回一个public构造器对象
   - `Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)`：返回一个构造器对象

   反射会破坏封装性

3. 获取成员变量

   - 从class类获取

     - `Field[] getDeclaredFields()`：获取所有成员变量

     - `Field getDeclaredField(String name)`：获取某一个成员变量

   - 从Field类获取

     - `void set(Object obj, Object value)`：为某个对象注入某个从成员变量值

     - `void get(Object obj)`：获取某个对象的某个成员变量的值

4. 获取方法对象：类似成员变量

   - 调用方法：`Object invoke(Object obj, Object... args)`
     - 参数1：用obj对象调用该方法
     - 参数2：方法参数
     - 返回值：方法返回值

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        Class<?> c = Class.forName("Person");
        // 获取构造器
        Constructor<?>[] constructors = c.getConstructors();
        for (Constructor<?> con : constructors) {
            System.out.println(con);
        }
        Constructor<?> con1 = c.getDeclaredConstructor();
        System.out.println(con1);
        con1.setAccessible(true); // 暴力反射
        Person p = (Person) con1.newInstance(); // 私有构造器无法创建对象
        System.out.println(p);
        Constructor<?> con2 = c.getConstructor(int.class, String.class);
        System.out.println(con2);
        Person p2 = (Person) con2.newInstance(3, "Mary");
        System.out.println(p2);

        // 获取所有成员变量属性
        Field[] declaredFields = c.getDeclaredFields();
        for (Field f : declaredFields) {
            System.out.println(f);
        }
        Field id = c.getDeclaredField("id");
        System.out.println(id.getName() + ": " + id.getType());
        // 为某个对象某个成员变量赋值
        id.setAccessible(true);
        id.set(p2, 999); // 该字段是private，需要暴力反射才能修改
        System.out.println(p2);
        System.out.println(id.get(p2));

        // 获取方法对象
        Method[] declaredMethods = c.getDeclaredMethods();
        for (Method method : declaredMethods) {
            System.out.println(method);
        }
        Method m1 = c.getDeclaredMethod("learn");
        m1.invoke(p2);
    }
}
```

## 反射作用

反射是作用在运行时的技术，此时集合的泛型无法再产生约束，此时是可以为集合存入其他任意类型的元素的。这是因为泛型只是**在编译阶段**可以约束集合只能操作某种类型，在编译成Class文件进入运行阶段时，其真实类型都是不带泛型的，相当于**泛型被擦除**了。

```java
import java.lang.reflect.Method;
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) throws Exception {
        ArrayList<Integer> arrayList1 = new ArrayList<>();
        ArrayList<String> arrayList2 = new ArrayList<>();
        System.out.println(arrayList1.getClass());
        System.out.println(arrayList2.getClass());
        System.out.println(arrayList1.getClass() == arrayList2.getClass());

        Method addMethod = arrayList1.getClass().getDeclaredMethod("add", Object.class);
        addMethod.invoke(arrayList1, true); // 可以在运行时候加入任何类型
        addMethod.invoke(arrayList1, "hello");
        System.out.println(arrayList1);

        ArrayList list3 = arrayList1; // 这样不用反射都行
        list3.add(4.4);
        System.out.println(arrayList1);
    }
}
```

# 注解

- Java注解（Annotation）时JDK5中引入的一种注释机制
- Java中类，构造器，方法，成员变量，参数等都可以被注解标注
- 作用是对Java中的类，方法，成员变量做标记，然后进行特殊处理。至于做什么特殊处理由业务决定。例如，在JUnit框架中，标记了注解的的方法就可以被当成测试方法执行。

## 自定义注解

```
public @interface 注解名称{
	public 属性名称 属性名() default 默认值;
}
```

```java
public @interface MyAnno {
    int id();
    String name();
}
```

```java
public class Main {
    public static void main(String[] args) throws Exception {

    }

    @MyAnno(id = 13, name = "abc")
    public static void show() {
        System.out.println("hello");
    }
}
```

- value属性，如果只有一个value的情况下，使用value属性的时候可以省略value名称不写

- 如果有多个属性，且没有默认值，那么value不能省略

  ```java
  public @interface MyAnno2 {
      int value(); // 特殊属性
      String price() default "abc";
  }
  ```

  ```java
  public class Main {
      @MyAnno2(1)
      private float a;
  
      public static void main(String[] args) throws Exception {
  
      }
  
      @MyAnno(id = 13, name = "abc")
      public static void show() {
          System.out.println("hello");
      }
  }
  ```

## 元注解

就是放在注解上面的注解。

常用的有两个：

1. `@Target`：约束自定义注解只能在哪些地方使用

   可使用的值定义在ElementType枚举类中，常用值有

   - TYPE：类，接口
   - FIELD：成员变量
   - METHOD
   - PARAMETER：方法参数
   - CONSTRUCTOR
   - LOCAL_VARIABLE

2. `@Retention`：申明注解的声明周期

   可用值定义在RetentionPolicy枚举类中，常用值有

   - SOURCE：只在源码阶段
   - CLASS：在源码阶段和字节码阶段（默认）
   - RUNTIME：一直到运行时都存在（常用）

## 注解解析

即判断是否存在注解，存在就解析出内容。

- Annotation：注解的顶级接口，注解都是这个类的实现对象
- AnnotatedElement：该接口定义了与注解解析相关的解析方法
- 所有类成分如Class，Method，Field，Constructor等都实现了AnnotatedElement接口，他们都有解析注解的能力
- 先获取类成分对象，然后再取解析相关的注解

```java
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        Class<?> c = Class.forName("Person");
        Method showMethod = c.getDeclaredMethod("show");
        if (showMethod.isAnnotationPresent(MyAnno.class)) {
            MyAnno ma = showMethod.getDeclaredAnnotation(MyAnno.class);
            System.out.println(ma.id());
            System.out.println(ma.name());
        }
    }
}
```

# 动态代理

对对象的行为做一些额外的操作。Spring里的aop就是这个

- Java中代理的代表类是java.lang.reflect.Proxy
- Proxy中提供了一个静态方法Proxy.newProxyInstance，用于为对象产生一个代理对象返回
- 使用代理对象调用方法可以增强方法

```java
package proxyDemo;

public interface UserService {
    String login(String username, String pwd);

    void query();
}
```

```java
package proxyDemo;

public class UserServiceImpl implements UserService {
    @Override
    public String login(String username, String pwd) {
        System.out.println("login...");
        if ("admin".equals(username) && "123".equals(pwd)) {
            return "succeed";
        }
        return "fail";
    }

    @Override
    public void query() {
        System.out.println("query...");
    }
}
```

```java
package proxyDemo;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class Main {
    public static void main(String[] args) {
        UserService u = getProxy(new UserServiceImpl());
        u.login("admin", "123");
        u.query();
    }

    public static UserService getProxy(UserService obj) {
        return (UserService) Proxy.newProxyInstance(obj.getClass().getClassLoader(),
                obj.getClass().getInterfaces(), new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("Verifying...");
                        Object rs = method.invoke(obj, args);
                        System.out.println("Exit...");
                        return rs;
                    }
                });
    }
}
```

## 优点

- 不改变方法源码的情况下，实现对方法的增强，提高代码复用
- 简化编程，提高效率
- 可以为被代理对象所有方法做代理
- 灵活，支持任意接口及其实现类做代理

# XML

全程可扩展标记语言（eXtensible Markup language）

可用于传输数据或者作为配置文件。

XML标签元素规则

- 由一堆尖括号和合法标识符组成，必须存在有且只有一个的根标签
- 大多数标签必须成对出现，特殊标签可以不成对，但是必须有结束标记，如`<br/>`
- 标签中可以定义属性，属性和标签名空格隔开，属性值必须用引号引起来`<student id="1"></name>`  
- 特殊字符，在xml中使用需要用转义
  - `&lt;`：<  小于
  - `&gt;`：>  大于
  - `&amp;`：&  和号
  - `&apos;`：'  单引号
  - `&quot;`："  双引号
- 或者使用CDATA区，也不会冲突

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!--这是注释-->
<student>
    <id>1</id>
    <name>abc</name>
    <sql>
        select * from user where age &lt; 18
        <![CDATA[
            select * from user where age < 18
        ]]>
    </sql>
</student>
```

## 文档约束

用来限定xml文件中标签以及属性怎么写。即制定好规范，比如有几级标签，一级标签是啥，二级标签是啥。。。。然后作为规范导入到xml文件中，即可约束xml文档。

主要有两类

- DTD（.dtd）：在需要编写的xml文件中导入DTD约束文档，无法约束数据类型。
- schema（.xsd）：可以约束数据类型

## xml解析

使用程序读取xml数据。

两种常见的解析技术

- SAX解析：一行行解析，适合大文件 
- DOM解析：将整个文件读入内存

工具：dom4j

### dom解析xml文档对象模型

- Document：整个xml文档
- Element：标签
- Attribute：属性
- Text：文本内容
- 后三者组成node对象

通过xpath可以检索某一个标签

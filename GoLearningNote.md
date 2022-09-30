# Go简介

- 能达到静态编译语言的安全和性能，同时还有动态语言开发维护的高效率
- 从c中继承了很多理念，如表达式语法，控制结构，基础数据类型，调用参数传值，指针等；保留了和c一样的编译执行方式及弱化的指针
- 自动垃圾回收机制
- 引入包的概念，用于组织程序结构，Go语言的每一个文件都要属于一个包，不能独立存在
- 天然支持并发
  - 语言层面支持并发，实现简单
  - goroutine，轻量级线程，可实现大并发处理，高效利用多核
  - 基于CPS并发模型(Communicating Sequential Processes)实现
- 吸收了管道通信机制，形成Go语言特有的管道channel。通过管道channel，可以实现不同的goroute之间的相互通信
- 函数返回多个值
- 新创新：如切片，延时执行defer等
- 下载SDK：
  - msi是类似exe的可执行文件，图形化界面安装
  - zip解压使用，需要自己配置环境变量
    - GOROOT：指定SDK的安装路径
    - Path：添加ADK的/bin目录
    - GOPATH：工作目录，Go项目的工作路径

# 入门

- 源码文件后缀是go，package表示该文件所属包名，import引入一个包，func是一个关键字，main函数是程序入口

  ```go
  // test.go
  // 每个文件都必须所属一个包
  package main
  import "fmt"
  func main(){
  	fmt.Println("hello world")
  }
  ```

  使用go bulid命令对go文件进行编译，生成exe文件。编译时候可以指定生成可执行文件的文件名。

  ```shell
  go build test.go
  test.ext
  go build -o newname.ext test.go
  
  go run test.go
  ```

  注意：通过go run命令可以直接运行test.go程序，类似执行一个脚本文件。工作中一般还是先编译后执行。编译之后会把依赖库也编译进去，所以会变大。

注意事项

- 入口是main()函数
- Golang严格区分大小写
- 每条语句后不需要分号（Go语言会在每行后自动加分号）
- Go编译器是一行行进行编译，因此我们一行就写一条语句，不可多条语句写在一行（实际上因为编译器会在每行末尾自动加分号，所以如果两个语句之间有一个分号，编译是可以通过的）
- **定义的变量或者import的包如果没有使用到，编译不能通过**
- Go是强类型语言，在编译时候确定类型
- 转义字符
  - \t：制表符
  - \n：换行符
  - \r：回车符，直接定位到行首开始输出，覆盖掉之前的内容
- main函数一定要在main包里，但是如果两个文件中都有main函数就会提示重名函数
- 块注释：/\*代码\*/；行注释：//
- 建议使用行注释
- 大括号必须是行尾风格
- 单行一般不超过80字符
- 调用函数的方法：import 包  然后包名.函数名

# 变量

## 变量声明

```go
package main

import "fmt"

// 全局变量声明方式1
var t1 = 12
var t2 = 2

// 全局变量声明方式2
var (
	t3 = 3.5
	t4 = "hi"
)

func main() {
	// 一定遵循先定义后使用
	// 变量声明方式1
	var a int
	// 变量声明方式2
	var b = 10.1
	// 变量声明方式3
	// 注意：变量不能是声明过的，如果没有冒号则是赋值语句
	c := "hello"
	// 多变量声明方式1
	var n1, n2, n3 int
	// 多变量声明方式2
	var m1, m2, m3 = 100, "hey", 8.8
	// 多变量声明方式3
	k1, k2, k3 := 100, "hey", 8.8

	fmt.Println(a, b, c)
	fmt.Println(n1, n2, n3)
	fmt.Println(m1, m2, m3)
	fmt.Println(k1, k2, k3)
}
```

- 在go中，函数外声明的变量就是全局变量

## 数据类型

- 基本数据类型
  - 数值型
    - 整数类型：int，int8，int16，int32，int64，uint，uint8，uint32，uint64，byte(uint8别名)，rune(大小和其一样，表示一个unicode编码)；int和uint根据系统决定，32位系统就是4字节，64位系统8字节
    - 浮点类型：float32，float64
  - 字符类型：没有，用byte保存单个字符，go里用utf8编码，单个字节无法存储汉字
  - 布尔型：bool
  - 字符串：string（官方将string归属到基本数据类型）
- 派生/复杂数据类型
  - 指针(Pointer)
  - 数组
  - 结构体(struct)
  - 管道(Channel)
  - 函数(也是一种类型)
  - 切片(silce)
  - 接口(interface)
  - map

注意事项

- 整数默认用int类型
- 浮点数
  - 使用可能有精度损失
  - 在go里有固定的范围和字段长度，不受os影响
  - 默认是float64，也推荐使用float64，精度更高

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	var i, j = 10, 3.4
	// 格式化输出，%T是输出类型，%d输出整数；后面函数可以返回一个变量所占字节大小
	fmt.Printf("i的类型是%T, 占%d字节", i, unsafe.Sizeof(i)) // int, 8字节
	fmt.Printf("j的类型是%T, 占%d字节", j, unsafe.Sizeof(j)) // float64, 8字节
	// 浮点数有可能有精度损失
	var n1 float32 = -123.0000901
	var n2 float64 = -123.0000901
	fmt.Println(n1, n2) // 输出结果-123.00009 -123.0000901
	// 科学计数法
	n3 := 5.1234e2
	n4 := 6.4321e3
	n5 := 7.8965e-2
	fmt.Println(n3, n4, n5)
}
```

### 字符类型

- golang中没有专门的字符类型，如果要存储单个字符，一般用byte来保存

- golang里字符串就是一串固定长度的字符链接起来的字符序列。**golang 的字符串是由单个字节连接的，而传统字符串是由字符组成的**

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	var c1 byte = 'a'
  	var c2 byte = 'b'
  	fmt.Println(c1, c2)
  	fmt.Printf("%c, %c", c1, c2)
  	// var c3 byte = '好'  // untyped rune constant 22909
  	var c3 int = '好'
  	fmt.Printf("%c, %d", c3, c3)
  }
  
  ```


### 布尔类型(bool)

- 只能取ture或者false，不可以用0或者非0数
- 占一个字节

### 字符串

- 固定长度字符连接成的字符序列
- 字节使用utf8编码标识Unicode文本
- 字符串一旦赋值，字符串就不能修改了，**在Golang中字符串是不可变的**
- 表示形式有两种
  - 双引号：会识别转义字符
  - 反引号：以字符串原生形式输出，包括换行和特殊字符，可以实现防止攻击、输出源代码等效果
- 拼接方式：+
  - 两边都是字符串可以直接拼接
  - 字符串太长需要分行写时候，加号要保留在上面一行末尾，这样编译器才不会在末尾加分号

```go
package main

import (
	"fmt"
)

func main() {
	var s string = "你好 hello"
	fmt.Printf("s: %v\n", s)
	var s1 = "hello"
	// s1[0] = 'a'  // 报错，不可修改
	s1 = "hi"
	fmt.Printf("s1: %v\n", s1)
	// 反引号原生输出
	s2 := `\n"""""`
	fmt.Printf("s2: %v\n", s2)
	// 字符串拼接
	s3 := "hello" + "world"
	s3 += " china"
	// 字符串很长时候可以分行写，注意+的位置，编译器看到+就不会加分号在末尾了
	s3 += "aaaaaaaaaaa" +
		"aaaaaaaaaa"
}
```

### 基本数据类型默认值

- 在go中，所有数据类型都有默认值，又叫零值
- 整型、浮点型：0
- 字符串：""
- 布尔型：false

```go
package main

import (
	"fmt"
)

func main() {
	var a int
	var b float32
	var c bool
	var d string
	// %d表示以十进制整数输出，%f表示以浮点数形式输出，%v表示默认值输出
	fmt.Printf("a = %d, b = %v, c = %v, d = %v", a, b, c, d)
}
```

### 基本数据类型转换

- **Golang在不同类型的变量之间赋值时需要显示转换**

- 基本语法：T(v)；将值v转换为类型T

  - T：就是数据类型，如int32，int64，float32等
  - v：需要转换的变量

- 注意

  - 范围小->大或者大->小都可以，但是大->小数据会溢出截断

  - 被转换的变量本身并不会发生变化（即可以理解为强转是有一个返回值，不改变被转变量本身）

    ```go
    package main
    
    import (
    	"fmt"
    )
    
    func main() {
    	var a uint = 256
    	var b uint8 = uint8(a)
    	fmt.Printf("b: %v\n", b)
    	// 报错 不可以将布尔类型转换成整型
    	// var c = false
    	// d := int(c)
    
    	// var n1 int32 = 10
    	// var n2 int8
    	// var n3 int64
    	// 报错，n1 + 10是int32类型，要写成n2 = int8(n1) + 10
    	// n2 = n1 + 10
    	// n3 = n1 + 10
    
    	var i int8 = 1
    	var j int8 = i + 127 // 编译通过，但是溢出
    	// var k int8 = i + 128  //编译不通过，128超过int8能表示范围
    	fmt.Printf("j: %v\n", j)
    }
    ```

#### 基本数据类型->string的转换

- 方式1：fmt.Sprintf("%参数", 表达式)

- 方式2：使用strconv包的函数

  ```go
  package main
  
  import (
  	"fmt"
  	"strconv"
  )
  
  func main() {
  	n1 := 12
  	n2 := 1.234
  	b := true
  	c := 'a'
  	// 方式1：根据format参数以格式胡字符串形式返回该变量字符串，类似Java的toString()
  	str := fmt.Sprintf("%d, %f, %v, %c", n1, n2, b, c)
  	fmt.Println(str)
  	// 方式2：使用strconv包的函数
  	str2 := strconv.FormatInt(int64(n1), 10)
  	s := strconv.Itoa(n1) // 对于int，可以使用这个简化方式
  	fmt.Println(s)
  	fmt.Printf("%T\n", str2)
  	// 'f'：格式标志  10：保留10位小数  64：表示这个小数是float64const
  	str3 := strconv.FormatFloat(n2, 'f', 10, 64)
  	fmt.Println(str3)
  }
  ```

#### string->基本数据类型转换

- 使用strconv包里的函数
- 如果转换的数据不是有效数据，不会报错，直接返回类型默认值（但是字符串"1"转布尔类型是ture）

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	s1 := "ture"
	s2 := "1"
	s3 := "2.3"
	var b bool
	var n int64
	var f float64
	b, _ = strconv.ParseBool(s1)
	n, _ = strconv.ParseInt(s2, 10, 0)
	f, _ = strconv.ParseFloat(s3, 64)
	fmt.Println(b, n, f)
	b1, err := strconv.ParseBool(s2)
	fmt.Printf("b1: %v\n", b1)
	fmt.Println(err)
}
```

### 指针

- 基本数据类型，变量存储的是值，也叫值类型；指针类型：变量存储的是一个地址，这个地址空间指向的空间存的才是值，如`var ptr *int = &num`

- 获取变量的地址，用&

- 获取指针类型所指向的值，用*

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	var i int = 10
  	fmt.Println(&i) // 0xc000012098
  	var ad *int = &i
  	fmt.Printf("%T \n", ad) // *int
  	fmt.Println(*ad)        // 10
  }
  ```

- 值类型都有对应的指针类型，形式为 *数据类型

- **值类型**：包括基本数据类型int系列、float系列、bool、string、**数组和结构体struct**

### 值类型和引用类型

- 值类型：包括基本数据类型int系列、float系列、bool、string、数组和结构体struct；

  变量直接存储值，内存**通常**在**栈**中分配

- 引用类型：指针、slice切片、map、管道chan、interface等；

  **变量存储地址，地址对应的空间才真正存储数据。**内存**通常**在**堆**上分配，当没有任何变量引用这个地址时候，该地址对应数据空间就成为一个垃圾，由GC来回收

- 通常是这样，但是Golang中栈和堆区分不是很明确，**逃逸分析**可能不按通常情况

### 标识符

Golang对各种变量、方法、函数等命名的字符序列称之为标识符。凡是自己可以起名字的地方都叫标识符。

#### 规则

- 英文字母大小写，0-9，下划线
- 数字不可开头
- 严格区分大小写
- 不能含空格
- 下划线在Golang中是一个特殊的标识符，成为空白标识符。它可以代表任何其他的标识符，但是他**对应的值会被忽略**（例如忽略某个返回值）。所以仅能被用作占位符使用，不能作为标识符使用。
- 保留关键字不能作为标识符（int不是保留关键字，是预定义的标识符，但是也不要使用）

#### 特点和规范

- 尽量保证文件的package 包名和所在目录一致，且不要和标准库名一样

- 变量名、函数名、常量名采用驼峰命名法

- 如果变量名、函数名、常量名**首字母大写，则可以背其他的包访问；如果首字母小写，则只能在本包中使用。**（可以简单理解为首字母大写是共有的，小写是私有的）

- 例子

  ```go
  package utils
  
  var Name string = "Zev"
  ```
  
  ```go
  package main
  
  import (
  	"HelloGo/src/utils" // Hello是go mod里的模块名
  	"fmt"
  )
  
  func main() {
  	fmt.Println("hello world")
  	fmt.Println(utils.Name) // 使用包名.变量即可调用，不需要文件名
  }
  ```
  

### 常量

- 使用const修饰

- 定义的时候必须初始化

- 不能修改

- 只能修饰bool，数值类型，string类型

- 语法：`const identifier [type] = value`

- 使用

  ```go
  package main
  
  func main() {
  	const a int = 3
  	const b = 1.4
  	// 写法1
  	const (
  		c = 3
  		d = 4
  	)
  	// 写法2
  	const (
  		e = iota //表示e给0，f为1，依次递增
  		f
  		g
  	)
  }
  ```

- 没有常量名必须全大写字母的规定

- 仍然通过首字母控制访问范围

# 运算符

- 算数运算符
  - 有i++，没有++i；-同理
  - 在Golang中，i++只能独立使用，不支持a = i++这样的用法
  
- 逻辑运算符
  - &&：逻辑与，同时也是短路与
  - ||：逻辑或，同时也是短路或
  
- 赋值运算符
  - +=之类的不再赘述
  - <<=：左移后赋值，a <<= 3等价于a = a<<3
  - \>\>=：右移后赋值
  - &=：按位与后赋值，a &= 3等价于a = a & 3
  - ^=：按位异或后赋值
  - |=：按位或后赋值
  
- 运算符优先级：只有单目运算符和赋值运算是从右向左，其他是从左向右

- 其他运算符
  - &：取地址
  - *：取指针指向空间值
  
- **运算符两端必须是同一类型，且Golang没有隐式转换，所有类型转换都是显式转换。**

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	var n1 int32 = 1
  	var n2 int64 = 2
  	// n3 := n1 + n2 报错
  	n3 := n1 + int32(n2)
  	fmt.Println(n3)
  }
  ```

# 获取输入

使用fmt包下的Scanln函数和Scanf函数。

- Scanln：类似Scan，但会在换行时停止扫描。最后一个条目必须有换行或者到达结束位置。
- Scanf：从标准输入扫描文本，根据format参数指定的格式将成功读取的空白分隔的值保存进成功传递给本函数的参数。返回成功扫描的条目个数和遇到的任何错误。

```go
package main

import "fmt"

func main() {
	var name string
	var age byte
	// 方式1：使用Scanln
	fmt.Print("Plz input name:")
	fmt.Scanln(&name) // 传地址进去才能改变外面的值，不然就是传值进去了
	fmt.Print("Plz input age:")
	fmt.Scanln(&age)
	fmt.Printf("name: %v, age: %d \n", name, age)
	// 方式2：使用Scanf
	fmt.Print("Plz input name and age seperate by space:")
	fmt.Scanf("%s %d", &name, &age)
	fmt.Println(name, age)
}
```

# 进制

```go
package main

import "fmt"

func main() {
	var i int = 5
	fmt.Printf("%b \n", i)
	var j int = 011 //八进制，以0开头
	fmt.Println(j)
	var k int = 0x11 //十六进制，以0x开头，不区分大小写
	fmt.Println(k)
}
```

或，异或，与，位移这些操作都是以补码来做运算

# 流程控制

## if-else分支

```go
package main

import "fmt"

func main() {
	var i int = 20
	if i > 5 {
		fmt.Println("20 > 5")
	}
	// Golang里的特殊用法，可在if后面直接声明变量 注意此时j的作用域在if语句块内
	if j := 35; j > 50 {
		fmt.Println(j)
	} else if j < 20 {
		fmt.Println("aaa")
	} else { //else可以没有
		fmt.Println("bbb")
	}
}
```

就算只有一条语句，大括号也必须有。

## switch分支

- 用于基于不同条件执行不同动作，每一个case分支都是唯一的，从上到下逐一测试直到匹配为止
- **匹配项后面不需要加break**
- 可以匹配多个表达式，执行一个行为
- 匹配的数据类型要一致，否则报错
- default非必须

```go
package main

import "fmt"

func main() {
	var n byte = 'a'
	switch n {
	case 'a', 'b', 'c':
		fmt.Println("abc")
		fallthrough // switch穿透，会向下执行下一个case的语句（不经过判断），只会穿透一次
	case 'd', 'f':
		fmt.Println("df")
	default:
		fmt.Println("default")
	}
	// 可以当作一个if-else分支使用
	switch {
	case n > 'd':
		fmt.Println("hello")
	case n < 'b':
		fmt.Println("world")
	}
	// 甚至还可以在switch后直接声明一个变量，不推荐使用
	switch m := 'a'; {
	case m > 'd':
		fmt.Println("hello")
	case m < 'b':
		fmt.Println("world")
	}
}
```

- 还可用于type-switch

```go
package main

import "fmt"

func main() {
	var x interface{}
	var y = 3
	x = y
    switch x.(type) { //这里x.(type)好像是switch才有的固定用法
	case nil:
		fmt.Println("nil")
	case int:
		fmt.Println("int")
	case float64:
		fmt.Println("float64")
	default:
		fmt.Println("unknow")
	}
}
```

## for循环

```go
package main

import "fmt"

func main() {
	// 写法1
	for i := 0; i < 2; i++ {
		fmt.Println("hello")
	}
	// 循环变量在循环外初始化
	j := 0
	for ; j < 3; j++ {
		fmt.Println("hi")
	}
	fmt.Println(j) // 3
	// 写法2
	k := 0
	for k < 4 {
		k++
	}
	// 死循环写法
	for {
		break
	}
}

```

for-range方式遍历字符串或者数组

```go
package main

import "fmt"

func main() {
	var str string = "hello 你好"
	// 传统方式，按一个字节遍历，有中文会乱码
	for i := 0; i < len(str); i++ {
		fmt.Printf("%c\n", str[i])
	}
	// 将字符串转换成[]rune切片可以解决
	str2 := []rune(str)
	for i := 0; i < len(str2); i++ {
		fmt.Printf("%c\n", str2[i])
	}
	// for-range方式 可以处理中文
	for index, val := range str {
		fmt.Printf("%d, %c\n", index, val)
	}
}
```

## while和do...while实现

Golang中没有，通过for去实现

## break/continue

基本使用

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	var cnt int = 0
	// 初始化随机种子的语句如果放在for内部 1秒只会有一个随机数
	rand.Seed(time.Now().Unix())
	for {
		n := rand.Intn(100) + 1
		fmt.Println("n =", n)
		cnt++
		if n == 99 {
			break
		}
	}
	fmt.Println("cnt =", cnt)
}
```

在多层嵌套的语句块中，可以通过标签指明要终止那一层的语句块。不加标签默认跳出最近的循环。continue同理。

```go
package main

func main() {
label1:
	for i := 0; i < 3; i++ {
		label2:
		for j := 0; j < 4; j++ {
			for k := 0; k < 5; k++ {
				break label1
			}
            continue label1
		}
	}
}
```

## goto

- 无条件转移到指定的行
- 通常与条件语句配合使用，用来实现条件转移，跳出循环体等功能
- **一般不主张使用**，以免造成程序流程混乱，使理解和调试程序困难

```go
package main

import "fmt"

func main() {
	fmt.Println("a")
	if n := 20; n > 10 {
		goto label1
	}
	fmt.Println("b")
label1:
	fmt.Println("c")
}
```

# 函数

- 基本语法：

  ```
  func 函数名 (形参列表) (返回值列表) {
  	执行语句...
      return 返回值列表
  }
  ```

- 简单示例

```go
package main

import "fmt"

func cal(n1 float64, n2 float64, op byte) float64 {
	var res float64
	switch op {
	case '+':
		res = n1 + n2
	case '-':
		res = n1 - n2
	case '*':
		res = n1 * n2
	case '/':
		res = n1 / n2
	}
	return res
}

func main() {
	f := cal(3.3, 5.5, '+')
	fmt.Printf("f: %v\n", f)
}
```

- _表示占位忽略

## 注意事项

- **基本数据类型**和**数组**默认都是值传递，即进行值拷贝。

- 如果希望函数内能修改函数外的变量（基本数据类型，数组），可以传入变量的地址&，函数内以指针的方式操作变量。从效果上类似引用

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func f(p *int) {
  	*p += 1
  }
  
  func main() {
  	var n int = 3
  	f(&n)
  	fmt.Println(n)
  }
  ```

- Golang函数**不支持重载**。因为Golang支持可变参数和空接口，可以实现相似功能

- 在Golang中，**函数也是一种数据类型**，可以赋值给一个变量，该变量就是一个**函数类型的变量**了。通过该变量可以实现对函数的调用。

- 函数作为一种数据类型，还可以作为形参传递给函数

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func sum(a int, b int) int {
  	return a + b
  }
  
  // 第1、2个int是传入函数的形参类型，第3个int是传入函数变量的返回值
  func test(funcVar func(int, int) int, n1 int, n2 int) int {
  	return funcVar(n1, n2)
  }
  
  func main() {
  	// 函数也是一种数据类型，可以赋值给一个变量
  	f := sum
  	fmt.Println(f(3, 5))
  	fmt.Printf("f的类型: %T\n", f) // func(int, int) int
  
  	// 函数还可以作为形参传入函数
  	res := test(sum, 8, 9)
  	fmt.Println(res)
  }
  ```

- Golang支持自定义数据类型

  ```go
  package main
  
  import "fmt"
  
  // 给函数类型取别名
  type myFun func(int, int) int
  
  //在形参列表中用函数类型别名myFun
  func test(funcVar myFun, n1 int, n2 int) int {
  	return funcVar(n1, n2)
  }
  
  func sum(a int, b int) int {
  	return a + b
  }
  
  func main() {
  	type myInt int // 相当于给int取了别名；但是在Go中认为myInt和int是两个类型
  	var n1 myInt = 1
  	fmt.Println(n1)
  	// var n2 int = n1 // 报错Go中认为myInt和int是两个类型
  	var n2 int = int(n1)
  	fmt.Println(n2)
  
  	test(sum, 1, 2)
  }
  ```

- 支持对函数返回值命名

  ```go
  package main
  
  import "fmt"
  
  func getSumAndSub(a int, b int) (sum int, sub int) {
  	sum = a + b
  	sub = a - b
  	return
  }
  
  func main() {
  	n, m := getSumAndSub(4, 1)
  	fmt.Printf("sum=%d, sub=%d", n, m)
  }
  ```

- 当不需要某个返回值时，使用_作为返回值占位符

- Golang支持可变参数，可变参数一定放在最后

  ```go
  package main
  
  // 支持0到多个参数 args是slice切片，通过args[index]访问到各个值
  func sum1(args ...int) int {
  	sum := 0
  	for i := 0; i < len(args); i++ {
  		sum += args[i]
  	}
  	return sum
  }
  
  // 1到多个参数
  func sum2(n1 int, args ...int) int {
  	sum := n1
  	for i := 0; i < len(args); i++ {
  		sum += args[i]
  	}
  	return sum
  }
  func main() {
  
  }
  ```

- 形参中多个形参类型相同，可以写在一起，最后写类型（和声明变量一样）。如`func f(n1 int, n2 int)` 可以写成`func f(n1, n2 int)`

## init函数

每一个源文件都可以包含一个init函数，该函数会在main函数执行前被Golang运行框架调用，主要用于初始化工作。

如果一个文件同时包含全局变量定义，init函数和main函数，则执行流程是 **全局变量定义->init函数->main函数**。

如果引入了其他包，则是 **引入包的全局变量定义->引入包init函数->main文件全局变量定义->main文件init函数->main文件main函数**。

```go
// main.go
package main

import (
	"HelloGo/src/utils"
	"fmt"
)

var v int = test()

func test() int {
	fmt.Println("main.go全局变量初始化...")
	return 1
}

func init() {
	fmt.Println("main.go init...")
}

func main() {
	fmt.Println("main...")
	fmt.Println(utils.Name)
}
```

```go
// util.go
package utils

import "fmt"

// var Name string = "mart"

var Name string = setName()

func setName() string {
	fmt.Println("util.go全局变量初始化...")
	return "Zev"
}

func init() {
	fmt.Println("util.go init...")
}
```

```
// 输出结果
util.go全局变量初始化...
util.go init...
main.go全局变量初始化...
main.go init...
main...
Zev
```

## 匿名函数

Golang支持匿名函数，如果我们某个函数只是希望使用一次，可以考虑使用匿名函数。匿名函数也可以实现多次调用。

- 使用方式1：定义匿名函数时就直接调用
- 使用方式2：将匿名函数赋给一个变量（函数变量），再通过该变量来调用匿名函数

如果将匿名函数赋值给一个全局变量，则成为一个全局匿名函数，可以在程序中使用。

```go
package main

import (
	"fmt"
)

// 全局匿名函数
var myFun = func(n1, n2 int) int {
	return n1 * n2
}

func main() {
	// 定义时候直接调用，没有函数名
	// 这样就可以在函数中定义一个函数
	// 方式1
	res1 := func(n1, n2 int) int {
		return n1 + n2
	}(2, 5)
	fmt.Println(res1)
	// 方式2
	f := func(n1, n2 int) int {
		return n1 - n2
	}
	res2 := f(5, 1)
	fmt.Println(res2)

	res3 := myFun(3, 5)
	fmt.Println(res3)
}
```

## 闭包

闭包就是一个**函数**和与**其相关的引用环境**组合的一个**整体**（实体）。

```go
package main

import (
	"fmt"
)

// 返回值是一个函数类型
func AddUpper() func(int) int {
	var n int = 10
	return func(x int) int {
		n += x
		return n
	}
}

func main() {
	f := AddUpper()
	fmt.Println(f(1)) // 11
	fmt.Println(f(2)) // 13
	fmt.Println(f(3)) // 16

}
```

上面这个例子，返回一个匿名函数，但是这个匿名函数引用到函数外的n，因此这个匿名函数和n形成一个整体，构成闭包（有点类似一个类）。n只会初始化一次，每调用一次就进行累计。

闭包的关键，就是分析出返回的函数它使用（引用）到哪些变量，因为函数和它引用到的变量共同构成闭包。

有点像Java里，`f := AddUpper()`就类似new了一个对象，返回了`func(int) int`给f，然后调用成员方法对静态变量进行修改。

### 一个例子

```go
// 一个函数makeSuffix接收一个文件后缀，返回一个闭包
// 对于没有指定后缀的文件名，加上.jpg；有的话直接返回
package main

import (
	"fmt"
	"strings"
)

// 返回值是一个函数类型
func makeSuffix(suffix string) func(string) string {

	return func(fileName string) string {
		if !strings.HasSuffix(fileName, suffix) {
			return fileName + suffix
		}
		return fileName
	}
}

func main() {
	f := makeSuffix(".jpg") // 有点类似Java里new了一个对象，".jpg"类似构造器传参
    fmt.Println(f("a")) // 然后f("a")像是调用了对象的成员方法，方法里利用到了类的静态全局变量(".jpg")
	fmt.Println(f("b.jpg"))
}
```

这里类似Java里的类实例化然后调用成员函数，调用过程中用到了类的静态成员变量(".jpg")。

## defer关键字

在函数中，程序员经常需要创建资源（如数据库连接、文件句柄、锁等），为了在函数执行完毕后，即使的释放资源，Golang提供了defer（延时机制）。

当执行到defer语句时，先不执行，将defer语句压入到defer栈中。**入栈同时会将当前相关变量拷贝一份入栈**。当目前函数执行完毕之后，再从defer栈出栈执行。

## 例子

```go
package main

import (
	"fmt"
)

// 返回值是一个函数类型
func sum(n1, n2 int) int {
	defer fmt.Println("hello, n1 =", n1) // 输出3
	defer fmt.Println("world, n2 =", n2) // 输出2
	n1 *= 2
	n2 *= 2
	res := n1 + n2
	fmt.Println("!!!") // 输出1
	return res
}

func main() {
	res := sum(10, 30)
	fmt.Println("res =", res) // 输出4
}

```

```
// 输出结果
!!!
world, n2 = 30
hello, n1 = 10
res = 80
```

最大作用就是用于**释放创建的资源**。比如数据库连接，io流等，创建完直接defer close()，然后直接使用。函数结束就会自动执行。

## 函数参数传递方式

- 值传递：基本数据类型int系列、float系列、bool、string、数组和结构体struct
- 引用传递：指针、slice切片、map、管道chan、interface等

**在Java中，所有传递都是值传递，只不过对于引用类型，传递的是引用的值；这是因为在Java中没有指针和取地址符&；而在Golang中，值类型直接传值，引用类型直接传的是引用。对于值类型也可以通过取地址符&得到其地址。**

## 变量作用域注意事项

全局变量和局部变量基本和其他语言一致。

注意下面这个例子

```go
package main

import (
	"fmt"
)

var Age int = 20 // 这条语句没问题，声明时候赋值
Name := "Tom" // 这条语句报错
// 因为上条语句等价于
// var Name string
// Name = "Tom"
// 但是赋值语句不能再函数体外
func main() {
	
}
```

# 包 

- 本质就是创建文件夹存放文件
- Golang每一个文件都属于一个包，它以包的形式管理程序文件和项目目录结构
- 作用
  - 区分相同名字的函数，变量等标识符
  - 程序文件很多时候，可以很好的管理项目
  - 控制函数，变量等的访问范围，即作用域
- 给一个文件打包时候，该包对应一个文件夹，文件包名通常与文件所在文件夹一致，一般为小写字母
- 当一个文件要使用其他包函数或变量时，需要先引入对应的包
- package指令在文件第一行，然后是import指令
- 需要跨包访问的函数和变量首字母要大写
- 可以取别名，在引入的包名前写一个别名即可；使用时用别名
- 包里不能有相同的函数和变量名，**即使是不同的文件**（即一个包名作为一个命名空间，而不是文件名）
- **如果要编译成一个可执行文件，就需要将这个包声明为main，即package main。**如果是写一个库，则可以自定义。**编译时候需要编译main包所在的文件夹。**

# 常用API

## 字符串

- `len(str)`：统计字符串长度，按字节，是一个内建(builtin)函数
- `r := []rune(str)`：字符串遍历，处理有中文的情况
- `n, err := strconv.Atoi("23")`：字符串转整数
- `strconv.Itoa(123)`：整数转字符串
- `var bytes []byte = []byte("hello world")`：字符串转字节切片
- `str = string([]byte{97, 98, 99})`：[]byte转字符串
- `strconv.FomatInt(i int64, base int) string`：10进制转2进制
- `strings.Contains(str, substr)`：查找str中是否有substr
- `strings.Count(str, substr)`：返回str中有几个不重复的substr
- 比较字符串，"=="区分大小写；`strings.EqualFold(str1, str2)`不区分大小写
- `strings.Index(str, substr)`：返回子串在主串出现的第一个位置下标
- `strings.LastIndex(str, substr)`：返回子串在主串最后一次出现位置的下标
- `strings.Replace(str, substr, n)`：将指定子串替换成另一个子串，`n`指定希望替换几个，`n = -1`表示全部替换
- `strings.Split(str, sperator)`：以分隔符分割成字符串数组
- `strings.ToLower(str)``和str.toUpper(str)`：将字符串大小写转换
- `strings.TrimSpace(str)`：去掉字符串两端的空格
- `strings.Trim(str, str)`：去掉字符串两端指定字符串
- `strings.TrimLeft(str, str)`/`strings.TrimRight(str, str)`：去掉左/右边某指定字符串
- `strings.HasPrefix(str, str)`/`strings.HasSuffix(str, str)`：判断字符出啊你是否以指定字符串开头/结尾

## 时间和日期

通过`time`包实现。

### 格式化输出时间

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now()
	fmt.Println(now.Year())
	// Month()返回的是int类型，但是go内部重写了它的String()方法所以打印July
	fmt.Println(now.Month(), int(now.Month()))
	// 格式化输出
	//方式1：使用Printf输出
	fmt.Printf("%d %d %d", now.Year(), now.Month(), now.Day())
	str := fmt.Sprintf("%d %d %d", now.Year(), now.Month(), now.Day())
	fmt.Println(str)
	// 方式2：使用time.Format
	date1 := now.Format("2006,01,02 15:04:05") // 数字不能改
	fmt.Println(date1)
	date2 := now.Format("2006-01-02")
	fmt.Println(date2)
	date3 := now.Format("15:04:05")
	fmt.Println(date3)
}

```

`"2006/01/02 15:04:05"`这一串字符串的数字不能更改，比如2006就代表取年，如果只要年则只要个`"2006"`就可以了。这个"2006"就相当于Java中的"YY"。

### 时间常量与unix时间戳

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	// 使用时间常量 每隔0.5秒输出一下
	i := 1
	for {
		time.Sleep(500 * time.Millisecond) //只能整数*毫秒 不能秒除以2
		fmt.Println(time.Now())
		i++
		if i > 3 {
			break
		}
	}

	//
	fmt.Println(time.Now().Unix())
}

```

## 内置函数

1. `len()`：求长度，如string、array、slice、map、channel

2. `new()`：分配内存，主要用来分配值类型，如int、float系列、struct等；**返回指针**

   ```go
   package main
   
   import (
   	"fmt"
   )
   
   func main() {
   	n1 := 100
   	fmt.Printf("Type: %T, Value: %v, address: %v\n", n1, n1, &n1)
   	// 结果；Type: int, Value: 100, address: 0xc000012098
   	n2 := new(int)
   	fmt.Printf("Type: %T, Value: %v, address: %v, pointToVal: %v\n", n2, n2, &n2, *n2)
   	// 结果：Type: *int, Value: 0xc0000120b8, address: 0xc000006030, pointToVal: 0
   }
   
   ```

3. `make()`：分配内存，主要用来分配引用类型，如chan、map、slice等；

## 错误

```go
package main

import (
	"fmt"
)

func main() {
	n1 := 10
	n2 := 0
	n3 := n1 / n2
	fmt.Println(n3)
	fmt.Println("...")
}

```

```
// 输出
panic: runtime error: integer divide by zero

goroutine 1 [running]:
main.main()
        D:/Code/GoLearning/src/main/test.go:10 +0x11
exit status 2
```

程序发生错误`panic`会报错。但是在Golang中不支持传统的`try...catch...finally`处理。Golang引入的处理方式为`defer,panic,recover`。

简单来说，Golang中可以抛出一个`panic`异常，然后再`defer`中通过`recover`捕获这个异常然后处理。

```go
package main

import (
	"fmt"
)

func test() {
	// 使用defer + recover来捕获和处理异常
	defer func() {
		// recover()是内置函数，可以捕获到异常
		if err := recover(); err != nil {
			fmt.Println("err =", err)
			// 对异常进行处理
		}
	}()
	n1 := 10
	n2 := 0
	n3 := n1 / n2
	fmt.Println(n3) // 程序出错后直接终止，然后到错误处理部分，此句话不执行
}

func main() {
	test()
	fmt.Println("main...")
}

```

```
// 输出结果
err = runtime error: integer divide by zero
main...
```

### 自定义错误

Golang中也支持自定义错误，使用`errors.New`和`panic`内置函数。

1. `errors.New("错误说明")`会返回一个error类型的值，表示一个错误
2. `panic`内置函数，接受一个interface{}类型的值（也就是任何值了）作为参数。可以接受`error`类型的变量，输出错误信息并推出程序。

```go
package main

import (
	"errors"
)

func readFile(name string) (err error) {
	if name == "config.ini" {
		// do...
		return nil
	} else {
		return errors.New("读取文件错误") // 类似new了一个异常
	}
}

func main() {
	err := readFile("config1.ini")
	if err != nil {
		panic(err) // 类似throw了一个一场
	}
}

```

# 数组和切片

## 数组

一种数据类型，存放多个同一类型数据。在Golang中，数组是一种**值类型**。

### 一个例子

```go
package main

import (
	"fmt"
)

func main() {
	var arrs [5]int
	// 初始化数组的几种方式方式
	var arrs1 [3]int = [3]int{1, 2, 3}
	var arrs2 = [5]int{1, 2, 3, 4, 5}
	var arrs3 = [...]int{5, 6} // 让编译器自己推断个数
	var arrs4 = [2]int{1: 11, 0: 00}
	arrs5 := [...]string{1: "Tom", 0: "Zev", 2: "Mary"}
	fmt.Println(arrs1, arrs2, arrs3, arrs4, arrs5)

	// 常规for循环遍历
	for i := 0; i < len(arrs); i++ {
		arrs[i] = i
	}
	// for range方式，快捷键是forr
	for index, value := range arrs {
		fmt.Printf("index:%d, value:%d", index, value)
	}
	for _, v := range arrs {
		fmt.Print(v)
	}
    // 数组的地址就是第一个元素的地址，&arr可以取到地址，但是得使用格式化输出打印出来，fmt.Println()打不出来
	fmt.Printf("%p\n", &arrs) // 0xc00000a330
	fmt.Println(&arrs[0])     // 0xc00000a330
	fmt.Println(&arrs[1])     // 0xc00000a338
}

```

### 注意事项和细节

1. 长度固定，不能变化

2. `var arr []int`中括号里没写大小，这个不是数组，是一个slice切片；数组的话一定得写大小

3. 数组创建后，有默认零值

4. Golang数组是值类型，**默认情况下是值传递**，因此会进行拷贝。要改原数组，可通过传递引用。

   

5. 在Golang里，数组长度是数组类型的一部分。比如`[3]int`和`[4]int`会被认为两个不同的数据类型

   ```go
   var arr1 [3]int
   var arr2 [4]int
   fmt.Printf("%T, %T", arr1, arr2)
   // 输出[3]int, [4]int
   ```

   所以在传递函数参数时，需要考虑数组长度。

### 二维数组

每个元素是一个一维数组。声明方式类似一维数组。

- `var arr2D [3][4]int`声明一个3 * 4大小的二维数组

- `var arr2D = [...][3]int{{1, 2}, {2, 3}, {3, 4}}`：输出`[[1 2 0] [2 3 0] [3 4 0]]`

- 只有第一个`[]`里能用[...]，第二个不能

- 遍历

  - 双层for循环

  - for range

    ```go
    package main
    
    import (
    	"fmt"
    )
    
    func main() {
    	// var arr2D [3][4]int
    	var arr2D = [...][3]int{{1, 2}, {2, 3}, {3, 4}}
    	fmt.Println(arr2D)
    	for _, v := range arr2D {
    		for _, k := range v {
    			fmt.Printf("%d ", k)
    		}
    		fmt.Println()
    	}
    }
    ```

## 切片

### 基本概念

1. 英文是slice
2. 切片是数组的一个引用，因此切片是引用类型。在进行形参传递时候，尊时候引用传递的机制。
3. 切片使用类似数组
4. 切片长度可以变化，因此切片是一个可以动态变化的数组
5. 定义语法：`var mySlice []int`

```go
package main

import "fmt"

func main() {
	var arr [5]int = [...]int{1, 2, 3, 4, 5}
	sl := arr[1:3]                // 含头不含尾
	fmt.Println(sl)               // [2 3]
	fmt.Println("元素个数:", len(sl)) // 元素个数: 2
	fmt.Println("容量:", cap(sl))   // 容量: 4
	// sl存了切片第一个元素的地址
	fmt.Println(&arr[1] == &sl[0]) // true
}
```

从底层来说，slice就是一个数据结构（struct结构体），包含三部分

```go
type slice struct {
	ptr *[2]int
	len int
	cap int
}
```

### 切片创建方式

1. 定义一个切片，让切片取引用一个已经创建好的数组，上面例子就是
2. 通过make创建切片
   - 通过这种方式可以指定切片大小和容量
   - 此时相当于创建了一个结构体，然后指针部分指向了一个内部的、对外不可见的数组，该数组由make底层维护，只能通过slice访问
3. 直接赋值，原理类似make

```go
package main

import "fmt"

func main() {
	var slice1 []int
	fmt.Println(slice1) // []
	// 方式2：make创建
	// 创建一个int类型切片，长度为4，容量为10
	var slice2 []int = make([]int, 4, 10)
	fmt.Println(slice2)                   // [0 0 0 0]
	fmt.Println(len(slice2), cap(slice2)) // 4 10
	// 方式3：直接赋值，原理类似make
	var slice3 []int = []int{1, 3, 5}
	fmt.Println(slice3, len(slice3), cap(slice3)) // [1 3 5] 3 3
}
```

### 注意事项与细节

- 第一种方式初始化时候

  - `slice := arr[0:end]`可以简写成`slice := arr[:end]`
  - `slice := arr[start:len(arr)]`可以简写成`slice := arr[start:]`
  - `slice := arr[0:len(arr)]`可以简写成`slice := arr[:]`

- `var slice []int`只是这样定义后，本身是空的，需要要让其引用到一个数组或者make一个空间后再使用

- 切片可以再次切片

- `append(slice, data1, data2, ...)`

  - 如果追加元素未超过容量，则在原先数组上增加，但是**会返回一个新的slice**（即slice本身是新的，但是里面指向数组的地址并不改变）
  - 如果超过了容量，则会新建一个地址，返回新的数组地址和新的slice地址

  ```go
  package main
  
  import "fmt"
  
  func main() {
  	var slice []int = make([]int, 3, 4)
  	slice[0] = 0
  	slice[1] = 1
  	slice[2] = 2
  	fmt.Printf("before appending, slice = %p, &slice[0] = %p\n", slice, &slice[0])
  	slice2 := append(slice, 400)
  	fmt.Printf("after appending, slice2 = %p, &slice2[0] = %p\n", slice2, &slice2[0])
  
  	fmt.Println(slice, slice2)
  
  	// 还可以追加切片（只能是切片，不能是数组）
  	slice = append(slice, slice...) // ...是固定写法
  	fmt.Println(slice)
  }
  ```
  
  ```
  // 输出
  before appending, slice = 0xc0000101c0, &slice[0] = 0xc0000101c0
  after appending, slice2 = 0xc0000101c0, &slice2[0] = 0xc0000101c0
  [0 1 2] [0 1 2 400]
  [0 1 2 0 1 2]
  ```
  
  - 如果从一个数组创建切片然后append操作，在为达到扩容时，切片元素的改变也会影响到原数组

  ```go
  package main
  
  import "fmt"
  
  func main() {
  	var arr [5]int = [5]int{10, 20, 30, 40, 50}
  	var slice = arr[1:4]
  
  	fmt.Println("arr =", arr)
  	fmt.Println("slice =", slice, "len =", len(slice), "cap =", cap(slice))
  	fmt.Printf("&arr[1] = %p, &slice = %p\n", &arr[1], &slice[0])
  	slice = append(slice, 888)
  	fmt.Println("arr =", arr)
  	fmt.Println("slice =", slice, "len =", len(slice), "cap =", cap(slice))
  	fmt.Printf("&arr[1] = %p, &slice = %p\n", &arr[1], &slice[0])
  	slice = append(slice, 999)
  	fmt.Println("arr =", arr)
  	fmt.Println("slice =", slice, "len =", len(slice), "cap =", cap(slice))
  	fmt.Printf("&arr[1] = %p, &slice = %p\n", &arr[1], &slice[0])
  }
  ```
  
  ```
  // 输出结果
  arr = [10 20 30 40 50]
  slice = [20 30 40] len = 3 cap = 4
  &arr[0] = 0xc0000cc038, &slice = 0xc0000cc038
  arr = [10 20 30 40 888]
  slice = [20 30 40 888] len = 4 cap = 4
  &arr[0] = 0xc0000cc038, &slice = 0xc0000cc038
  arr = [10 20 30 40 888]
  slice = [20 30 40 888 999] len = 5 cap = 8
  &arr[0] = 0xc0000cc038, &slice = 0xc0000c20c0
  ```
  
- `copy(targetSlice, sourceSlice)`：将sourceSlice拷贝给targetSlice

  - 必须都是切片
  - targetSlice和sourceSlice大小没有限制

### string和slice

- string**底层是一个byte数组**，因此string也可以进行切片处理

- **对一个string进行切片操作后返回的还是一个string**

- **string是不可变的**，也就是不能通过`str[0]  = 'z'`方式来修改字符串。如果要修改，得先转成`[]byte`或者`[]rune`，重写之后再转成string。

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	str := "hello world"
  	slice := str[3:]
  	// invalid operation: cannot take address of str[3] (value of type byte)
  	// fmt.Printf("&str[3] = %p, &slice[0] = %p\n", &str[3], &slice[0]) 报错，无法寻址对象
  	fmt.Printf("slice Type = %T\n", slice[0])
  	bytes := []byte(str)
  	bytes[0] = 'a'
  	str = string(bytes)
  	fmt.Println(str)
  }
  ```

# map

## 基本语法

- 声明：`var myMap map[keyType]valueType`    **声明不会分配内存，初始化需要make分配内存后才可以使用。数组在声明时候就分配内存了**

- key类型：可以是bool、数字、string、指针、channel，还可以是包含前面类型的接口、结构体、数组。

  通常为int或者string类型

  注意：slice、map还有function不可以，因为这几个没法用==来判断

- value类型：基本和key一样

```go
package main

import (
	"fmt"
)

func main() {
	var m map[int]string
	fmt.Println(m) // map[]
	// m[1] = "Zev" // panic: assignment to entry in nil map
	m = make(map[int]string)
	m[1] = "Zev"
	fmt.Println(m)
	var m2 = map[int]string{2: "abc", 3: "ddd"} // 这样最后那里不用加,
	var m3 = map[int]string{
		2: "abc",
		4: "ccc", // 这里必须加,
	}
	var m4 = make(map[int]map[int]string)
	m4[0] = m2
	m4[1] = m3
	fmt.Println(m4)
}
```

## 增删改查

- 增加和修改：`m[key] = value`，key存在就修改，key不存在就增加

- 删除：`delete(map, key)`：若有存在key则删除，若无此key或key为nil，则不操作

  - 如果要删除所有key，没有专门方法，要么遍历删除，要么直接让变量指向一个新的空map，让旧的map被gc回收

- 查找：直接使用`value, exist = map[key]`

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	var m map[int]string = make(map[int]string)
  	m[1] = "Zev"
  	fmt.Println(m)
  	v, ok := m[1]
  	fmt.Println(v, ok) // Zev true
  	v1, ok1 := m[99]
  	fmt.Println(v1, ok1) // "" false
  }
  ```

## map遍历

只能用for range来遍历。

```go
package main

import "fmt"

func main() {
	var m = map[int]string{1: "Zev", 2: "Tom", 5: "Kevin"}
	for i, v := range m {
		fmt.Println(i, v)
	}
}
```

## map切片

切片的数据类型如果是map，就被称之为slice of map，map切片。这样map的个数就会动态变化（一个数据类型为map的动态大小数组）。

```go
package main

import "fmt"

func main() {
	var sm []map[string]string = make([]map[string]string, 1, 3)
	// if sm[0] == nil {
	// 	sm[0] = make(map[string]string)
	// 	sm[0]["name"] = "Zev"
	// 	sm[0]["gender"] = "male"
	// }
	// 越界 不能这样做 应该用append函数动态增加
	// if sm[1] == nil {
	// 	sm[1] = make(map[string]string)
	// 	sm[1]["name"] = "Zev"
	// 	sm[1]["gender"] = "male"
	// }
	person := map[string]string{"name": "Tom", "gender": "male"}
	sm = append(sm, person)
	fmt.Println(sm) // [map[] map[gender:male name:Tom]]
	fmt.Println(sm[0])
}
```

如果创建长度为1的切片，则

1. 如果先赋值sm[0]，则后面的元素需要用append函数增加，否则会报数组越界错误
2. 如果不赋值，直接append函数添加，则第一个位置还是为空，第二个位置是添加的元素

## map排序

老版本默认无序，需要自己对key排序然后输出。

1. 先将map的key放入到切片中

2. 对切片进行排序

3. 遍历切片，然后按照key来输出map

   ```go
   package main
   
   import (
   	"fmt"
   	"sort"
   )
   
   func main() {
   	var m = map[int]string{1: "abc", 3: "ddd", 2: "aaa"}
   	var keys []int
   	for k, _ := range m {
   		keys = append(keys, k)
   	}
   	sort.Ints(keys)
   	fmt.Println(keys)
   	for _, k := range keys {
   		fmt.Printf("k = %d, v = %s\n", k, m[k])
   	}
   }
   ```

现版本有序，默认按字典排序。

## 注意事项

1. map是引用类型，遵循引用传递
2. map会自动扩容，不会发生panic
3. map的value经常使用struct类型

# Golang面向"对象"编程

1. Golang支持OOP，但是和传统的OOP有区别，不是纯粹的面向对象语言。所以我们说Golang支持面向对象编程特性是比较准确的。
2. Golang没有类，而是用struct结构体来实现面向对象。
3. 非常简洁，去掉了传统OOP语言的继承、方法重载、构造函数、析构函数、隐藏的this指针等。
4. 仍有OOP的继承，封装和多态特性，只是实现方式和其他OOP语言不一样。比如继承，没有extends关键字，**继承是通过匿名字段来实现。**
5. Golang的OOP很优雅 ,OOP本身就是语言类型系统的一部分，通过接口关联，耦合性低，非常灵活。在Golang中面向接口编程是非常重要的特性。

## 结构体

```go
package main

import "fmt"

type Person struct {
	Id   int
	Name string
}

func main() {
	var p Person
	fmt.Println(p) // {0 }
	p.Id = 1
	p.Name = "Xeon"
	fmt.Println(p)
}
```

- **一个结构体在声明时候就已经分配了空间**，只不过若未赋值，则是默认值。
- 结构体名、变量名首字母大小写依旧影响作用域。大写则可被其他包访问。
- 如果字段是引用类型（指针，slice，map），则零值为nil，需要make之后才能使用

### 几种声明方式

```go
package main

import "fmt"

type Person struct {
	Id   int
	Name string
}

func main() {
	// var p1 Person
	// var p2 = Person{}
	// var p2 = Person{3, "kat"}
	// p4 := Person{4, "batman"}
	// p5 := Person{
	// 	Id:   3,
	// 	Name: "baby",
	// }
	var pp *Person = new(Person)
	(*pp).Id = 5
	pp.Name = "spider" // Golang底层优化 实际还是和上一行一样
	fmt.Println(*pp)
	var pp2 *Person = &Person{8, "Mary"}
	fmt.Println(*pp2)
}
```

### 注意事项

- 结构体所有字段在内存中是连续的。

- 结构体是用户单独定义的类型，和其它类型进行转换时候需要有完全相同的字段（名字，个数，类型）

- 如果给结构体定义别名，Golang认为是新的数据类型，需要强转才能赋值

  ```go
  package main
  
  import "fmt"
  
  type Person struct {
  	Id   int
  	Name string
  }
  
  type Human Person
  
  func main() {
  	var p = Person{1, "Zev"}
  	var h Human = Human(p)
  	fmt.Println(h)
  }
  ```

- struct的每一个字段上，可以协商要给tag，该tag可以通过反射机制获取，常见的使用场景就是序列化和反序列化。

  - 序列化：把一个对象转成一个json字符串返回
  - 用处：Golang里用首字母大写来设置权限，但是对于前端，可能不习惯这种命名方式

  ```go
  package main
  
  import (
  	"encoding/json"
  	"fmt"
  )
  
  type Person struct {
  	Id   int    `json:"id"` // 这个就是tag
  	Name string `json:"name"`
  }
  
  func main() {
  	var p = Person{1, "Zev"}
  	b, _ := json.Marshal(p) // 底层用到了反射
  	fmt.Println(string(b)) // {"id":1,"name":"Zev"}
  }
  ```

## 方法

方式是作用在指定数据类型上 （即和指定数据类型绑定），因此自定义类型，都可以有方法，而不仅仅是struct。

```go
package main

import (
	"fmt"
)

type Person struct {
	Id   int
	Name string
}

func (p Person) test() {
	fmt.Println(p)
}

func main() {
	var me = Person{1, "Zev"}
	me.test()
}
```

- `test()`方法和Person类型绑定，只能通过Person类型的变量来调用
- `func (p Person) test()`中的`p`代表**该函数对象的一个副本**，即**结构体是值传递**

### 调用和传参原理

- 方法和函数的区别就是：方法调用时候会将调用方法的变量也当作实参传递给方法。

```go
package main

import "fmt"

type Person struct {
	Id   int
	Name string
}

func (p *Person) test() {
	p.Id = 222
	(*p).Name = "abccc"
}

func main() {
	var me = Person{1, "Zev"}
	me.test() // 编译器会优化，所以可以这么用
	fmt.Println(me)
	me.Id = 3
	fmt.Println(me)
}
```

- 对于int、float32也能有自己的方法，但是要先`type int myInt`，不然会报错`invalid receiver type int (basic or unnamed type)`

- 首字母大小写控制访问范围

- 如果一个类型实现了`String()`这个方法，那么`fmt.Println()`会默认调用这个方法进行输出。相当于Java的`toString()`

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  type Person struct {
  	Id   int
  	Name string
  }
  
  func (p *Person) String() string { // 一般会写成指针形式
  	str := fmt.Sprintf("id=%v, name=%s", p.Id, p.Name) // 编译器优化
  	return str
  }
  
  func main() {
  	me := Person{1, "Zev"}
  	fmt.Println(&me) // 实现了*Person类型的String()方法
  }
  ```

  注意`func (p *Person) String() string { // 一般会写成指针类型`，因为一般都是相当于在当前对象上调用而不是值拷贝对象上调用，**所以一般用指针类型**，减少拷贝时间

### 方法和函数区别

1. 调用方式不一样，方法要通过对象去调用

2. 对于普通函数，形参类型要求很严格，要值类型传指针会报错，反之亦然；**但是对于方法（如struct的方法），形参接收值类型，可以直接用指针类型变量调用方法，反之亦然**

   ```go
   package main
   
   import (
   	"fmt"
   )
   
   type Person struct {
   	Id   int
   	Name string
   }
   
   func (p Person) byValue() {
   	p.Name = "Mary"
   	fmt.Println("byValue")
   }
   
   func (p *Person) byPointer() {
   	p.Name = "Jerry"
   	fmt.Println("byPointer")
   }
   
   func main() {
   	me := Person{1, "Zev"}
   	fmt.Println(me)
   
   	// 虽然是通过指针调用 但是调用的test1()是通过值调用，编译器默认优化编程值传递
   	(&me).byValue()
   	fmt.Println(me) // {1 Zev}
   
   	// 虽然是通过值调用 但是调用的test2()是通过指针调用，编译器默认优化编程指针传递
   	me.byPointer()
   	fmt.Println(me) // {1 Jerry}
   
   }
   ```

### 构造方法

Golang的结构体没有构造函数，通常可以使用工厂模式来解决这个问题。

如果结构体首字母小写，只能在包内访问。如果需要在别的包里来创建访问，则需要使用工厂模式。

```go
package class

type person struct {
	id   int
	name string
}

func NewPerson(id int, name string) person {
	return person{id, name}
}

func (p person) GetIdAndName() (int, string) {
	return p.id, p.name
}
```

```go
package main

import (
	"class"
	"fmt"
)

func main() {
	p := class.NewPerson(1, "Mary")
	i, s := p.GetIdAndName()
	fmt.Println(i, s)
}
```

其实就是类似Java的getter和setter。

## 封装

1. 结构体、属性首字母小写，限制在包内使用
2. 结构体所在包提供一个工厂模式函数，首字母大写，类似一个构造函数
3. 首字母大写的Getter和Setter

上面构造方法的例子就是一个封装的例子。

## 继承

继承解决代码复用问题。

在Go中不需要重新定义这些属性，只需要嵌入一个匿名结构体即可。也就是说，在Golang中，如果一个struct嵌套了另一个匿名结构体，那么这个结构体也可以直接访问匿名结构体的字段和方法，从而实现继承特性。

```go
package main

import (
	"fmt"
)

type Person struct {
	id   int
	name string
}

func (p *Person) eat() {
	fmt.Println("eat...")
}

type graduate struct {
	Person
	Unv string
}

func (g *graduate) test() {
	fmt.Println("graduate")
}
func main() {
	g := graduate{}
	g.Person.id = 1
	g.name = "mary" // 等价于g.Person.name = "mary"
	g.test()
	g.eat() // 等价于g.Person.eat()
	fmt.Println(g.Unv)
}
```

### 注意事项

1. 结构体可以使用嵌套匿名结构体所有的字段和方法，**无论首字母大小写**。
2. 匿名结构体字段访问可以简化
3. 当结构体和匿名结构体有相同的字段或者方法时，编译器采用就近原则访问；此时如果希望访问匿名结构体的字段和方法，可以通过匿名结构体来区分
4. 当一个结构体嵌入多个匿名结构体时候(多重继承），匿名结构体里有同名方法或者字段，需要指定访问哪个匿名结构体的字段
5. 当一个结构体嵌入一个有名结构体时，这种模式就是组合。要访问该有名结构体的字段则必须带上该结构体名字。（此时相当于Java的成员变量）
6. 嵌套匿名结构体之后，也可以在创建结构体实例时，直接给结构体各个字段赋值`g := graduate{Person{1, "zev"}, "gdut"}`
7. 基本数据类型也可以作为匿名字段嵌入到一个结构体中，访问时候直接用变量名.基本数据类型名，如`p.int`

## 接口

### 基本介绍

interface类型可以定义一组方法，但是这些方法不需要实现。**并且interface不能包含任何变量**。某个结构体实现了接口里的所有方法即实现了这个接口。

### 基本语法

```
type 接口名 interface{
	method1(参数列表) 返回值列表
	method2(参数列表) 返回值列表
	...
	methodn(参数列表) 返回值列表
}
```

**Golang中的接口，不需要显式的实现。只要一个变量，含有接口类型中的所有方法。那么这个变量就实现了这个接口。**因此Golang中没有implements这种关键字。

一个例子

```go
package main

import (
	"fmt"
)

type Usb interface {
	start()
	stop()
}

type Phone struct {
}

func (p *Phone) start() {
	fmt.Println("start phone...")
}

func (p *Phone) stop() {
	fmt.Println("stop phone...")
}

type Traveller struct {
}

func (t *Traveller) start() {
	fmt.Println("start Traveller...")
}

func (t *Traveller) stop() {
	fmt.Println("stop Traveller...")
}

func accept(usb Usb) {
	usb.start()
	usb.stop()
}

func main() {
	p := Phone{}
	var t Traveller = Traveller{}
	accept(&p)
	accept(&t)
}
```

### 注意事项和细节

1. 接口本身不能创建实例，**但是可以指向一个实现了该接口的自定义类型的实例**

2. 接口中只有方法声明，没有方法体

3. 要实现一个接口，得实现它所有的方法

4. 一个自定义类型只有实现了某个接口，才能将该自定义类型的实例赋给接口类型

5. 只要是自定义数据类型，就可以实现接口，不仅仅时结构体类型

6. 一个自定义类型可以实现多个接口

7. Golang接口中不能有任何变量

8. 一个接口可以继承多个别的接口，这时如果要实现A接口，也必须将B,C接口方法也全部实现。

   ```go
   package main
   
   import (
   	"fmt"
   )
   
   type AInterface interface {
   	testA()
   }
   
   type BInterface interface {
   	AInterface
   	testB()
   }
   
   type Person struct {
   }
   
   func (p *Person) testA() {
   	fmt.Println("testA")
   }
   func (p *Person) testB() {
   	fmt.Println("testB")
   }
   
   func main() {
   	var a BInterface = &Person{}
   	a.testA()
   	a.testB()
   }
   ```

9. interface类型默认是一个指针（引用类型），如果未初始化就使用，那么会输出nil

10. 空接口`interface{}`没有任何方法，所以所有类型都实现了空接口。**换言之，任意类型变量都可以赋给空接口。**

    ```go
    package main
    
    import (
    	"fmt"
    )
    
    type Void interface {
    }
    
    type Person struct {
    }
    
    func main() {
    	var v Void = Person{}
    	fmt.Println(v)
        var v2 interface{} = Person{} // 也是可以的，inteface{}也是一种类型
        fmt.Println(v2)
        var num int = 3
        v2 = num
    }
    ```

### 一个接口的实现案例

```go
package main

import (
	"fmt"
	"math/rand"
	"sort"
)

type Person struct {
	Id   int
	Name string
}

type PersonList []Person

func (l PersonList) Len() int {
	return len(l)
}

func (l PersonList) Less(i, j int) bool {
	return l[i].Id < l[j].Id
}

func (l PersonList) Swap(i, j int) {
	l[i], l[j] = l[j], l[i]
}

func main() {
	var list PersonList
	for i := 0; i < 10; i++ {
		list = append(list, Person{rand.Intn(100), fmt.Sprintf("person%d", rand.Intn(100))})
	}
	for _, v := range list {
		fmt.Println(v)
	}
	sort.Sort(list)
	fmt.Println("sorted---------")
	for _, v := range list {
		fmt.Println(v)
	}
}
```

### 多态

1. 多态参数：实现了某个接口的自定义类型，即可作为参数传入要求为接口类型的形参
2. 多态数组

#### 类型断言

类似Java的instanceof来实现堆实际类型的判断。

```go
package main

import "fmt"

type Person struct {
	Id   int
	Name string
}

func (p *Person) work() {
	fmt.Println("people work...")
}

type creature interface {
	eat()
}

func (p *Person) eat() {
	fmt.Println("people eat...")
}

func main() {
	var c creature = &Person{1, "Mary"}
	c.eat()
	// c.work() // 报错
	// 带检测的类型断言
	if d, ok := c.(*Person); ok {
		d.work()
	} else {
		fmt.Println("convert fail...")
	}
}
```

# 文件操作

os.File封装所有文件相关操作，File是一个结构体

1. 打开一个文件：`func Open(name string) (file *File, err error)`

2. 关闭一个文件：`func (f *File) Close() error`

   ```go
   package main
   
   import (
   	"fmt"
   	"os"
   )
   
   func main() {
   	f, err := os.Open("d:/a.txt")
   	if err != nil {
   		fmt.Println(err)
   	}
   	fmt.Println(f)
   
   	err2 := f.Close()
   	if err2 != nil {
   		fmt.Println(err2)
   	}
   }
   ```

3. bufferReader读文件

   ```go
   package main
   
   import (
   	"bufio"
   	"fmt"
   	"io"
   	"os"
   )
   
   func main() {
   	f, err := os.Open("d:/a.txt")
   	defer f.Close()
   	if err != nil {
   		fmt.Println(err)
   	}
   	fmt.Println(f)
   	r := bufio.NewReader(f) // 默认缓冲区4kb
   	for {
   		str, err := r.ReadString('\n') // 读到换行就结束
   		fmt.Print(str)                 // 这里不用Println，因为默认会把文件中的\n也打出来
   		if err == io.EOF {             // io.EOF表示文件末尾
   			break
   		}
   	}
   }
   ```

4. 使用ioutil一次将整个文件读入到内存中，适用于文件不大的情况。使用ioutil包下的`func ReadFile(filename string) ([]byte, error)`函数

   ```go
   package main
   
   import (
   	"fmt"
   	"io/ioutil"
   )
   
   func main() {
   	// 不需要显式打开和关闭，因为直接隐藏到下面那个函数里了
   	b, err := ioutil.ReadFile("d:/a.txt")
   	if err != nil {
   		fmt.Println(err)
   	}
   	fmt.Printf("%s", b)
   }
   ```

5. 向文件中写入内容`func OpenFile(name string, flag int, perm FileMode) (file *File, err error)`

   FileMode只在Unix/Linux下有效，类似Linux下的权限控制rwx，Windows下无效。

   ```go
   package main
   
   import (
   	"bufio"
   	"fmt"
   	"os"
   )
   
   func main() {
   	f, err := os.OpenFile("d:/a.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
   	defer f.Close()
   	if err != nil {
   		fmt.Println(err)
   		return
   	}
   	w := bufio.NewWriter(f)
   	w.WriteString("hello")
   	w.Flush()
   }
   ```

6. 复制一个文件

   ```go
   package main
   
   import (
   	"io/ioutil"
   )
   
   func main() {
   	b, _ := ioutil.ReadFile("d:/a.txt")
   	ioutil.WriteFile("d:/b.txt", b, 0234)
   }
   ```

7. 判断一个文件是否存在

   通过方法`os.Stat()`返回错误值来判断。

   1. 如果返回的错误是nil，说明文件或者文件夹存在
   2. 如果返回的错误类型使用`os.IsNotExist()`判断为true，说明文件或者文件夹不存在
   3. 如果返回的错误为其他类型，则不确定是否存在

   ```go
   func PathExists(path string) (bool, error) {
   	_, err := os.Stat(path)
   	if err == nil {
   		return true, nil
   	}
   	if os.IsNotExist(err) {
   		return false, nil
   	}
   	return false, err
   }
   ```

8. 拷贝文件：io包下的`func Copy(dst Writer, src Reader) (written int64, err error)`

   ```go
   package main
   
   import (
   	"bufio"
   	"fmt"
   	"io"
   	"os"
   )
   
   func main() {
   	sf, err := os.Open("d:/a.jpg")
   	if err != nil {
   		fmt.Println(err)
   	}
   	defer sf.Close()
   	r := bufio.NewReader(sf)
   	df, err2 := os.OpenFile("d:/b.jpg", os.O_WRONLY|os.O_CREATE, 0123)
   	if err2 != nil {
   		fmt.Println(err2)
   	}
   	w := bufio.NewWriter(df)
   	defer df.Close()
   	written, err3 := io.Copy(w, r)
   	fmt.Println(written, err3)
   }
   ```

# 命令行传参

os.Args是一个string切片，用来存储命令行参数。

带有指定参数形式

```go
package main

import (
	"flag"
	"fmt"
)

func main() {
	// 定义变量用于命令行参数
	var user string
	var pwd string
	var port int
	flag.StringVar(&user, "u", "", "用户名，默认为空") // 第三个参数代表是默认值
	flag.StringVar(&pwd, "pwd", "", "密码，默认为空")
	flag.IntVar(&port, "port", 3306, "端口号，默认为3306")
	flag.Parse()
	fmt.Println(user, pwd, port)
}
```

# JSON

## 序列化

将有key-value结构的数据类型（如结构体，map，切片）转换成json字符串的操作。使用encoding/json包下的`func Marshal(v interface{}) ([]byte, error)`函数。

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Person struct {
	Id   int    `json:"id"` // tag标签，使得json序列化后可以用标签的值作key 用的反射机制
	Name string `json:"name"`
}

func main() {
	var p Person = Person{1, "Mary"}
	b, err := json.Marshal(p)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(string(b))

	var m map[int]interface{} = make(map[int]interface{})
	m[1] = 123
	m[2] = "abc"
	m[3] = p
	b2, err2 := json.Marshal(m)
	if err2 != nil {
		fmt.Println(err2)
	}
	fmt.Println(string(b2))
}
```

## 反序列化

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Person struct {
	Id   int    `json:"id"` // tag标签，使得json序列化后可以用标签的值作key 用的反射机制
	Name string `json:"name"`
}

func main() {
	str := "{\"id\":1,\"name\":\"Mary\"}"
	var p Person
	err := json.Unmarshal([]byte(str), &p)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(p)

	// 反序列化时候不想需要make，unmarshal会自动make
	var m map[int]interface{}
	str2 := "{\"1\":123,\"2\":\"abc\",\"3\":{\"Id\":1,\"name\":\"Mary\"}}"
	err2 := json.Unmarshal([]byte(str2), &m)
	if err2 != nil {
		fmt.Println(err2)
	}
	fmt.Printf("%T", m[3])
}
```

1. 反序列化字符串时候，要保证数据类型一致
2. 若字符串是通过程序得到，则不需要转义\\"，因为程序内部会自己加

# 单元测试

Golang中自带一个轻量级的测试框架testing和自带的go test命令实现单元测试和性能测试。和其他语言的测试框架类似，可以基于这个框架写针对函数的测试用例，也可以写压力测试用例。

## 入门

导入testing包。它提供对Go包的自动化测试的支持。通过`go test`命令，能够自动执行如下形式的任何函数：

```go
func TestXxx(*testing.T)
```

其中`Xxx`可以是任意字母数字字符串（但第一个字母不能是小写a-z），用于识别测试用例。

```go
package utils

func sum(a, b int) int {
	return a + b
}
```

```go
package utils

import "testing"

func TestSum(t *testing.T) {
	s := sum(3, 5)
	if s != 9 {
		t.Fatalf("expect %d, but get %d", 9, s)
	}
	t.Logf("abc")
}
```

在`utils`这个目录下执行`go test`命令，会自动执行**所有**以`_test`结尾的go文件下，所有`TestXxx(*testing.T)`形式的函数。

## 注意事项

1. 测试用例文件名必须以`_test.go`结尾，包含`TestXxx(*testing.T)`形式的函数；将该文件放在与被测试包相同的包中。该文件将被排除在正常的程序包之外，但在运行`go test`命令时候将会被包含。
2. 一个测试用例文件中，可以有多个测试用例函数
3. `go test`：若运行正确，则不输出日志，后面加一个参数`-v`则是不论正确错误都输出日志
4. 当出现错误时，可以使用`t.Fatalf`函数来格式化输出错误信息并**退出程序**
5. `t.Logf`方法可以格式化输出日志
6. 测试时候不用放在main函数中执行，很方便
7. 测试单个文件的时候，一定要带上被测试的原文件：`go test cal_test.go cal.go`
8. 测试单个函数：`go test -test.run 测试用例文件中的方法名`

# goroutine入门

## Go主线程和协程

一个主线程上可以起多个协程。协程可以理解为轻量级线程。

### 协程的特点

1. 有独立的栈空间
2. 共享程序堆空间
3. 调度由用户控制
4. 是轻量级的线程

## 入门

```go
package main

import (
	"fmt"
	"time"
)

func test() {
	for i := 0; i < 100; i++ {
		fmt.Printf("test() %d\n", i)
		time.Sleep(500 * time.Millisecond)
	}
}

func main() {
	go test()
	for i := 0; i < 10; i++ {
		fmt.Printf("main() %d\n", i)
		time.Sleep(500 * time.Millisecond)
	}
}
```

注意：如果主进程结束，协程就算未结束也会被强制结束

小结

1. 主线程是一个物理线程，直接作用于CPU上，是重量级的，非常耗费资源
2. 协程从主线程开启，是轻量级的线程，是逻辑态。对资源消耗相对较小；其他语言往往是内核态，消耗较大
3. 协程是Go的重要特性，可以轻松开启上万协程。其他语言并发机制一般基于线程，资源消耗较大

MPG模型：M代表操作系统的主线程（物理线程），P代表协程执行需要的上下文，G代表协程

1. `runtime.NumCPU()`：返回当前运行机子的逻辑CPU数
2. `runtime.GOMAXPROCS(n)`：设置运行的CPU数，GO1.8后不用设置，默认多核，以前要设置一下

# 管道channel

解决多个Goroutine之间通信的问题。

想知道某个程序是否会在运行时候产生资源竞争的问题，只需要在编译时候加上参数`-race`，即`go build -race xxx.go`。

## 问题代码

使用多个Goroutine计算1~20的阶乘

```go
package main

var m = make(map[int]int, 10)

func cal(n int) {
	res := 1
	for i := 1; i <= n; i++ {
		res *= i
	}
	m[n] = res
}
func main() {
	for i := 1; i <= 20; i++ {
		go cal(i)
	}
}
```

```
报错：fatal error: concurrent map writes
```

## 解决方案

1. 全局变量加锁

   ```go
   package main
   
   import (
   	"fmt"
   	"sync"
   )
   
   var (
   	m = make(map[int]int, 10)
   	// 在这里声明，表明这是一个全局的锁
   	lock sync.Mutex // 互斥锁
   )
   
   func cal(n int) {
   	res := 1
   	for i := 1; i <= n; i++ {
   		res *= i
   	}
   	lock.Lock()
   	m[n] = res
   	lock.Unlock()
   }
   func main() {
   	for i := 1; i <= 20; i++ {
   		go cal(i)
   	}
   
   	for i, v := range m {
   		fmt.Printf("map[%d] = %d\n", i, v)
   	}
   }
   ```

   缺点：

   - 依旧无法解决当主线程执行完就结束所有协程的问题
   - 加锁操作不利于多个协程对于全局变量的读写操作

2. channel

   1. 本质上就是一种数据结构：队列
   2. 先进先出
   3. 线程安全，即多协程访问时候，不需要加锁，它本身就是线程安全的
   4. channel是有类型的（类似Java的泛型？），比如一个string的channel只能存放string类型的数据

## channel

- 声明：`var 变量名 chan 数据类型`  比如`var intChan chan int`；`var abc chan string`；`var personList chan *Person`
- 必须初始化之后才能写入数据，即make之后才能使用

```go
package main

import "fmt"

func main() {
	var intChan chan int
	intChan = make(chan int, 10) // 写入数据不能超容量，否则报错
	// 向管道中写入
	intChan <- 10
	i := 11
	intChan <- i
	// 查看管道长度和容量
	fmt.Printf("len = %v, cap = %v\n", len(intChan), cap(intChan))

	// 从管道中获取
	var num int = <-intChan
	<-intChan // 这样也可以
	fmt.Println(num)

	// 未使用协程情况下，管道数据取空后再取会报错
}
```

### channel的关闭

使用内置函数close()可以关闭channel，关闭后即不能往channel内写数据，但仍可以读数据。

### channel的遍历

channel支持for range遍历，但是注意细节：

1. 在遍历时，如果channel还未关闭，则出现dead lock错误
2. 在遍历时，如果channel已经关闭，才能正常遍历

```go
package main

import "fmt"

func main() {
	var intChan chan int
	intChan = make(chan int, 10)
	for i := 0; i < 10; i++ {
		intChan <- 2 * i
	}
	close(intChan)
	for v := range intChan {
		fmt.Println(v)
	}
}
```

### 一个例子

```go
package main

import (
	"fmt"
	"time"
)

func writeData(intChan chan int) {
	for i := 0; i < 50; i++ {
		intChan <- i
		fmt.Printf("write: %d\n", i)
		time.Sleep(time.Millisecond * 500)
	}
	close(intChan)
}

func readData(intChan chan int, exitChan chan bool) {
	for {
		v, ok := <-intChan
		if !ok {
			break
		}
		fmt.Printf("read: %d\n", v)
	}
	exitChan <- true
	close(exitChan)
}

func main() {
	var intChan chan int = make(chan int, 50)
	var exitChan chan bool = make(chan bool, 1)
	go writeData(intChan)
	go readData(intChan, exitChan)
	for {
		_, ok := <-exitChan
		if !ok {
			break
		}
	}
}
```

如果只读不写会如何：会阻塞，然后等待。

### 注意事项

1. 管道可以声明为只读或者只写。默认情况下是双向的。

   - 只写：`var myChan chan<- string`
   - 只读：`var myChan <-chan string`

   在传参时候也可以将一个传入的双向channel限定为只读/只写：`func test(myChan chan<- string)`，这样可以并避免函数内误操作。

2. 使用select关键字可以解决从管道中取数据的阻塞问题。

   ```go
   package main
   
   import (
   	"fmt"
   	"strconv"
   )
   
   func main() {
   	var intChan chan int = make(chan int, 5)
   	var strChan chan string = make(chan string, 5)
   	for i := 0; i < 5; i++ {
   		intChan <- i
   		strChan <- "str" + strconv.Itoa(i)
   	}
   label:
   	for {
   		select {
   		// 注意 这里如果intChan没关闭，不会阻塞导致deadlock，而会匹配到下一个case
   		case v := <-intChan:
   			fmt.Println(v)
   		case v := <-strChan:
   			fmt.Println(v)
   		default:
   			fmt.Println("over")
   			break label
   		}
   	}
   }
   ```

3. 在Goroutine中使用recover，解决协程中出现的panic导致程序崩溃的问题。

   ```go
   package main
   
   import (
   	"fmt"
   	"time"
   )
   
   func sayHello() {
   	for i := 0; i < 10; i++ {
   		fmt.Printf("hello %d\n", i)
   		time.Sleep(time.Second)
   	}
   }
   
   func test() {
   	defer func() {
   		if err := recover(); err != nil {
   			fmt.Println("error!!")
   		}
   	}()
   	var m map[int]string
   	m[1] = "abc" // assignment to entry in nil map
   }
   
   func main() {
   	go sayHello()
   	go test()
   	time.Sleep(time.Second * 10)
   }
   ```

# 反射

## 基本介绍

1. 反射可以在运行时动态的获取变量的各种信息，比如类型(type)和类别(kind)
2. 如果是结构体变量，还可以获取结构体本身的信息（包括结构体字段，方法）
3. 通过反射，可以修改变量的值，可以调用关联的方法
4. 需要import "reflect"

重要函数

1. `reflect.TypeOf(变量名)`：获取变量的类型，返回reflect.Type类型
2. `reflect.ValueOf(变量名)`：获取变量的值，返回reflect.Value类型，是一个结构体类型

**特别注意：变量，interface{}和reflect.Value是可以相互转换的**

## 应用场景

1. 不知道接口调用哪个函数，根据传入参数在运行时确定调用的具体接口，这就需要对函数或者方法反射。例如桥接模式
2. 对结构体序列化时，如果结构体字段有指定tag，也需要反射生成对应字符串

## 快速入门

```go
package main

import (
	"fmt"
	"reflect"
)

func reflectTest(b interface{}) {
	// 获取reflect.TypeOf
	rt := reflect.TypeOf(b)
	fmt.Printf("%v, %T\n", rt, rt)
	// 获取reflect.Value
	rv := reflect.ValueOf(b)
	n := 3 + rv.Int() // 获取实际值
	fmt.Printf("%v, %T\n", n, rv)
	// 将reflect.Value转成Interface{}类型
	iv := rv.Interface()
	// 类型断言
	i := iv.(int)
	fmt.Printf("%v\n", i)
}

func main() {
	var a int = 20
	reflectTest(a)
}
```

### 注意事项和细节

1. reflect.Value.Kind：获取变量类别，返回的是一个常量
2. Type是类型，Kind是类别；他们可能相同，也可能不同。比如`var i int = 10`就是Type和Kind都是int；但是`var p Person`Type就是`报名.Person`，Kind是`struct`
3. 通过反射可以让变量在interface{}和reflect.Value之间转换
4. 使用反射来获取变量的值并返回对应类型，要求数据类型匹配。比如x是int，那就应该reflect.Value(x).Int()，而不能是其他的，否则会报panic
5. 通过反射来修改变量，注意当使用SetXxx方法来设置需要通过对应的指针类型来完成，这样才能改变传入变量的值，同时需要使用到reflect.Value.Elem()方法

### 最佳实践

使用反射来遍历结构体的字段，调用结构体的方法，并获取结构体标签的值

```go
package main

import (
	"fmt"
	"reflect"
)

type Person struct {
	Id     int    `json:"id"`
	Name   string `json:"name"`
	Salary float64
}

func (this *Person) Show() {
	fmt.Printf("Id: %d, Name: %s, Salary: %.2f\n", this.Id, this.Name, this.Salary)
}

func (this *Person) GetId() int {
	return this.Id
}

func (this *Person) Set(id int, name string, salary float64) {
	this.Id = id
	this.Name = name
	this.Salary = salary
}

func TestReflect(a interface{}) {
	rtp := reflect.TypeOf(a)
	rt := rtp.Elem()
	rvp := reflect.ValueOf(a)
	rv := rvp.Elem()

	kd := rv.Kind()
	fmt.Println(kd) // struct

	// 获取结构体字段数量
	numOfField := rv.NumField()
	fmt.Printf("struct num of field: %d\n", numOfField)
	for i := 0; i < numOfField; i++ {
		fmt.Printf("Field %d: %v\n", i, rv.Field(i)) // 打印第i个i段值
		// 注意要用reflect.Type类型的值调用Field()获取tag标签的值
		tagVal := rt.Field(i).Tag.Get("json") // 获取字段的tag值
		nameVal := rt.Field(i).Name           // 获取字段名称
		fmt.Printf("Field %d name：%s tag: %v\n", i, nameVal, tagVal)
	}
	// 结构体绑定的方法数量
	numOfMethod := rvp.NumMethod()
	fmt.Printf("struct num of method: %d\n", numOfMethod)

	rvp.Method(2).Call(nil)

	var params []reflect.Value
	params = append(params, reflect.ValueOf(5))
	params = append(params, reflect.ValueOf("Zev"))
	params = append(params, reflect.ValueOf(9000.1))
	rvp.Method(1).Call(params)
	rvp.Method(2).Call(nil)
}

func main() {
	var p Person = Person{3, "Mary", 8000.0}
	TestReflect(&p)
}
```

```
// 输出
struct
struct num of field: 3
Field 0: 3
Field 0 name：Id tag: id
Field 1: Mary
Field 1 name：Name tag: name
Field 2: 8000
Field 2 name：Salary tag:
struct num of method: 3
Id: 3, Name: Mary, Salary: 8000.00
Id: 5, Name: Zev, Salary: 9000.10
```

# TCP编程


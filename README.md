# Go语言笔记



### 一. 变量

#### 1. 变量的定义

1. 单变量声明			

   ```go
   //变量定义的三种方式	
   //1. 默认型
   var i int
   //2. 类型推导型
   var m = 10	//根据值来确定类型
   //3. := 型 省略var
   n := "abc"  //相当于var n string = "abc"
   
   ```

2. 多变量声明

   ```go
   //声明多个同类型变量
   var a1, a2, a3 int
   //声明多个不同类型变量
   var i1, i2, i3 = 100, "cba",50.23
   //省略var 使用:= 
   i1, i2, i3 := 100, "cba",50.23
   //var()型
   var(
   	v1 = 10
       v2 = "nn"
   )
   ```

### 二. 数据类型

#### 1. 基本数据类型

##### 1. 整数型

| 类型   | 有无符号 | 占用存储空间 | 表数范围       |
| ------ | -------- | ------------ | -------------- |
| int8   | 有       | 1字节        | -128~127       |
| int16  | 有       | 2字节        | -2^15~2^15-1   |
| int32  | 有       | 4字节        | -2^31~2^31-1   |
| int64  | 有       | 8字节        | -2^63~2^63-1   |
| uint8  | 无       | 1字节        | 0~255          |
| uint16 | 无       | 2字节        | 0~2^16-1       |
| uint32 | 无       | 4字节        | 0~2^32-1       |
| uint64 | 无       | 8字节        | 0~2^64-1       |
| int    | 有       | 4字节或8字节 | 参考上面       |
| uint   | 无       | 4字节或8字节 | 参考上面       |
| rune   | 有       | 与int32一样  | 表示Unicode码  |
| byte   | 无       | 与unit8一样  | 存储字符用byte |

```go
//查看某变量的占用字节大小和数据类型
package main
import (
	"fmt"
	"unsafe"
)
func main() {
	a := 654
    //输出a=int 占用字节8
	fmt.Printf("a = %T 占用字节%d", a, unsafe.Sizeof(a))
}
```

##### 2. 浮点型

| 类型          | 占用存储空间 | 表数范围             |
| ------------- | ------------ | -------------------- |
| 单精度float32 | 4字节        | -3.403E38~3.403E38   |
| 双精度float64 | 8字节        | -1.798E308~1.798E308 |

##### 3. 字符型

```go
//使用byte存储字母字符
var c1 byte = 'a'
//格式化输出
fmt.Printf("c1=%c",c1)
//输出字符a   同时也相当于整数使用UTF-8
```

##### 4. 布尔型

​	只有true 和false, 默认值为false

##### 5. 字符串型

1. 字符串的定义

   ```go
   //定义
   var s1 string = "abcde"//双引号会识别转义字符
   fmt.Println(s1)
   var s2 string = `12345`//反引号以字符原生形式输出，包括换行和特殊字符
   fmt.Println(s2)
   ```

2. 字符串一旦赋值就不能再次修改

3. 字符串拼接

   ```go
   //字符串用加号拼接
   var s1 string = "hello" + "world"
   s1 += "abc"
   //字符串换行时应保持 + 在末尾而不是句首
   s2 := "helloworld" + 
   	"abcd"
   ```

#### 2. 基本数据类型转换

1. 不同类型之间赋值时需要显式转化，Golang中数据类型不能自动转换

   ```go
   //语法规则: 数据类型(数据)
   var i1 int32 = 100			//原类型
   var i2 float32 = float(i1)	//现类型
   ```

2. 转换传递的是值，转换后的数据类型与原数据类型不冲突，原数据类型不改变

3. 基本数据类型转string

   ```go
   //使用fmt下面的Sprintf方法将返回一个字符串
   func main() {
   	var a int = 56
   	var b float64 = 12.23
   	var c bool = true
   	var s string
   	s = fmt.Sprintf("%d", a)
   	fmt.Println(s)
   	s = fmt.Sprintf("%f", b)
   	fmt.Println(s)
   	s = fmt.Sprintf("%t", c)
   	fmt.Println(s)
   }
   ```

4. string类型转基本类型

   ```go
   func main() {
       //1. string类型转bool类型
   	var s1 string = "true"
   	var b1 bool
   	b1, _ = strconv.ParseBool(s1)//函数有两个返回值，第二个返回值用_忽略
   	fmt.Printf("b1的类型是%T b1=%v", b1, b1)
       
       //2. string类型转int类型
       var s2 string = "123"
   	var i1 int64
   	i1, _ = strconv.ParseInt(s2, 10, 64)//字符串，10进制，64格式
   	fmt.Printf("b1的类型是%T b1=%v", i1, i1)
       
       //3. string类型转float64类型
       var s3 string = "56.25"
   	var f1 float64
   	f1, _ = strconv.ParseFloat(s3, 64)
   	fmt.Printf("b1的类型是%T b1=%v", f1, f1)
   }
   ```

#### 3. 指针

1. 指针类型，变量存的是地址，地址所指向的空间是值

2. 获取变量的地址用&

3. 获取指针类型所指向的值用*

   ```go
   func main() {
   	var a int = 54		//定义一个int型变量a
   	var ptr *int = &a	//定义一个指针变量指向a
   	fmt.Printf("%v\n", ptr)	//输出ptr的值即a的地址
   	fmt.Printf("%v", *ptr)	//输出ptr所指向地址的值即a的值
   }
   ```

#### 4. 标识符命名规则

1. 包名：保持package的名字与目录保持一致，且不和标准库冲突

2. 变量名，函数名，常量名采用驼峰法 xxxYyyZzz

3. 变量名，函数名，常量名的首字母大写则可以被其他包访问 (公有的) ，首字母小写则只能在本包访问 (私有的) 

   ```go
   package main
   import(
       "fmt"
       //导入其他包
       "Learn/Test/demo01"
   )
   func main() {
   	//打印其他包的变量值
   	fmt.Printf("%v", StudentName)//首字母大写是公有可访问的，小写则不可访问
   	
   }
   ```

### 三. 运算符

#### 1. 算数运算符	

| + 加 | - 减 | * 乘 | / 除 | % 取余 | ++ 自增 | -- 自减 |
| ---- | ---- | ---- | ---- | ------ | ------- | ------- |

1. 除法运算如果两个操作数都是整数则结果也是整数，直接去掉小数部分
2. 没有++i运算  且i++运算是独立的，不能混合使用

#### 2. 关系运算符

| > 大于 | < 小于 | >= 大于等于 | <= 小于等于 | != 不等于 | == 等于 |
| ------ | ------ | ----------- | ----------- | --------- | ------- |

#### 3. 逻辑运算符

| && 逻辑与          | \|\| 逻辑或        | ! 逻辑非 |
| ------------------ | ------------------ | -------- |
| 全真为真，否则为假 | 全假为假，否则为真 | 真假转换 |

#### 4. 位运算符

| & 按位与     | \| 按位或    | ^ 按位异或      | << 左移             | >> 右移                                  |
| ------------ | ------------ | --------------- | ------------------- | ---------------------------------------- |
| 两位全1，则1 | 两位有1，则1 | 两位一1一0，则1 | 符号位不变，低位补0 | 低位溢出，符号位不变，符号位补溢出的高位 |

### 四. 流程控制

#### 1. 分支控制if-else

1. 条件为true时执行大括号里面的语句

   ```go
   //if后面不需要写(),但必须要写{}
   if true {
   	//语句
   }else if true{
       //语句
   }else{
   	//语句
   }
   ```

#### 2. switch分支控制

1. switch语句基本语法

   ```go
   func main() {
   	var key int = 1
       //switch后面加表达式
   	switch key {
           
   	case 1:		//结果为key的值
   		fmt.Println("结果为",key)
   	case 2 , 3:	//可以是多个
   		fmt.Println("结果为",key)
   	default:	//当没有匹配时执行default
   		fmt.Println("没有结果")
   	}
   }
   ```

2. case后面的表达式的数据类型必须和switch表达式的数据类型一样

3. case后面可以加多个表达式，用逗号分隔

4. case后面不需要加break

5. 全部不满足则执行default

6. default不是必须的，可以省略

7. switch后面可以不加表达式，类似if else来使用

   ```go
   switch {
       //即当key为1时执行
   	case key == 1:
   		fmt.Println("结果为", key)
       //即当key为2时执行
   	case key == 2:
   		fmt.Println("结果为", key)
       //即其他结果时执行
   	default:
   		fmt.Println("没有结果")
   }
   ```

8. switch穿透fallthrough，则会继续输出下面一个case

   ```go
   switch key { 
   	case 1:	
   		fmt.Println("结果为",key)
       	fallthrough	
       //会继续执行case2
   	case 2:	
   		fmt.Println("结果为",key)
       //不会执行case3
       case 3:	
   		fmt.Println("结果为",key)
   	default:
   		fmt.Println("没有结果")
   	}
   ```

### 五. 循环控制

#### 1. for循环

1. for循环基本语法

   ```go
   //for后面不需要加()
   for i := 0; i < 10; i++ {
      //语句
   }
   ```

2. for循环的第二种用法

   ```go
   //初始条件写在外面
   i:=0
   for i<10 {
       //语句
       //迭代变量写在里面
       i++
   }
   ```

3. for的无限循环

   ```go
   //无限循环 配合break
   for {
   	//语句
   	break
   }
   //相当于
   for;; {
       //语句
       break
   }
   ```

4. for-range遍历字符串和数组

   ```go
   //定义一个数组
   str := "abc"
   //遍历数组
   for index, val := range str {
       fmt.Printf("index=%d,val=%c\n", index, val)
   }
   ```

#### 2. break语句

1. break用于中断某个语句块的执行，用于中断当前for循环或跳出switch语句  

2. break默认跳出最近的循环体

3. break可以使用标签来选择跳出到标签所在的位置

   ```go
   //标签可以自己设置
   label1:
   	for i := 0; i < 4; i++ {
   	label2:
   		for j := 0; j < 4; j++ {
   			if true {
   				break label1//跳出到label1
   			} else {
   				break label2//跳出到label2
   			}
   		}
   	}
   ```

#### 3. continue语句

1. continue语句用于结束本次循环，继续执行下一次循环
2. continue语句出现在多层嵌套循环时，可以通过标签指定跳过哪一层循环

#### 4. goto语句

1. goto语句可以无条件跳转到某个标签位置(不推荐使用)

#### 5. return语句

1. 结束或跳出所在的方法或者函数

### 六. 函数

#### 1. 函数的定义

1. 基本语法

   ```go
   func main() {
   	var a int
   	var b float64
       //调用pri函数
   	a, b = pri(5, 35.2)
   	fmt.Println(a)
   	fmt.Println(b)
   }
   //func 函数名 (形参列表) (返回值列表) { }
   func pri(n1 int, n2 float64) (int, float64) {
   	//语句
   	return n1, n2
   }
   ```

2. 返回值只有一个时，可以省略返回值列表的括号

3. 返回多个值时，可以用 _ 符号 来忽略某个返回值，表示站位忽略

#### 2. 函数的特点

1. go语言不支持函数重载

2. 函数也是一种数据类型，可以赋给一个变量，则该变量就是一个函数类型的变量

   ```go
   func sum(n1 int, n2 int) int {
   	return n1 + n2
   }
   func main() {
   	a := sum
       //输出：a的类型func(int, int) int，sum的类型func(int, int) int
   	fmt.Printf("a的类型%T，sum的类型%T\n", a, sum)
       
       res := a(10, 20)
       //输出30
   	fmt.Printf("%v", res)
   }
   ```

3. 函数可以作为形参，并且调用

4. go语言可以自定义函数别名使用type

   ```go
   type myInt int
   var num1 myInt//相当于 var num1 int
   fmt.Printf("%v", num1)
   
   ```

5. type定义的数据类型与原数据类型不是同一个数据类型，要使用可以进行数据类型转换

6. 可以对函数返回值命名

   ```go
   //对返回值命名后，不用再return后面加值
   func cul(n1 int, n2 int) (sum int, sub int) {
   	sum = n1 + n2
   	sub = n1 - n2
   	return
   }
   ```

7. go中支持可变参数

   ```go
   func main() {
       //可以传任意个同类型的参数
      a := cul(10, 5, 45)
      fmt.Println(a)
   }
   //args...int  代表多个int类型的参数，可以在前面加固定参数，最后加args...
   func cul(args ...int) (sum int) {
      for i := 0; i < len(args); i++ {
         sum += args[i]
      }
      return
   }
   ```

#### 3. init函数

1. 每一个源文件都包含一个init函数，该函数会在main函数执行前，被Go运行框架调用，即init函数在main函数前被调用
2. 优先顺序：全局变量定义 > init函数调用 > main函数调用
3. 如果含有包的引用，则优先执行被调用的包的全局变量定义和init函数，再执行调用包的优先顺序

#### 4. 匿名函数

1. 定义匿名函数的时候直接使用，这种方式只能调用一次

   ```go
   func main() {
      //匿名函数不写函数名，定义后直接传参 
      res := func(n1 int, n2 int) int {
         return n1 + n2
      }(5, 6)
      fmt.Println(res)
   }
   ```

2. 将匿名函数赋给一个变量，则这个变量就是匿名函数

   ```go
   func main() {
       //将res定义为匿名函数，则res的类型就是函数类型
      res := func(n1 int, n2 int) int {
         return n1 + n2
      }
      fmt.Println(res(5, 6))
   }
   ```

3. 如果将匿名函数赋给一个全局变量，则这个变量就是全局匿名函数

### 七. 包

#### 1. 包的基本概念

1. go语言每一个文件都属于一个包，go是以包的形式来管理文件和项目目录结构的

2. 打包使用 package 包名

3. 导包使用 import "路径"，从src下开始引入

4. package指定放在文件的第一行，然后是import指令

5. 变量或函数首字母大写代表公有的，即可以被导包后使用，小写则为私有的

6. 可以给包取别名，取别名之后不能再用原有名

   ```go
   package main
   import(
       "fmt"
   	util "路径" //util就是别名
   )
   ```

7. 可执行文件需要将这个包声明为main，即package main




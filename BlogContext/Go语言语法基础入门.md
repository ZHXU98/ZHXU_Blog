# Go语言语法基础入门

仅用来 学习go语法的简单笔记

源自菜鸟教程

## 1、Go 语言结构

### Go 语言的基础组成有以下几个部分：

- 包声明
- 引入包
- 函数
- 变量
- 语句 & 表达式
- 注释

```go
package main //包声明

import "fmt" //引入包

func main() { // 函数
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!") //语句 & 表达式
}
```

**main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 init() 函数则会先执行该函数）。**

Go 程序是通过 **package** 来组织的。
只有 package 名称为 main 的源码文件可以包含 main 函数。
一个可执行程序有且仅有一个 **main** 包。
通过 **import** 关键字来导入其他非 **main** 包。
可以通过 **import** 关键字单个导入:

```go
import "fmt"
import "io"
```

也可以同时导入多个:

```go
import (
    "fmt"
    "math"
)
```

例如：

```go
package main
import (
    "fmt"
    "math"
)
func main() {
    fmt.Println(math.Exp2(10))  // 1024
}
```

使用 <PackageName>.<FunctionName> 调用:

```go
package 别名：
// 为fmt起别名为fmt2
import fmt2 "fmt"
```

省略调用(不建议使用):

```
// 调用的时候只需要Println()，而不需要fmt.Println()
import . "fmt"
```

前面加个点表示省略调用，那么调用该模块里面的函数，可以不用写模块名称了:

```go
import . "fmt"
func main (){
    Println("hello,world")
}
```

通过 **const** 关键字来进行常量的定义。
通过在函数体外部使用 **var** 关键字来进行全局变量的声明和赋值。
通过 **type** 关键字来进行结构(struct)和接口(interface)的声明。
通过 **func** 关键字来进行函数的声明。

**可见性规则**
Go语言中，使用**大小写来**决定该常量、变量、类型、接口、结构或函数**是否可以被外部包所调用。**
函数名首字母小写即为 **private** :

```go
func getId() {}
```

函数名首字母大写即为 **public** :

```go
func Printf() {}
```

### 执行 Go 程序

- **`go run hello.go`**  执行代码
- **`go build`** 命令来生成二进制文件

**ps**: 需要注意的是 **{** 不能单独放在一行，{} 分块代码 代码可以

## 2、Go 语言基础语法

- Go 标记

  Go 程序可以由多个标记组成，可以是关键字，标识符，常量，字符串，符号。

  `fmt.Println("Hello, World!")`由6个标记组成：

  1. fmt  | 2. .  | 3. Println  | 4. (   | 5. "Hello, World!"  | 6. )

- 行分隔符

如果你打算将**多个语句写在同一行，它们则必须使用 ;** 人为区分，但在实际开发中我们并不鼓励这种做法。一行一个不需要

-  注释

和C一样

- 标识符

  一个标识符实际上就是一个或是多个字母(A~Z和a~z)数字(0~9)、下划线_组成的序列，但是第一个字符必须是字母或下划线而不能是数字。

- 关键字

下面列举了 Go 代码中会使用到的 25 个关键字或保留字：

| break    | default     | func   | interface | select |
| -------- | ----------- | ------ | --------- | ------ |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

除了以上介绍的这些关键字，Go 语言还有 36 个预定义标识符：

| append | bool    | byte    | cap     | close  | complex | complex64 | complex128 | uint16  |
| ------ | ------- | ------- | ------- | ------ | ------- | --------- | ---------- | ------- |
| copy   | false   | float32 | float64 | imag   | int     | int8      | int16      | uint32  |
| int32  | int64   | iota    | len     | make   | new     | nil       | panic      | uint64  |
| print  | println | real    | recover | string | true    | uint      | uint8      | uintptr |

程序一般由关键字、常量、变量、运算符、类型和函数组成。

程序中可能会使用到这些分隔符：括号 ()，中括号 [] 和大括号 {}。

程序中可能会使用到这些标点符号：.、,、;、: 和 …。

- 字符串连接

  Go 语言的字符串可以通过 **+** 实现：

## 3、Go 语言数据类型

数据类型的出现是为了把数据分成所需内存大小不同的数据，编程的时候需要用大数据的时候才需要申请大内存，就可以充分利用内存。

- **布尔型**
  布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。

- **数字类型**
  整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。

  正数的原码、反码、补码均相同。
  负数的反码：把原码的符号位保持不变，数值位逐位取反，即可得原码的反码。
  负数的补码：在反码的基础上加 1 即得该原码的补码。

- **字符串类型:**
  字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。

- **派生类型:**
  包括：

  - 指针类型（Pointer）
  - 数组类型
  - 结构化类型(struct)
  - Channel 类型
  - 函数类型
  - 切片类型
  - 接口类型（interface）
  - Map 类型

## 4、Go 语言变量

### 声明变量

的一般形式是使用 var 关键字：

```go
var identifier type
```

~~敢问type放后面不蹩脚吗~~

可以一次声明多个变量：

```go
var identifier1, identifier2 type
```

- 变量声明

  **第一种，指定变量类型，如果没有初始化，则变量默认为零值**。

  **第二种，根据值自行判定变量类型。**

  **第三种，省略 var, 注意 \**:=\** 左侧如果没有声明新的变量，就产生编译错误，格式：**

- 数值类型（包括complex64/128）为 **0**

- 布尔类型为 **false**

- 字符串为 **""**（空字符串）

- 以下几种类型为 **nil**：

  ```go
  var a *int
  var a []int
  var a map[string] int
  var a chan int
  var a func(string) int
  var a error // error 是接口
  ```

  ```GO
  var intVal int 
  
  intVal :=1 // 这时候会产生编译错误
  
  intVal,intVal1 := 1,2 // 此时不会产生编译错误，因为有声明新的变量，因为 := 是一个声明语句
  ```

  ps: vscode 声明了不用也会报错醉了

### 值类型和引用类型

当使用等号 `=` 将一个变量的值赋值给另一个变量时，如：`j = i`，实际上是在内存中将 i 的值进行了拷贝：**深拷贝**

ref类型: 当使用赋值语句 r2 = r1 时，只有引用（地址）被复制。
如果 r1 的值被改变了，那么这个值的所有引用都会指向被修改后的内容，在这个例子中，r2 也会受到影响。

### 简短形式，使用 := 赋值操作符

如果在相同的代码块中，我们不可以再次对于相同名称的变量使用初始化声明，例如：a := 20 就是不被允许的，编译器会提示错误 no new variables on left side of :=，但是 a = 20 是可以的，因为这是给相同的变量赋予一个新的值。

如果你在定义变量 a 之前使用它，则会得到编译错误 undefined: a。

如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误，例如下面这个例子当中的变量 a：

如果你想要交换两个变量的值，则可以简单地使用 **a, b = b, a**，两个变量的类型必须是相同。

空白标识符 _ 也被用于抛弃值，如值 5 在：_, b = 5, 7 中被抛弃。

_ 实际上是一个只写变量，你不能得到它的值。这样做是因为 Go 语言中你必须使用所有被声明的变量，但有时你并不需要使用从一个函数得到的所有返回值。

## 5、Go 语言常量

常量的定义格式：你可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型。

```go
const identifier [type] = value
```

### iota

- iota 只是在同一个 const 常量组内递增，每当有新的 const 关键字时，iota 计数会重新开始。

- ```c
  const (
              a = iota   //0
              b          //1
              c          //2
              d = "ha"   //独立值，iota += 1
              e          //"ha"   iota += 1
              f = 100    //iota +=1
              g          //100  iota +=1
              h = iota   //7,恢复计数
              i          //8
      )
  const (
      i=1<<iota // 1
      j=3<<iota // 3 << 2
      k		  // 3 << 3
      l		  // 3 << 4
  )
  ```

## 6、Go 语言运算符

大部分和C语言一样

```go
a++ // 这是允许的，类似 a = a + 1,结果与 a++ 相同
a-- //与 a++ 相似
a = a++ // 这是不允许的，会出现变异错误 syntax error: unexpected ++ at end of statement
```

## 7、Go循环 条件语句

### 条件语句

*注意：Go 没有三目运算符，所以不支持* **?:** *形式的条件判断。*

```go
var a int = 10
   /* 使用 if 语句判断布尔表达式 */
if a < 20 {
    /* 如果条件为 true 则执行以下语句 */
    fmt.Printf("a 小于 20\n" )
}
if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
} else {
  /* 在布尔表达式为 false 时执行 */
}
```

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var grade string = "B"
   var marks int = 90

   switch marks {
      case 90: grade = "A"
      case 80: grade = "B"
      case 50,60,70 : grade = "C"
      default: grade = "D"  
   }

   switch {
      case grade == "A" :
         fmt.Printf("优秀!\n" )    
      case grade == "B", grade == "C" :
         fmt.Printf("良好\n" )      
      case grade == "D" :
         fmt.Printf("及格\n" )      
      case grade == "F":
         fmt.Printf("不及格\n" )
      default:
         fmt.Printf("差\n" );
   }
   fmt.Printf("你的等级是 %s\n", grade );      
}

select {
    case communication clause  :
       statement(s);      
    case communication clause  :
       statement(s);
    /* 你可以定义任意数量的 case */
    default : /* 可选 */
       statement(s);
}
```

select 是 Go 中的一个控制结构，类似于用于**通信的 switch 语句**。每个 case 必须是一个通信操作，要么是发送要么是接收。

select 随机执行一个可运行的 case。如果没有 case 可运行，它将阻塞，直到有 case 可运行。一个默认的子句应该总是可运行的。

如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行。
否则：

1. 如果有 default 子句，则执行该语句。
2. 如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值。

### 循环语句

```go
for init; condition; post { }
for condition { }
for { }
// for 循环的 range 格式可以对 slice、map、数组、字符串等进行迭代循环。格式如下：
for key, value := range oldMap {
    newMap[key] = value
}
// For-each range 循环
strings := []string{"google", "runoob"}
for i, s := range strings 
	if i == 0 {
        continue
    }
    fmt.Println(i, s)
}
```

**使用标记**

```go
package main

import "fmt"

func main() {

    // 不使用标记
    fmt.Println("---- continue ---- ")
    for i := 1; i <= 3; i++ {
        fmt.Printf("i: %d\n", i)
            for i2 := 11; i2 <= 13; i2++ {
                fmt.Printf("i2: %d\n", i2)
                continue
            }
    }

    // 使用标记
    fmt.Println("---- continue label ----")
    re:
        for i := 1; i <= 3; i++ {
            fmt.Printf("i: %d\n", i)
                for i2 := 11; i2 <= 13; i2++ {
                    fmt.Printf("i2: %d\n", i2)
                    continue re
                }
        }

    ---- continue ---- 
i: 1
i2: 11
i2: 12
i2: 13
i: 2
i2: 11
i2: 12
i2: 13
i: 3
i2: 11
i2: 12
i2: 13
---- continue label ----
i: 1
i2: 11
i: 2
i2: 11
i: 3
i2: 11
```

**goto**:

```go
goto label;
..
.
label: statement;
```

## 8、Go语言函数

函数是基本的代码块，用于执行一个任务。

Go 语言最少有个 main() 函数。

### 函数定义

Go 语言函数定义格式如下：

```go
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

- func：函数由 func 开始声明
- function_name：函数名称，函数名和参数列表一起构成了函数签名。
- parameter list：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
- return_types：返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
- 函数体：函数定义的代码集合。

实例

```go
func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

### 函数参数

- 值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
- 引用传递是指在调用函数时将实际参数的地址传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。

### 函数用法

| 函数用法                   | 描述                                     |
| :------------------------- | :--------------------------------------- |
| 函数作为另外一个函数的实参 | 函数定义后可作为另外一个函数的实参数传入 |
| 闭包                       | 闭包是匿名函数，可在动态编程中使用       |
| 方法                       | 方法就是一个包含了接受者的函数           |

闭包： 奇怪的语法？

```go
package main

import "fmt"

func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}
```

方法：

Go 语言中同时有函数和方法。一个方法就是一个包含了接受者的函数，接受者可以是命名类型或者结构体类型的一个值或者是一个指针。所有给定类型的方法属于该类型的方法集。语法格式如下：

```go
func (variable_name variable_data_type) function_name() [return_type]{
   /* 函数体*/
}

package main

import (
   "fmt"  
)

/* 定义结构体 */
type Circle struct {
  radius float64
}

func main() {
  var c1 Circle
  c1.radius = 10.00
  fmt.Println("圆的面积 = ", c1.getArea())
}

//该 method 属于 Circle 类型对象中的方法
func (c Circle) getArea() float64 {
  //c.radius 即为 Circle 类型对象中的属性
  return 3.14 * c.radius * c.radius
}
```

## 9、Go 语言变量作用域

- 函数内定义的变量称为局部变量
- 函数外定义的变量称为全局变量
- 函数定义中的变量称为形式参数

1. 局部变量

   在函数体内声明的变量称之为局部变量，它们的作用域只在函数体内，参数和返回值变量也是局部变量。

2. 全局变量

   在函数体外声明的变量称之为全局变量，全局变量可以在整个包甚至外部包（被导出后）使用。

   全局变量可以在任何函数中使用
   Go 语言程序中全局变量与局部变量名称可以相同，但是函数内的局部变量会被优先考虑。

3. 形式参数

   形式参数会作为函数的局部变量来使用

## 10、Go 语言数组

数组 与 切片

区分 **数组** 与 **切片**

\- Go 语言的数组是值，其长度是其类型的一部分，作为函数参数时，是 **值传递**，函数中的修改对调用者不可见

\- Go 语言中对数组的处理，一般采用 **切片** 的方式，切片包含对底层数组内容的引用，作为函数参数时，类似于 **指针传递**，函数中的修改对调用者可见

## 11、GO语言指针

- 定义指针变量。
- 为指针变量赋值。
- 访问指针变量中指向地址的值。

当一个指针被定义后没有分配到任何变量时，它的值为 nil。

nil 指针也称为空指针。

nil在概念上和其它语言的null、None、nil、NULL一样，都指代零值或空值。

一个指针变量通常缩写为 ptr。

接下来我们将为大家介绍Go语言中更多的指针应用：

| 内容                   | 描述                                         |
| :--------------------- | :------------------------------------------- |
| Go 指针数组            | 你可以定义一个指针数组来存储地址             |
| Go 指向指针的指针      | Go 支持指向指针的指针                        |
| Go 向函数传递指针参数] | 通过引用或地址传参，在函数调用时可以改变其值 |

## 12、Go 语言结构体

```go
package main

import "fmt"

type Books struct {
   title string
   author string
   subject string
   book_id int
}


func main() {

    // 创建一个新的结构体
    fmt.Println(Books{"Go 语言", "www.runoob.com", "Go 语言教程", 6495407})

    // 也可以使用 key => value 格式
    fmt.Println(Books{title: "Go 语言", author: "www.runoob.com", subject: "Go 语言教程", book_id: 6495407})

    // 忽略的字段为 0 或 空
   fmt.Println(Books{title: "Go 语言", author: "www.runoob.com"})
}
```

## 13、Go 语言切片(Slice)

Go 语言切片是对数组的抽象。

Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

- 定义切片

  ```go
  var identifier []type
  切片不需要说明长度。
  或使用make()函数来创建切片:
  var slice1 []type = make([]type, len)
  也可以简写为
  slice1 := make([]type, len)
  也可以指定容量，其中capacity为可选参数。
  make([]T, length, capacity)
  ```

- 切片初始化

  ```
  s :=[] int {1,2,3 } 
  直接初始化切片，[]表示是切片类型，{1,2,3}初始化值依次是1,2,3.其cap=len=3
  s := arr[:] 
  初始化切片s,是数组arr的引用
  s := arr[startIndex:endIndex] 
  将arr中从下标startIndex到endIndex-1 下的元素创建为一个新的切片
  s := arr[startIndex:] 
  默认 endIndex 时将表示一直到arr的最后一个元素
  s := arr[:endIndex] 
  默认 startIndex 时将表示从arr的第一个元素开始
  s1 := s[startIndex:endIndex] 
  通过切片s初始化切片s1
  s :=make([]int,len,cap) 
  通过内置函数make()初始化切片s,[]int 标识为其元素类型为int的切片
  ```

切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。

一个切片在未初始化之前默认为 nil，长度为 0

- append() 和 copy() 函数

  如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。

## 14、Go 语言范围(Range)

Go 语言中 range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。**在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 key-value 对**。

```go
package main
import "fmt"
func main() {
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    //range也可以用在map的键值对上。
    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }
    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```

## 15、Go 语言Map(集合)

Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是**使用 hash 表来**实现的。

- 定义 Map

可以使用内建函数 make 也可以使用 map 关键字来定义 Map:

```
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type
/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

**如果不初始化 map，那么就会创建一个 nil map。nil map 不能用来存放键值对**

- delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key

##

```go
package main

import "fmt"

func fibonacci(n int) int {
  if n < 2 {
   return n
  }
  return fibonacci(n-2) + fibonacci(n-1)
}

func main() {
    var i int
    for i = 0; i < 10; i++ {
       fmt.Printf("%d\t", fibonacci(i))
    }
}
```

## 16、Go 并发

Go 允许使用 go 语句开启一个新的运行期线程， 即 goroutine，以一个不同的、新创建的 goroutine 来执行一个函数。 同一个程序中的所有 goroutine 共享同一个地址空间。

```go
package main

import (
        "fmt"
        "time"
)

func say(s string) {
        for i := 0; i < 5; i++ {
                time.Sleep(100 * time.Millisecond)
                fmt.Println(s)
        }
}

func main() {
        go say("world")
        say("hello")
}
```

### 通道（channel）

通道（channel）是用来传递数据的一个数据结构。

通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 `<-` 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。

```
ch <- v    // 把 v 发送到通道 ch
v := <-ch  // 从 ch 接收数据
           // 并把值赋给 v
```

声明一个通道很简单，我们使用chan关键字即可，通道在使用前必须先创建：

```
ch := make(chan int)
```

**注意**：默认情况下，通道是不带缓冲区的。发送端发送数据，同时必须有接收端相应的接收数据。

- ### 通道缓冲区

通道可以设置缓冲区，通过 make 的第二个参数指定缓冲区大小：

```
ch := make(chan int, 100)
```

带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据。

不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

**注意**：如果通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；如果缓冲区已满，则意味着需要等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直阻塞。

```c
package main

import (
	"fmt"
)

func fibonacci(n int, c chan int) {
	x, y := 0, 1
	var t int
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
		t := <-c
		fmt.Println("|||", t)
	}
	fmt.Println(t)
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(10, c)
	// range 函数遍历每个从通道接收到的数据，因为 c 在发送完 10 个
	// 数据之后就关闭了通道，所以这里我们 range 函数在接收到 10 个数据
	// 之后就结束了。如果上面的 c 通道不关闭，那么 range 函数就不
	// 会结束，从而在接收第 11 个数据的时候就阻塞了。
	fmt.Println(cap(c))
	for i := range c {
		fmt.Println(i)
	}
}
```


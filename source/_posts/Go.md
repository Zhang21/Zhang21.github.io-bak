---
title: Go
date: 2020-07-22 19:02:32
mathjax: true
categories: Go
tags:
  - Go
---


参考:

- github: <https://github.com/golang/go>
- Docs: <https://golang.org/doc/>
- awesome-go: <https://github.com/avelino/awesome-go>

<br>

版本:

- go v1.14






<br>
<br>

---

<!--more-->

<br>
<br>














# 介绍


Go编程语言是一个开源项目，使开发人员更高效。

Go是传神，简洁，干净，高效的。它的并发机制(concurrency mechanisms)可充分利用多核和网络机器编写程序，它的新颖类型系统允许灵活和模块化结构。它是一个快速、静态类型、编译型语言，像一个动态类型、解释型语言。










<br>
<br>

---

<br>
<br>















# 安装

下载对应平台的二进制包，解压，添加路径。

<br>

测试安装:

```go
package main

import "fmt"

func main() {
	fmt.Printf("hello, world\n")
}
```

```
# 编译
go build hello.go
# 执行
./hello
```

<br>

安装其它版本:

```
go get golang.org/dl/go1.10.7

go1.10.7 version
```







<br>
<br>

---

<br>
<br>









# 学习


<br>


## 旅程

Tour: <https://tour.go-zh.org/list>

<br>

交互式地分三部分介绍Go：

- 基本语法和数据结构
- 方法和接口
- 并发原语(concurrency primitives)


可在线上或本地开启旅程：

- 线上: <https://tour.golang.org/welcome/1>
- 本地: `go get golang.org/x/tour`，会在go path的`bin/tour`。

<br>

`sandbox.go`测试程序显示时间：

```go
package main

import (
    "fmt"
    "time"
)

func main() {
  fmt.Println("Welcomt to the playground!")
  fmt.Println("The time is", time.Now())
}
```


<br>
<br>


### 基础语法

学习go程序的基本结构。


<br>

#### 包

每个go程序都是由包构成。程序从`main`包开始运行。

按照约定，包名与导入路径的最后一个元素一致。例如，`math/rand`包中的源码均以`package rand`语句开始。

```go
// package.go
package main

// 导入两个包
import (
    "fmt"
    "math/rand"
)

func main() {
    fmt.Println("My favorite number is", rand.Intn(10))
}
```


<br>
<br>


#### 导入

使用圆括号进行分组导入，也可以编写多个导入语句。分组导入语句是更好的形式。

```go
// 分组导入
import (
    "fmt"
    "math"
)

// 单独导入

import "fmt"
import "math"
```


<br>
<br>


#### 导出名

在Go中，如果一个名字以大写字母，那么它就是已导出的。

在导入一个包时，你只能引用其中已导出的名字。任何未导出的名字在该包外均无法访问。

```go
// exporter-nams.go

package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Println(math.pi)
    //fmt.Println(math.Pi)
}
```

```
运行math.pi会报错
./prog.go:9:14: cannot refer to unexported name math.pi
./prog.go:9:14: undefined: math.pi

运行math.Pi
3.141592653589793
```


<br>
<br>


#### 函数

函数可以没有参数或接受多个参数。注意类型在变量名之后。

```go
// functions.go

package main

import "fmt"

func add(x int, y int) int {
    return x + y
}
// 省略模式: x, y int

func main() {
  fmt.Println(add(42, 13))
}
```


<br>
<br>


#### 多值返回

函数可以返回任意数量的返回值。

```go
// multiple-results.go

import "fmt"

func swap(x, y string) (string, string) {
    return y, x
}

func main() {
    a, b := swap("hello", "world")
    fmt.Println(a, b)
}
```


<br>
<br>


#### 命名返回值

go的返回值可被命名，它们会被视作定义在函数顶部的变量。返回值的名称应当具有一定的意义。

没有参数的`return`语句返回已命名的返回值，也就是**直接**返回。直接返回语句应当仅在短函数中，在长函数中会影响代码的可读性。

```go
// name-result.go

package main

import "fmt"

func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum -x
    return
}

func main() {
    fmt.Println(split(17))
}
```


<br>
<br>


#### 变量

`var`语句用于声明一个变量列表。

```go
// variables.go

package main

import "fmt"

var c, python, java bool

func main() {
    var i int
    fmt.Println(i, c, python, java)
}
```


<br>
<br>


#### 变量初始化

变量声明可以包含初始值。
如果初始化值已存在，则可以省略类型，变量会从初始值中获得类型。

```go
// var-ini.go

package main

impoort "fmt"

var i, j, int = 1, 2

func main() {
    var c, python, java = true, false, "no!"
    fmt.Println(i, j, c, python, java)
}
```


<br>
<br>


#### 短变量声明

在函数中，简洁赋值语句`:=`可在类型明确的地方代替`var`声明。

函数外的每个语句都必须以关键字(var, func...)开始，因此`:=`结构不能在函数外使用。

```go
// short-var-declarations.go

package main()

import "fmt"

func main() {
    var i, j int = 1, 2
    k := 3
    c, python, java := true, false, "no!"

    fmt.Println(i, j, k, c, python, java)
}
```


<br>
<br>


#### 数据类型

go的基本类型有：

- bool
- string
- int, int8, int16, int32, int64
- uint, uint8, uint16, uint32, uint64, uintptr
- byte(uint8的别名)
- rune(int32的别名，表示一个unicode码点)
- float32, float64
- complex64, complex128

int, uint 和 uintptr 在 32 位系统上通常为 32 位宽，在 64 位系统上则为 64 位宽。 当你需要一个整数值时应使用 int 类型，除非你有特殊的理由使用固定大小或无符号的整数类型。

```go
// basic-types.go

package main

import (
    "fmt"
    "math/cmplx"
)

var (
    ToBe bool = false
    MaxInt uint64 = 1<<64 - 1
    x complex128 = cmplx.Sqrt(-5 ++ 12i)
)

func main() {
    fmt.Printf("Type: %T Value: %v\n", ToBe, ToBe)
    fmt.Printf("Type: %T Value: %v\n", MaxInt, MaxInt)
    fmt.Printf("Type: %T Value: %v", z, z)
}
```


<br>
<br>


#### 零值

没有明确初始值的变量声明会被赋予它们的**零值**。

零值是:

- 数值类型为 `0`
- 布尔类型为 `false`
- 字符串为空字符串

```go
// zero.go

package main

import "fmt"

func main() {
    var i int
    var f float64
    var b bool
    var s string
    fmt.Printf("%v %v %v %q\n", i, f, b, s)
}

```


<br>
<br>


#### 类型转换

`T(v)`将v转换为T类型。

```go
// type-conversions.go

package main

import (
    "fmt"
    "math"
)

func main() {
    var x, y int = 3, 4
    var f float64 = math.Sqrt(float64(x*x + y*y))
    var z unit = unit(f)
    fmt.Println(x, y, z)
}
```


<br>
<br>


#### 类型推导

在声明一个变量而不指定其类型时，变量的类型由右值推导而出。

```go
// type-inference.go

package main

import "fmt"

func main() {
    v := 42 // int
    fmt.Printf("v is of type %T\n", v)
}
```


<br>
<br>


#### 常量

常量的声明与变量类似，只不过使用`const`关键字。

常量可以是字符、字符串、布尔值、数值。

常量不能用`:=`语法声明。

```go
// constants.go

package main

import "fmt"

const Pi = 3.14

func main() {
    const World = "世界"
    fmt.Println("Hello", World)
    fmt.Println("Happy", Pi, "Day")

    const Truth = true
    fmt.Println("Go rules?" Truth)
}
```


<br>
<br>


#### 数值常量

数值常量是高精度的**值**。一个未指定类型的常量由上下文来决定其类型。

```go
// nemeric-constants.go

package main

import "fmt"

const (
    // 将1左移100位来创建一个非常大的数字，即这个数的二进制是1后面跟着100个0
    Big = 1 << 100
    // 再往右移99位，即Small = 1 << 1，或Small = 2
    Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1}
func needFloat(x float64) float64 {return x * 0.1}

func main() {
    fmt.Println(needInt(Small))
    fmt.Println(needFloat(Small))
    fmt.Println(needFloat(Big))
}
```



<br>
<br>



### 流程控制

flowcontrol: <https://tour.go-zh.org/flowcontrol>

学习如何使用条件、循环、分支和推迟语句来控制代码的流程。


<br>

#### for

go只有一种循环结构: `for`循环。它由三部分组成：

- 初始化语句： 在第一次迭代前执行
- 条件表达式：在每次迭代前求值
- 后置语句： 在每次迭代的结尾执行

初始化语句和后置语句是可选的。

初始化语句通常为一句短变量声明，该变量声明仅在for语句的作用域中可见。一旦条件表达式的布尔值为false，循环迭代就会终止。

```go
// for.go

package main

import "fmt"

func main() {
    sum := 0
    for i := 0; i < 10; i++ {
        sum += i
    }
    fmt.Println(sum)
}
```

```go
// for-continued.go

package main

import "fmt"

func main() {
    sum := 1
    for ; sum < 1000; {
        sum += sum
    }
    fmt.Println(sum)
}
```


<br>
<br>


#### for是while

go的for就是while。

```go
// for-is-while.go

package main

import "fmt"

func main() {
    sum := 1
    for sum < 1000 {
        sum += sum
    }
    fmt.Println(sum)
}
```


<br>
<br>


#### 无限循环

如果省略循环条件，该循环就不会结束，因此无限循环可以写的很紧凑。

```go
// forever.go

package main

func main() {
    for {}
}
```


<br>
<br>


#### if

```go
// if.go

package main

import (
    "fmt"
    "math"
)

func sqrt(x float64) string {
    if x < 0 {
        return sqrt(-x) + "i"
    }
    return fmt.Sprint(math.Sqrt(x))
}

func main() {
    fmt.Println(sqrt(2), sqrt(-4))
}
```


<br>
<br>


#### 简短的if

if语句可在条件表达式前执行一个简单的语句。该语句声明的变量作用域仅在if之内。

```go
// if-short.go

package main

import (
    "fmt"
    "math"
)

func pow(x, n, lim float64) float64 {
    if v := math.Pow(x, n); v < lim {
        return v
    }
    return lim
}

func main() {
    fmt.Println(
        pow(3, 2, 10),
        pow(3, 3, 20),
        )
}
```


<br>
<br>


#### else

```go
// else.go

package main

import (
    "fmt"
    "math"
)

func pow(x, n, lim float64) float64 {
    if v := math.Pow(x, n); v < lim {
        return v
    } else {
        fmt.Printf("%g >= %g\n", v, lim)
    }
    return lim
}

func main() {
    fmt.Println(
        pow(3, 2, 10),
        pow(3, 3, 20),
    )
}
```


<br>
<br>


#### switch

switch是一连串的if-else语句的简单写法。它运行第一个值等于条件表达式的case语句。

```go
// switch.go

package main

import (
    "fmt"
    "runtime"
)

func main() {
    fmt.Print("Go runs on ")
    switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("OS X.")
    case "linux":
        fmt.Println("Linux.")
    default:
        fmt.Printf("%s. \n", os)
    }
}
```

<br>

switch的case语句从上到下依次执行，知道匹配成功时停止。

```go
// switch-order.go

package main

import (
    "fmt"
    "time"
)

func main() {
    fmt.Println("When's Saturday?")
    today := time.NOw().Weekday()
    switch time.Saturday {
    case today + 0:
        fmt.Println("Today.")
    case today + 1:
        fmt.Println("Tomorrow.")
    case today + 2:
        fmt.Println("In two days.")
    default:
        fmt.Println("Too far away.")
    }
}
```

<br>

没有条件的switch同`switch true`一样。这种形式能将一长串if-then-else写得更加清晰。

```go
// switch-no-condition.go

package main

import (
    "fmt"
    "time"
)

func main() {
    t := Now()
    switch {
    case t.Hour() < 12:
        fmt.Pringln("Good morning!")
    case t.Hour() < 17:
        fmt.Println("Good afternoon.")
    default:
        fmt.Println("Good evening.")
    }
}
```


<br>
<br>


#### defer

defer语句会将函数推迟到外层函数返回之后执行。

推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。

```go
// defer.go

package main

import "fmt"

func main() {
    defer fmt.Println("world")

    fmt.Pringln("hello")
}
```

<br>

推迟的函数调用会被压入一个栈中。当外层函数返回时，被推迟的函数会按照**后进先出**的顺序调用。

```go
// defer-multi.go

package main

import "fmt"

func main() {
    fmt.Pringln("counting")
    for i := 0; i < 10; i++ {
        defer fmt.Pringln(i)
    }
    fmt.Println("done")
}
```



<br>
<br>



### 更多类型

学习如何基于现有类型定义新的类型，包含结构体、数组、切片和映射。

<br>

#### 指针

go拥有指针。指针保存了值的内存地址。类型`*T`是指向T类型值的指针。其零值位nil。`&`操作符会生成一个指向其操作数的指针。`*`操作符表示指针指向的底层值。这也就是常说的间接引用和重定向。

与C不同，go没有指针运算。

```go
// pointers.go

package main

import "fmt"

func main() {
    i, j := 42, 2701

    p := &i // 指向i
    fmt.Pringln(*p) // 通过指针读取i的值
    *p = 21 // 通过指针设置i的值
    fmt.Pringln(i)

    p = &j
    *p = *p /37
    fmt.Pringln(j)
}
```


<br>
<br>


#### 结构体

一个结构体(struct)就是一组字段(field)。

```go
// structs.go

package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
  fmt.Println(Vertex{1, 2})
}
```

<br>

结构体字段使用点号来访问。

```go
// struct-fields.go

package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    v := Vertex{1, 2}
    v.X = 4
    fmt.Println(v.X)
}
```

<br>

结构体字段可以通过结构体指针来访问。

如果有一个指向结构体的指针P，那么可通过`(*p).X`来访问其字段X。不过这样写太啰嗦，可隐式间接引用，直接写`p.X`。

```go
// struct-pointers.go

package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    v := Vertex{1, 2}
    p :=  &v // 指针
    p.X = 1e9
    fmt.Println(v)
}
```

<br>

结构体文法通过直接列出字段的值来新分配一个结构体。

```go
// struct-literals.go

package main

import "fmt"

type Vertex struct {
    X, Y int
}

var (
    v1 = Vertex{1, 2} // 创建一个Vertex类型的结构体
    v2 = Vertex{X: 1} // Y:0被隐式地赋予
    v3 = Vertex{} // X:0 Y:0
    p = &Vertex{1, 2} // 创建一个*Vertex类型的结构体(指针)
)

func main() {
    fmt.Println(v1, p, v2, v3)
}
```


<br>
<br>


#### 数组

类型`[n]T`表示拥有n个T类型的值的数组。

数组的长度是其类型的一部分，因此数组不能改变大小。

```go
// array.go

package main

import "fmt"

func main() {
    var a [2]string
    a[0] = “Hello"
    a[1] = "World"
    fmt.Pringln(a[0], a[1])
    fmt.Println(a)

    primes := [6]int{2, 3, 5, 7, 11, 13}
    fmt.Println(primes)
}
```


<br>
<br>


#### 切片

每个数组大小都是固定的，而切片则为数组元素提供动态大小的、灵活的视角。在实践中，切片比数组更常用。

类型`[]T`表示一个元素类型为T的切片。

```go
// slices.go

package main

import "fmt"

func main() {
    primes := [6]int{2, 3, 5, 7, 11, 13}

    var s []int = primes[1:4]
    fmt.Println(s)
}
```

切片并不存储任何数据，它只是描述了底层数组中的一段。更改切片的元素会修改其底层数组中对应的元素。与它共享底层数组的切片都会观测到这些修改。

```go
// slices-pointers.go

package main

import "fmt"

func main() {
    names := [4]string{"John", "Paul", "George", "Ringo",}
    fmt.Println(names)

    a := names[0:2]
    b := names[1:3]
    fmt.Pringln(a, b)

    b[0] = "XXX"
    fmt.Pringln(a, b)
    fmt.Pringln(names)
}
```

<br>

切片文法类似于没有长度的数组文法。

```go
// slice-literals.go

package main

import "fmt"

func main() {
    q := []int{2, 3, 5, 7, 11, 13} // 创建一个数组，并构建一个引用数组的切片
    fmt.Pringln(q)

    r := []bool{true, false, true, true, false, true}
    fmt.Pringln(r)

    s := []struct {
        i int
        b bool
    } {
        {2, true},
        {3, false},
        {5, true},
        {7, true},
        {11, false},
        {13, true},
    }
    fmt.Pringln(s)
}
```

<br>

在进行切片时，你可以利用它的默认行为来忽略上下界。

切片拥有长度和容量。切片的长度就是它所包含的元素个数。切片的容量从第一个元素开始数，到元素末尾的个数。

可通过`len()`和`cap()`来获取。

```go
// slice-len-cap.go

package main

import "fmt"

func main() {
    s := []int{2, 3, 5, 7, 11, 13}
    printSlice(s)

    // 截取切片使其长度为0
    s = s[:0]
    printSlice(s)

    // 扩展长度
    s = s[:4]
    printSlice(s)

    // 舍弃前两个值
    s = s[2:]
    printSlice(s)
}

func printSlice(s []int) {
    fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

<br>

切片的零值`nil`。nil切片的长度和容量为0且没有底层数组。

```go
// nil-slices.go

package main

import "fmt"

func main() {
    var s []int
    fmt.Println(s, len(s), cap(s))
    if s == nil {
        fmt.Pringln("nil!")
    }
}
```

<br>

切片可以使用内建函数`make`来创建，这也是创建动态数组的方式。`make`函数会分配一个元素为零值的数组并返回一个引用了它的切片。

```go
// making-slices.go

package main

import "fmt"

func main() {
    a := make([]int, 5)
    PrintSlice("a", a)

    b := make([]int, 0, 5)
    printSlice("b", b)

    c := b[:2]
    printSlice("c", c)

    d := c[2:5]
    printSlice("d", d)
}

func printSlice(s string, x []int) {
    fmt.Printf("%s lend=%d cap=%d %v\n",
        s, len(x), cap(x), x)
}
```

<br>

切片可包含任何类型，甚至包括其他切片。

```go
// slices-of-slices.go

package main

import (
    "fmt"
    "STRINGS"
)

func main() {
    // 创建一个井字板
    board := [][]string{
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
    }

    // 两个玩家轮流打上 x和o
    board[0][0] = "X"
    board[2][2] = "O"
    board[1][2] = "X"
    board[1][0] = "O"
    board[0][2] = "X"

    for i := 0; i < len(board); i++ {
        fmt.Printf("%s\n", strings.Join(board[i], " "))
    }
}
```

<br>

向切片追加新的元素是常用的操作，为此go提供了内建的`append`函数。

```go
// append.go

package main

import "fmt"

func main() {
    var s []int
    printSlice(s)

    // 添加一个空切片
    s = append(s, 0)
    printSlice(s)

    s = append(s, 1)
    printSlice(s)

    // 一次性添加多个元素
    s = append(s, 2, 3, 4)
    printSlice(s)
}

func printSlice(s []int) {
    fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```


<br>
<br>


#### range

for循环的range形式可以遍历切片或映射。

```go
// range.go

package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
    // 下标，元素副本
    for i, v := range pow {
        fmt.Printf("2**%d = %d\n", i, v)
    }
}
```

<br>

可将下标或值赋予下划线(`_`)来忽略它。

```go
for i, _ := range pow

for _, value := range pow

# 若只要索引，忽略第二个变量即可
for i := range pow
```

```go
// range-continued.go

package main

import "fmt"

func main() {
    pow := make([]int, 10)
    for i := range pow {
        pow[i] = 1 << uint(i) // == 2**i
    }
    for _, value := range pow {
        fmt.Printf("%s\n", value)
    }
}
```


<br>
<br>


#### 映射

映射将键映射到值。映射的零值为`nil`。nil映射既没有键，也不能添加键。`make`函数会返回给定类型的映射，并将其初始化备用。

```go
// maps.go

package main

import "fmt"

type Vertex struct {
    Lat, Long float64
}

var m map[string]Vertex

func main() {
    m = make(map[string]Vertex)
    m["Bell Labs"] = Vertex{
        40.68433, -74.39967,
    }
    fmt.Println(m["Bell Labs"])
}
```

<br>

映射的文法与结构体类似，不过必须有键名。

```go
// map-literals.go

package main

import "fmt"

type Vertex struct {
    Lat, Long float64
}

var m = map[string]Vertex{
    "Bell Labs": Vertex{40.68433, -74.39967,},
    "Google": Vertex{37.42202, -122.08408},
}

func main() {
    fmt.Println(m)
}
```

<br>

若顶级类型只有一个类型名，可以在文法的元素中省略它。

```go
// map-literals-continued.go

package main

import "fmt"

type Vertex struct {
    Lat, Long float64
}

var m = map[string]Vertex{
    "Bell Labs": {40.68433, -74.39967},
    "Google": {37.42202, -122.08408},
}

func main() {
    fmt.Println(m)
}
```

<br>

可对映射进行增删查改。

```go
# 插入或修改
m[key] = elem
# 获取
elem = m[key]
# 删除
delete(m, key)

# 通过双赋值检测某个键是否存在
elem, ok = m[key]
# 若 key 在 m 中，ok 为 true ；否则，ok 为 false。
# 若 key 不在映射中，那么 elem 是该映射元素类型的零值。
# 当从映射中读取某个不存在的键时，结果是映射的元素类型的零值。
```

```go
// mutating-maps.go

package main

import "fmt"

func main() {
    m := make(map[string]int)

    m["Answer"] = 42
    fmt.Println("The value:", m["Answer"])

    m["Answer"] = 48
    fmt.Println("The value:", m["Answer"])

    delete(m, "Answer")
    fmt.Println("The value:", m["Answer"])

    v, ok := m["Answer"]
    fmt.Println("The value:", v, "Present?", ok)
}
```


<br>
<br>


#### 函数值

函数也是值。它们可以像其它值一样传递。

函数值可以用作函数的参数或返回值。

```go
// function-values.go

package main

import (
    "fmt"
    "math"
)

func compute(fn func(float64, float64) float64) float64 {
    return fn(3, 4)
}

func main() {
    hypot := func(x, y float64) float64 {
        return math.Sqrt(x*x + y*y)
    }
    fmt.Println(hypot(5, 12))

    fmt.Println(compute(hypot))
    fmt.Println(compute(math.Pow))
}
```


<br>
<br>


#### 函数的闭包

go函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。该函数可以访问并赋予其引用的变量的值，换句话说，该函数被这些变量绑定在一起。

例如，函数adder返回一个闭包。每个闭包都被绑定在其各自的sum变量上。

```go
// functtion-closures.go

package main

import "fmt"

func adder() func(int) int {
    sum := 0
    return func(x int) int {
        sum += x
        return sum
    }
}

func main() {
    pos, neg := adder(), adder()
    for i := 0; i < 10; i++ {
        fmt.Println(
            pos(i),
            neg(-2*i),
        )
    }
}
```

<br>

斐波那契闭包

```go
// fibonacci-closure.go

package main

import "fmt"

// 返回一个int函数
func fibonacci() func() int {
}

func main() {
    f := fibonacci()
    for i := 0; i < 10; i++ {
        fmt.Println(f())
    }
}
```



<br>
<br>



### 方法和接口

docs: <https://tour.go-zh.org/methods/1>

包含方法和接口，可以用这种构造来定义对象及其行为。


<br>


#### 方法

go没有类。

不过你可以为结构体类型定义方法。方法就是一类带特殊的接收者参数的函数。方法接收者在它自己的参数列表内，位于func关键字和方法名之间。

```go
// methods.go

package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float64
}

// Abs方法拥有一个名为v，类型为Vertex的接收者
func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
    v := Vertex{3, 4}
    fmt.Println(v.Abs())
}
```

<br>

方法只是个带接收者参数的函数。

```go
// metheods-funcs.go

package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float
}

func Abs(v Vertex) float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
    v := Vertex(3, 4)
    fmt.Println(Abs(v))
}
```

<br>

也可以为非结构体类型声明方法。接收者的类型定义和方法声明必须在同一包内，不能为内建类型声明方法。

```go
// methods-continued.go

package main

import (
    "fmt"
    "math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}

func main() {
    f := MyFloat(-math.Sqrt2)
    fmt.Println(f.Abs())
}
```


<br>
<br>


#### 指针接收者

可以为指针接收者声明方法。

```go
// methods-pointers.go

package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float64
}

func (v, Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f
}

func main() {
    v := Vertex{3, 4}
    v.Scale(10)
    fmt.Println(v.Abs())
}
```


<br>
<br>


#### 指针与函数

```go
// methods-pointers-explained.go

package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float64
}

func Abs(v Vertex) float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func Scale(v *Vertex, f float64) {
    v.X = v.X * f
    x.Y = x.Y * f
}

func main() {
    v := Vertex{3, 4}
    Scale(&v, 10)
    fmt.Println(Abs(v))
}
```


<br>
<br>


#### 方法与指针重定向

```go
// indirection.go

package main

import "fmt"

type Vertex struct {
    X, Y float64
}

func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    x.Y = v.Y * f
}

func ScaleFunc(v *Vertex, f float64) {
    v.X = V.X * f
    v.Y = v.Y * f
}

func main() {
    v := Vertex{3, 4}
    v.Scale(2)
    ScaleFunc(&v, 10)

    p := &Vertex{4, 3}
    p.Scale(3)
    ScaleFunc(p, 8)

    fmt.Println(v, p)
}
```

<br>

```go
// indirection-values.go

package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float64
}

func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func AbsFunc(v Vertex) float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
    v := Vertex{3, 4}
    fmt.Println(v.Abs())
    fmt.Println(AbsFunc(v))

    p := &Vertex{4, 3}
    fmt.Println(p.Abs())
    fmt.Println(AbsFunc(*p))
}
```


<br>
<br>


#### 选择值或指针作为接收者

使用指针接收者的原因有二：

- 方法能够修改其接收者指向的值
- 可以避免在每次调用方法时复制该值。若值的类型为大型结构体时，这样做会更加高效

```go
// methods-pointer-receivers.go

package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float64
}

func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
    v := &Vertex{3, 4}
    fmt.Printf("Before scaling: %+v, Abs: %v\n", v, v.Abs())
    v.Scale(5)
    fmt.Printf("After scaling: %+v, Abs: %v\n", v, v.Abs())
}
```


<br>
<br>


#### 接口

接口类型是由一组方法签名定义的集合。接口类型的变量可以保存任何实现了这些方法的值。

类型通过实现一个接口的所有方法来实现该接口。既然无需专门显式声明，也就没有`implements`关键字。隐式接口从接口的实现中解耦了定义，这样接口的实现可以出现在任何包中，无需提前准备。因此，也就无需在每一个实现上增加新的接口名称，这样同时也鼓励了明确的接口定义。

```go
// interfaces-implicitly.go

package main

import "fmt"

type I interface {
    M()
}

type T struct {
    S string
}

// 此方法表示类型T实现了接口I，但我们无需显式声明
func (t T) M() {
    fmt.Println(t.S)
}

func main() {
    var i I = T{"Hello"}
    i.M()
}
```

<br>

接口也是值。它们可以像其它值一样传递。接口值可以用作函数的参数或返回值。

在内部，接口值可以看做包含值和具体类型的元组：`(value, type)`。接口值保存了一个具体底层类型的具体值。接口值调用方法时会执行其底层类型的同名方法。

```go
// interface-values.go

package main

import (
    "fmt"
    "math"
)

type I interface {
    M()
}

type T struct {
    S string
}

func (t *T) M() {
    fmt.Println(t.S)
}

type F float64

func (f F) M() {
    fmt.Println(f)
}

func main() {
    var i I

    i = &T["Hello"]
    describe(i)
    i.M()

    i = F(math.Pi)
    describe(i)
    i.M()
}

func describe(i I) {
    fmt.Printf("(%v, %T)\n", i, i)
}
```

<br>

底层值为nil的接口值。

即便接口内的具体值为nil， 方法仍然会被nil接收者调用。注意，保存了nil具体值的接口其自身并不为nil。

```go
// interface-values-nil.go

package main

import "fmt"

type I interface {
    M()
}

type T struct {
    S tring
}

func (t *T) M() {
    if t == nil {
        fmt.Println("<nil>")
        return
    }
    fmt.Println(t.S)
}

func main() {
    var i I

    var t *T
    i = t
    describe(i)
    i.M()

    i = &T{"hello"}
    describe(i)
    i.M()
}

func describe(i I) {
    fmt.Printf("(%v, %T)\n", i, i)
}
```


<br>
<br>


#### nil接口值

nil接口值既不保存值也不保存具体类型。

```go
// nil-interface-values.go

package main

import "fmt"

type I interface {
    M()
}

func main() {
    var i I
    describe(i)
    i.M()
}

func describe(i I) {
    fmt.Printf("(%v, %T)\n", i, i)
}
```


<br>
<br>


#### 空接口

指定了零个方法的接口值被称为**空接口**: `interface{}`

空接口可保存任何类型的值，因为每个类型都至少实现了零个方法。空接口被用来处理未知类型的值。

```
// empty-interface.go

package main

import "fmt"

func main() {
    var i interface{}
    describe(i)

    i = 42
    describe(i)

    i = "hello"
    describe(i)
}

func describe(i interface{}) {
    fmt.Printf("(%v, %T)\n", i, i)
}
```


<br>
<br>


#### 类型断言

类型断言提供了访问接口值底层具体值的方式。

```
// 该语句断言接口值i保存了具体类型T，并将其底层类型为T的值赋予变量t
t := i.(T)


// 类型断言可返回两个值
// 底层值以及一个报告断言是否成功的布尔值
t, ok := i.(T)
```


```go
// type-assertions.go

package main

import "fmt"

func main() {
    var i interface{} = "hello"

    s := i.(string)
    fmt.Pringln(s)

    s, ok := i.(string)
    fmt.Println(s, ok)

    f, ok := i.(float64)
    fmt.Println(f, ok)

    f = i.(float64) // 报错(panic)
    fmt.Println(f)
}
```


<br>
<br>


#### 类型选择

类型选择是一种按顺序从几个类型断言中选择分支的结构。

```go
// type-switches.go

package main

import "fmt"

func do(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Printf("Twice $v is %v\n", v, v*2)
    case string:
        fmt.Printf("%q is %v bytes long\n", v, len(v))
    default:
        fmt.Printf("I don't how about type %T!\n", v)
    }
}

func main() {
    do(21)
    do("hello")
    do(true)
}
```


<br>
<br>


#### Stringer

`fmt`包中定义的`Stringer`是最普遍的接口之一。

```
// stringer.go

package main

import "fmt"

type Person struct {
    Name string
    Age int
}

func (p Person) String() string {
    return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
    a := Person{"Arthur Dent", 42}
    z := Person{"Zaphod Beeb12brox", 9001}
    fmt.Println(a, z)
}
```


<br>
<br>


#### 错误

go程序使用`error`值来表示错误状态，它是一个内建接口。

```go
// errors.go

package main

import (
    "fmt"
    "time"
)

type MyError struct {
    When time.Time
    What string
}

func (e *MyError) Error() string {
    return fmt.Sprintf("at %v, %s",
        e.When, e.What)
}

func run() error {
    return &MyError{
        time.Now(),
        "it didn't work",
    }
}

func main() {
    if err := run(); err != nil {
        fmt.Println(err)
    }
}
```


<br>
<br>


#### Reader

`io`包指定了`io.Reader`接口，它表示从数据流的末尾进行读取。

```go
// reader.go

package main()

import (
    "fmt"
    "io"
    "strings"
)

func main() {
    r := strings.NewReader("Hello, Reader!")

    b := make([]byte, 8)
    for {
        n, err := r.Read(b)
        fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
        fmt.Printf("b[:n] = %q\n", b[:n])
        if err == io.EOF {
            break
        }
    }
}
```


<br>
<br>


#### 图像

`image`包定义了`Image`接口。

```go
// images.go

package main

import (
    "fmt"
    "image"
)

func main() {
    m := image.NewRGBA(image.Rect(0, 0, 100, 100))
    fmt.Println(m.Bounds())
    fmt.Println(m.At(0, 0).RGBA())
}
```



<br>
<br>



### 并发

doc: <https://tour.go-zh.org/concurrency/1>

作为语言的核心部分，go提供了并发的特性。这一部分概览了goroutine和channel，以及如何使用它们来实现不同的并发模式。


<br>


#### goroutine

go程(goroutine)是由go运行时管理的轻量级线程。

```go
go f(x, y, z)
# 会启动一个新的goroutine并执行
f(x, y, z)
# f, x, y, z的求值发生在goroutine中
# 而f的执行发生在新的goroutine中
```

goroutine在相同的地址空间中运行，因此在访问共享的内存时必须进行同步。

```go
// goroutines.go

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
    sqy("hello")
}
```


<br>
<br>


#### 信道

信道是带有类型的管道，通过它用信道操作符`<-`来发送或接收值。

```go
// 信道在使用前必须创建
ch := make(chan int)

// 箭头就是数据流的方向
ch <- v   // 将v发送至信道ch
v := <-ch // 从ch接收值并赋予v
```

```go
// channels.go

package main

import "fmt"

func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // 将和送入c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    c := make(chan int)
    go sum(s[:len(s)/2], c)
    go sum(s[len(s)/2:], c)
    x, y := <-c, <-c // 从c中接收

    fmt.Println(x, y, x+y)
}
```


<br>
<br>


#### 带缓冲的信道

将缓冲长度作为第二个参数提供给`make`来初始化一个带缓冲的信道，仅当信道的缓冲区填满后，向其发送数据时才会阻塞。当缓冲区为空时，接受方会阻塞。

```go
// buffered-channels.go

package main

import "fmt"

func main() {
    ch := make(chan int, 2)
    ch <- 1
    ch <- 2
    // ch <- 3 填满缓冲区
    fmt.Println(<-ch)
    fmt.Println(<-ch)
}
```


<br>
<br>


#### close

发送者可通过`close`关闭一个信道来表示没有需要发送的值。接收者可以通过为接收表达式分配第二个参数来测试信道是否被关闭。

只有发送者才能关闭信道，而接收者不能。向一个已经关闭的信道发送数据会引发程序恐慌(panic)。信道与文件不同，通常情况下不需要关闭它们。只有在必须告诉接收者不再有需要发送的值时才有必要关闭，例如终止一个range循环。

```go
// 若没有值可接收且信道已关闭，在执行完后,ok会被设置为false
v, ok := <-ch

// 循环for i := range c会不断从信道接收值，直到它关闭
```

```go
// range-and-close.go

package main

import (
    "fmt"
)

func fibonacci(n int, c chan int) {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        c <- x
        x, y = y, x+y
    }
    close(c)
}

func main() {
    c := make(chan int, 10)
    go fibonacci(cap(c), c)
    for i := range c {
        fmt.Println(i)
    }
}
```


<br>
<br>


#### select

select语句使一个go routine可以等待多个通信操作。
它会阻塞到某个分支可以继续执行为止，这时就会执行该分支。当多个分支都准备好时会随机选择一个执行。

```go
// select.go

package main

import "fmt"

func fibonacci(c, quit chan int) {
    x, y := 0, 1
    for {
        select {
            case c <- x:
                x, y = y, x+y
            case <-quit:
                fmt.Println("quit")
                return
        }
    }
}

func main() {
    c := make(chan int)
    quit := make(chan int)
    go func() {
        for i := 0; i < 10; i++ {
            fmt.Println(<-c)
        }
        quit <- 0
    }()
    fibonacci(c, quit)
}
```

<br>

当select中的其它分支都没有转杯好时，default分支就会执行。

```go
// default-selection.go

package main

import (
    "fmt"
    "time"
)

func main() {
    tick := time.Tick(100 * time.Millisecond)
    boom := time.After(500 * time.Millisecond)
    for {
        select {
        case <-tick:
            fmt.Println("tick.")
        case <-boom:
            fmt.Println("BOOM!")
            return
        default:
            fmt.Println("    .")
            time.Sleep(50 * time.Millisecond)
        }
    }
}
```


<br>
<br>


#### 互斥锁

信道非常适合在各个Go routine间进行通信。但如果并不需要通信，只想保证每次只有一个go routine能够访问一个共享的变量，从而避免冲突。

这里面涉及的概念就做**互斥**(mutual exclusion)，通常使用**互斥锁**(Mutex)这一数据结构来提供这种机制。go标准库提供了`sync.Mutex`互斥锁及其两个方法: `Lock`, `Unlock`。


```go
// mutex-counter.go

package main

import (
    "fmt"
    "sync"
    "time"
)

// SafeCounter 的并发使用是安全的
type SafeCounter struct {
    v map[string]int
    mux sync.Mutex
}

// Inc 增加给定 key 的计数器的值
func (c *SafeCounter) Inc(key string) {
    c.mux.Lock()
    // Lock 之后同一时刻只有一个 go routine 能访问c.v
    c.v[key]++
    c.mux.Unlocak
}

// Value 返回给定key的计数器的当前值
func (c *SafeCounter) Value(key string) int {
    c.mux.Lock()
    // Lock之后同一时刻只有一个 go routine 能访问c.v
    defer c.mux.Unlock()
    return c.v[key]
}

func main() {
    c := SafeCounter{v: make(map[string]int)}
    for i := 0; i < 1000; i++ {
        go c.Inc("somekey")
    }

    time.Sleep(time.Second)
    fmt.Println(c.Value("somekey"))
}
```










<br>
<br>
<br>


## 如何编写go代码

doc: <https://golang.org/doc/code.html>


<br>


### 介绍

本文介绍如何开发一个模块内的一组简单的go包集合，并使用go工具，以标准的方式去fetch, build, install go modules, packages, commands。

> 注意:<br>
> 本文使用go `v1.13+`，并且没有设置`GO111MODULE`环境变量。


<br>
<br>


### 代码组织

go程序被组织到包。包是编译在同一目录中的源文件的集合。定义在一个源文件中的函数、类型、变量、常量对同一个包中的其它源文件可见。

一个仓库(repo)包含一个或多个模块。模块是发布到一起关联go包的集合。一个go仓库通常只包含一个模块，位于该库的根目录。`go.mod`文件声明了模块路径，该模块内所有包的导入路径前缀。该模块包含了`go.mod`文件此目录及其子目录的包。

注意，在你可以构建之前，你并不需要将代码发布到远程仓库。一个模块可以定义在本地而不属于一个仓库。然而，如果你某天希望发布你的代码，那么组织你的代码是一个很好的习惯。

每个模块的路径不仅作为其包的导入路径前缀，也预示着go命令在哪里下载它。例如，要下载`golang.org/x/tools`模块，go命令会通过协商表示`https://golang.org/x/tools`。

导入路径是用来导入包的字符串。一个包的导入路径是它和模块内子目录的加入模块的路径。例如，模块`github.com/google/go-cmp`在`cmp/`目录下包含一个包，这个包的导入路径是`github.com/google/go-cmp/cmp`。标准库中的包没有模块路径前缀。



<br>
<br>



### 第一个程序

要编译和运行一个简单的程序，首先要选择一个模块路径（如`example.com/user/hello`）并创建一个`go.mod`文件来声明它。

```
make hello
cd hello

go mod init example.com/user/hello
go: creating new go.mod: module example.com/user/hello

cat go.mod
module example.com/user/hello

go 1.14
```

<br>

go源文件的第一个语句必须是包名(package name)。可执行命令必须使用`package main`。

```go
// hello.go

package main

import "fmt"

func main() {
    fmt.Println("Hello, world.")
}
```

<br>

现在你可以使用go工具来构建和安装程序。

```
go install example/user/hello
```

此命令构建`hello`命令，产生一个可执行二进制文件，安装此二进制到`$HOME/go/bin/hello`。

安装目录由`GOPATH`和`GOBIN`环境变量控制。如果`GOBIN`有设置，则安装到它这个目录。如果`GOPATH`有设置，二进制被安装到`$GOPATH/bin/`下。否则，二进制文件被安装到默认`$GOPATH/bin`目录下。

可以使用`go env`命令来设置和取消环境变量:

```
# 设置
go env -w GOBIN=/somewhere/else/bin

# 取消
go env -u GOBIN
```

<br>

像`go install`这样的命令应用在包含当前工作目录的模块上下文内。如果当前工作目录不在`example.com/user/hell`模块内，则`go install`命令可能会失败。

为了方便，如果没有给定其它路径，go命令接收相对于当前工作目录的路径，默认为包的当前路径。因此，在当前工作目录下，下面的命令是等效的：

```
go install example.com/user/hello

go install .

go install
```

<br>

接下来，让我们运行此程序以确保它工作。

```
export PATH=$PATH:$(dirname $(go list -f '{{.Target}}' .))
hello
Hello, world
```

<br>

如果你在使用版本控制，那现在是初始化仓库，添加文件并提交你的第一个变化的好时机。这一步是可选的，你不需要使用版本控制编写go代码。

```
go init

git add go.mod hello.go
git commit -m "initial commit"
```

go命令通过请求HTTPS URL和从HTML响应中读取元数据来定位仓库包含的模块路径(`go help importpath`)。许多托管服务已经提供了包含go代码的元数据，使你的模块对其他人可用的最简单的方法通常是——使模块路径匹配仓库URL。


<br>
<br>


#### 从你的模块导入包

Importing packages from your module

让我们编写一个`morestrings`包，并从`hello`程序来使用它。首先，为包创建一个目录`$HOME/hello/morestrings`，并在目录下编写`reverse.go`源文件。

```go
// Package morestrings implements additional functions to manipulate UTF-8
// encoded strings, beyond what is provided in the standard "strings" package.
package morestrings

// ReverseRunes returns its argument string reversed rune-wise left to right.
func ReverseRunes(s string) string {
	r := []rune(s)
	for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
		r[i], r[j] = r[j], r[i]
	}
	return string(r)
}
```

测试并使用`go build`来编译包：

```
cd hello/morestrings

go build
```

这不会生成一个输出文件。相反，它在本地构建缓存(local build cache)中保存编译包(compiled package)。

在确认了`morestrings`包构建之后，让我们修改`hello.go`来使用morestrings包:

```go
package main

import (
    "fmt"
    "example.com/usr/hello/morestrings"
)

func main() {
    fmt.Println(morestrings.ReverseRunes("!oG ,0lleH"))
}
```

```
// install hello
go install example.com/user/hello

hello
Hello, Go!
```


<br>
<br>


#### 从远程模块导入包

Importing packages from remote modules

导入路径可以描述如何使用版本控制获得源代码。go工具使用该属性从远程仓库自动获取包。比如，在程序中使用`github.com/google/go-cmp/cmp`：

```go
package main

import (
    "fmt"
    "example.com/user/hello/morestrings"
    "github.com/google/go-cmp/cmp"
)

func main() {
	fmt.Println(morestrings.ReverseRunes("!oG ,olleH"))
	fmt.Println(cmp.Diff("Hello World", "Hello Go"))
}
```

当你运行`go install`, `go build`, `go run`这些命令时，go命令会自动下载远程模块并在`go.mod`文件中记录版本。

```
$ go install example.com/user/hello
go: finding module for package github.com/google/go-cmp/cmp
go: downloading github.com/google/go-cmp v0.4.0
go: found github.com/google/go-cmp/cmp in github.com/google/go-cmp v0.4.0
$ hello
Hello, Go!
  string(
- 	"Hello World",
+ 	"Hello Go",
  )
$ cat go.mod
module example.com/user/hello

go 1.14

require github.com/google/go-cmp v0.4.0
$
```

模块依赖关系自动下载到`$GOPATH/pkg/mod`目录。一个模块的特定版本的下载内容，要求该版本与所有其它模块之间共享，因此go命令标记目录和文件为只读。

```
# 删除所有下载的模块
go clean --modcache
```



<br>
<br>



### 测试

go有一个轻量测试框架`go test`命令和`testing`包。

你可以通过创建一个以`_test.go`名称结尾的文件来编写一个测试，此测试文件包含以`func (t *testing.T)`签名的`TestXXX`函数。测试框架运行每个这样的函数，如果此函数调用一个失败的函数（如`t.Error`或`t.Fail`），则测试被认为失败。

通过创建包含以下代码的`morestrings/reverse_test.go`文件，对`morestrings`包添加一个测试。

```go
package main

import "test"

func TestReverseRunes(t *testing.T) {
    cases := []struct {
        in, want string
    }{
        {"Hello, world", "dlrow ,olleH"},
        {"Hello, 世界", "界世 ,olleH"},
        {"", ""},
    }
    for _, c := range cases {
        got := ReverseRunes(c.in)
        if got != c.want {
            t.Errorf("ReverseRunes(%q) == %q, want %q, c.in, got, c.want")
        }
    }
}
```

接着使用`go test`运行测试:

```
$ go test
PASS
ok  	example.com/user/morestrings 0.165s
$

# 帮助
go help test
```




<br>
<br>
<br>



## ide和插件

doc: <https://golang.org/doc/editors.html>


- vim-go: <https://github.com/fatih/vim-go>
- Visual Studio Code: <https://marketplace.visualstudio.com/items?itemName=golang.Go>

<br>

我是用的k-vim已经添加了vim-go，只需要将`let g:bundle_groups=`中添加golang即可。



<br>
<br>
<br>


## 高效go编程

Effective Go: <https://golang.org/doc/effective_go.html>


<br>

### 介绍

Go是一门新语言。要把go写好，了解其性质和惯用语法是很重要的。同样重要的是要知道在go中程序所建立的约定。如命名、格式、项目建设等，让你写的程序会很容易为其他go程序员所理解。

此文档对编写清晰、惯用的go代码给出了一些技巧。


<br>
<br>


### 示例

[go package sources](https://golang.org/src/) 不仅作为核心库，而且为如何使用语言做了示例。


<br>
<br>


### 格式化

格式问题最具争议，但却始终没有形成统一的定论。若所有人都遵循相同的编码风格，在这类问题上浪费的时间将会更少。

在go中我们另辟蹊跷，让机器来处理大部分的格式问题。`gofmt`程序将go程序安装标准风格 进行缩进、对齐，保留注释并在需要时重新格式化。

举例来说，你无需花时间将结构体中的字段对其，`gofmt`将会为你代劳。

```go
type T struct {
        name string // 对象名
        value int // 对象值
}
```

`gofmt`会将它按列对齐：

```go
type T struct {
        name    string // 对象名
        value   int    // 对象值
}
```

标准包中的所有go代码都已经用`gofmt`格式化过了。一些关于格式化的细节：

- 缩进(Indentation)
使用制表符`tab`，`gofmt`也默认使用它。在你认为有必要的时候使用空格符(space)。

- 行长度(Line length)
go对行长度没有限制。如果一行实在太长，可以拆行并插入适当的tab缩进。

- 括号(Parentheses)
比起C和Java，Go所需的括号更少。控制结构(`if, for, switch`)在语法上并不需要圆括号。此外，操作符优先级处理变得更加简洁：

```go
x<<8 + y<<16
```


<br>
<br>


### 注释

go提供了C风格的块注释(`/* */`)和c++风格的行注释(`//`)。

godoc既是一个程序，又是一个Web服务器，它对go源码进行处理，并提取包中的文档内容。出现在顶级声明之前，且与该声明之间没有空行的注释，将与该声明一起被提取出来，作为该条目的说明文档。让我想起了Python的文档字符串(docstring)。

每个包都应包含一个包说明(package comment)——即放置在包子句前的一个块注释。对于包含多个文件的包，包注释只需出现在其中的任一文件中即可。包注释应在整体上对该包进行介绍，并提供包的相关信息。

```go
/*
regexp 包为正则表达式实现了一个简单的库。

它接受的正则表达式语法为：

    正则：
        串联 { '|' 串联}
    串联：
        { 闭包 }
    闭包：
        条目 [ '*' | '+' | '?' ]
    条目：
        '^'
        '$'
        '.'
        字符
        '[' [ '^' ] 字符遍历 ']'
        '(' 正则表达式 ')'
*/
package regexp
```

如果包比较简单，包说明可以简洁些：

```go
// Package path implements utility routines for
// manipulating slash-separated filename paths.
```

注释无需额外的格式化。`godoc`会像`gofmt`一样处理好一切。注释是不会被解析的纯文本，因此特定的格式不会被渲染。`godoc`是否会重新格式化注释取决于上下文，因此必须确保它看起来清晰易辨：使用正确的拼写、标点、句子结构以及折叠长行等。

在包中，任何顶级声明前的注释都作为该声明的文档说明。每个可导出名称的程序(首字母大写)都有该用文档说明。这让我想起了Python的类。

文档注释最好是完整的句子，这样它才能适应各种自动化的展示。 第一句应当以被声明的东西开头，并且是单句的摘要。

```go
// Compile parses a regular expression and returns, if successful,
// a Regexp that can be used to match against text.
func Compile(str string) (*Regexp, error) {
```

若注释总是以名称开头，`godoc`的输出就能通过`grep`变得更加有用。

```
go doc -all regexp | grep -i parse
```

go的声明语法允许成组声明。单个文档注释应介绍一组相关的常量或变量。 由于是整体声明，这种注释往往较为笼统。

```go
// Error codes returned by failures to parse an expression.
var (
    ErrInternal      = errors.New("regexp: internal error")
    ErrUnmatchedLpar = errors.New("regexp: unmatched '('")
    ErrUnmatchedRpar = errors.New("regexp: unmatched ')'")
    ...
)
```

即便是对于私有名称，也可通过成组声明来表明各项间的关系，例如某一组由互斥体保护的变量。

```go
var (
    countLock   sync.Mutex
    inputCount  uint32
    outputCount uint32
    errorCount  uint32
)
```


<br>
<br>


### 命名

names

命名在编程语言中很重要！


<br>


#### 包名

package names

当一个包被导入后，包名就会成为内容的访问器。

```go
import "bytes
```

包的名称应该简洁明了以便于理解。按照惯例，包应当以小写的单个单词来命名，且不应该使用下划线或驼峰记法(mixedCaps)。包名是就是导入时所需的默认名称，它并不需要在所有源码中保持唯一，即使在少数发生冲突的情况下，也可为导入的包选择一个别名来局部使用。无论如何，通过文件名来判定使用的包，基本不会产生混淆。

另一个约定就是包名应为其源码目录的基本名称。在`src/encoding/base64`中的包应作为`encoding/base64`导入，其包名为`base64`，而非`encoding_base64`或`encodingBase64`。

包的导入者可通过包名来引用其内容，因此包中的可导出名称可以此来避免冲突。请勿使用`import .`记法，它可以简化必须在被测试包外运行的测试， 除此之外应尽量避免使用。

另一个简短的例子是`once.Do`，`once.Do(setup)`表述足够清晰， 使用`once.DoOrWaitUntilDone(setup)`完全就是画蛇添足。 长命名并不会使其更具可读性。一份有用的说明文档通常比额外的长名更有价值。


<br>
<br>


#### 获取器

Getters

Go并不对获取器（getter）和设置器（setter）提供自动支持。 你应当自己提供获取器和设置器，通常很值得这样做，但若要将 Get 放到获取器的名字中，既不符合习惯，也没有必要。

```go
owner := obj.Owner()
if owner != user {
	obj.SetOwner(user)
}
```


<br>
<br>


#### 接口名

Interface names

按照约定，只包含一个方法的接口应当以该方法的名称加上`-er`后缀来命名，如`Reader`、`Writer`、 `Formatter`、`CloseNotifier`等。


<br>
<br>


#### 驼峰记法

MixedCaps

最后，go中约定使用`MexedCaps`或`mixedCaps`而不是下划线来编写多个词的名字。



<br>
<br>



### 分号

Semicolons

和C一样，Go的正式语法使用分号(`;`)来结束语句。但和C不同的是，这些分号不会出现在源码中。取而代之，词法分析器会使用一条简单的规则来自动插入分号，因此大部分输入文本是自由的。

若在新行前的最后一个标记为一个标识符(包括`int`, `float64`)，数值或字符串常量的基本字面或以下标记之一:

```
break continue fallthrough return ++ -- ) }
```

词法分析器将始终在该标记后面插入一个分号。这可以概括为：**如果新行前的标记为语句的末尾，则插入一个分号**。

分号也可以在关闭括号之前直接省略，因此一个语句像如下这样，不需要分号。

```
go func() { for { dst <- <-src } }()
```

通常go程序只在诸如`for`循环子句这样的地方使用分号。如果在一行中写多个语句，也需要使用分号分隔。

无论如何，你都不应该将控制结构(`if, for, switch, select`)的左括号放到下一行。你应该这样写：

```go
if i < f() {
    g()
}
```

而不是这样：

```go
if i < f() // wrong!
{          // wrong!
    g()
}
```


<br>
<br>


### 控制结构

Control structures

go的控制结构与C有许多相似之处，但其不同才是独到之处。go不使用`do`或`while`循环，只有一个更通用的`for`；`switch`要更灵活些；`if`和`switch`像`for`一样接受一个可选的初始化语句；`break`和`continue`语句有一个可选的标签来确定那些break或continue；此外，还有一个包含类型选择和多路通信复用器的新控制结构——`select`。它们的语法也有些许不同，没有圆括号，主体必须始终使用大括号括住。


<br>


#### if

```go
if x > 0 {
    return y
}
```

由于`if`和`switch`可接收初始化语句，因此用它们来设置局部变量很常见。

```go
if err := file.Chmod(0644); err != nil {
    log.Print(err)
    return err
}
```


<br>
<br>


#### 重新声明和重新赋值

Redeclaration and reassignment

```go
f, err := os.Open(name)

d, err := f.Stat()
```

满足下列条件时，已被声明的变量可出现在`:=`声明中：

- 本次声明与已声明的变量出于同一作用域（若变量已在外层作用域中声明过，则此次声明会创建一个新的变量`§`）
- 在初始化中与其类型相应的值才能赋予变量，且在此次声明中至少另有一个变量是新声明的


<br>
<br>


#### for

go的`for`循环统一了for和while。它有三种形式，但只有一种需要分号。

```go
// Like a C for
for init; condition; post { }

// Like a C while
for condition { }

// Like a C for(;;)
for { }
```

简短的声明使得更容易在循环中声明下标变量：

```go
sum := 0
for i := 0; i < 10; i++ {
    sum += i
}
```

若你想遍历数组、切片、字符串、映射，或从信道中读取消息，`range`子句能够帮你轻松实现循环。

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

```go
// 只需要遍历下标，去掉第二个
for key := range m {
    if key.expired() {
        delete(m, key)
    }
}
```

```go
// 只需要值，使用空白标识符(_)来丢弃下标
sum := 0
for _, value := range array {
        sum += value
}
```


<br>
<br>


#### switch

go的`switch`比C更通用。其表达式无需为常量或整数，`case`语句会自上而下逐一进行求值直到匹配为止。如果`switch`后面没有表达式，它将匹配`true`。因此，我们可以将`if-else-if-else`链写成一个`switch`，这也更符合go的风格。

```go
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}
```

`switch`并不会自动下溯，但`case`可通过逗号分隔来列举相同的处理条件。

```go
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

`break`语句可以使`switch`提前终止。不仅是`switch`，有时候也需要打破层层的循环。在go中，只需将标签(label)放置到循环外，然后break到标签。下例展示了两者的用法：

```go
Loop:
	for n := 0; n < len(src); n += size {
		switch {
		case src[n] < sizeOne:
			if validateOnly {
				break
			}
			size = 1
			update(src[n])

		case src[n] < sizeTwo:
			if n+1 >= len(src) {
				err = errShortInput
				break Loop
			}
			if validateOnly {
				break
			}
			size = 2
			update(src[n] + src[n+1]<<shift)
		}
	}
```

当然，`continue`语句也能接受一个可选的标签，不过它只能应用在循环中。

作为这一节的结束，下例使用两个switch语句对字节切片进行比较：

```go
// Compare returns an integer comparing the two byte slices,
// lexicographically.
// The result will be 0 if a == b, -1 if a < b, and +1 if a > b
func Compare(a, b []byte) int {
    for i := 0; i < len(a) && i < len(b); i++ {
        switch {
        case a[i] > b[i]:
            return 1
        case a[i] < b[i]:
            return -1
        }
    }
    switch {
    case len(a) > len(b):
        return 1
    case len(a) < len(b):
        return -1
    }
    return 0
}
```


<br>
<br>


#### 类型选择

type switch

`switch`也可用于判断接口变量的动态类型。如type switch通过括号中的关键字`type`使用类型断言。若`switch`在表达式中声明了一个变量，那么该变量的每个子句中都将有该变量对应的类型。

```go
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
    fmt.Printf("unexpected type %T\n", t)     // %T prints whatever type t has
case bool:
    fmt.Printf("boolean %t\n", t)             // t has type bool
case int:
    fmt.Printf("integer %d\n", t)             // t has type int
case *bool:
    fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
case *int:
    fmt.Printf("pointer to integer %d\n", *t) // t has type *int
}
```



<br>
<br>



### 函数

function


<br>


#### 多值返回

multiple return values

以下简单的函数可从字节数组中的特定位置获取其值，并返回该数值和下一个位置。

```go
func nextInt(b []byte, i int) (int, int) {
    for ; i < len(b) && !isDigit(b[i]); i++ {
    }
    x := 0
    for ; i < len(b) && isDigit(b[i]); i++ {
        x = x*10 + int(b[i]) - '0'
    }
    return x, i
}
```

获取多值：

```go
    for i := 0; i < len(b); {
        x, i = nextInt(b, i)
        fmt.Println(x)
    }
```


<br>
<br>


#### 命名结果形参

Named result parameters

go函数的返回值(return)或结果(result)**行参**可被命名，并作为常规变量使用。就像传入的形参一样。命名后，一旦该函数开始执行，它们就会被初始化为与其类型相应的零值；若该函数执行了一条不带参数的`return`语句，则结果形参的当前值将作为返回值。

此名称不是强制性的，但它们能使代码更加简洁明了：它们就是文档。如果我们命名了nextInt的结果，那么它返回的int就值如其意了：

```go
func nextInt(b []byte, pos int) (value, nextPos int) {
```

由于被命名的结果已经初始化，且已经关联至无参数的返回，它们就能让代码简单而清晰。

```go
func ReadFull(r Reader, buf []byte) (n int, err error) {
    for len(buf) > 0 && err == nil {
        var nr int
        nr, err = r.Read(buf)
        n += nr
        buf = buf[nr:]
    }
    return
}
```


<br>
<br>


#### Defer

go的`defer`语句用于预设一个函数调用(即推迟执行函数(deferred function))，该函数会在执行`defer`的函数返回之前立即执行。它显得非比寻常， 但却是处理一些事情的有效方式，例如无论以何种路径返回，都必须释放资源的函数。 典型的例子就是解锁互斥和关闭文件。

```go
// Contents returns the file's contents as a string.
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()  // f.Close will run when we're finished.

    var result []byte
    buf := make([]byte, 100)
    for {
        n, err := f.Read(buf[0:])
        result = append(result, buf[0:n]...) // append is discussed later.
        if err != nil {
            if err == io.EOF {
                break
            }
            return "", err  // f will be closed if we return here.
        }
    }
    return string(result), nil // f will be closed if we return here.
}
```

推迟如`Close`之类的函数调用有两个好处。第一， 它能确保你不会忘记关闭文件。如果你以后又为该函数添加了新的返回路径时，这种情况往往就会发生。第二，它意味着**关闭**离**打开**很近， 这总比将它放在函数结尾处要清晰明了。

推迟函数（如果函数是一个方法则还包括接收者）的实参在推迟执行时就会求值，而不是在调用执行时才求值。这样不仅无需担心变量值在函数执行时被改变， 同时还意味着单个已推迟的调用可推迟多个函数的执行。一个简单的例子：

```go
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
```

被推迟的函数会按照**后见先出(LIFO)**的顺序执行，因此上述返回为`4 3 2 1 0`。一个更具实际意义的例子，让程序跟踪函数的运行：

```go
func trace(s string)   { fmt.Println("entering:", s) }
func untrace(s string) { fmt.Println("leaving:", s) }

// Use them like this:
func a() {
    trace("a")
    defer untrace("a")
    // do something....
    fmt.Println("---")
}
```

输出结果如下：

```
entering: a
---
leaving: a
```

我们可以充分利用这个特点，即被推迟函数的实参在`defer`执行时才会求值。跟踪go程可针对反跟踪go程设置实参。

```go
func trace(s string) string {
    fmt.Println("entering:", s)
    return s
}

func un(s string) {
    fmt.Println("leaving:", s)
}

func a() {
    defer un(trace("a"))
    fmt.Println("in a")
}

func b() {
    defer un(trace("b"))
    fmt.Println("in b")
    a()
}

func main() {
    b()
}
```

输出如下：

```
entering: b
in b
entering: a
in a
leaving: a
leaving: b
```



<br>
<br>



### 数据

Data


<br>


#### new

go有两种分配原语，即内建函数`new`和`make`。`new`用来分配内存，但与其它同名函数不同，它不会初始化内存，只会将内存置零(zero)。`new(T)`会为类型T的新项分配已置零的内存控制，并返回它的地址，也即是类型`*T`的值。用go的术语，它返回一个指针，该指针指向新分配的类型为T的零值。

既然`new`返回的内存已置零，那么当你设计数据结构时，每种类型的零值就不必进一步初始化，这意味着该数据结构的使用者只需用`new`创建一个新的对象就能正常工作。

零值属性有各种好处，考虑以下声明：

```go
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
```

`SyncedBuffer`类型的值也是在声明时就分配好内存就绪了。后续代码中， p和v无需进一步处理即可正确工作。

```go
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```


<br>
<br>


#### 构造函数与复合字面

Constructors and composite literals

有时零值还不够好，这时就需要一个初始化构造函数。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := new(File)
    f.fd = fd
    f.name = name
    f.dirinfo = nil
    f.nepipe = 0
    return f
}
```

这里显得代码过于冗长。我们可通过复合字面来简化它， 该表达式在每次求值时都会创建新的实例。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := File{fd, name, nil, 0}
    return &f
}
```

请注意，返回一个局部变量的地址完全没有问题，这点与C不同。该局部变量对应的数据 在函数返回后依然有效。实际上，每当获取一个复合字面的地址时，都将为一个新的实例分配内存， 因此我们可以将上面的最后两行代码合并：

```go
return &File{fd, name, nil, 0}
```

复合字面的字段必须按顺序全部列出。但如果以`k:v`对的形式明确地标出元素，初始化字段时就可以按任何顺序出现，未给出的字段值将赋予零值。 因此，我们可以用如下形式：

```go
return &File{fd: fd, name: name}
```


<br>
<br>


#### make

再回到内存分配上来。不同于`new`，`make`只用于创建切片、映射和信道，并返回类型为T的一个已初始化的值。出现这种差异的原因在于，这三种类型本质上为引用数据类型，它们在使用前必须初始化。对于切片、映射和信道，`make`用于初始化其内部的数据结构并准备好将要使用的值。

```go
// ew([]int) 会返回一个指向新分配的，已置零的切片结构， 即一个指向 nil 切片值的指针
// 会分配一个具有100个int的数组空间，接着创建一个长度为10， 容量为100并指向该数组中前10个元素的切片结构
make([]int, 10, 100)
```

<br>

`new`和`make`的区别：

```go
var p *[]int = new([]int)       // allocates slice structure; *p == nil; rarely useful
var v  []int = make([]int, 100) // the slice v now refers to a new array of 100 ints

// Unnecessarily complex:
var p *[]int = new([]int)
*p = make([]int, 100, 100)

// Idiomatic:
v := make([]int, 100)
```

请记住，`make`只适用于映射、切片和信道且不返回指针。若要获得明确的指针， 请使用`new`分配内存。


<br>
<br>


#### Arrays

在详细规划内存布局时，数组非常有用，有时还能避免过多的内存分配，但它们主要用作切片的构件。

Go中数组：

- 数组是值。将一个数组赋予另一个数组会复制其所有元素。
- 若将某个数组传入某个函数，它将接收到该数组的一份副本而非指针。
- 数组的大小是其类型的一部分。类型`[10]int`和`[20]int`是不同的。

数组为值的属性很有用，但代价高昂。若你想要C那样的行为和效率，你可以传递一个指向该数组的指针。但这并不是Go的习惯用法，切片才是。

```go
func Sum(a *[3]float64) (sum float64) {
    for _, v := range *a {
        sum += v
    }
    return
}

array := [...]float64{7.0, 8.5, 9.1}
x := Sum(&array)  // Note the explicit address-of operator
```


<br>
<br>


#### Slices

切片通过对数组进行封装，为数据序列提供了更通用、强大而方便的接口。 除了矩阵变换这类需要明确维度的情况外，Go中的大部分数组编程都是通过切片来完成的。

切片保存了对底层数组的引用，若你将某个切片赋予另一个切片，它们会引用同一个数组。只要切片不超出底层数组的限制，它的长度就是可变的。尽管`append`可修改切片的元素，但切片自身（其运行时数据结构包含指针、长度和容量）是通过值传递的。


<br>
<br>


#### 二维切片

Two-dimensional slices

Go的数组和切片都是一维的。要创建等价的二维数组或切片，就必须定义一个数组的数组， 或切片的切片。像下面这样：

```go
type Transform [3][3]float64  // A 3x3 array, really an array of arrays.
type LinesOfText [][]byte     // A slice of byte slices.
```


<br>
<br>


#### Maps

映射是方便而强大的内建数据结构，它可以关联不同类型的值。其键可以是任何相等性操作符支持的类型， 如整数、浮点数、复数、字符串、指针、接口（只要其动态类型支持相等性判断）、结构以及数组。 切片不能用作映射键，因为它们的相等性还未定义。与切片一样，映射也是引用类型。 若将映射传入函数中，并更改了该映射的内容，则此修改对调用者同样可见。

```go
var timeZone = map[string]int{
    "UTC":  0*60*60,
    "EST": -5*60*60,
    "CST": -6*60*60,
    "MST": -7*60*60,
    "PST": -8*60*60,
}
```

赋值和获取映射值的语法类似于数组，不同的是映射的索引不必为整数：

```go
offset := timeZone["EST"]
```

有时你需要区分某项是不存在还是其值为零值。可以使用多重赋值的形式来分辨这种情况。

```go
var seconds int
var ok bool
seconds, ok = timeZone[tz]
```

若仅需判断映射中是否存在某项而不关心实际的值，可使用空白标识符(`_`)来代替该值的一般变量。

```go
_, present := timeZone[tz]
```

要删除映射中的某项，可使用内建函数 delete，它以映射及要被删除的键为实参。 即便对应的键不在该映射中，此操作也是安全的。

```go
delete(timeZone, "PDT")  // Now on Standard Time
```


<br>
<br>


#### Printing

Go采用的格式化打印风格和C的`printf`族类似，但却更加丰富而通用。这些函数位于`fmt`包中，且函数名首字母均为大写：如`fmt.Printf`、`fmt.Fprintf`，`fmt.Sprintf`等。

```go
fmt.Printf("Hello %d\n", 23)
fmt.Fprint(os.Stdout, "Hello ", 23, "\n")
fmt.Println("Hello", 23)
fmt.Println(fmt.Sprint("Hello ", 23))
```


<br>
<br>


#### append

内建函数`append`像这个：

```go
func append(slice []T, elements ...T) []T
```



<br>
<br>



### 初始化

Initialization

尽管从表面上看，Go的初始化过程与C或C++并不算太大，但它确实更为强大。 在初始化过程中，不仅可以构建复杂的结构，还能正确处理不同包对象间的初始化顺序。


<br>


#### 常量

Constants

Go中的常量就是不变量。它们在编译时创建，即便它们可能是函数中定义的局部变量。 常量只能是数字、字符（符文）、字符串或布尔值。由于编译时的限制， 定义它们的表达式必须也是可被编译器求值的常量表达式。


<br>
<br>


#### 变量

变量的初始化与常量类似，但其初始值也可以是在运行时才被计算的一般表达式。

```go
var (
    home   = os.Getenv("HOME")
    user   = os.Getenv("USER")
    gopath = os.Getenv("GOPATH")
)
```


<br>
<br>


#### init

The init function

最后，每个源文件都可以通过定义自己的无参数`init`函数来设置一些必要的状态。而它的结束就意味着初始化结束： 只有该包中的所有变量声明都通过它们的初始化器求值后`init`才会被调用， 而那些`init`只有在所有已导入的包都被初始化后才会被求值。

除了那些不能被表示成声明的初始化外，init 函数还常被用在程序真正开始执行前，检验或校正程序的状态。

```go
Besides initializations that cannot be expressed as declarations, a common use of init functions is to verify or repair correctness of the program state before real execution begins.

func init() {
    if user == "" {
        log.Fatal("$USER not set")
    }
    if home == "" {
        home = "/home/" + user
    }
    if gopath == "" {
        gopath = home + "/go"
    }
    // gopath may be overridden by --gopath flag on command line.
    flag.StringVar(&gopath, "gopath", gopath, "override default GOPATH")
}
```



<br>
<br>



### 方法

Methods

<br>

#### 指针与值

Pointers vs. Values

以指针或值为接收者的区别在于：值方法可通过指针和值调用， 而指针方法只能通过指针来调用。



<br>
<br>



### 接口和其它类型

<br>

#### Interfaces

Go中的接口为指定对象的行为提供了一种方法：如果某样东西可以完成这个， 那么它就可以用在这里。

每种类型都能实现多个接口。


<br>
<br>


#### 类型转换

Conversions


<br>
<br>


#### 接口转换与类型断言

Interface conversions and type assertions

类型选择是类型转换的一种形式：它接受一个接口，在选择中根据其判断选择对应的情况， 并在某种意义上将其转换为该种类型。

```go
type Stringer interface {
    String() string
}

var value interface{} // Value provided by caller.
switch str := value.(type) {
case string:
    return str
case Stringer:
    return str.String()
}
```

类型断言接受一个接口值， 并从中提取指定的明确类型的值。


<br>
<br>


#### 通用性

Generality

若某种现有的类型仅实现了一个接口，且除此之外并无可导出的方法，则该类型本身就无需导出。 仅导出该接口能让我们更专注于其行为而非实现，其它属性不同的实现则能镜像该原始类型的行为。 这也能够避免为每个通用接口的实例重复编写文档。

在这种情况下，构造函数应当返回一个接口值而非实现的类型。


<br>
<br>


#### 接口和方法

Interfaces and methods

由于几乎任何类型都能添加方法，因此几乎任何类型都能满足一个接口。

```go
// 一个很直观的例子就是 http 包中定义的 Handler 接口。任何实现了 Handler 的对象都能够处理HTTP请求
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}
```


<br>
<br>


### 空白标识符

The blank identifier

空白标识符(`_`)可被赋予或声明为任何类型的任何值，而其值会被无害地丢弃。它有点像Unix中的`/dev/null`文件：它表示只写的值，在需要变量但不需要实际值的地方用作占位符。

<br>

#### 多重赋值中的空白标识符

The blank identifier in multiple assignment

`for range`循环中对空表标识符的用法是一种具体情况，更一般的情况即为多重赋值。

```go
if _, err := os.Stat(path); os.IsNotExist(err) {
	fmt.Printf("%s does not exist\n", path)
}
```


<br>
<br>


#### 未使用的导入和变量

Unused imports and variables

若导入某个包或声明某个变量而不使用它就会产生错误。未使用的包会让程序膨胀并拖慢编译速度， 而已初始化但未使用的变量不仅会浪费计算能力，还有可能暗藏着更大的Bug。

要让编译器停止关于未使用导入的抱怨，需要空白标识符来引用已导入包中的符号。

```go
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

var _ = fmt.Printf // For debugging; delete when done.
var _ io.Reader    // For debugging; delete when done.

func main() {
    fd, err := os.Open("test.go")
    if err != nil {
        log.Fatal(err)
    }
    // TODO: use fd.
    _ = fd
}
```


<br>
<br>


#### 为副作用而导入

Import for side effect

有时导入某个包只是为了其副作用， 而没有任何明确的使用。只为了其副作用来导入该包， 只需将包重命名为空白标识符：

```go
import _ "net/http/pprof"
```

这种导入格式能明确表示该包是为其副作用而导入的，因为没有其它使用该包的可能： 在此文件中，它没有名字。（若它有名字而我们没有使用，编译器就会拒绝该程序。）


<br>
<br>


#### 接口检查

Interface checks

一个类型无需显式地声明它实现了某个接口。取而代之，该类型只要实现了某个接口的方法， 其实就实现了该接口。在实践中，大部分接口转换都是静态的，因此会在编译时检测。

若只需要判断某个类型是否是实现了某个接口，而不需要实际使用接口本身 （可能是错误检查部分），就使用空白标识符来忽略类型断言的值：

```go
if _, ok := val.(json.Marshaler); ok {
    fmt.Printf("value %v of type %T implements json.Marshaler\n", val, val)
}
```

在这种结构中出现空白标识符，即表示该声明的存在只是为了类型检查。 不过请不要为满足接口就将它用于任何类型。作为约定， 仅当代码中不存在静态类型转换时才能这种声明，毕竟这是种罕见的情况。



<br>
<br>



### 内嵌

Embedding

Go并不提供典型的，类型驱动的子类化概念，但通过将类型内嵌到结构体或接口中， 它就能借鉴部分实现。



<br>
<br>



### 并发

Concurrency

<br>

#### 通过通信共享内存

Share by communicating

并发编程是个很大的话题。这里只讨论一些go特有的东西。

在并发编程中，为实现对共享变量的正确访问需要精确的控制，这在多数环境下都很困难。 Go语言另辟蹊径，它将共享的值通过信道传递，实际上，多个独立执行的线程从不会主动共享。 在任意给定的时间点，只有一个Go程能够访问该值。数据竞争从设计上就被杜绝了。

为了提倡这种思考方式，我们将它简化为一句口号： **不要通过共享内存来通信，而应通过通信来共享内存(Do not communicate by sharing memory; instead, share memory by communicating)。**

这种方法意义深远。例如，引用计数通过为整数变量添加互斥锁来很好地实现。 但作为一种高级方法，通过信道来控制访问能够让你写出更简洁，正确的程序。


<br>
<br>


#### go程

Goroutines

称它为**GO程**是因为现有的术语——线程(threads), 协程(coroutines), 进程(process)无法准确表达它的含义。Go程具有简单的模型：它是与其它Go程并发运行在同一地址空间的函数。它是轻量级的， 所有小号几乎就只有栈空间的分配。而且栈最开始是非常小的，所以它们很廉价， 仅在需要时才会随着堆空间的分配（和释放）而变化。

Go程在多线程操作系统上可实现多路复用，因此若一个线程阻塞，比如说等待I/O， 那么其它的线程就会运行。Go程的设计隐藏了线程创建和管理的诸多复杂性。

```go
// 在函数或方法前添加go关键字能够在新的Go程中调用它。当调用完成后， 该Go程也会安静地退出
// 效果有点像Unix Shell中的 & 符号，它能让命令在后台运行
go list.Sort() // run list.Sort concurrently; don't wait for it.
```

函数字面在Go程调用中非常有用。

```go
func Announce(message string, delay time.Duration) {
    go func() {
        time.Sleep(delay)
        fmt.Println(message)
    }()  // Note the parentheses - must call the function.
}
```

在Go中，函数字面都是闭包(closures)：其实现在保证了函数内引用变量的生命周期与函数的活动时间相同。这些函数没什么实用性，因为它们没有实现完成时的信号处理。因此，我们需要信道。


<br>
<br>


#### 信道

Channels

信道与映射一样，也需要通过`make`来分配内存，其结果充当了对底层数据结构的引用。若提供了一个可选的整数形参，它就会为该信道设置缓冲区大小。默认值是零，表示不带缓冲(unbuffered)的或同步(synchronous)的信道。

```go
ci := make(chan int)            // unbuffered channel of integers
cj := make(chan int, 0)         // unbuffered channel of integers
cs := make(chan *os.File, 100)  // buffered channel of pointers to Files
```

无缓冲信道在通信时会同步交换数据，它能确保（goroutine）计算处于确定状态。


信道有很多惯用方法。

```go
c := make(chan int)  // Allocate a channel.
// Start the sort in a goroutine; when it completes, signal on the channel.
go func() {
    list.Sort()
    c <- 1  // Send a signal; value does not matter.
}()
doSomethingForAWhile()
<-c   // Wait for sort to finish; discard sent value.
```

接收者在收到数据前会一直阻塞。若信道是不带缓冲的，那么在接收者收到值前， 发送者会一直阻塞；若信道是带缓冲的，则发送者仅在值被复制到缓冲区前阻塞； 若缓冲区已满，发送者会一直等待直到某个接收者取出一个值为止。

带缓冲的信道可被用作信号量，例如限制吞吐量。

回到编写服务器的一般问题上来。另一种管理资源的好方法就是启动固定数量的`handle`Go程，一起从请求信道中读取数据。Go程的数量限制了同时调用`process`的数量。Serve同样会接收一个通知退出的信道， 在启动所有Go程后，它将阻塞并暂停从信道中接收消息。

```go
func handle(queue chan *Request) {
    for r := range queue {
        process(r)
    }
}

func Serve(clientRequests chan *Request, quit chan bool) {
    // Start handlers
    for i := 0; i < MaxOutstanding; i++ {
        go handle(clientRequests)
    }
    <-quit  // Wait to be told to exit.
}
```


<br>
<br>


#### 信道中的信道

Channels of channels

Go最重要的特性就是信道是一等值，它可以被分配并像其它值到处传递。 这种特性通常被用来实现安全(safe)、并行(parallel)的多路分解(demultiplexing)。


<br>
<br>


#### 并行化

Parallelization

这些设计的另一个应用是在多CPU核心上实现并行计算。如果计算过程能够被分为几块 可独立执行的过程，它就可以在每块计算结束时向信道发送信号，从而实现并行处理。

```go
const numCPU = 4 // number of CPU cores

func (v Vector) DoAll(u Vector) {
    c := make(chan int, numCPU)  // Buffering optional but sensible.
    for i := 0; i < numCPU; i++ {
        go v.DoSome(i*len(v)/numCPU, (i+1)*len(v)/numCPU, u, c)
    }
    // Drain the channel.
    for i := 0; i < numCPU; i++ {
        <-c    // wait for one task to complete
    }
    // All done.
}
```

目前Go运行时的实现默认并不会并行执行代码，它只为用户层代码提供单一的处理核心。 任意数量的Go程都可能在系统调用中被阻塞，而在任意时刻默认只有一个会执行用户层代码。 它应当变得更智能，而且它将来肯定会变得更智能。但现在，若你希望CPU并行执行， 就必须告诉运行时你希望同时有多少Go程能执行代码。除了为CPU数量创建一个创建，还有两种方法：

```go
// 1
var numCPU = runtime.NumCPU()

// 2
var numCPU = runtime.GOMAXPROCS(0)
```

<br>

注意不要混淆并发(concurrency)和并行(parallelism)的概念。并发是用可独立执行的组件构造程序的方法， 而并行则是为了效率在多CPU上平行地进行计算。

尽管Go的并发特性能够让某些问题更易构造成并行计算， 但Go仍然是种并发而非并行的语言，且Go的模型并不适合所有的并行问题。


<br>
<br>


#### 泄露的缓冲区

A leaky buffer

并发编程的工具甚至能很容易地表达非并发的思想。

这里有个提取自RPC包的例子。 客户端Go程从某些来源，可能是网络中循环接收数据。为避免分配和释放缓冲区， 它保存了一个空闲链表，使用一个带缓冲信道表示。若信道为空，就会分配新的缓冲区。 一旦消息缓冲区就绪，它将通过serverChan被发送到服务器。

```go
var freeList = make(chan *Buffer, 100)
var serverChan = make(chan *Buffer)

func client() {
    for {
        var b *Buffer
        // Grab a buffer if available; allocate if not.
        select {
        case b = <-freeList:
            // Got one; nothing more to do.
        default:
            // None free, so allocate a new one.
            b = new(Buffer)
        }
        load(b)              // Read next message from the net.
        serverChan <- b      // Send to server.
    }
}
```

服务器从客户端循环接收每个消息，处理它们，并将缓冲区返回给空闲列表。

```go
func server() {
    for {
        b := <-serverChan    // Wait for work.
        process(b)
        // Reuse buffer if there's room.
        select {
        case freeList <- b:
            // Buffer on free list; nothing more to do.
        default:
            // Free list full, just carry on.
        }
    }
}
```

客户端试图从freeList中获取缓冲区；若没有缓冲区可用， 它就将分配一个新的。服务器将b放回空闲列表freeList中直到列表已满，此时缓冲区将被丢弃，并被垃圾回收器回收。依靠带缓冲的信道和垃圾回收器的记录， 我们仅用短短几行代码就构建了一个可能导致缓冲区槽位泄露的空闲列表。



<br>
<br>


### 错误

error

库例程通常需要向调用者返回某种类型的错误提示。之前提到过，Go语言的多值返回特性， 使得它在返回常规的值时，还能轻松地返回详细的错误描述。

按照约定，错误的类型通常为`error`，这是一个内建的简单接口。

```go
type error interface {
    Error() string
}
```

库的编写者通过更丰富的底层模型可以轻松实现这个接口，这样不仅能看见错误， 还能提供一些上下文。

```go
// PathError records an error and the operation and
// file path that caused it.
type PathError struct {
    Op string    // "open", "unlink", etc.
    Path string  // The associated file.
    Err error    // Returned by the system call.
}

func (e *PathError) Error() string {
    return e.Op + " " + e.Path + ": " + e.Err.Error()
}


// 生成的错误信息例子
// open /etc/passwx: no such file or directory
```

错误字符串应尽可能地指明它们的来源，例如产生该错误的包名前缀。若调用者关心错误的完整细节，可使用类型选择或者类型断言来查看特定错误，并抽取其细节。


<br>
<br>


#### Panic

向调用者报告错误的一般方式就是将`error`作为额外的值返回。但如果错误时不可恢复的呢？有时程序就是不能继续运行。为此，我们提供了内建的`panic`函数，它会产生一个运行时错误并终止程序。该函数接受一个任意类型的实参（一般为字符串），并在程序终止时打印。 它还能表明发生了意料之外的事情，比如从无限循环中退出了。

```go
// A toy implementation of cube root using Newton's method.
func CubeRoot(x float64) float64 {
    z := x/3   // Arbitrary initial value
    for i := 0; i < 1e6; i++ {
        prevz := z
        z -= (z*z*z-x) / (3*z*z)
        if veryClose(z, prevz) {
            return z
        }
    }
    // A million iterations has not converged; something is wrong.
    panic(fmt.Sprintf("CubeRoot(%g) did not converge", x))
}
```

实际的库函数应避免`panic`。若问题可以被屏蔽或解决， 最好就是让程序继续运行而不是终止整个程序。


<br>
<br>


#### Recover

当`panic`被调用后，程序将立刻终止当前函数的执行，并开始回溯Go程的栈，运行任何被推迟的函数。 若回溯到达Go程栈的顶端，程序就会终止。不过我们可以用内建的`recover`函数来重新或来取回Go程的控制权限并使其恢复正常执行。

调用`recover`将停止回溯过程，并返回传入`panic`的实参。 由于在回溯时只有被推迟函数中的代码在运行，因此`recover`只能在被推迟的函数中才有效。

recover的一个应用就是在服务器中终止失败的Go程而无需杀死其它正在执行的Go程。

```go
func server(workChan <-chan *Work) {
    for work := range workChan {
        go safelyDo(work)
    }
}

func safelyDo(work *Work) {
    defer func() {
        if err := recover(); err != nil {
            log.Println("work failed:", err)
        }
    }()
    do(work)
}
```

在此例中，若`do(work)`触发了Panic，其结果就会被记录， 而该Go程会被干净利落地结束，不会干扰到其它Go程。我们无需在推迟的闭包中做任何事情， `recover`会处理好这一切。

通过恰当地使用恢复模式，do函数（及其调用的任何代码）可通过调用 panic 来避免更坏的结果。我们可以利用这种思想来简化复杂软件中的错误处理。 让我们看看regexp包的理想化版本，它会以局部的错误类型调用 panic 来报告解析错误。以下是一个error类型的 Error方法和一个Compile函数的定义：

```go
// Error is the type of a parse error; it satisfies the error interface.
type Error string
func (e Error) Error() string {
    return string(e)
}

// error is a method of *Regexp that reports parsing errors by
// panicking with an Error.
func (regexp *Regexp) error(err string) {
    panic(Error(err))
}

// Compile returns a parsed representation of the regular expression.
func Compile(str string) (regexp *Regexp, err error) {
    regexp = new(Regexp)
    // doParse will panic if there is a parse error.
    defer func() {
        if e := recover(); e != nil {
            regexp = nil    // Clear return value.
            err = e.(Error) // Will re-panic if not a parse error.
        }
    }()
    return regexp.doParse(str), nil
}
```

顺便一提，这种重新触发Panic的惯用法会在产生实际错误时改变Panic的值。 然而，不管是原始的还是新的错误都会在崩溃报告中显示，因此问题的根源仍然是可见的。 这种简单的重新触发Panic的模型已经够用了，毕竟他只是一次崩溃。 但若你只想显示原始的值，也可以多写一点代码来过滤掉不需要的问题，然后用原始值再次触发Panic。



<br>
<br>



### A web server

让我们以一个完整的Go程序作为结束吧，一个Web服务器。该程序其实只是个Web服务器的重用。

```go
package main

import (
    "flag"
    "html/template"
    "log"
    "net/http"
)

var addr = flag.String("addr", ":1718", "http service address") // Q=17, R=18

var templ = template.Must(template.New("qr").Parse(templateStr))

func main() {
    flag.Parse()
    http.Handle("/", http.HandlerFunc(QR))
    err := http.ListenAndServe(*addr, nil)
    if err != nil {
        log.Fatal("ListenAndServe:", err)
    }
}

func QR(w http.ResponseWriter, req *http.Request) {
    templ.Execute(w, req.FormValue("s"))
}

const templateStr = `
<html>
<head>
<title>QR Link Generator</title>
</head>
<body>
{{if .}}
<img src="http://chart.apis.google.com/chart?chs=300x300&cht=qr&choe=UTF-8&chl={{.}}" />
<br>
{{.}}
<br>
<br>
{{end}}
<form action="/" name=f method="GET">
    <input maxLength=1024 size=70 name=s value="" title="Text to QR Encode">
    <input type=submit value="Show QR" name=qr>
</form>
</body>
</html>
`
```

Go语言强大到能让很多事情以短小精悍的方式解决。





<br>
<br>
<br>





## 调试

Diagnostics: <https://golang.org/doc/diagnostics.html>

总结工具和方法来诊断Go程序

<br>

### 介绍

Go生态提供了一套API和工具来诊断go程序的逻辑和性能问题。本章总结了可用的工具，帮助用户去选择正确的工具来解决问题。

调试方案可分为一下几组：

- **Profiling**： 分析工具分析go程序的复杂性和成本，如内存使用和调用函数的频率，以确定go程序的昂贵的部分；
- **Tracing**： 追踪是分析整个延迟和调用或用户请求的生命周期的一种方法；
- **Debugging**： 调试可以让我们暂停go程序，检查并执行。程序的状态和流程可通过调试进行验证；
- **Runtime statistics and events**： 收集和分析运行时状态和事件，提供go程序运行状态的高度概括。


<br>
<br>


### 分析

Profiling

对于识别昂贵的或频繁调用的代码部分，分析很有用。go runtime通过`pprof`可视化工具以格式化形式提供了分析数据。可通过`go test`或`net/http/pprof`包来收集分析数据。用户需要在代码顶级路径使用`pprof`工具来收集分析路径。

由`runtime/pprof`包预分析：

- **cpu**: cpu porfile，报告程序花费的CPU时间。
- **heap**： heap profile，报告内存分配样本，监控当前和历史的内存使用，并检查内存泄漏。
- **threadcreate**： thread profile，报告程序的操作系统的线程创建部分。
- **goroutine**： goroutine profile，报告当前所有goroutine的栈追踪(stack trace)。
- **block**： block profile，报告goroutine在哪里等待同步原语(synchronization primitives)阻塞。此功能默认关闭，使用`runtime.SetBlockProfileRate`开启。
- **mutex**： mutex profile，报告锁的争用情况。当你认为由于互斥锁争用，CPU没有得到充分利用时，使用此功能。此功能默认关闭，使用`runtime.SetMutexProfileFraction`启用。


<br>
<br>


### 追踪

Tracing

追踪是一种来分析整个调用链的生命周期的延迟的方法。go提供了`golang.org/x/net/trace`包作为每个go节点的最小化追踪后端，并使用一个简单的面板来提供一个小型的仪器库。go还提供了一个可执行的追踪程序在内部追踪运行时事件。

追踪使我们能够：

- 在go程序内工具和分析应用延迟。
- 衡量一个长链调用的特定调用的开销。
- 计算使用率和性能优化。

go的生态提供了多种追踪库。


<br>
<br>


### 调试

Debugging

调试是识别一个程序行为不端的过程。调试器让我们了解程序的执行流程和当前状态。有几种调试风格，本章节将仅聚焦于一个调试器附加到一个程序和核心转储(core dump)调试。

go用户大多使用以下调试器：

- **(Delve)[https://github.com/go-delve/delve]**： Delve是一个go lang调试器。它支持go runtime和内建类型。它正努力成为一个go程序的全功能可靠的调试器。
- **(GDB)[https://golang.org/doc/gdb]**： go通过标准的go编译器和Gccgo提供了GDB支持。尽管GDB可以用来调试go程序，但这不理想，可能导致混乱。


<br>
<br>


### 运行时统计数据和事件

Runtime statistics and events

运行时(runtime)提供了统计信息和内部事件的报告，为用户在运行时级别诊断性能和利用率的问题。

用户可以监控这些数据，以便于更好地了解go程序的总体运行状况和性能。一些常用的监控统计数据和状态：

- `runtime.ReadMemStats`： 报告与堆分配(heap allocation)和垃圾回收(garbage collection)相关的指标。内存统计数据对监控进程消耗了多少内存资源是有用的，进程是否能很好地利用内存，并捕捉到内存泄漏。
- `debug.ReadGCStats`： 阅读关于垃圾回收的统计数据。查看多少资源都花在了垃圾回收阶段也是很有用的。它还报告垃圾回收暂停和暂停事件百分数的时间线。
- `debug.Stack`： 返回当前的栈追踪。栈追踪对于查看有多少goroutine正在运行，它们在做什么，它们是否阻塞很有用。
- `debug.WriteHeapDump`： 中止所有goroutine的执行，并允许转存(dump)堆(heap)到文件。一个堆转存是go程序在特定时间内存的快照。它包含所有分配的对象，以及goroutine, finalizers...
- `runtime.NumGoroutine`： 返回当前的goroutine数量。该值可以被监测、以了解是否有足够的goroutine被利用，或检测goroutine泄漏。


<br>
<br>


#### 执行追踪

Execution tracer

go使用runtime execution tracer来捕获广泛的运行时事件。调度、系统调用、垃圾回收、堆大小和其它收集的事件。执行追踪器是一个检测延迟和使用率问题的工具。你可以检查CPU如何利用，网络或系统调用时，抢占对goroutine的原因。

追踪器对这些有用：

- 理解你的goroutine如何执行
- 理解一些核心(core)的运行时事件，如垃圾回收
- 确定不佳的并行执行

<br>

然而，它不是很大用于识别热点（如分析内存溢出或CPU使用的原因）。使用分析工具而不是先定位它们。

详细信息查看`go tool trace`，来收集和分析运行时追踪。


<br>
<br>


#### GODEBUG

如果`GODEBUG`环境变量相应地设置，运行时也会发出事件和信息。

- `GODEBUG=gctrace=1`： 在每个收集中打印垃圾回收器事件，汇总内存收集量和停顿的长度。
- `GODEBUG=schedtrace=X`： 每个x毫秒打印调度事件。

GODEBUG环境变量可用于在标准库和运行时中禁用指令集扩展。

- `GODEBUG=cpu.all=off`： 禁止使用所有可选的扩展指令集。
- `GODEBUG=cpu.extension=off`： 禁止从指定的指令集扩展中使用指令。





<br>
<br>
<br>





## FAQ

docs: <https://golang.org/doc/faq>

有关go的常见问答。




<br>
<br>
<br>




## Go wiki

docs: <https://github.com/golang/go/wiki>

由GO社区维护的wiki。








































<br>
<br>

---

<br>
<br>



























# 参考

References


<br>
<br>


## 包

Package Documentation: <https://golang.org/pkg/>

Go标准库文档。



<br>
<br>
<br>



## 命令

Command Documentation: <https://golang.org/doc/cmd>

Go工具文档。



<br>
<br>
<br>



## 语言规范

Language Specification: <https://golang.org/ref/spec>

官方Go语言规范。



<br>
<br>



## 内存模型

The Go Memory Model: <https://golang.org/ref/mem>






































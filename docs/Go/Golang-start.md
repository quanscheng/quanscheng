---
sidebar_position: 1
---

# golang start

[toc]

# 基础

## 包

- 每个 Go 程序都是由包构成的。
- 程序从 `main` 包开始运行。
- 按照约定，包名与导入路径的最后一个元素一致。例如，`import "math/rand"` 包中的源码均以 `package rand` 语句开始 `rand.Intn(10)`

## 导入

- 此代码用圆括号组合了导入，这是“分组”形式的导入语句。

- ```go
  import "fmt"
  import "math"
  // 等价于
  import (
  	"fmt"
  	"math"
  )
  ```

## 导出名

- 在 Go 中，如果一个名字以**<u>大写字母开头</u>**，那么它就是<u>**已导出**</u>的。例如，`Pizza` 就是个已导出名，`Pi` 也同样，它导出自 `math` 包。

- 在导入一个包时，你只能引用其中已导出的名字。任何“未导出”的名字在该包外均无法访问。

## 函数

- 函数可以没有参数或接受多个参数。

- ```go
  func add(x int, y int) int {
  	return x + y
  }
  ```

- 当连续两个或多个函数的已命名形参类型相同时，除最后一个类型以外，其它都可以省略。

- ```go
  func add(x, y int) int {
  	return x + y
  }
  ```

- 多值返回 : 函数可以返回任意数量的返回值。

- ```go
  func swap(x, y string) (string, string) {
  	return y, x
  }
  ```

- Go 的返回值可被命名，它们会被视作定义在函数顶部的变量。返回值的名称应当具有一定的意义，它可以作为文档使用。没有参数的 `return` 语句返回已命名的返回值。也就是 `直接` 返回。

- ```go
  func split(sum int) (x, y int) {
  	x = sum * 2 / 3
  	y = sum - x
  	return
  }
  // 相当于隐式返回了 x, y
  func main() {
  	fmt.Println(split(12))
  }
  // 8 , 4
  ```

## 变量

- `var` 语句用于声明一个变量列表，跟函数的参数列表一样，类型在最后。

- ```go
  var c, python, java bool // 布尔值, 默认false

  func main() {
  	var i int // 整数 , 默认 0
  	fmt.Println(i, c, python, java)
  }
  // 0 false false false
  ```

- 初始化 : 变量声明可以包含初始值，每个变量对应一个。如果初始化值已存在，则可以省略类型；变量会从初始值中获得类型。

- 短变量声明 : 在函数中，简洁赋值语句 `:=` 可在类型明确的地方代替 `var` 声明。<u>函数外的每个语句</u>都必须以关键字开始（`var`, `func` 等等），因此 `:=` 结构**不能在函数外使用**。

## 基本类型

- Go 的基本类型有

- ```go
  bool

  string

  int  int8  int16  int32  int64
  uint uint8 uint16 uint32 uint64 uintptr

  byte // uint8 的别名

  rune // int32 的别名
      // 表示一个 Unicode 码点

  float32 float64

  complex64 complex128
  ```

- 同导入语句一样，变量声明也可以“分组”成一个语法块。

- ```go
  var (
  	ToBe   bool       = false
  	MaxInt uint64     = 1<<64 - 1
  	z      complex128 = cmplx.Sqrt(-5 + 12i)
  )
  ```

- 零值 : 没有明确初始值的变量声明会被赋予它们的 **零值**。

- ```go
  零值是：

  数值类型为 0，
  布尔类型为 false，
  字符串为 ""（空字符串）。
  ```

- 类型转换 : 表达式 `T(v)` 将值 `v` 转换为类型 `T`。

- ```go
  var i int = 42
  var f float64 = float64(i)
  var u uint = uint(f)
  // 等价于
  i := 42
  f := float64(i)
  u := uint(f)
  ```

- 类型推导 : 在声明一个变量而不指定其类型时（即使用不带类型的 `:=` 语法或 `var =` 表达式语法），变量的类型由右值推导得出。

- ```go
  var i int
  j := i // j 也是一个 int
  // 不过当右边包含未指明类型的数值常量时，新变量的类型就可能是 int, float64 或 complex128 了，这取决于常量的精度：
  i := 42           // int
  f := 3.142        // float64
  g := 0.867 + 0.5i // complex128
  ```

- 常量 :

  - 常量的声明与变量类似，只不过是使用 `const` 关键字。
  - 常量可以是字符、字符串、布尔值或数值。
  - 常量不能用 `:=` 语法声明。

- 数值常量 : 数值常量是高精度的 **值**。一个未指定类型的常量由上下文来决定其类型。

## 流程控制

### for

Go 只有<b>一种</b>循环结构：for 循环。

基本的 for 循环由三部分组成，它们用分号隔开：

- 初始化语句：在第一次迭代前执行
- 条件表达式：在每次迭代前求值
- 后置语句：在每次迭代的结尾执行

初始化语句通常为一句短变量声明，该变量声明仅在 for 语句的作用域中可见。

一旦条件表达式的布尔值为 false，循环迭代就会终止。

> 注意：和 C、Java、JavaScript 之类的语言不同，Go 的 for 语句后面的三个构成部分外没有小括号， 大括号 { } 则是必须的。

```go
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

初始化语句和后置语句是可选的。

```go
func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```

### for 是 Go 中的 “while”

此时你可以去掉分号达到 while 的效果，因为 C 的 while 在 Go 中叫做 for。

```go
func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

```

### 无限循环

如果省略循环条件，该循环就不会结束，因此无限循环可以写得很紧凑。

```go
func main() {
	for {
	}
}
```

### if 语句

Go 的 if 语句与 for 循环类似，表达式外无需小括号 ( ) ，而大括号 { } 则是必须的。

```go
func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}
// 1.4142135623730951 2i
```
# Go基础语法

## 第一个Go程序

```go
package main

import "fmt"

func main(){
    fmt.Println("Hello World!")
}
```

* 只有在main包下的`main`函数才能运行

* 使用import导入包

  * 使用import导入单个包

    ```go
    import "package"
    ```

  * 使用import导入多个包

    ```go
    import (
        "package1"
        "package2"
    )
    ```

  * 使用import导入包，但是**包中的方法不使用**时编译会报错

    ![image-20220713123828884](G:\桌面\笔记\Go\assets\image-20220713123828884.png)

  * 需要使用包的init函数，但是不使用包中的方法，使用`_`在包前，表示匿名导入，只需要包的init函数

    ```go
    import  _"fmt"
    ```

    ![image-20220713124041001](G:\桌面\笔记\Go\assets\image-20220713124041001.png)

* `package`可以和文件夹不同名

* 在同一文件夹的.go文件需要属于同一个`package`，不然会报错

  ![image-20220713124340760](G:\桌面\笔记\Go\assets\image-20220713124340760.png)

![image-20220713124350680](G:\桌面\笔记\Go\assets\image-20220713124350680.png)

![image-20220713124359388](G:\桌面\笔记\Go\assets\image-20220713124359388-16576874566471.png)

![image-20220713124429706](G:\桌面\笔记\Go\assets\image-20220713124429706.png)

* `{}`要和函数名放在一行，否者会报错(golang的格式检查非常严格)

  ![image-20220713130740806](G:\桌面\笔记\Go\assets\image-20220713130740806.png)

* `func`关键字，用于定义一个方法

## Go语言的字符串

```go
package main

import "unicode/utf8"

func main(){
    println(len("你好"))//输出6
    println(utf8.RuneCountInString("你好"))//输出2
    println(utf8.RuneCountInString("你好ab"))//输出4
    
    var a string = "Hello"
	var b string = "World"
	var ab string = a + "," + b
    println(ab)//输出"Hello,World
    var c int = 10
	var ac string = a + c //会报错，因为c不是字符串类型的
}
```

* Go语言中string的长度很特殊

  * 字节长度：和编码无关，使用len(str)获取的是
  * 字符数量：和编码有关，用编码库来计算

* Go语言中字符串的拼接

  * 和其他语言一样的是，都是使用`+`来拼接两个字符串

  * 和其他语言不一样的是，Go不会帮你做隐式类型转换,string类型的只能和string类型的拼接，和其他类型的拼接会报错(**Go是一个非常强类型的语言，不会做任何隐式类型转换**)

    ![image-20220713130224100](G:\桌面\笔记\Go\assets\image-20220713130224100.png)



* Go中关于string的操作都放在strings这个包中，例如切割、查找...等

## Go语言中的rune类型

```go
// rune is an alias for int32 and is equivalent to int32 in all ways. It is
// used, by convention, to distinguish character values from integer values.
type rune = int32

//这是标准库中的定义
```

* `rune`，直观理解，就是字符
* `rune`不是byte
* `rune`本质是int32,一个`rune`四个字节
* `rune`在其他很多语言里面是没有的，与之对应的，golang没有char类型。`rune`不是数字，也不是char，也不是byte
* 一般这个类型用的很少

## Go语言中的数字类型

* bool:true,false
* int8,int16,int32,int64,int
  * golang中没有long，long long这种长整型,它直接将整形的位数表示在int后面，需要那种类型直接定义就好
* uint8,uint16,uint32,uint64,uint
  * 这是golang中的无符号整形
* float32,float64
  * 同样的,golang中也没有double这种类型，是将类型占用的位数接在类型后面来表示的

## Go语言中的byte类型

```go
// byte is an alias for uint8 and is equivalent to uint8 in all ways. It is
// used, by convention, to distinguish byte values from 8-bit unsigned
// integer values.
type byte = uint8

//标准库中对byte的定义
```

* `byte`本质是`unit8`，占8位，表示一个字节
* `byte`类型的相应操作在`bytes`包中

## Golang类型总结

* golang的数字类型标注了长度、有无符号
* golang不会帮你做类型转换，类型不同无法通过编译。因此`string`也只能和`string`拼接
* golang中有一个很特殊的`rune`类型，接近一般语言的char或者character的概念
* `string`,`byte`对应的操作都在`strings`和`bytes`包中

***



## Golang中变量的声明和定义

```go
package main

var Global string
var local string
var (
	a int
	b string
)

func main() {
    c:=13 //简短变量声明
}
```

* golang中使用变量首字符的大小写来控制变量的可访问性

  * 首字母大写代表全局变量，包外可访问
  * 首字母小写代表局部变量，仅包内可访问

* 多组变量声明使用，如下形式

  ```go
  var (
      var1 type
      var2 type
  )
  ```

* golang有类型推断机制，在有初始化值的情况下某些变量可以省略掉类型

  ```go
  var First="First"
  var Second=2
  
  var c uint=15 //unit类型不能省略类型，因为golang会把数字推断为int类型
  ```

* golang是一个非常强类型的语言，不会做任何类型转换

  ```go
  var a uint=15
  var b=10
  
  println(a==b) //编译会报错，因为a和b不是同类型的，不会转换
  ```

* 使用`:=`声明变量,只能在函数内部声明使用，在函数外面声明会报错

  ![image-20220713134533703](G:\桌面\笔记\Go\assets\image-20220713134533703.png)

***

## Golang方法定义

```go
func FuncName(arg1 arg1type,arg2 arg2type) returnType{
    
}

func FuncName2(arg1 arg1type,arg2 arg2type) (returnType1,returnType2){
    return returnValue1,returnValue2
}//多返回值函数定义
```

* golang方法的定义使用如上格式定义

* golang方法支持多返回值

* 使用多个变量接收多返回值，如果不想接收某个变量可以使用`_`来省略该值

  ```go
  package main
  
  func ReturnTwo() (int, int) {
  	return 1, 2
  }
  func main() {
  	a, b := ReturnTwo()
  	println(a, b)
  	_, c := ReturnTwo()
  	println(c)
  }
  ```

* golang还支持命名返回值(**一般还是不命名，不符合大多数的使用习惯，但是要知道**)

  ```go
  func ReturnTwo() (r1 int,r2 int) {
  	r1=1
      r2=2
      return
  }
  ```

* golang还支持不定参数(使用不定参数时，要将不定参数放在最后，当做最后一个参数)

  ```go
  func Func4(a string,b int,names ...string){
      for _,name:=range names{
          //将不定参数当做切片使用即可
          println(name)
      }
  }
  ```

***

## Go中的数组和切片

### 数组

```go
package main

func main(){
    a1:=[3]int{9,8,7}
    var a2 [3]int
    var a3 [3]int ={0,0,0}
}
```

* 数组定义定义的语法是: [cap]type
* 初始化要指定容量
* 数组会直接初始化，如果你没有手动初始化的话，go会自动帮你把数组中的值初始化为0
* go数组创建好之后，内存就已经分配好了(不能更改)
* 数组中len==cap

### 切片

```go
package main

func main(){
    a1:=[]int{1,2,3}
    var a2 []int
    s1:=make([]int,3,4)//创建一个有三个元素的，但是容量是4的切片
    s2:=make([]int,4)//等价于make([]int,4,4)
    s1=append(s1,5)
}
```

* 切片的定义方法和数组类似，但是不指定空间的大小

* 切片的len<=cap ,cap >=len，和数组不一样

* 向切片里添加一个元素使用append

  ```go
  package main
  import "fmt"
  func main() {
  	s1 := make([]int, 3, 4)
  	s1 = append(s1, 5)
  	fmt.Printf("s1:%v s1.len:%d s1.cap:%d\n", s1, len(s1), cap(s1))
      //添加元素后没超出容量不会触发扩容
  	s1 = append(s1, 6)
  	fmt.Printf("s1:%v s1.len:%d s1.cap:%d\n", s1, len(s1), cap(s1))
      ///添加元素后超出容量后会触发扩容机制
  }
  ```

  ![image-20220713143429301](assets/image-20220713143429301.png)

* 推荐写法 s1:=make([]type,0,capacity)

### 子切片

```go
package main

func main(){
    s1:=[]int{2,4,6,8}
    s2:=s1[1:3] //获得[start,end)之间的元素，左闭右开
    s3:=s1[:3]  //获得[0:end)之间的元素
    s4:=s1[0:]  //获得[start,end)之间的元素
    s5:=s1[:]   //获得该切片的全部元素
}
```

* 注意切片是左闭右开的区间就可以了

### 如何理解切片

* 切片的操作很有限，不支持随机的增删，没有add和delete方法

* 只有append操作

* 切片支持子切片操作(子切片和原本切片共用一个底层数组)

  * 子切片和切片是否会互相影响，根据它们是否还共享数组

    * 如果子切片或者原本切片的结构发生变化了，它们就不共享同一个数组了(**结构发生变化了就是扩容了，扩容之后会将原数组的数据拷贝到新开辟的数组，这样子切片和原切片就不共享同一个数组了，就不会互相影响了**)

      ```go
      package main
      
      import "fmt"
      
      func main() {
      	s1 := make([]int, 3, 3)
      	s2 := s1[0:3]
      	println("修改S2前")
      	fmt.Printf("s1:%p s1:%v \n s2:%p,s2:%v\n", s1, s1, s2, s2)
      	s2[0] = 1
      	println("修改S2后")
      	fmt.Printf("s1:%p s1:%v \n s2:%p,s2:%v\n", s1, s1, s2, s2)
      	println("S1扩容:修改S2前")
      	s1 = append(s1, 3)
      	fmt.Printf("s1:%p s1:%v \n s2:%p,s2:%v\n", s1, s1, s2, s2)
      	println("S1扩容:修改S2后")
      	s2[1] = 2
      	fmt.Printf("s1:%p s1:%v \n s2:%p,s2:%v\n", s1, s1, s2, s2)
      }
      ```

      ![image-20220713145950794](assets/image-20220713145950794-16576956262521.png)

    * 可以看到，扩容之前，s1和s2指向的是同一块内存地址，所以他们会互相影响，但是扩容之后，s1和s2就指向不同的内存地址了，就不会再互相影响了

* **注意：**用子切片，只是用来**只读**，不要修改里面的值

 ###  数组和切片的对比

|            |    数组    |     切片     |
| :--------: | :--------: | :----------: |
| 直接初始化 |    支持    |     支持     |
|    make    | **不支持** |     支持     |
|  访问元素  |   arr[i]   |    arr[i]    |
|    len     |    长度    | 已有元素个数 |
|    cap     |    长度    |     容量     |
|   append   | **不支持** |     支持     |
| 是否可扩容 | **不可以** |     可以     |

***

## 控制结构

### for

```go
package main

func main(){
    for true{
        
    }//无限循环,true加不加无所谓
    for i:=0;i<10;i++{
        
    }//标准for循环
    arr := []int{1,2,3,4}
    for index,value:=range arr{
        
    }
    M:=map[string]string{"test":"just is a test"}
    for k,v:=range M{
        
    }
}
```

* golang中没有while循环，无限循环上述第一种格式

* 使用for...range这种格式的for循环，可以用于数组、切片和map,对于数组和可以直接拿到index和value，对于map可以直接拿到key和value

  * 如果不想要index或者key可以用`_`来不接收(**只能不接收第一个值，不能不接收第二个值**)

    ```go
        arr := []int{1,2,3,4}
        for _,value:=range arr{
            
        }
        M:=map[string]string{"test":"just is a test"}
        for _,v:=range M{
            
        }
    ```

### if-else

```go
package main

func main(){
    a:=10
    if a<100{
        
    }else{
        
    }//普通for
    if i:=10;a==i{
        
    }else{
        
    }//先定义一个变量(也可以是表达式)，再判断
    i:=10;
    if a==i{
        
    }else{
        
    }//和上述的一样
}
```

* 使用if i:=10;a==i...,这种格式的`if`时
  * `i`只能在if的作用域使用，出了`if`的作用域就不能使用了

### switch

```go	
package main

func main(){
    a:=10
    switch a{
    case 10:
        doSomething...
     case 20:
        doSomething...
     default:
        doSomething...
    }
}
```

* golang中`switch`和其他语言大体差不多，但是`case`后面不用加`break`了,它执行完一个case里面的语句后会自动跳出，不会执行后续语句，如果想让它执行后续语句可以用一个关键字`fallthrough`

  ```go
  package main
  
  func main(){
      a:=10
      switch a{
      case 10:
          doSomething...
          fallthrough//使用该关键字后，后面所有的case包括default都会执行
       case 20:
          doSomething...
       default:
          doSomething...
      }
  }
  ```

* `switch`后面可以是基础类型和字符串，或者满足特定条件的结构体

  ```go
  package main
  type user struct{
      name string
  }
  func main{
      u:=user
      switch u{
      default:
      }
  }
  ```


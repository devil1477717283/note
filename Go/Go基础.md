### Go依赖包：

* 依赖包按“深度优先”的次序进行初始化；
* 每个包内按以“常量 -> 变量 -> init 函数”的顺序进行初始化；
* 包内的多个 init 函数按出现次序进行自动调用。

### init函数的用途

* 重置包级变量值
* 实现对包级变量的复杂初始化
* 在 init 函数中实现“注册模式”。

## Go项目标准布局

### 可执行项目

```sh
exe-layout
├── cmd/
│   ├── app1/
│   │   └── main.go
│   └── app2/
│       └── main.go
├── go.mod
├── go.sum
├── internal/
│   ├── pkga/
│   │   └── pkg_a.go
│   └── pkgb/
│       └── pkg_b.go
├── pkg1/
│   └── pkg1.go
├── pkg2/
│   └── pkg2.go
└── vendor/
```

* cmd目录存放要编译构建的可执行文件对应的main包的源文件
* pkgN目录，存放项目自身要使用、同样也是可执行文件对应main包所要依赖的库文件，同时这些目录下的包还可以被外部项目引用
* go.mod和go.sum是Go包依赖管理使用的配置文件
* vendor是用于项目再本地缓存特定版本依赖包的机制（可选项）

### Go库项目的布局

```sh
lib-layout
├── go.mod
├── internal/
│   ├── pkga/
│   │   └── pkg_a.go
│   └── pkgb/
│       └── pkg_b.go
├── pkg1/
│   └── pkg1.go
└── pkg2/
    └── pkg2.go
```

## Go Module的常规操作

* 为当前项目添加一个依赖包

  ```sh
  $go get github.com/google/uuid
  ```

* 自动分析源码中的依赖变化，识别新增依赖项并下载它们

  ```sh
  $go mod tidy
  ```

* 升/降级依赖的版本

  * 项目的 module 根目录下，执行带有版本号的 go get 命令：

    ```sh
    $go get github.com/sirupsen/logrus@v1.7.0
    ```

  * 使用万能命令 go mod tidy 来帮助我们降级，但前提是首先要用 go mod edit 命令

    ```sh
    $go mod edit -require=github.com/sirupsen/logrus@v1.7.0
    $go mod tidy
    ```

* 添加一个主版本号大于 1 的依赖

  * 语义导入版本：如果新旧版本的包使用相同的导入路径，那么新包与旧包是兼容的。

    * 在声明它的导入路径的基础上，加上版本号信息。

      ```sh
      $go get github.com/go-redis/redis/v7
      ```

      


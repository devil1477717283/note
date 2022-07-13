# 简单的Go Web Server

## http库

### Request

* Request中的`Body`只能读取一次，但是多次读取不报错(但是你什么都读不到)
* `GetBody`:原则上是可以读取多次的，但是在原生库中是`nil`
  * `GetBody`在原生库中是将一个函数声明为字段
* `Query`:获取在URL中的参数
  * 所有的值都被解析成字符串
* `URL`
* `Header`
* `From`

要点总结：

> Body和GetBody:重点在于Body是一次性的，而GetBody默认情况下是没有，一般中间件会考虑帮你注入这个方法
>
> URL：注意URL里面的字段的含义可能不是我们所理解的那样
>
> From：记得调用前先用ParseFrom，并且不要忘了在请求中加上http头(**Content-Type: application/x-www-form-urlencoded**)

## 从Http Server开始


Apache Camel 2.20.1

Camel是一个集成框架。

# 基本概念

## 端点（Endpoint）

端点是负责把应用与通道连接起来的一组代码。

![端点](resources/Camel/端点.png)

Camel的端点必须实现`org.apache.camel.Endpoint`接口。

## CamelContext

`CamelContext`表示Camel的运行时系统。

## CamelTemplate

`CamelTemplate`是对`CamelContext`的一个浅封装。

## 组件（Components）

Camel的组件实际上是一个端点工厂，用于创建端点实例。但是，应用通常不直接调用组件来创建端点实例，而是通过`CamelContext`来间接访问组件。

## 消息（Message）

例如：请求消息（in消息）、响应消息（out消息）或异常消息（fault消息）。

## 交换（Exchange）

`Exchange`用于表示消息之间的交换。

## 处理器（Processor）

## 路由（Routes、RouteBuilders）


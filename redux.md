## 动机
如果一个应用需要在多个组件中共享状态，简单的方式是将状态保存到它们共同的父组件中（状态提升），但是这样做导致代码的冗余。Redux的动机是把这些状态集中到一个独立的store中去管理。
任何组件都可以通过action与store进行交互。

## 基本概念
ref: https://cn.redux.js.org/tutorials/essentials/part-1-overview-concepts

### action
action 只是一个普通的js对象，必须要有type字段。

### reducer
reducer 是一个函数，接受state,  action，返回newState。
- 必须使用不可变逻辑，不能直接修改state，必须复制一个state再修改
- 禁止任何异步逻辑、依赖随机值或导致其他“副作用”的代码
- 如果使用toolkit的话，可能允许在reducer里写可变的逻辑，但是这实际上是不可变的, toolkit会完成剩下的事情。

## redux in flutter

### flutter_redux
ref: https://pub.dev/packages/flutter_redux
- 将redux的特性封装成dart语言的风格
- StoreProvider 包装需要state的若干组件
- StoreConnect 逻辑比较复杂，分converter和builder. converter默认接受state为参数，可以返回一个匿名函数或者一个值res。builder接受context和res, context是上下文的运行环境，res就是converter返回的。通过StoreConnector可以实现dart Widget + action + subscribe 的功能。

### redux
ref: https://pub.dev/packages/redux
- 更加贴近web端风格，使用了querySelector, event listener等，比较像js的写法


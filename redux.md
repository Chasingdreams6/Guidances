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



# Reconciliation

![](https://user-gold-cdn.xitu.io/2019/10/19/16de3834ffcc66f4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

> Diff

React 采用虚拟 DOM（VDOM）

每次属性（props）和状态（state）发生变化，render 函数返回不同的元素树

React 会检测当前返回的元素树和上次渲染的元素树之间的差异

针对差异进行更新，最后渲染为真实 DOM

这就是整个 Reconciliation 过程，其核心是进行新旧 DOM 树对比的 diff 算法

> shouldComponentUpdate

shouldComponentUpdate 会在 props 或 state 发生变化时触发

返回 true,表示重新渲染，从而调用 render 函数

返回 false,则不更新当前组件，可以控制 VDOM 树的 Diff 过程

![](https://user-gold-cdn.xitu.io/2019/10/19/16de39cf997a808f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

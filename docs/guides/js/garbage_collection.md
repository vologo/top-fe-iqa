---
title: 谈谈JS中的垃圾回收机制
---

JS不同于C和C++ 自动管理内存分配和闲置资源 的回收：确定哪个变量没有再被使用，然后释放占用的内存

 - 周期性 每隔一定的时间自动运行 周而复始 从一而终
 - 算法  简单的靠算法 不能够准备正确的定位某块内存是否正在使用，存在一定的不可预判性

函数中的局部变量在使用之后就不再需要了，一般来说可以正常的释放掉，但是不是唯一的，需要内部去跟踪标记，大致有两种方式

## 标记清理

当变量进入上下文标记一下 理论上可能会被用到 ，离开的时候再标记一下，在任何的上下文中都找不到他们了就开始清理

## 引用计数

记录一个变量被引用的次数，引用数+1 当当前的引用数是0的时候，就是访问不到这个变量了，存在循环引用的问题


- 离开作用域的值会被自动标记为可回收，然后在垃圾回收期间被删除。
- 主流的垃圾回收算法是标记清理，即先给当前不使用的值加上标记，再回来回收它们的内存。
- 引用计数是另一种垃圾回收策略，需要记录值被引用了多少次。JavaScript引擎不再使用这种算法，但某些旧版本的IE-仍然会受这种算法的影响，原因是JavaScript会访问非原生JavaScript对象（如DOM元素）。
- 引用计数在代码中存在循环引用时会出现问题。
- 解除变量的引用不仅可以消除循环引用，而且对垃圾回收也有帮助。为促进内存回收，全局对象、全局对象的属性和循环引用都应该在不需要时解除引用。

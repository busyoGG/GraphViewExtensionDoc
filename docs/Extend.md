# 拓展

## 节点

通过修改 `RootNode` 类的 `Init` 方法，可以支持更多的 UI 类型。

## 图

可以在 `GGraph` 构造函数中添加自己需要的功能，如监听组合键等。

在 `SetupToolbar` 中可以添加自定义功能按钮，如果继承 `GGraph` ，则可以通过重写 `SetupToolbar` 来修改功能按钮。

## 反序列化

反序列化目前只做了常规的 `string` 、 `float` 、 `bool` 类型转换，如果有更多类型可以拓展 `GGraph` 的 `ToGDataNode` 方法。
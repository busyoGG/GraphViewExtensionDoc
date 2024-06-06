# 快速开始

## 简介

### 功能

通过 Attribute 标签简化节点编辑器开发。

支持选中描边，自定义节点大小，未手动修改大小的时候自适应节点大小。

支持打开、保存、关闭节点图。支持 Ctrl+S 保存当前节点图（焦点需要在 GraphView 内）。

### 演示视频

**节点操作**

<video src='https://video.spup.buzz/2024-06-06-21-09-32.mov'></video>

**文件操作**

<video src='https://video.spup.buzz/2024-06-06-21-09-28.mov'></video>

## 节点

所有自定义节点需要继承 `RootNode`。

### 节点 UI

节点 UI 可以通过 `Attribute` 标签的方式，在 `Field` 打上对应的标签，就可以显示对应的 UI。

例如：

![](https://img.busyo.buzz/imgUpload/20240512-152404-612.png)

```C#
[GraphNode(NodeTypeEnum.Input), GName("测试输入")]
private string _test1 = "";
```

- `GraphNode` 标签负责控制节点 UI 的类型。
- `GName` 标签负责在 UI 有额外显示名字的情况下定义名字。
- `GWidth` 标签负责控制 UI 的宽度。
- `GColor` 标签负责控制 UI 颜色，目前只在 Box 应用。

**`GparhNode` 标签参数**

|  名称  |  类型  |  枚举  |                                                        参数（params string[]）                                                         |
| :----: | :----: | :----: | :------------------------------------------------------------------------------------------------------------------------------------: |
|  标签  | string | Label  |                                                                                                                                        |
| 输入框 | string | Input  |                                                                                                                                        |
|  注释  | string |  Note  |                                                       Custom：是否可修改的备注。                                                       |
|  枚举  |  Enum  |  Enum  |                                                                                                                                        |
| 滑动条 | float  | Slide  | 两种情况：<br>1. 两个参数，参数 1 代表初值，参数 2 代表终值。<br>2. 三个参数，参数 1 填 Int，表示整型滑动条，后面两个参数和情况 1 同理 |
|  开关  |  bool  | Toggle |                                                     一个任意字符串，表示开关的名字                                                     |
|  单选  |  int   | Radio  |                                                     任意数量字符串，表示单选的选项                                                     |
| 矩形框 |  任意  |  Box   |                             1.一个任意整数，接下来有几个 UI 被框在 Box<br>2.Column：竖方向，不传则为横方向                             |

**`GName` 标签参数**

一个参数，string 类型，表示名字。

**`GWidth` 标签参数**

两个参数：

1. 宽度，float 类型。
2. 宽度种类，LengthUnit 类型，像素或百分比。

**`GColor` 参数**

一共四个参数，对应 r, g, b, a

**初始化**

通过重写 InitConfig 方法可以初始化原始 Node UI 需要的字段值，例如：

```C#
protected override void InitConfig()
{
    title = "测试节点";
}
```

默认 `title` 的名字为默认节点，重写该方法的时候可以重写 `title`。

**客制化**

如果有复杂的 UI 难以以现有的方式实现，可以通过重写 `CustomUI` 来实现。

### 节点数据

节点内设有一个 ExpandoObject 类型字段 `_data` 来存储数据。

通过重写 `SetData` 方法来设置数据。

如：

```C#
protected override void SetData()
{
    _data.desc = _desc;
}
```

如果需要在节点重新打开的时候还原数据，则需要重写 `ResetData` 方法来手动设置数据。

```C#
protected override void ResetData()
{
    _desc = _data.desc;
}
```

## 图

### Graphview

默认基本不需要改动。要动的话就继承 `GGraph` 。

### SearchWindow

多级菜单，通过修改 `Assets/Editor/GraphViewExtension/Graph/Menu.json` 文件来自定义多级菜单。菜单支持搜索。

name 代表菜单名，type 代表点击菜单后创建的节点类型，child 代表子菜单。

```json
[
  {
    "name": "装饰节点",
    "child": [
      {
        "name": "胜利节点",
        "type": "DNodeSuccess"
      },
      {
        "name": "失败节点",
        "type": "DNodeFail"
      },
      {
        "name": "反转节点",
        "type": "DNodeReverse"
      }
    ]
  },
  {
    "name": "测试节点",
    "type": "TestNode"
  }
]
```

## 编辑器

编辑器只需要初始化 GraphView 即可。

```C#
public void InitGraph()
{
    var provider = CreateInstance<GSearchWindow>();
    var graph = new GGraph(this, provider);
    rootVisualElement.Add(graph);
    //如果有需要可以保存graphview
    _view = graph;
}
```

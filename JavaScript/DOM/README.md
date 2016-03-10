# 什么是 DOM
DOM 是一套对文档的内容进行抽象和概念化的方法，即文档对象模型。  

## DOM 中的 D
当创建一个网页并加载到 Web 浏览器中时，DOM 将根据你编写的网页文档创建一个文档对象。

## DOM 中的 O
对象是一种独立的数据集合。与某个特定对象相关联的变量被称为属性，通过特定对象调用的函数被称为这个对象的方法。  

JS 对象分 3 种类型：

- 自定义对象
- 内建对象
- 宿主对象（浏览器提供的对象）

window 对象对应浏览器窗口本身，这个对象的属性和方法被称为 BOM （浏览器对象模型）或者 WOM（窗口对象模型）。  
我们重点讨论如何对网页的内容进行处理，而用来实现这一目标的载体就是 document 对象。

## DOM 中的 M
M 代表 Model 或者可以认为是 Map。DOM 把一份文档表示为一棵树。

## 节点
文档是由节点构成的集合。  

1.元素节点
2.文本节点
3.属性节点

## getElementById() 方法
这个方法与 document 对象相关联的函数，调用返回 document 对象里的一个独一无二的元素。一般情况，我们不需要为文档的每一个元素分别定义一个唯一的 id 值。

## getElementsByTagName() 方法
返回一个对象数组，每个对象分别对应这文档指定标签的元素。  
getElementsByTagName() 方法允许我们把一个通配符当做参数，这意味着文档里的每个元素包含在返回的数组中。

## getAttribute() 方法
object.getAttribute(attribute)

## setAttribute() 方法
object.setAttribute(attribute, value)

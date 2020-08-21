# HTML Note

```html
<!DOCTYPE html>
<html>
    <head>
        <title>TITLE</title>
    </head>
    <body>
        CONTENT  # </br>
    </body>
</html>
```

## 1. 概述: 标签
- HTML标签是HTML的基本单位
	- 标签是存储和管理数据的容器
	- 一个标签存储一组数据
		- 标签的数据可以是字节序列, 文本, 图像或其他标签
	- 一个标签有自己的属性, 标记在标签中
		- 每个属性有属性名与属性值
		- 属性用于描述标签
	- 一个HTML文档形成一个以html标签为根节点的标签树
## 2. 标签
### 2.1. 结构
- 标签含有名称, 属性和数据
- 标签的格式:
- 非空标签: (普通标签) 一对
	- 有标签及其结束标签构成
```
<<LabelName>( <AttributeName>=<AttributeValue>)*>
	<Contents>
</<LabelName>>
```
- 空标签: 一个
	- 如换行标签`<br>`
	- 无结束标签
```
<<LabelName>( <AttributeName>=<AttributeValue>)*>
```
### 2.2. 内容
- 标签的内容
	- 在标签之间的数据称为标签的内容
	- 标签的内容可以是字符串或另一个标签 (嵌套)
### 2.3. 分类
- `<!--<Comments>-->`: 注释
	- 空标签
- `<!DOCTYPE <DocType>>`: 声明html版本
	- 位于html文档第一行
	- 空标签
- `<a href = "<Url>"><Text></a>`: 超链接
	- 名为`<Text>`, 指向`<Url>`的标签
- `<p><Text></p>`: 普通文本段落
- `<b><Text></b>`: 粗体文本段落
- `<div><Content></div>`: 给文档分节

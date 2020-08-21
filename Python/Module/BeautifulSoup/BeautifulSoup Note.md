# BeautifulSoup Note

[TOC]

## 1. Install

- `pip install beautifulsoup4`

## 2. Cheat Sheet

- Every thing is tag
    - `bs4.element.Tag`
    - `bs4.BeautifulSoup`
    - `bs4.element.NavigableString`

```python
from bs4 import BeautifulSoup
from bs4.element import Tag, NavigableString

# Construct
marks: BeautifulSoup = BeautifuleSoup(open(r'<HTMLDir>', 'r', encoding='utf-8')|<HTMLDocStr>, features=('html.parser'|'xml'))

# Filter
def filterFunction(tag: Tag) -> bool:
    # ...
    tag.has_attr('<AttrName>')
    isinstance(tag, (Tag|BeautifulSoup|NavigableString))
filter = ('<Value>' | re.compile(r'<Value>') | [<Filter>, ...] | True | filterFunction | lambda tag: <TagMatchKey>)

# Search
tag = tag(.tag)*
tagList = tag(.find_(all|parents|(next|previous)_siblings|all_(next|previous))?(name=filter, attrs=(filter|{'<Attr>': '<Value>', ...}), text=filter, <Attr>=filter, recursive[bool], limit[int])  # Use `class_` to refer class attr
tag = tag.find(_(parent|(next|previous)(_sibling)?))?(name=filter, attrs=filter, text=filter, <Attr>=(filter|{'<Attr>': '<Value>', ...}), recursive[bool])
tagList = tag.select(('<Tag>( > <Tag>)*'|'<ClassAttrValue>'|'<(Name)?#<IdAttrValue>'|'<Name>[<AttrName>](*="<AttrValue>")?'))

# Tag Obj

#     Name
tag.name  # Tag Name

#     SubTag
tag.<SubTag>  # SubTag 可以是任意子孙 Tag
tag.contents  # Tag 子节点 (而非子孙节点) 列表
tag.descendants  # Tag 子孙节点列表 (先序遍历)

#     Tag Navigate
tag.parents?  # Parent Tag | Ancestor Tag List
tag.(next|previous)_siblings?  # Sibling Tag( List)?; Cousin is not Sibling; Watch out blank NavigableString Tag
tag.(next|previous)_elements?  # HTML Parse Order

#     Text
tag.strings?  # 唯一 NavigableString SubTag (若存在) | NavigableString SubTag List
tag.stripped_strings  # Stripped NavigableString SubTag List

#     Attr
tag['<AttrName>']  # Attr Value
tag.attrs  # Attr Dict

str(tag)

# NavigableString
str(navigableString)
navigableString.replace_with(<NavigableString>|str)

# Print
tagStr = tag.prettify()
tagText = tag.get_text('<Spliter>', strip[bool])
```

## 3. 机制概述: 标签 (Tag)
- html文档中的所有内容都保存在标签中
	- 标签有名字, 属性, 内容
		- 标签的属性有属性名和属性值
		- 标签的内容可以是文本 (text) 或其他标签
	- 一个html文件是一个以html标签为根节点的标签树
- BeautifulSoup使用标签对象来管理html文档
    - `bs4.element.Tag`
    - Beautiful Soup将HTML文档转换成一个树形结构
    - 每个节点都是Python对象
    - 每个节点就是一个标签
## 4. BeautifulSoup
- BeautifulSoup对象
	- 是`<html>`标签的父标签
	- 即HTML文档的总标签
	- 获取该文档中其他标签的入口
	- 操作文档的句柄, 类似树的根节点
- BeautifulSoup对象即是HTML文档的总标签: html标签
	- BeautifulSoup对象及其子对象都是html中的标签
- 特殊的Tag对象
	- 没有attribute域
### 4.1. 获取
- 创建BeautifulSoup对象:
	- 文档均解释为Unicode

```python
from bs4 import BeautifulSoup # 导入BeautifulSoup类

bSoup = BeautifulSoup(<response>.text, features="html.parser") # 输入html字符串即可
bSoup = BeautifulSoup(open("<FileDir>"), features = "html.parser") # 输入html文件构造BSoup对象
```

### 4.2. 使用
同Tag类对象
## 5. Tag
对应HTML文档中的普通标签
### 5.1. 获取
- html文档中的一切标签都通过BSoup对象, 作为它的子标签来获取
### 5.2. 使用
- Name: `tag.name` 标签名
	- 获取标签名:
```python
tag.name
```
	- 可以修改
- Attribute: 标签属性
	- 字典
	- 获取标签属性:
		- 字典
		- 点取
		- get方法
```python
tag["<AttributeName>"] # get AttributeValue
tag.attrs['<AttributeName>'] # get AttributeValue
tag.get("<AttributeName>") # get AttributeValue
```
	- 可以修改, 删除
	- 多值属性的值是一个列表
		- 不支持的多值属性的值会是一个字符串
- 子标签: 内容
### 5.3. 子标签
- 子标签: 内容
	- 一个Tag可能包含多个字符串或其它的Tag, 这些都是这个Tag的子节点
	- 获取子标签:
		- 点取子孙标签: 获取该标签的所有子孙标签中名称为`<ChildTagName>`的第一个子孙标签对象
```python
<tag>.<ChildTagName> # 取得第一个<ChildTagName>对象
```
	- 获取 (直接) 子标签列表:
```python
<tag>.contents # 返回子标签列表, 每一项都是子标签
```
	- 对 (直接) 子标签进行循环遍历:
```python
for childTag in tag.children:
	# do something with childTag
```
	- 对子孙标签进行递归循环遍历:
```python
for childTag in tag.descendants:
	# do something with childTag
```
	- 查找子孙标签, 返回匹配的子孙标签组成的的列表:
		- 使用标签名和标签属性来搜索
```python
tagList[] = <tag>.find_all("<TagName>", id = "<IdAttributeValue>", class_ = "<ClassAttributeValue>") # get ChildTagList
```
	- 获取标签中的所有子孙文本标签内容:
		- string仅在仅有一个直接子节点或仅有一个直接字符串子节点时返回对象, 否则返回None
		- strings返回子孙节点中所有字符串节点组成的列表
		- stripped_strings返回去除多余空白的strings列表
```python
tag.get_text()
tag.string
strList[] = tag.strings
strList[] = tag.stripped_strings
```
### 5.4. 遍历
#### 5.4.1. 父节点
- 获取父标签
```python
tag.parent # 获取父标签
```
	- 递归获取所有祖先节点
```python
for parent in tag.parents:
	# do something with parent
```
#### 5.4.2. 兄弟节点
同一个标签的不同直接子节点称为兄弟节点
```python
tag.next_sibling
for sibling in tag.next_siblings:
	# do something with sibling
tag.previous_sibling
for sibling in tag.previous_siblings:
	# do something with sibling
```
#### 5.4.3. 按解析顺序遍历
进入节点, 解析内容, 退出节点...
```python
tag.next_element
for element in tag.next_elements:
	# do something with element
tag.previous_element
for element in tag.previous_elements:
	# do something with element
```
### 5.5. 搜索
#### 5.5.1. 过滤器
- 搜索方法的每一个参数大都支持下列过滤器
- 字符串
	- 要求**完整**匹配
	- 解释为字面值
- 正则表达式
	- 通过search()方法匹配
```python
import re
tag.find_all(re.compile("<regex>"))
```
- 列表
	- 与列表中任一元素匹配则认为匹配
- True
	- 恒匹配
	- 但不会匹配字符串节点
- 方法
	- 接受Tag类参数
	- 返回值为True时匹配

```python
def check(tag: Tag) -> bool:
    # ...

tagList = bSoup(check)
```

#### 5.5.2. 搜索方法
- find_all(name, attrs, recursive, text)
	- 参数
		- name: tag名
			- 不匹配字符串标签
		- attrs: tag属性
			- 格式: `<AttrName> = <AttrValue>`
			- 注意: class属性应使用`class_`属性名称
			- E.g.: `bSoup(<AttrName>=<Filter>)`
		- recursive: 递归搜索 (默认为True)
		- text: 字符串内容, 用于搜索文档字符串
	- 返回tag列表
	- 简写: 对对象调用等于调用find_all `soup() == soup.find_all()`
- find(name, attrs, recursive, text)
	- 仅返回一个tag的find_all
- find_parents
- find_parent
- 其他搜索方法
	- `find_next_siblings`
	- `find_next_sibling`
	- `find_previous_siblings`
	- `find_previous_sibling`
	- `find_all_next`
	- `find_next`
	- `find_all_previous`
	- `find_previous`
### 5.6. 输出
- 获取格式化html字符串:
```python
tag.prettify() # 返回格式化html str
```
- 压缩输出: str()
- 获取标签中的所有子孙文本标签内容:
	- get_text: 取得所有子孙节点的text内容
		- 参数
			- str: 分隔符
			- strip: 去除前后空白
				- True or False
	- string仅在仅有一个直接子节点或仅有一个直接字符串子节点时返回对象, 否则返回None
	- strings返回子孙节点中所有字符串节点组成的列表
	- stripped_strings返回取出多余空白的strings列表
```python
tag.get_text()
tag.string
strList[] = tag.strings
strList[] = tag.stripped_strings
```
## 6. NavigableString 可遍历字符串
- 特殊标签: 代表标签内部的text文本
	- 标签名: string
	- 与str类似
- 转化为`str`: `str(navigableString)`
### 6.1. 获取
```python
tag.string # 获取tag的字符串子标签
```
### 6.2. 使用
- 没有contents域 (没有子标签)
- 没有string域 (没有子标签)
- 没有find()方法: 不能查找子标签
```python
str(string) # 转化为str
string.replace_with(<str>) # 替换内容
```
## 7. Comment
对应文档的注释
### 7.1. 获取
```python
comment = BeautifulSoup("<!--<Comment>-->").string
```

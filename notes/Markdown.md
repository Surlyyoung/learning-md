---
title: Markdown
created: '2020-02-17T08:04:19.242Z'
modified: '2020-02-17T11:19:26.722Z'
---

# Markdown

### basic programma

#### 标题
+ #、##、###...为标题，最多六级；

#### 修饰
+ `*斜体*` *斜体* 或 _斜体_
+ `**粗体**` **粗体**
+ `***加粗斜体***` ***加粗斜体***
+ `~~删除线~~` ~~删除线~~
+ `++下划线++` ++下划线++
+ `==背景高亮==` ==背景高亮==

#### 链接
+ `[链接行内式](http://surlyyoung.github.io "i am title")`[链接行内式](http://surlyyoung.github.io "i am title")；
+ `{#index}`锚点{#index}；
+ `[^1]`注脚[^1]；
+ `[surlyyoung][1]`参考式[surlyyoung][1]；
+ `&lt;http://surlyyoung.github.io&gt`自动连接 &lt;http://surlyyoung.github.io&gt;

[^1]:我是脚注；
[1]:http://surlyyoung.github.io 我是参考式

#### 列表
+ +、-、*无序列表；
+ 1.、2.数字加英文点符号表示有序列表；

#### 对齐
+ <center>文字居中</center>
+ <p align="left">文字居左</p>
+ <p align="right">文字居右</p>

#### 图片
<center>

![图片alt属性](https://github.githubassets.com/images/modules/site/sponsors/logo-mona.svg "i am title")
</center>

#### 引用
> 确实

>> 不吹不黑

>>> 有一说一

#### 块级
+ 代码块 反引号``包裹 如 `Object.prototype`
+ 多行代码：
```
let obj = new Object();
obj.sayHello = function(str){
  retrun str;
}
```

#### 表格
|name|sex|phone|
|:-:|:-:|:-:|
|surly|man|18888888888|
|young|man|19999999999|

#### 分格线
+ ---
+ ***

#### 插入原始html
*直接写html会转义，放在代码块*

```
<div>
  <h5>5级标题</h5>
  <p>我是html段落</p>
  <span>我是span</span>
</div> 
```
 


 # end

---
layout: default
title:  "markdown语法自学总结"
date:   2019-03-14 15:22:00
categories: main
---

# MARKDOWN语法总结 #
- ## 其实github自带了MARKDOWN的教程和examples：_**[github的markdown教程](https://guides.github.com/features/mastering-markdown/)**_ ##
### _本文中所有TIPS均以google浏览器下git的issues预览效果为准_ ###
### _在某些情况下有些符号可以相互替代，例：\_和\*，我仅以我的习惯用法作为示例_ ###
## 1. 标题  ##
\# 你要写的文字  （#号后一定有一个空格）
\# 你要写的文字 \# （前#后 和 后#前一定有一个空格）
标题用# 表示，可放置在行的开头，也可放在首尾
markdown语法支持1-6级标题：
\#
\##
\###
\####
\#####
\######
效果如下：
# 你的标题
## 你的标题
### 你的标题
#### 你的标题
##### 你的标题
###### 你的标题
### _TIPS: #号之后一定要加空格，首尾也一定要加空格，否则会出错_ ###
### _TIPS: 如果在开头想输出#，可使用转义字符"\\"_ ###

## 2. 加粗，倾斜 ##

**加粗用两个星号\*\***
例：
\*\*你的文字\*\*
效果：
**你的文字**

**倾斜用一个下划线\_**
例：
\_你的文字\_
效果：
_文字_

**如果我既想加粗又想倾斜怎么办呢？**
**可以组合使用**
例：
\*\*\_你的文字\_\*\*
效果：
**_你的文字_**
### _TIPS:下划线_和星号**必须前后紧贴你的内容，不能加空格，否则就会失效_ ###

## 3. 超链接，图片 ##

超链接语法：
\[超链接文字\]\(url地址\)
例：
\[我的github\]\(https://github.com/voidstatic888\)
效果：
[我的github](https://github.com/voidstatic888)

图片语法：
\!\[图片描述\]\(图片url地址\ \"鼠标悬停提示\")
例：
\!\[喵！\]\(https://octodex.github.com/images/yaktocat.png  \"喵~\")
效果：
![喵！](https://octodex.github.com/images/yaktocat.png "喵~")

## 4. 列表，表格 ##

列表语法：
无序列表语法是一个横线-加一个空格：
例：
\- one
\- two
\- three
效果：
- one
- two
- three

有序列表语法是一个数字加\.加空格：
例：
1\. one
6\. two
9\. three
效果如图：
1. one
6. two
9. three
### _TIPS：数字只跟第一个有关，后面的数字不影响排序的数字的值_ ###

列表语法如下：
| 第一栏 | 第二栏 | 第三栏 |
| :- | :-: | -: |
| 文字居左 | 文字居中 | 文字居右 |
### _不知道为什么github的issue编辑器显示不了表格_ ###

## 5. 分割线，代码，引用 ##

分割线语法三个星号***
例：
one
\***
two
效果：
one
***
two

代码语法  \` (键盘esc下的键)：

例：
\`public static void main(String[] args){
   System.out.println('hello world');
} \`
或者
\```
public static void main(String[] args){
   System.out.println('hello world');
}
\```
效果如下：
```
public static void main(String[] args){
   System.out.println('hello world');
}
```
第二种能保存代码的原有格式，推荐使用
`public static void main(String[] args){
   System.out.println('hello world');
} `
**_可以看到代码风格确实发生了变化_**
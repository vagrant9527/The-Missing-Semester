
- [一、标题](#一、标题)
	- [1.1](#二号标题)
	- [1.2](#三号标题)


- [二、分割线](#二、分割线)
- [三、分割线](#三、斜体和粗体)

(列表和页内超链接结合做出的目录效果)
[第一次跳转](#jump1)





## 一、标题
### 1.使用#表示标题，其中#必须在行首，例如：

# 一号标题
## 二号标题
### 三号标题
#### 四号标题
##### 五号标题
###### 六号标题

*******************************************
## 二、分割线
- - - - - - - - - 
* * * * * * * * *
*******************************************

## 三、斜体和粗体
使用 * 和 ** 表示斜体和粗体  
*这是斜体*  
**这是粗体**
***这是粗体加斜体***  
  

拓展：删除线用两个 ~ 表示  
~~这是要删除的文字~~


## 四、超链接和图片
+ 1、添加链接的语法

格式1：[文字描述](链接) (注意这里的括号都是英文版的) 
格式2：通过变量设置一个链接，变量赋值在文档末尾进行  

示例1：[百度网址](https://www.baidu.com/)  
示例2：用1作为链接地址--》[百度网址][1]
  
图片则是在超链接前面多了一个！(注意是英文的！)   

![markdown-logo](http://static.runoob.com/images/runoob-logo.png)  

[1]: https://www.baidu.com/

+ 2.页内跳转(添加锚点)
锚点是网页制作中的一种，又叫命名锚记。命名锚记像一个迅速定位器一样是一种页面内的超级链接，运用相当普遍。
目前查到两种跳转的方式，一种是通过链接+标题，一种是结合html
(1)链接+标题：
格式：[显示内容](#标题)
可以直接利用各级标题实现跳转，这里的#不论是几级标题，也只有写一个#，而不是写多个＃。具体示例见本文档开始的目录使用。
(2)html形式：对于 Markdown 涵盖范围之外的标签，可以直接在markdown中使用HTML本身（HTML 的行级內联标签如 <span>、<cite>、<del> 不受限制）。所以结合html的用法能够实现页面的跳转。
<span id="jump1">目标位置1</span>
具体使用示例同样见本文档开头。


## 五、无序列表
使用- 、+、* 表示无序列表，前后留一行空白，可嵌套
+ 一层
	* 二层  
	这是二层的内容
	* 二层  
	这是二层的内容
		* 三层  
		这是三层的内容
		+ 三层
			* 四层
* 一层

## 六、有序列表
使用1.（.号后面有个空格）表示有序列表，可嵌套  
1. 一层
	1. 一层
	2. 二层
		3. 三层

## 七、文字引用
使用> 表示，可以有多个>表示层级更深，如：  
>第一层
>>第二层
>>>还可以更深

## 八、行内代码快
其实上面已经使用过很多次了，也就是用'表示，例如：  
'行内代码块'  
拓展：很多字符需要转义，使用反斜杠'\'进行转义  

## 九、代码块
使用四个空格缩进或者一个制表符表示代码块。  
使用 ` ``` `包裹一段代码可以实现代码高亮
```
	public class HelloWorld{	
		public static void main(String[] args)
	}
```

## 十、表格

## 十一、复选框
- [ ] A
- [ ] B
- [x] C

注释1：# - + * 效果相同
注释2：框里的x表示选中
注释3：[ ]这里面有个空格
注释4：注意内容和[]之间也有空格

## 十二、目录
只需要在恰当的位置添加 [TOC] 符号，凡是以 # 定义的标题都会被编排到目录中。











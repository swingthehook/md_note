# 标题
以#开头，几级标题，几个#
后面加 {#id} ，可以给标题赋id
# 段落
两个回车生成一个新段落，中间会空一行  
前面不要加空格，后面最好也不要有

首行缩进，markdown不支持，要在生成结果上加个缩进
# 换行
两个及以上的空格 + 回车

一般不用 \
# 粗体斜体删除线
## 粗体
两个 * 或者两个 _ ，前后各两个  
**像这样**
## 斜体
一个 * 或者一个 _  
_像这样_
## 粗斜体
三个 * 或者 _ ，粗在外，斜在内  
***像这样***
# 列表
## 有序列表
数字加一个 . ，

只能指定以某个数字开头，不能指定后续序号
## 无序列表
以 - 、 * ， + 开头
* 列表是支持嵌套的
## 任务列表
有序/无序 + [ ]，中括号里写 x 是选中，空格是未选中
# 引用
> ">" + " "，生成这样的引用
> 
> 引用和段落是嵌套的，引用里面也是两个回车下一个段落  
>
> 引用嵌套，两个 > 即可
> >就像这样
>
> 和列表也可以嵌套
> 1. 像这样
> - 或者这样
# 代码
1. 行内代码  
   一个反引号(波浪)`code demo`  
2. 代码块  
   四个空格或者一个tab  

        像这样
        但为什么我是两个tab
        列表里嵌套代码块还要再打一个回车
3. 围栏式代码块
   三个反引号`  
    ```
    还可以高亮
    
    ```
# 分割线
3个+的 * or - or _ 即可
***
---
___
# 超链接
- 链接到网站  
  [index]+(address)，中间不要空格
- 其他markdown  
  [index]+(./title) 中间不要空格
- 无标签，链接本身  
  <>，在尖括号里填入完整网址/邮箱  
  <https://www.baidu.com>
- 自动超链接  
  有的解析器会自动解析，如果不想被转换，可以写成行内代码(反引号) `https://www.baidu.com`
- []+()可以和粗斜体配合，还可以和代码块配合
- [`test`](https://www.baidu.com)
# 图片
- 超链接前加一个!，网址变成图片地址即可  
完整版：`![pic](pic_path)`  
- 图片可以嵌套到超链接的标题部分  
`[![pic](pic_path)](web_page_address)`   
- 带title的图片  
  ![pic](pic_path)
# 表格
- 用竖线 | 区分列
- 两个竖线之间是这一列的内容
- 表头和表之间要有  |-----|

    |tast_table|
    |-----|
    |first row|
- 对齐  
  冒号 : ，左边左对齐，右边右对齐，两边都有是居中
- 使用其他元素  
  嵌套使用即可
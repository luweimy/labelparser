labelparser
===========

####一个类XML标签解析库，将标签解析成Lua中的表结构####
* 被解析的富文本显示内容不能含有"<xxx>"这种样式的字符串, 单独的'<', '>'是可以支持的，但是为了避免解析错误最好不要使用
* 标签中可以存在属性，但是属性必须以空格分隔，如：<div fontname=nihao fontsize=28>hello,world</div>
* 标签中的属性值默认可以是字符串、数字、布尔值(true,false)，若是字符串可以加单引号，也可以不加，为了避免解析错误最好添加单引号
* 属性名和属性值之间用‘=’连接，但是不能存在空格
* 标签名和属性名不区分大小写，全部转化为小写(主要是为了避免大小写造成的书写错误，减小富文本串的编写难度)
* 标签属性值不允许存在空白字符，例如：<div fontname="ms yh.ttf"></div>
* 属性保留字'content', 不可重名，即不可使用属性名content，否则解析出来没有文本内容存在
* 若开头和结尾没有标签的话，内部会自动在最外层包上一层<div></div>(可以修改LABEL_DIV_BEGIN从而包上其他标签)
* 支持自闭和标签
* 标签范围不可交叉，但是可以嵌套包含
* 空的非自闭合标签没有任何意义，不会被解析，直接忽略
* 自闭合标签的'/>'与属性值之间最好存在空格,否则这种情况出错<img src=dir/s/>

[**详细讲解**](http://www.cnblogs.com/luweimy/p/4098380.html)

####示例：
```
local labelparser = require("labelparser")
local text1 = "hello worldd   <div>hello world</div> 你好 <div fontName='nihao' fontColore=#ff33ee>hello,world</div><div></div>"
local parsedtable = labelparser.parse(text1)
-- output:
<parsedtable> = {
    {
        content = "hello worldd   ",
        labelname = "div",
    },
    {
        content = "hello world",
        labelname = "div",
    },
    {
        content = " 你好 ",
        labelname = "div",
    },
    {
        content = "hello,world",
        fontname = "nihao",
        fontsize = "#123456",
        labelname = "div",
    },
}
```

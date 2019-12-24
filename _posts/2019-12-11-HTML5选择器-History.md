---
layout:     post
title:      HTML5选择器/History
subtitle:   HTML5选择器/History
date:       2019-12-11 12:03:16
tags: 
  - HTML5
  - 选择器
  - History
author: Felix
location: Beijing  
---
# HTML5选择器/History

## HTML5选择器
HTML4 选择器：
```javascript
//返回第一个指定Id属性的元素（常用）
document.getElementById("ID");
//返回所有指定Name属性的元素（不常用，使用场景少）
document.getElementsByName("Name");
document.getElementsByClassName("ClassName");
//返回所有指定标签属性的元素（不常用，使用场景少）
document.getElementsByTagName("TagName");
document.getElementsByTagNameNS("TagNameNS");
```
JQuery有很多方便的获取元素的API。
#### HTML5新增选择器
HTML5选择器是基于选择器规则进行查找，并且可以方便获取DOM元素，同时编写原生JavaScript更加方便。

 - querySelector：根据选择器规则，返回相匹配的第一个元素，没有找到则返回null。
 - querySelectAll：根据选择规则，返回文档中所有符合要求的元素，并且返回NodeList对象。

```javascript
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>学习</title>
</head>

<body>
    <p id="demo" class="title-color">欢迎来学习</p>
    <p id="demo1" class="title-color">欢迎来学习1</p>
    <p id="demo2" class="title-color">欢迎来学习2</p>
    <button onclick="myclick()">点我</button>
</body>
<script type="text/javascript">
    function myclick() {
        document.querySelector("p").innerHTML = "我的学习";
        document.querySelectorAll("p")[2].innerHTML = "修改最后一个";
    }
</script>

</html>
```
##### CSS基础选择器

 - 元素选择器 - 标记名{/*声明块*/}（所有与该标记名匹配的元素，都将应用声明块中的规则）
 - 类选择器 - **.**类名{/*声明块*/}（所有class属性为指定类名的元素，都将应用声明块中的规则，其中类型名可以写多个，方便代码复用所有标签都可以有class属性）
 - id选择器 - #id值{/*声明块*/}  \<h1 id="属性名">(属性id为指定值得元素，都将应用声明块中的规则，在同一个HTML文档中，元素的id值必须唯一)
 - 通配符选择器 - *+{/*声明块*/}
 - 并集选择器/组合选择器 - 元素或类或id + "," + 元素或类或id+ "," + 元素或类id "," +{/*声明块*/}

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /*======基础选择器======*/

        /*元素选择器*/
        div {}

        /*类选择器*/
        .my-class {}

        /*id选择器*/
        #test {}

        /*通配符选择器*/
        * {}

        /*并集选择器*/
        div,
        .my-class,
        #test {}

        /*组合选择器*/
        div.my-class {}

        div .my-class {}
    </style>
</head>
<body>
</body>
</html>
```
 /*组合选择器*/
 div.my-class {} 指的是dive元素并且class是my-class的元素则匹配上
 div .my-class {}指的是div元素里面的子孙元素如果有class为my-class则匹配上
##### 选择器之层次选择器

 - 子集选择器
```javascript
/*父级元素名称+ ">" + 子集元素名称 + {声明块}，仅仅影响到子级，孙子级不影响*/
div>p{color:red}
```
 - 后代选择器
```javascript
/*祖先元素名称+ 空格 + 后代元素名称 + {声明块}*，影响所有后代/
div p{color:red}
```
 - 兄弟选择器
```javascript
/*A元素名称+ "+" + B元素名称 + {声明块}*，影响跟在后面的第一个兄弟元素/
main_span + p{color:red}
```
 - 通用选择器
```javascript
/*同级元素A+ "~" + 同级元素B + {声明块}*， 影响所有跟在后面的兄弟元素/
main_span ~ p{color:red}
```
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .inline{
            float: left;
            padding: 20px;
        }
        /*======层次选择器======*/
        .parent1 > .childs{
            color:red;
        }
        .parent2 .childs{
            color:red;
        }
        .brother1 + .brothers{
            color: green;
        }
        .brother2 ~ .brothers{
            color: green;
        }
    </style>
</head>
<body>
    <div class="inline">
        <div class="parent1">
            <span class="childs">child1</span>
            <div>
                <span class="childs">grandchild1</span>
            </div>
        </div>
        <br>
        <div class="parent2">
            <span class="childs">child2</span>
            <div>
                <span class="childs">grandchild2</span>
            </div>
        </div>
    </div>
    <div class="inline">
        <div>
            <span class="brother1">brother1</span>
            <span class="brothers">brother1</span>
            <span class="brothers">brother1</span>
        </div>
        <br>
        <div>
            <span class="brother2">brother2</span>
            <span class="brothers">brother2</span>
            <span class="brothers">brother2</span>
        </div>
    </div>
</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122400270146.png)
##### 选择器之动态伪类选择器
 - 未访问
```javascript
a:link{color:black;}
```
 - 鼠标悬停
```javascript
a:hover{color:pink;}
```
 - 鼠标点击时
```javascript
a:active{color:yellow;}
```
 - 点击后
```javascript
a:visited{color:red;}
```
 - 获焦点
```javascript
a:focus{color:red;}
```
hover可以用在多个元素身上，其他通常使用在超链接上。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /*======选择器案例======*/
        .btn{
            width: 150px;
            height: 30px;
            text-align: center;
            line-height: 30px;
            border-radius: 3px;
            border: 1px solid #b1b1b1;
            background-color: #fff;
            color: #bbb;
            cursor: pointer;
        }
        .btn:hover{
            border: 1px solid #000;
            color: #000;
        }
    </style>
</head>
<body>
        <div class="btn">Submit</div>
</body>
</html>
```
##### 选择器之结构伪类选择器
 - 选中第一个
```javascript
/*格式: 元素名称+ ":first-child" + {声明块}*/
p:first-child{color:red;}
```
 - 选中最后一个
```javascript
/*格式: 元素名称+ ":last-child" + {声明块}*/
p:last-child{color:blue;}
```
 - 选中奇数项
```javascript
/*格式: 元素名称+ ":nth-child(odd)" + {声明块}*/
p:nth-child(odd){color:red;}
```
 - 选中偶数项
```javascript
/*格式: 元素名称+ ":last-child(even)" + {声明块}*/
p:nth-child(even){color:red;}
```

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /*======结构化伪类选择器案例======*/
        .table{
            width: 40%;
        }
        .table thead tr:first-child{
            background-color: #2d61af;
            color: #fff;
        }
        .table tbody tr:nth-child(3n){
            background-color: #abcdef;
        }
    </style>
</head>
<body>
    <table class="table" border="1">
        <caption>Optional table caption.</caption>
        <thead>
        <tr>
            <th>#</th>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Username</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <th scope="row">1</th>
            <td>Mark</td>
            <td>Otto</td>
            <td>@mdo</td>
        </tr>
        <tr>
                <th scope="row">2</th>
                <td>Jacob</td>
                <td>Thornton</td>
                <td>@fat</td>
            </tr>
            <tr>
                <th scope="row">3</th>
                <td>Larry</td>
                <td>the Bird</td>
                <td>@twitter</td>
            </tr>
            <tr>
                    <th scope="row">2</th>
                    <td>Jacob</td>
                    <td>Thornton</td>
                    <td>@fat</td>
                </tr>
                <tr>
                    <th scope="row">3</th>
                    <td>Larry</td>
                    <td>the Bird</td>
                    <td>@twitter</td>
                </tr>
                <tr>
                        <th scope="row">2</th>
                        <td>Jacob</td>
                        <td>Thornton</td>
                        <td>@fat</td>
                    </tr>
                    <tr>
                        <th scope="row">3</th>
                        <td>Larry</td>
                        <td>the Bird</td>
                        <td>@twitter</td>
                    </tr>
                    <tr>
                            <th scope="row">2</th>
                            <td>Jacob</td>
                            <td>Thornton</td>
                            <td>@fat</td>
                        </tr>
                        <tr>
                            <th scope="row">3</th>
                            <td>Larry</td>
                            <td>the Bird</td>
                            <td>@twitter</td>
                        </tr>
        </tbody>
    </table>
</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191225000409213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_5,color_FFFFFF,t_70)
##### 选择器之querySelector

```javascript
    //获取第一个<p>元素
    var p1 = document.querySelector("p");
    //获取class="title-color"的第一个元素
    var p2 = document.querySelector(".title-color");
    //获取class="title-color"第一个<p>元素
    var p3 = document.querySelector("p.title-color");
    //获取带有"data"属性的第一个<p>元素
    var p1 = document.querySelector("p[data]");
    //通过属性选择器取得data="1"元素
    var p1 = document.querySelector("[data='1']");
```
##### 选择器之querySelectorAll
```javascript
    //获取所有<p>元素
    var p1 = document.querySelectorALL("p");
    //获取class="title-color"的所有元素
    var p2 = document.querySelectorALL(".title-color");
    //获取class="title-color"所有<p>元素
    var p3 = document.querySelectorALL("p.title-color");
    //获取带有"data"属性的第一个<p>元素
    var p1 = document.querySelectorALL("p[data]");
    //通过属性选择器取得data="1"元素
    var p1 = document.querySelectorALL("[data='1']");
    //如果给多个元素修改样式，需要循环
    for(var i = 0; i < pAll4.length; i++){
        pAll4[i].style.border = "10px solid red";
    }
```
## HTML5 History
| 方法名 | 描述 |
|--|--|
| back | 前往上一页，用户可点击浏览器左上角的反回按钮模拟此方法。注：当浏览器会话历史记录处于第一页时调用此方法没有效果，而且也不会报错。等价：history.go(-1) |
| forward | 前往l浏览器历史记录下一页，用户可点击浏览器左上角的前进按钮模拟此方法。注：当浏览器会话历史记录处于最后一页时调用此方法没有效果，而且也不会报错。等价：history.go(1)  |
| go | 通过当前页面的相对位置从浏览器历史记录（会话记录）加载页面 |
#### HTML5 History定义以及新特性
history是历史状态管理，允许操作浏览器曾经在标签页或者框架里访问的会话历史纪录。
新增特性：
| 名称 | 描述 |
|--|--|
| pushState | 每执行一次都会增加一条历史记录，浏览器在返回时就不会返回前一个页面，并且不会刷新浏览器 |
| replaceState | 用来修改当前的历史记录，而不是创建一个新的历史记录，点击返回按钮照样会返回上一个页面 |
| onpopsState | 它是popstate在window对象上的事件，pushstate或者replacestate不会触发popstate事件，只会在点击后退，前进按钮或者调用history.back(), history.forward(), history.go()方法 |

##### pushState语法
history.pushState(state, title, url)
 - state:状态对象

 		记录状态，通过pushState创建新的历史记录并且存储起来
 		（FireFox存储有大小限制必须小于640k，否则会抛出异常）
 - title：标题（目前被忽略）

		FireFox目前忽略这个参数，但是未来可能用到。传递一个空字符串在这里是安全的，而将来是不安全的。
		二选一的话，你可以为跳转的state传递一个短标题。
 - url：需要修改的地址
 
 		重新定义新的url记录，浏览器不会立即加载，可以是相对路径并且必须要和当前URL同源，否则抛出异常；
 		执行完浏览器会更新，但是不会真的向服务器发送请求。
##### HTML5 History常用场景
最常用的场景之一：单页面应用

 - 单页应用定义： 只有一张Web页面的应用，并且一开始会加载必须的HTML，CSS和JavaScript，可以动态重写页面而不是通过服务器加载整个新页面来与用户交互，可以提高用户体验。
 - HIML5 History主要解决问题：实现网页无刷新更新数据的同时，解决浏览器无法前进/后退的问题
 - HIML5 History解决方案：pjax(ajax + pushState)等。
 
 
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <a href="https://www.163.com/">go to netease 163</a><br>
    <button onclick="back()">Back History</button>
    <button onclick="forward()">Forward History</button><br>
    <button onclick="push()">pushState</button>
    <script>
        function push(){
            var stateObj = { foo: 'bar' };
            window.history.pushState(stateObj, 'page 2', 'bar.html');
        }
        function back(){
            // 在history中向后跳转,等同于window.history.go(-1);
            window.history.back();
        }
        function forward(){
            // 在history中向前跳转,等同于window.history.go(1);
            window.history.forward();
        }
        window.onpopstate = function(event){
            console.log("location: " + document.location + ", state: " + JSON.stringify(event.state));
        }
    </script>
</body>
</html>
<!-- 
    Mac：
    chrome49以前版本
    open -a "Google Chrome" --args --disable-web-security
    chrome49以后版本
    open -a /Applications/Google\ Chrome.app --args --disable-web-security --user-data-dir
    Safari
    open -a '/Applications/Safari.app' --args --disable-web-security
    Window：
    chrome.exe --disable-web-security 
-->
```

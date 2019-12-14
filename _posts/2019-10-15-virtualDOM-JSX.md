---
layout:     post
title:      虚拟DOM和JSX
subtitle:   虚拟DOM和JSX
date:       2019-10-15 12:03:16
tags: 
  - 虚拟DOM和JSX
  - react
author: Felix
location: Beijing  
---
# 虚拟DOM和JSX

## 1 什么是虚拟DOM？

vdom可以看作是一个使用javascript模拟了DOM结构的树形结构，这个树结构包含整个DOM结构的信息，如下图：

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015002913180.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)       ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015002743675.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)

当我们在jsx文件中写这样的代码的时候，react会帮我们创建一个元素出来。 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015003419123.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015003431215.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)  
 

## 2 为什么要使用虚拟DOM？

之前使用原生js或者jquery写页面的时候会发现操作DOM是一件非常麻烦的一件事情，往往是DOM标签和js逻辑同时写在js文件里，数据交互时不时还要写很多的input隐藏域，如果没有好的代码规范的话会显得代码非常冗余混乱，耦合性高并且难以维护。

另外一方面在浏览器里一遍又一遍的渲染DOM是非常非常消耗性能的，常常会出现页面卡死的情况；所以尽量减少对DOM的操作成为了优化前端性能的必要手段，vdom就是将DOM的对比放在了js层，通过对比不同之处来选择新渲染DOM节点，从而提高渲染效率

## 3 react-diff

然后再来了解react的diff算法，

标签的改变
页面中一个标签的改变，react 会移除div 增加 span

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015003610276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)

 - 组件的改变

react会卸载Header组件 加载Content组件,测试代码

```javascript
import React from 'react'
 
class Header extends React.Component {
    render() {
        return (
            <div>
               header
            </div>
        )
    }
    componentDidMount() {
        console.log('Header mount')
    }
   componentWillUnmount() {
    console.log('Header unMount')
   }
}
class Footer extends React.Component {
    render() {
        return (
            <div>
               footer
            </div>
        )
    }
    componentDidMount() {
        console.log('Footer mount')
    }
    componentWillUnmount() {
    console.log('Footer unMount')
   }
}
// 条件渲染
class App extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            flag: true
        }
    }
    render() {
        return (
            <div>
               {
                   this.state.flag ? <Header/> : <Footer/>
               }
            </div>
        )
    }
}
export default App
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015003714544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)
 

在react-devtool中对状态改变，看到页面的打印结果

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015003728741.png)

 - key是react用来标记组件的。

1 列表的渲染验证 key的重要性，下面看没有key的的情况

```javascript
import React from 'react'
 
class ListItem extends React.Component {
    render() {
        return (
            <p>{this.props.item}</p>
        )
    }
    componentWillUpdate(nextPros, nextState) {
        console.log(`${this.props.item}更新`)
    }
    componentDidMount() {
        console.log(`${this.props.item}加载`)
    }
    componentWillUnmount() {
        console.log(`${this.props.item}卸载`)
    }
}
 
// 列表渲染
class App extends React.Component {
    constructor(props) {
        super(props)
        window.app = this
        this.state = {
            list: ['a', 'b', 'c' , 'd']
        }
    }
    render() {
        const { list } = this.state
        return (
            <div>
              {
                  list.map((item, index) => (
                    <ListItem item={item}/>
                  ))
              }
            </div>
        )
    }
}
export default App
```

这里的this.props.item是上一个props的

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015003810100.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)

 从第一个节点的组件开始比较，发现第一个组件，内容不同，更新一下，内容换成b,b组件也是一样，c,d组件卸载了。

加上key 

```javascript
render() {
        const { list } = this.state
        return (
            <div>
              {
                  list.map((item, index) => (
                    <ListItem 
                     key={item} item={item}/>
                  ))
              }
            </div>
        )
    }
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015003845625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)

渲染b组件的时候会去找key是b的组件，找到后更新，然后把a组件和d组件都卸载了

回忆一下组件更新的情况，父组件重新render 直接使用,每当父组件重新render导致的重传props，子组件将直接跟着重新渲染

2 用index作为key，react会把key相同的组件当作同一个组件

```javascript
import React from 'react'
 
class ListItem extends React.Component {
    render() {
        return (
             <li>{this.props.name} <input/></li>
        )
    }
    componentWillUpdate() {
        console.log(`${this.props.name}更新`)
    }
    componentDidMount() {
        console.log(`${this.props.name}加载`)
    }
    componentWillUnmount() {
        console.log(`${this.props.name}卸载`)
    }
}
class List extends React.Component {
    render() {
        return (
            this.props.list.map((item, index) => (
                <ListItem name={item} key={item}/>
              ))
        )
    }
    componentWillUpdate() {
        console.log(`list更新`)
    }
    componentDidMount() {
        console.log(`list加载`)
    }
    componentWillUnmount() {
        console.log(`list卸载`)
    }
}
 
// 列表渲染
class App extends React.Component {
    constructor(props) {
        super(props)
        window.app = this
        this.state = {
            list: ['a', 'b', 'c' ]
        }
    }
    render() {
        const { list } = this.state
        return (
            <div>
              <List list={list} />
            </div>
        )
    }
}
export default App
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015004009893.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015004018815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015004025565.png)


input内容没有改变，原因看下图

```javascript
{this.state.data.map((v,idx)=><Item key={idx} v={v} />)}
// 开始时：['a','b','c']=>
<ul>
    <li key="0">a <input type="text"/></li>
    <li key="1">b <input type="text"/></li>
    <li key="2">c <input type="text"/></li>
</ul>
 
// 数组重排 -> ['c','b','a'] =>
<ul>
    <li key="0">c <input type="text"/></li>
    <li key="1">b <input type="text"/></li>
    <li key="2">a <input type="text"/></li>
</ul>
```

数组重排以后，同一个key就是同一个组件，由于父组件render函数执行，所以不管子组件有没有发生变化，都会重新渲染。但由于有key的组件有标记，内容改变了，会重新更换内容， input节点没有发生改变，所有input 没有变。

3 我们把key={item},换个值，不使用index

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015004123891.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019101500413083.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)


```javascript
{this.state.data.map((v,idx)=><Item key={idx} v={v} />)}
// 开始时：['a','b','c']=>
<ul>
    <li key="a">a <input type="text"/></li>
    <li key="b">b <input type="text"/></li>
    <li key="c">c <input type="text"/></li>
</ul>
 
// 数组重排 -> ['c','b','a'] =>
<ul>
    <li key="c">c <input type="text"/></li>
    <li key="b">b <input type="text"/></li>
    <li key="a">a <input type="text"/></li>
</ul>
```

同一个相当于同一个组件，内容没有发生改变，不会重新渲染。所以inpput内容也不会发生变化。只是将整个组件移动了位置。
此处应注意 Component 和 PureComponent 区别，当使用 PureComponent 时 b，c，a 不会更新。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015004223808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015004232182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)
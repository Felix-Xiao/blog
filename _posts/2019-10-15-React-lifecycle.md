---
layout:     post
title:      React生命周期与数据操作
subtitle:   React生命周期与数据操作
date:       2019-10-15 12:03:16
author: Felix
location: Beijing
tags: 
  - React生命周期与数据操作
  - react
---
# React生命周期与数据操作

## **1 react生命周期**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191014235347560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)

 - componentWillMount:

在组件挂载到DOM前调用，且只会被调用一次，在这边调用this.setState不会引起组件重新渲染。

 - render

根据组件的props和state（无两者的重传递和重赋值，论值是否有变化，都可以引起组件重新render） ，return 一个React元素（描述组件，即UI），不负责组件实际渲染工作，之后由React自身根据此元素去渲染出页面DOM。render是纯函数（Pure function：函数的返回结果只依赖于它的参数；函数执行过程里面没有副作用），不能在里面执行this.setState，会有改变组件状态的副作用。

 - componentDidMount:

组件挂载到DOM后调用，且只会被调用一次

 - componentWillReceiveProps(nextProps)

只要父组件的render函数被调用，那么当前组件(子组件)都会触发这个函数，所以在此方法中根据nextProps和this.props来查明重传的props是否改变，以及如果改变了要执行啥

使用场景

```javascript
import React from 'react'
 
class ListItem extends React.Component {
    render() {
        return (
            <li>{this.props.name} <input/></li>
        )
    }
   
    componentWillUpdate(nextProps, nextState) {
        console.log(nextProps.name, this.props.name)
        console.log(`listItem更新`)
    }
    componentDidMount() {
        console.log(`listItem加载`)
    }
    componentWillUnmount() {
        console.log(`listItem卸载`)
    }
}
 
// 列表渲染
class App extends React.Component {
    constructor(props) {
        super(props)
        window.app = this
        this.state = {
            name: '麦乐',
            person: '花花'
        }
    }
    render() {
        const { name, person } = this.state
        return (
            <div>
              <p>{person}</p>
              <ListItem name={name}/>
            </div>
        )
    }
}
export default App
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191014235540676.png)
可以看到，我们并没有改变子组件的props，子组件也进行了一次更新，这是因为父组件的状态的改变，调用了render函数，导致了左右子组件的重新渲染。我们可以在这个函数内部判断，nextProps跟this.props是否相等 然后做相应的操作。不要这这个组件中发ajax请求，不要调用setState。

 - shouldComponentUpdate(nextProps, nextState)

此方法通过比较nextProps，nextState及当前组件的this.props，this.state，返回true时当前组件将继续执行更新过程，返回false则当前组件更新停止，以此可用来减少组件的不必要渲染，优化组件性能。就比如说上面的问题，父组件状态的改变，触发render，子组件也触发了render，不管props有没有发生改变。这是我们可以这样做性能优化。

子组件增加下面代码

```javascript
 shouldComponentUpdate(nextProps, nextState) {
        console.log(nextProps.name, this.props.name)
        if(nextProps.name === this.props.name) return false
        return true
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191014235620464.png)

可以看到子组件没有做不必要的更新。

componentWillUpdate(nextProps, nextState)
此方法在调用render方法前执行，在这边可执行一些组件更新发生前的工作，一般较少用。

 - render

render方法在上文讲过，这边只是重新调用。

 - componentDidUpdate(prevProps, prevState)

此方法在组件更新后被调用，可以操作组件更新的DOM，prevProps和prevState这两个参数指的是组件更新前的props和state

子组件中增加下面代码

```javascript
componentDidUpdate(prevProps, prevState) {
    console.log('prevProps.name',prevProps.name)
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191014235722296.png)

 - componentWillUnmount

此方法在组件被卸载前调用，可以在这里执行一些清理工作，比如清楚组件中使用的定时器，清楚componentDidMount中手动创建的DOM元素等，以避免引起内存泄漏。

 

## **2 造成组件重新渲染的情况**

1.父组件重新render

每当父组件重新render导致的重传props，子组件将直接跟着重新渲染，无论props是否有变化。可通过shouldComponentUpdate方法优化。

```javascript
class Child extends Component {
   shouldComponentUpdate(nextProps){ // 应该使用这个方法，否则无论props是否有变化都将会导致组件跟着重新渲染
        if(nextProps.someThings === this.props.someThings){
          return false
        }
    }
    render() {
        return <div>{this.props.someThings}</div>
    }
}
```

2.组件本身调用setState，无论state有没有变化。可通过shouldComponentUpdate方法优化

```javascript
class Child extends Component {
   constructor(props) {
        super(props);
        this.state = {
          someThings:1
        }
   }
   shouldComponentUpdate(nexProps, nextStates){ // 应该使用这个方法，否则无论state是否有变化都将会导致组件重新渲染
        if(nextStates.someThings === this.state.someThings){
          return false
        }
        return true
    }
 
   handleClick = () => { // 虽然调用了setState ，但state并无变化
        const preSomeThings = this.state.someThings
         this.setState({
            someThings: preSomeThings
         })
   }
 
    render() {
        return <div onClick = {this.handleClick}>{this.state.someThings}</div>
    }
}
```

## **3 几个主要生命周期的执行顺序**

```javascript
import React from 'react'
class GrandSon extends React.Component {
    render() {
        console.log('GrandSon render')
        return (
            <div>{this.props.name}GrandSon </div>
        )
    }
    componentWillMount() {
        console.log('GrandSon将要加载')
    }
    componentDidMount() {
        console.log('GrandSon加载完成')
 
    }
    componentWillUpdate(nextProps, nextState) {
        console.log('GrandSon更新')
    }
    componentDidUpdate(preProps, preState) {
        console.log('GrandSon更新完成')
    } 
    componentWillUnmount(preProps, preState) {
        console.log('GrandSon卸载')
    }
}
class Son extends React.Component {
    render() {
        console.log('son render')
        return (
            <div>{this.props.name} Son
                { this.props.name ? <GrandSon  name={this.props.name}/> : '' }
             </div>
        )
    }
    componentWillMount() {
        console.log('Son将要加载')
    }
    componentDidMount() {
        console.log('son加载完成')
 
    }
    componentWillUpdate(nextProps, nextState) {
        console.log('son更新')
    } 
    componentDidUpdate(preProps, preState) {
        console.log('Son更新完成')
    } 
    componentWillUnmount(preProps, preState) {
        console.log('son卸载')
    }
}
 
// 列表渲染
class App extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            name: '麦乐',
        }
    }
    render() {
        console.log('app render')
        const { name } = this.state
        return (
            <div>
                {name} app
              {
                this.state.name ? <Son name={name}/> : ''
              }
            </div>
        )
    }
    componentWillMount() {
        console.log('app将要加载')
    }
    componentDidMount() {
        window.app = this
        console.log('app加载完成')
 
    }
    componentWillUpdate(nextProps, nextState) {
        console.log('app更新')
    } 
    componentDidUpdate(preProps, preState) {
        console.log('app更新完成')
    } 
    componentWillUnmount(preProps, preState) {
        console.log('app卸载')
    }
}
export default App
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015000010739.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZlcnJpc18=,size_16,color_FFFFFF,t_70)

## **4 数据操作**

state 与 props 相同之处：
 - 改变会触发render函数（UI的改变）

state 与 props 不同之处：
 - state（writeable，readable）props （readable）
 - state（组件内部数据）props （来自父组件）

上下文：
使用场景：组件在嵌套比较深的时候，必须要每层使用props进行传递，redux，mobex使用了这种特性

```javascript
import React from 'react';
import PropTypes from 'prop-types'

//使用context传递参数 context打破了组件之间必须通过props传递状态的过程，类似一个全局变量空间，其内容能被随意接触修改。这样导致程序不可控。
class App extends React.Component {
	//用来设置组件的context
	getChildContext() {
		return {color: "purple"};
	}
	constructor(props){
		super(props)
		this.state = {
			name: 'son'
		}
	}
	render() {
		console.log('App rerender')
		return (
			<div>
				{this.state.name && <Son1 name={this.state.name}/>}
			</div>
		);
	}
}

//是为context中的字段设置类型检查，必须设置
App.childContextTypes = {
	color: PropTypes.string
}
class Son1 extends React.Component{
	render() {
		return (
			<div>
				{this.props.name && <GrandSon1 name={this.props.name + '-grand'}/>}
			</div>
		);
	}
}

class GrandSon1 extends React.Component{
	render() {
		console.log('GrandSon1 rerender')
		return (
			<div>
				{/* this.context.<key>即可获取 */}
				{this.props.name} - {this.context.color}
			</div>
		);
	}
}

//首先要类型检查，必须的
GrandSon1.contextTypes = {
	color: PropTypes.string
};
export default App;

```
页面可以正常显示 son-grand-purple
# react-router

React Router 是一个基于 [React](http://facebook.github.io/react/) 之上的强大路由库，它可以让你向应用中快速地添加视图和数据流，同时保持页面与 URL 间的同步。

## 安装

```j
npm i --save react-router-dom
```

## 引入

```js
import {HashRouter as Router,Route,Redirect,Switch,Link} from 'react-router-dom'
```



## 定义路由及重定向

```jsx
const MyRouter = ()=>(
	<Router>
        <Switch>
    		<Route path='/home' component={Home}></Route>
             <Route path='/user' component={User}></Route>
             <Redirect from='/' to='/home' exact ></Redirect>
        </Switch>
    </Router>
)
```

### Redirect

注意事项：exact 精确匹配 (Redirect 即使使用了exact, 外面还要嵌套Switch 来用)

### HashRouter

注意事项：Warning: Hash history cannot PUSH the same path; a new entry will not be added to the history stack,这个警告只有在hash 模式会出现。在NavLink 加上 replace 来解决



## 嵌套路由

```jsx
const MyRouter = ()=>(
	<Router>
        <Switch>
    		<Route path='/home' component={Home}></Route>
             <Route path='/user' component={User}></Route>
             <Route path='/right' render={()=>(
                	<Right>
                    	<Route path='/right/roles' component={Roles}></Route>
                        <Route path='/right/find' component={Find}></Route>
                    </Right>
                )}</Route>
             <Redirect from='/' to='/home' exact ></Redirect>
        </Switch>
    </Router>
)
```



## 路由跳转方式

声明式导航

```jsx
import React,{Component} from 'react'
import {NavLink} from 'react-router-dom'
export default class Right extends Component {
    render () {
        return (
        	<div>
            	<ul>
                	<li>
                    	<NavLink to='/right/roles'>roles</NavLink>
                    </li>
                    <li>
                    	<NavLink to='/right/find'>find</NavLink>
                    </li>
                </ul>
            </div>
        )
    }
}
```

### 编程式导航

```js
handleClick = (data)=>{
    this.props.history.push(`/right/preview/${data}`)
}
```

```jsx
<Route path='/home/:id'></Route>
```

```jsx
<div>{this.props.match.params.id}</div>
```



## 路由匹配原理

[路由](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#route)拥有三个属性来决定是否“匹配“一个 URL：

1. [嵌套关系](http://react-guide.github.io/react-router-cn/docs/guides/basics/RouteMatching.html#nesting) 和
2. 它的 [`路径语法`](http://react-guide.github.io/react-router-cn/docs/guides/basics/RouteMatching.html#path-syntax)
3. 它的 [优先级](http://react-guide.github.io/react-router-cn/docs/guides/basics/RouteMatching.html#precedence)

### 嵌套关系

React Router 使用路由嵌套的概念来让你定义 view 的嵌套集合，当一个给定的 URL 被调用时，整个集合中（命中的部分）都会被渲染。嵌套路由被描述成一种树形结构。React Router 会深度优先遍历整个[路由配置](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#routeconfig)来寻找一个与给定的 URL 相匹配的路由。

### 路径语法

路由路径是匹配一个（或一部分）URL 的 [一个字符串模式](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#routepattern)。大部分的路由路径都可以直接按照字面量理解，除了以下几个特殊的符号：

- `:paramName` – 匹配一段位于 `/`、`?` 或 `#` 之后的 URL。 命中的部分将被作为一个[参数](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#params)
- `()` – 在它内部的内容被认为是可选的
- `*` – 匹配任意字符（非贪婪的）直到命中下一个字符或者整个 URL 的末尾，并创建一个 `splat` [参数](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#params)

```js
<Route path="/hello/:name">         // 匹配 /hello/michael 和 /hello/ryan
<Route path="/hello(/:name)">       // 匹配 /hello, /hello/michael 和 /hello/ryan
<Route path="/files/*.*">           // 匹配 /files/hello.jpg 和 /files/path/to/hello.jpg
```

如果一个路由使用了相对`路径`，那么完整的路径将由它的所有祖先节点的`路径`和自身指定的相对`路径`拼接而成。[使用绝对`路径`](http://react-guide.github.io/react-router-cn/docs/guides/basics/RouteConfiguration.html#decoupling-the-ui-from-the-url)可以使路由匹配行为忽略嵌套关系。

### 优先级

最后，路由算法会根据定义的顺序自顶向下匹配路由。因此，当你拥有两个兄弟路由节点配置时，你必须确认前一个路由不会匹配后一个路由中的`路径`。例如，千万**不要**这么做：

```js
<Route path="/comments" ... />
<Redirect from="/comments" ... />
```
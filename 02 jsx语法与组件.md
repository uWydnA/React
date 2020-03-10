# jsx语法与组件

JSX 将 HTML 语法直接加入到 JavaScript 代码中，再通过翻译器转换到纯 JavaScript 后由浏览器执行。在实际开发中，JSX 在产品打包阶段都已经编译成纯 JavaScript，不会带来任何副作用，反而会让代码更加直观并易于维护。 编译过程由Babel 的 JSX 编译器实现。

> https://reactjs.org/docs/hello-world.html

## 组件首字母大写

组件首字母是大写 会被认为是自定义组件,首字母是小写，会被认为是 原生dom节点

```js
import App from './App.js'
import React from 'react'
import ReactDOM from 'react-dom'

ReactDOM.render(<App/>,document.getElementById("root"))
//ReactDOM.render(<div>hello react</div>,document.getElementById("root"))
//只要使用jsx,必须引入React
//babel将上面的<div>hello react</div>转换成 ==> React.createElement('div','hello')
```



## 组件标签

组件最外层需要被一个标签包裹，不能有兄弟节点

return (加上小括号,可以回车)

```js
import React,{Component} from 'react'
export default class App extends Component {
    render () {
        return (
        	<div> //组件最外层标签
            	<ul><li>hello</li></ul>
            	<p>world</p>
            </div>
        )
    }
}
```



## 组件可以嵌套

```js
import React,{Component} from 'react'
export default class App extends Component {
    render () {
        return (
        	<div> //组件最外层标签
            	<ul><li>hello</li></ul>
            	<p>world</p>
            	<Child/>
            </div>
        )
    }
}
class Child extends Component {
    render () {
        return (
        	<div>child</div>
        )
    }
}
```



## 函数式写法

```js
import React,{Component} from 'react'
export default class App extends Component {
    render () {
        return (
        	<div> //组件最外层标签
            	<ul><li>hello</li></ul>
            	<p>world</p>
            	<Child1/>
            	<Child2/>
            	<Child3/>
            </div>
        )
    }
}
class Child extends Component {
    render () {
        return (
        	<div>child1</div>
        )
    }
}
function Child2 () {
    return (<div>child2</div>)
}
const Child3 = ()=>{
    return (<div>child3</div>)
}
```



## 样式

### class

```html
<div class='box'></div> ==> <div className='box'></div> 
```

### label for

```html
<label for='box'></label> ==> <label htmlFor='box'></label>
```

### 行内样式

```js
let styleobj = {
	fontSize:'10px',
    color:'red'
}
...
<div style={styleboj}>111</div>
<div style={{background:'red',color:'yellow'}}>111</div>
```

### 引入格式

```js
import './css/index.css'
```



## 事件

### 箭头函数

```js
import React,{Component} from 'react'
export default class App extends Component {
    render () {
        return (
        	<input type='text'/>
            <button onClick={()=>{
            	console.log('click')
        	}}>Add1</button>
  			<button onClick={()=>{
            	this.handleClick('aaa','bbb')
        	}}>Add2</button>
        )
    }
    handleClick (x,y) {
        console.log(x+y)
    }
}
```

### bind改变this指向

```js
import React,{Component} from 'react'
export default class App extends Component {
    render () {
        return (
        	<input type='text'/>
            <button onClick={this.handleClick.bind(this,'aaa','bbb')}>Add</button>
        )
    }
    handleClick (x,y) {
        console.log('click',x+y)
    }
}
```

### ES6语法糖

```js
import React,{Component} from 'react'
export default class App extends Component {
    render () {
        return (
            <input type='text'/>
            <button onClick={this.handleClick}>Add</button>
        )
    }
    handleClick = ()=>{
        console.log('click')
    }
}
```



## react组件的数据挂载方式

### 属性(默认属性与属性验证)

### 状态

```js
import React,{Component} from 'react'
export default class App extends Component {
    state = {
        myname: 'retr0'
    }
    render () {
        return (
        	<div>
            	<p>{myname}</p>
            </div>
        )
    }
}
```


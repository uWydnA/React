# props

## 父子通信-父传子

```jsx
import React,{Component} from 'react'

class Navbar extends Component {
    render () {
        return (
            <button>back</button>
        	<div>Navbar</div>
            <button>{this.props.mytitle}</button>
        )
    }
}

export default class App extends Component {
    render () {
        return (
        	<div>
            	<Navbar mytitle='home'></Navbar>
                <Navbar mytitle='list'></Navbar>
                <Navbar mytitle='shopbar'></Navbar>
            </div>
        )
    }
}
```



## 属性验证

1. 引入'prop-types'模块
2. 利用static声明符定义类属性
3. 在类中定义一个属性：propTypes，值为一个对象

```jsx
import React,{Component} from 'react'
import MyPropType from 'prop-types'

class Navbar extends Component {
   static propTypes = {
       isShow:MyPropType.bool,
       mytitle:MyPropType.string
   }
    render () {
        return (
        	<div>
            	<button>left</button>
                <div>Navbar</div>
                {
                    this.props.isShow?<button>{this.props.mytitle}</button>:null
                }
            </div>
        )
    }
}

export default class App extends Component {
    render () {
        return (
        	<div>
                <Navbar mytitle='home' isShow={true}/>
                <Navbar mytitle='list' isShow={false}/>
                <Navbar mytitle='shopbar' isShow={'false'}/>
                {//这里传入的是字符串而非布尔值}
            </div>
        )
    }
}
```



## 默认属性

```jsx
import React,{Component} from 'react'
import MyPropTypes from 'prop-types'

class Navbar extends Component {
    static propTypes = {
        mytitle:MyPropTypes.string
    }
	static defaultProps = {
        mytitle:'Click'
    }
    render () {
        return (
        	<div>
            	<button>left</button>
                <div>Navbar</div>
                <button>{this.props.mytitle}</button>
            </div>
        )
    }
}

export default class App extends Component {
    render () {
        return (
        	<div>
            	<Navbar mytitle='Home'></Navbar>
                <Navbar></Navbar>
            </div>
        )
    }
}
```



## React自带的属性展开语法

```jsx
import React,{Component} from 'react'
import MyPropTypes from 'prop-types'

class Navbar extends Component {
    static propTypes = {
        mytitle:MyPropTypes.string,
        myshow:MyPropTypes.bool
    }
	static defaultProps = {
        mytitle:'Click',
        myshow:true
    }
    render () {
        return (
        	<div>
            	<button>left</button>
                <div>Navbar</div>
                <button>{this.props.mytitle}</button>
            </div>
        )
    }
}

export default class App extends Component {
    render () {
        let obj = {
            mytitle:'list',
            myshow:false
        }
        return (
        	<div>
            	<Navbar mytitle='Home'></Navbar>
                <Navbar></Navbar>
                <Navbar {...obj}/>
            </div>
        )
    }
}
```


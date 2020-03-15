# slot

插槽的应用场景大致分为2种：

## 子组件通信-this.props.children

插槽可以很好地把按钮的事件处理函数留在父组件，但是插槽又可以将按钮插入子组件，非常方便的实现了2个子组件之间的通信

```jsx
import React,{Component} from 'react'

class Navbar extends Component {
    render () {
        return (
        	<div>
            	{this.props.children}
            </div>
        )
    }
}

class Sidebar extends Component {
    render () {
        return (
        	<div>
            	Sidebar
            </div>
        )
    }
}

export default class App extends Component {
    state= {
        isShow:true
    }
    render () {
        return (
        	<div>
            	<Navbar>
                	<button onClick={()=>{
                            this.setState({
                                isShow:!this.state.isShow
                            })
                        }}}></button>
                </Navbar>
                {
                    this.state.isShow?<Sidebar/>:null
                }
            </div>
        )
    }
}
```



## 嵌套路由-this.props.children

插槽可以很好地时间路由配置中，在父路由js中通过{this.props.children}实现子路由的插入

router.js

```jsx
import React,{Component} from 'react'
import {HashRouter as Router,Route,Redirect,Switch} from 'react-router-dom'
import Home from '....'
import Homelist from '....'
import Homenav from '....'
export default class MyRouter extends Component {
    render () {
        return (
        	<Router>
            	<Swtich>
                	<Route path='/home' render={()=>(
                    	<Home>
                      		<Route path='/home/list' compoennt={Homelist}></Route>
                             <Route path='/home/nav' compoennt={Homenav}></Route>
                        </Home>
                    )}></Route>
                </Swtich>
            </Router>
        )
    }
}
```

Home.js

```jsx
import React,{Component} from 'react'
export default class Home extends Component {
    render () {
        return (
        	<div>
            	Home
                {this.props.children} {//通过this.props.children即可引入路由中定义的component={}中的组件}
            </div>
        )
    }
}
```


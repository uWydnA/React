# onEvent

```jsx
import React,{Component} from 'react'

class Navbar extends Component {
    render () {
        return (
        	<div>
            	<button onClick={this.handleClick}>add</button>
            </div>
        )
    }
    handleClick = ()=>{
        this.props.onEvent() //通过调用父的函数，实现由子组件控制父组件状态的变化
    }
}

class Slidebar extends Component {
    render () {
        return (
        	<div>
            	<ul>
                	<li>1111</li>
                    <li>2222</li>
                    <li>3333</li>
                </ul>
            </div>
        )
    }
}

export default class App extends Component {
    state = {
        isShow:true
    }
    render () {
        return (
        	<div>
            	<Navbar onEvent={()=>{
                        this.setState({
                            isShow:!this.state.isShow
                        })
                    }}/>
                {
                    this.state.isShow?<Slidebar></Slidebar>:null
                }
            </div>
        )
    }
}
```


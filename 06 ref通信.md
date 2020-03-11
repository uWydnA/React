# ref通信

ref标记 (父组件拿到子组件的引用，从而调用子组件的方法)

```jsx
import React,{Component} from 'react'

class Navbar extends Component {
        state = {
            mytext = ''
        }
		reset= ()=>{
            this.setState({
                mytext:''
            })
		}
        render () {
            return (
                <div>
                    <input type='text' onChange={this.saveText}/>
                </div>
            )
        }
        saveText= (eve)=>{
            this.setState({
                mytext:eve.target.value
        })
    }
}

export default class App extends Component {
    render () {
        return (
        	<div>
                 {//建立子组件ref的引用}
            	<Navbar ref='text'/> 
              
                <button onClick={this.handleClick}>add</button>
            </div>
        )
    }
    handleClick = ()=>{
        console.log(this.refs.text.state.mytext){//查询子组件的state}
        this.refs.text.reset(){//调用子组件的方法}
    }
}
```


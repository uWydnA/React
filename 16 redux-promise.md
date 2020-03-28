# redux-promise

## 安装

```
npm i --save redux-promise
```

## 引入

```js
import ReduxPromise from 'redux-promise'
```

## store.js

```js
import {createStore,applyMiddleware} from 'redux'
const reducer = ()=>{
    ....
}
const store = createStore(reducer,applyMiddleware(ReduxPromise))
```

## 组件.js

```jsx
import React,{Component} from 'react'
import store from '.....'

class App extends Component {
    actionCreator = ()=>{
        return ( //返回一个promise对象
        	axios.....
        )
    }
    componentWillMount(){
    	if(store.getState().xxxx.length===0){
            //  store.dispath只能接受一个普通对象
            store.dispatch(this.actionCreator())
            .then(res=>{
                this.setState({
                 	...:res.payload
                })
            })
        }else{
           	this.setState({
                xxx:store.getState().xxxx
            })
        }
	}
    render () {
        return (
        	<div></div>
        )
    }
}
```




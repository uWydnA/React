## redux-thunk

# redux-promise

## 安装

```
npm i --save redux-thunk
```

## 引入

```js
import ReduxThunk from 'redux-promise'
```

## store.js

```js
import {createStore,applyMiddleware} from 'redux'
const reducer = ()=>{
    ....
}
const store = createStore(reducer,applyMiddleware(ReduxThunk))
```

## 组件.js

```jsx
import React,{Component} from 'react'
import store from '.....'

class App extends Component {
    actionCreator = ()=>{
        return dispatch =>{  //返回一个函数,并传了一个dispath函数到回调函数的形参中
            axios....then(res=>{
                dispatch(...) //就可以在任意自己想return的地方运行dispatch函数
            })
        }
    }
    componentWillMount(){
    	if(store.getState().xxxx.length===0){
            //  store.dispath只能接受一个普通对象
            store.dispatch(this.actionCreator())
        }else{
           	this.setState({
                xxx:store.getState().xxxx
            })
        }
        //要抓第一次redux改变的的时候，因此要订阅一下
        this.unsub = store.subscribe({
            this.setState({
            	...:store.getState().xxxx
        	})
        })
	}
    render () {
        return (
        	<div></div>
        )
    }
}
```




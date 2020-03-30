# React Hooks

## 使用hooks的理由

1. 高阶组件为了复用，导致代码层级复杂
2. 生命周期的复杂
3. 写成functional组件,无状态组件 ，因为需要状态，又改成了class,成本高

## useState

```js
import React , {useState} from 'react'
```

```jsx
export default function App () {
    const [name,setname] = useState('xiaoming')
    const changeName = ()=>{
		setname('retr0')
    }
	return (
    	<div>
        	{name}
            <button onClick={changeName}></button>
        </div>
    )
}
```

## useRef

```js
import React, {useRef} from 'react'
```

```jsx
export default function App () {
    const ref = useRef(null)
    return (
    	<div>
        	<input type='text' ref={ref}/>
             <button onClick={()=>{
                   	console.log(ref.current)
                }}></button>
        </div>
    )
}
```



## useEffect

> Function Component 不存在生命周期，所以不要把 Class Component 的生命周期概念搬过来试图对号入座。
>
> useEffect(处理函数，[依赖]) 副作用方法:每次依赖改变都会执行一次，如果没有依赖只有一个空数组，那么只会执行一次（有点像componentDidmount）

### 参数

1. 回调函数
2. 数组
   - 不传数组：无法起到只渲染一次的作用
   - 空数组：一次生命周期中只调用一次回调函数
   - [text,name]：如果text状态或者name状态变化，就会调用该回调函数

```js
import React , {useState,useEffect} from 'react'
```

```jsx
export default function App () {
    const [name,setname] = useState('xiaoming')
    useEffect(()=>{
        let index =1
        let t = setInterval(()=>{
            console.log(++index)
        },500)
        return ()=>{
            console.log('销毁')
            clearInterval(t)
        }
    },[])
    return (
    	<div></div>
    )
}
```

不要对 Dependencies 撒谎, 如果你明明使用了某个变量，却没有申明在依赖中，你等于向 React撒了谎，后果就是，当依赖的变量改变时，useEffect 也不会再次执行, eslint会报警告

Preview页面改造成函数式组件，在路径上从id=1切换到id=2也会自动重新加载，比class组件方便,class组件需要通过componentWillMout和

```jsx
useEffect(()=>{
    axios.get(`/articles/${props.match.params.id}`)
    .then(res => {
        settitle(res.data.title)
        setcontent(res.data.content)
        setcategory(res.data.category)
    })},[props])
```



## useCallback

> 防止因为组件重新渲染，导致方法被重新创建，起到缓存作用; 只有第二个参数变化了，才重新声明一次

```js
import React , {useState,useCallback} from 'react'
```

```jsx
export default function App () {
    const [text,settext] = useState('xxx')
    useCallback(()=>{
        console.log(text)
    },[text])
    return (
    	<div>
            {text}
            <button onClick={()=>{
                    settext('retr0')
                }}></button>
        </div>
    )
}
```



## useReducer和useContext

> redux-react-hook 无缝使用原来的 redux,和中间件 promise,thunk,saga

```js
import React, {useReducer,useContext} from 'react'
import reducer from './reducer'
const GlobalContext = React.createContext()
```

### reducer.js

```js
// 纯函数设计 reudx中的reducer一个概念
const reducer = (prevState,action)=>{
  let {type,payload} = action
  switch(type){
    case "isShow":
      //深复制老状态，返回新状态
    return {
      ...prevState,
      isShow:payload
    } // immutable
    case "list":
      //深复制老状态，返回新状态
    return {
      ...prevState,
      list:payload
    }
  }
}
export default reducer
```

### App.js

```jsx
const Child1 = ()=>{
    const {state,dispatch} = useContext(GlobalContext)
   	return (
    	<div>
        	<button onClick={()=>{
                    dispatch({
                        type:'isShow',
                        payload:!state.isShow
                    })
                }}></button>
        </div>
    )
}
const Child2 = ()=>{
    
}
export default function App () {
    let [state,dispatch] = useReducer(reducer,{
        isShow:false,
        list:[]
    })
    return (
        <GlobalContext.Provider value={{
                state,
                dispatch
            }}>
        	<Child1/>
            {
                state.isShow?<Child2/>:null
            }
        </GlobalContext.Provider>
    )
}
```



## 自定义hooks

> 当我们想在两个函数之间共享逻辑时，我们会把它提取到第三个函数中。必须以“use”开头吗？必须如此。这个约定非常重要。不遵循的话，由于无法判断某个函数是否包含对其内部 Hook 的调用，React 将无法自动检查你的 Hook 是否违反了 Hook 的规则

```jsx
import React, { useState, useEffect } from 'react'
import axios from 'axios'
import { PageHeader } from 'antd';
import store from '../../mobx/store'
//为preview提供数据
const usePreviewData = (props)=>{
  const [title, setTitle] = useState('')
  const [category, setCategory] = useState('')
  const [content, setContent] = useState('')
  useEffect(() => {
    store.set('isShow', false)
    axios.get(`http://localhost:12138/articles/${props.match.params.myid}`).then(res => {
      let { title, category, content } = res.data
      setTitle(title)
      setCategory(category)
      setContent(content)
    })
  }, [props.match.params.myid])
  return {
    title,
    category,
    content
  }
}
export default function HookPreview(props) {
   // console.log(this.props);//函数式组件从函数的形参中就能拿到属性
  let {title,category,content} = usePreviewData(props)
  return (
    <div>
      <PageHeader
        className="site-page-header"
        onBack={() => {
          props.history.goBack()
        }}
        title={title}
        subTitle={category ? category.join('/') : null}
      />
      <div style={{ padding: '24px' }} dangerouslySetInnerHTML={{
        __html: content
      }}>
      </div>
    </div>
  )
}

```


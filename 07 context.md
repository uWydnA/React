# context

> Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。

在一个典型的 React 应用中，数据是通过 props 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。

## 优缺点

###  优点

跨组件访问数据

###  缺点

react组件树种某个**上级组件shouldComponetUpdate返回false**,当 context更新时，**不会引起下级组件更新**



## React.createContext

```
const MyContext = React.createContext(defaultValue);
```

创建一个 Context 对象。当 React 渲染一个订阅了这个 Context 对象的组件，这个组件会从组件树中离自身最近的那个匹配的 `Provider` 中读取到当前的 context 值。

**只有**当组件所处的树中没有匹配到 Provider 时，其 `defaultValue` 参数才会生效。这有助于在不使用 Provider 包装组件的情况下对组件进行测试。注意：将 `undefined` 传递给 Provider 的 value 时，消费组件的 `defaultValue` 不会生效。

### ` `

## Context.Provider

```
<MyContext.Provider value={/* 某个值 */}>
```

每个 Context 对象都会返回一个 Provider React 组件，它允许消费组件订阅 context 的变化。

Provider 接收一个 `value` 属性，传递给消费组件。一个 Provider 可以和多个消费组件有对应关系。多个 Provider 也可以嵌套使用，里层的会覆盖外层的数据。

当 Provider 的 `value` 值发生变化时，它内部的所有消费组件都会重新渲染。Provider 及其内部 consumer 组件都不受制于 `shouldComponentUpdate` 函数，因此当 consumer 组件在其祖先组件退出更新的情况下也能更新。

通过新旧值检测来确定变化，使用了与 [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#Description) 相同的算法。



## Context.Consumer

注意：Context.Consumer内必须是回调函数，通过context方法改变根组件状态

```
<MyContext.Consumer>
  {value => /* 基于 context 值进行渲染*/}
</MyContext.Consumer>
```

这里，React 组件也可以订阅到 context 变更。这能让你在[函数式组件](https://react.docschina.org/docs/components-and-props.html#function-and-class-components)中完成订阅 context。

这需要[函数作为子元素（function as a child）](https://react.docschina.org/docs/render-props.html#using-props-other-than-render)这种做法。这个函数接收当前的 context 值，返回一个 React 节点。传递给函数的 `value` 值等同于往上组件树离这个 context 最近的 Provider 提供的 `value` 值。如果没有对应的 Provider，`value` 参数等同于传递给 `createContext()` 的 `defaultValue`。



## 实例

```jsx
import React,{Component} from 'react'
const myContext = React.createContext()

class Child extends Component{
  UNSAFE_componentWillUpdate (){
    console.log('componentWillUpdate1');
  }
  componentDidUpdate () {
    console.log('componentDidUpdate1');
  }
  render () {
    return (
      <myContext.Consumer>
        {
          context=>(
            <div>
              child -- {context.myname} -- {context.age}  -- {context.sex}
              <button onClick={this.handleClick.bind(this,context)}>change</button>
            </div>
          )
        }
      </myContext.Consumer>
    )
  }
  handleClick = (context)=>{
    context.changeState('xiaoming')
  }
}

export default class App extends Component {
  state={
    myname:'retr0'
  }
  UNSAFE_componentWillUpdate (){
    console.log('componentWillUpdate');
  }
  componentDidUpdate () {
    console.log('componentDidUpdate');
  }
  shouldComponentUpdate (nextProps,nextState) {
    return !(this.state.myname===nextState.myname)
  }
  changeState = (data)=>{
    this.setState({
      myname:data
    })
  }
  render () {
    return (
      <myContext.Provider value={{
        myname:this.state.myname,
        age:12,
        sex:1,
        changeState:this.changeState
      }}>
         <div>
          app
          <Child></Child>
        </div>
      </myContext.Provider>
    )
  }
}

```


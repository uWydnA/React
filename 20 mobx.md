# mobx

> https://cn.mobx.js.org/

## 介绍

1. Mobx是一个功能强大，上手非常容易的状态管理工具。
2. Mobx背后的哲学很简单: 任何源自应用状态的东西都应该自动地获得。
3. Mobx利用getter和setter来收集组件的数据依赖关系，从而在数据发生变化的时候精确知道哪些组件需要重绘，在界面的规模变大的时候，往往会有很多细粒度更 新。

## Mobx与redux的区别

- Mobx写法上更偏向于OOP 
- 对一份数据直接进行修改操作，不需要始终返回一个新的数据 
- 并非单一store,可以多store
-  Redux默认以JavaScript原生对象形式存储数据，而Mobx使 用可观察对象

### 优点

- 学习成本小 
- 面向对象编程 

### 缺点

- 过于自由：Mobx提供的约定及模版代码很少，代码编写很自由，如果不做一些约定，比较容易导致团队代码风格不统一
- 相关的中间件很少，逻辑层业务整合是问题。



## observable

```js
import {observable} from 'mobx'
```

### .box

```js
const store = observable.box(true) //生成一个可以观察的布尔值,box只能观察简单数据类型
```

### .map

```js
const store = observable.map({
    isShow:true,
    age:12,
    roleList:[],
    rightList:[]
})
//如果要观察复杂数据类型，需要用map方法
```



## autorun

```js
import {autorun} from 'mobx'
import store from '../../mobx/store'
```

```js
autorun(()=>{
	console.log(store.get())
})
```



## 实例

```js
import { observable, autorun } from 'mobx';
const value = observable.box(0);
const number = observable.box(100);
autorun(() => {
	console.log(value.get());
});
value.set(1);
value.set(2);
number.set(101);
//0,1,2。 // autorun 使用到才能被执行
//只能是同步，异步需要处理
//观察对象，通过map
const map = observable.map({ key: "value"});
//map.set("key", "new value");
//map.get("key")
//观察对象，不通过map
const map = observable({ key: "value"});
// map.key map.key="xiaoming"
//观察数组
const list = observable([1, 2, 4]);
list[2] = 3;

```



## mobx取消绑定

```js
componentWillMount () {
    this.cancel = autorun(()=>{
		this.setState({
            isShow:store.get('isShow')
        })
	})
}
componentWillUnMount () {
    this.cancel()
    //或者直接更改setState函数，这样就不会触发上一个组件销毁，但上一个组件中定义的autorun没销毁，导致进行setState操作，但上个组件已经销毁，因此会报一个warning：无法设置一个状态到unmount的组件
}
```


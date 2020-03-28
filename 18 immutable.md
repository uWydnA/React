# immutable

> https://github.com/immutable-js/immutable-js

## 介绍

每次修改一个 Immutable 对象时都会创建一个新的不可变的对象，在新对象上操作并不会影响到原对象的数据。这个库的实现是深拷贝还是浅拷贝？



## 深拷贝和浅拷贝的关系

1. ```js
   var arr = {};
   var arr2 = arr
   //浅拷贝，引用地址相同
   ```

2.  Object.assign() 只是一级属性复制，比浅拷贝多深拷贝了一层而已。

3. ```js
   const obj1 = JSON.parse(JSON.stringify(obj))
   //数组，对象都好用(缺点: 不能有undefined)
   ```



## Immutable优化性能的方式

Immutable 实现的原理是 Persistent Data Structure（持久化数据结构）， 也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 Structural Sharing（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。

https://upload-images.jianshu.io/upload_images/2165169-cebb05bca02f1772

```js
import {Map} from 'immutable'
let a = Map({
    select:'users',
    filter:Map({name:'Cam'})
})
let b = a.set('select','people');
a===b; //false
a.get('filter') === b.get('filter'); // true
```

延伸：如果上述select 属性 给一个组件用，因为此值改变了，shouldComponentUpdate 应该返回true, 而filter 属性给另一个组件用，通过判断并无变化，shouldComponentUpdate 应该返回false，此组件就避免了重复进行diff对比



## Immutable中常用类型（Map，List）

### Map

```js
const {Map} = require("immutable")
const preState = {
  a:1,
  b:2
}
let map1 = Map(preState)
let map2 = map1.set('b','3')
console.log(map1.toJS(),map2.toJS());
//{ a: 1, b: 2 } { a: 1, b: '3' }
```

### List

```js
const {List} = require("immutable")
const preState = ['111','222','333']
let arr1 = List(preState)
//List内置绝大部分的数组方法
let arr2 = arr1.push(444)
console.log(arr1.toJS(),arr2.toJS());
//['111','222','333']   ['111','222','333',444]
```

### merge

```js
const {Map} = require("immutable")
const preState = {
  a:1,
  b:2
}
let map1 = Map(preState)
let map2 = map1.merge({name:'admin'})
console.log(map1.toJS(),map2.toJS());
//{ a: 1, b: 2 } { a: 1, b: 2, name: 'admin' }
```

### concat

```js
const {List} = require("immutable")
const preState = ['111','222','333']
let arr1 = List(preState)
//List内置绝大部分的数组方法
let arr2 = arr1.concat([444,555,666])
console.log(arr1.toJS(),arr2.toJS());
//['111','222','333'] ['111','222','333',444,555,666]
```

### fromJS

```js
const {fromJS} = require("immutable")
const prevState = {
  name:'retr0',
  age:1,
  location:{
    city:1,
    sss:2,
    ddd:{
      aaa:34
    }
  }
} 
let map1 = fromJS(prevState)
let map2 = map1.setIn(['location','city'],231231)
console.log(map2.toJS());
/*
	{
      name: 'retr0',
      age: 1,
      location: { city: 231231, sss: 2, ddd: { aaa: 34 } }
  	}
*/
```



## Immutable+Redux的开发方式

```js
import {fromJS} from 'immutable'
const righteducer = (prevState=[],action)=>{
  let {type,payload} = action;
  switch(type){
    case "right":
      let newState = fromJS(prevState).concat(payload).toJS()
      return newState
    default:return prevState
  }
  
}
export default righteducer
```



## 缺点

1. 容易跟原生混淆
2. 文档与调试不方便
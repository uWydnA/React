# redux-persist

> 实现redux持久化，存到localStorage

## 安装

```
npm install redux-persist
```

## 基础使用

```js
// configureStore.js

import { createStore } from 'redux'
import { persistStore, persistReducer } from 'redux-persist'
import storage from 'redux-persist/lib/storage' // defaults to localStorage for web

import rootReducer from './reducers'

const persistConfig = {
  key: 'root',
  storage,
}

const persistedReducer = persistReducer(persistConfig, rootReducer)

export default () => {
  let store = createStore(persistedReducer)
  let persistor = persistStore(store)
  return { store, persistor }
}
```

## store.js

```js
import {createStore,applyMiddleware,combineReducers,compose } from 'redux' //createStore 方法创建一个store对象
//创建一个reducer，“修改状态”，（老状态，修改的值）深复制之后，再返回一个新的状态
import reduxThunk from 'redux-thunk'
import reduxPromise from 'redux-promise'
import roleListReducer from './reducess/role'
import rightListReducer from './reducess/right'
import sliderReducer from './reducess/slider'
import { persistStore, persistReducer } from 'redux-persist'
import storage from 'redux-persist/lib/storage' // defaults to localStorage for web
const persistConfig = {
  key: 'root',
  storage,
}

const reducer = combineReducers({
  isCollapsed : sliderReducer,
  rightList:rightListReducer,
  roleList:roleListReducer
})
const persistedReducer = persistReducer(persistConfig, reducer) //把reducer进行包裹


//store，只能接受一个reducer
//app开发，只能有一个store
//轮船运货车（reducer拆开一个一个，每个reducer一个文件，combineReducer：合并所有的reducer）
//redux devtools的配置：Advanced store setup
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(persistedReducer,/* preloadedState, */ composeEnhancers(applyMiddleware(reduxThunk,reduxPromise)))
const persistor = persistStore(store)
//默认action是普通对象
//创建store，顺便应用中间件thunk,如果action是函数，我来处理
//创建store，顺便应用中间件promise,那么action就变成了promise对象
export  {
  store,
  persistor
}
```


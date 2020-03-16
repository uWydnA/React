# json server

## 安装

Install JSON Server

```
npm install -g json-server
```

## 创建json

Create a `db.json` file with some data

```
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

## 开启服务

Start JSON Server

```
json-server --watch db.json --port 8080
```



## 配合axios

```js
axios({
    url:'http://localhost:8080/users',
    methods:'xxx',
    data:{
        xxx:'xxx'
    }
})
```


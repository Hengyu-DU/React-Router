# 第五章 React 路由
## 一、相关理解

### SPA
1. 单页Web应用（Single Page Web Application SPA）
2. 整个应用只有**一个完整的页面**
3. 点击页面中的链接**不会刷新**页面，只会做页面的**局部更新**
4. 数据都需要通过ajax请求获取，并在前端异步展现

### 路由
1. 什么是路由？
    1. 一个路由就是一个映射关系（key:value）
    2. key为路径，value可能是function或component
2. 路由分类
    1. 后端路由：
       - 理解： value 是 function, 用来处理客户端提交的请求。
       - 注册路由：router.get(path, function(req, res))
       - 工作过程：当node接收到一个请求时，根据请求路径找到匹配的路由，调用路由中的函数来处理请求，返回响应数据。
    2. 前端路由：
       - 浏览器端路由，value是component，用于展示页面内容
       - 注册路由：<Route path="/test" component={Test}>
       - 工作过程：当浏览器的path变成/test时，当前路由组件就会变成Test组件

### React-router（-DOM） 的理解
1. react 的一个插件库
    - 一共有三个库 web\native\anywhere
2. 专门用来实现一个SPA应用
3. 基于react的web项目基本都会用到这个库。

## 二、路由的基本使用
1. 明确好界面中的导航区、展示区
2. 导航区的a标签改为link标签 => 编写路由链接
    <Link to="/abc">Demo</Link>
3. 展示区写Route标签进行路径的匹配 => 注册路由
    <Route path='/abc' component={Demo}/>
4. \<App> 的最外侧包裹了一个<BrowserRouter>或<HashRouter>

## 三、路由组件与一般组件
1. 写法不同：
    * 一般组件：<Demo/>
    * 路由组件：<Route path='/demo' component={Demo}/>
2. 存放位置不同：
    * 一般组件：components
    * 路由组件：pages
3. 接收到的props不同：
    * 一般组件：写组件标签时传递了什么，就能收到什么
    * 路由组件：接收到三个固定的属性：
    ```
    history:
        go: ƒ go(n)
        goBack: ƒ goBack()
        goForward: ƒ goForward()
        push: ƒ push(path, state)
        replace: ƒ replace(path, state)
    location:
        pathname: "/about"
        search: ""
        state: undefined
    match:
        params: {}
        path: "/about"
        url: "/about"
    ```

## 四、NavLink的使用

### 1. 基本使用：
首先引入：
import {NavLink} from 'react-router-dom'

```jsx
 <NavLink activeClassName='atguigu' className='list-group-item' to="/about">About</NavLink>
 <NavLink activeClassName='atguigu' className='list-group-item' to="/home">Home</NavLink>
```
* activeClassName属性用来指定active状态下的类名，默认叫active
* 样式如果被bootstrap覆盖，则加 !important ：

```css
.atguigu{
        background-color: cornflowerblue !important;
        color: darkblue !important;
        font-weight: bold !important;
      }
```

### 2. 封装NavLink

封装一个MyNavLink：

1. index.js 中引入 NavLink
```jsx
  import React, { Component } from 'react'
  import { NavLink } from 'react-router-dom'

  export default class MyNavLink extends Component {
    render() {
      return (
        <NavLink activeClassName='atguigu' className='list-group-item' {...this.props} />
      )
    }
  }
```
* {...this.props} 作为一个对象展开，其中包括children属性（标签体的内容，this.props.children）

2. App.jsx 中：
只需将不同的部份以props的形式传入，即可完成封装组件的调用。
```js
  <MyNavLink to="/about">About</MyNavLink>
  <MyNavLink to="/home">Home</MyNavLink>
```


## 五、Switch的使用
用于解决路由单一匹配的效率问题，在相同路由下只会匹配一次。
引入Switch：
import {Switch, Route} from 'react-router-dom'

用<Switch>标签包裹注册的路由：
```jsx
  {/* 注册路由 */}
    <Switch>
      <Route path="/about" component={About}/>
      <Route path="/home" component={Home}/>
      <Route path="/home" component={Test}/>
    </Switch>
```

## 六、解决样式丢失的问题
在路由地址添加多级路径时，二次刷新网页会导致样式丢失，
解决方法一：
    public/index.html中css引入时，去掉绝对地址的./，直接用/
解决方法二：
    public/index.html中css引入时，绝对路径以：  %PUBLIC_URL%   开头，代表public文件夹
解决方法三：（不常用）
    index.js中引入{HashRouter},而非{BrowserRouter}.
  
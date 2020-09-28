# react 
1. 创建项目 
npx create-react-app 项目名  
2. 启动项目
cd 项目名  npm start/yarn start 

3. 项目目录
 nodemoules  依赖的包 
 public 静态资源 
 src  主要文件
 gitignore git忽略文件
 package.json  依赖包的版本 脚本运行  可通过npm i 把包下回来   
4. 入口文件是src/index.js  
   src下只保留index.js
5. window.root 拿到是index.html里面的divid为root元素  
window.root = document.getElementById('root')
document.createTextNode 创建文本节点 
6. 同级元素不想用标签包裹的时候 可以用 <></> 去包裹 
  和 <React.Fragment> 的使用完全相等 
	---
// react 主要负责逻辑层 
// reactdom 主要负责渲染  
- jsx js +xml(html)  的特点
 1. 只能有一个根元素
 2. jsx的特性  <> 表示html标签 {} 表示js 
 3. class className js里面class是关键字  
 4. label 的for 属性改成htmlFor  for是关键字  
 5. style => 对象形式{fontSize:"16px",color:"red"}  双括号 第一个括号表示这是一个js表达式 第二个括号表示这是一个对象 
 6. innerhtml  dangerouslySetInnerHTML  =  vue  v-html 
 7. jsx 语法注释的写法  
 ```js
 	 { 
		// 这是单行注释 
	 }
	 { /** 这个是多行注释 */}
```
8. 事件 事件名 on后面跟事件的驼峰命名 
9. { } 必须有返回值 
## 编程单词 
 Invalid  无效的    property 属性  dangerously 危险的 
## state变量渲染和setState修改数据

在组件⾥⾯我们通过{}在JSX⾥⾯渲染变量

5 如果数据需要修改，并且需要⻚页⾯同时响应改变，那就需要把变量 放在state⾥⾯，同时使⽤setState修改 初始化状态state

// 初始化状态

this.state = { count: 0 };

更新状态使⽤setState,不能直接this.state.count=xxx

// 更新状态

this.setState({ count: this.state.count + 1 });

注意事项

setState是异步的，底层设计同⼀个⽣命周期会批量操作更新 state setState第⼆个参数是⼀个可选参数，传⼊⼀个回调函数可以 获取到最新的state 当修改的state依赖上⼀次修改的state的值时，可使⽤以下这 种⽅法修改

6 this.setState((prevState, prevProps)=>({

//prevState：上⼀次修改的状态state //prevProps：上⼀次修改的属性props count: prevState.count + 1

}), () => {

//这⾥可以获取到最新的state console.log(this.state.count);

});
强制更新状态(如果没有特殊情况，不建议使用)
```js
 this.state.number +=1
    this.forceUpdate() //不管数据有没有改变 都进行强制更新  
    console.log(this.state)
```
## props属性传递

⽗组件向⼦组件传递属性利⽤props接收 使⽤例⼦如下：
//⽗组件传⼊

<PropsDemo title="Tim⽼师教react"></PropsDemo>

//⼦组件使⽤ //class组件使⽤ <h1>{this.props.title}</h1> //函数型组件使⽤ function xxx(props){

return <h1>{props.title}</h1> } //解构赋值写法 function xxx({title}){

return <h1>{title}</h1> }
##  函数组件hooks  react 16.8版本之前   
只在顶层调⽤Hooks
1. Hooks的调⽤尽量只在顶层作⽤域进⾏调⽤ 不要在循环，条件或者是嵌套函数中调⽤Hook，否则可能会⽆ 法确保每次组件渲染时都以相同的顺序调⽤Hook 
2. 只在函数组件调⽤Hooks

React Hooks⽬前只⽀持函数组件，所以⼤家别在class组件或 者普通的函数⾥⾯调⽤Hook钩⼦函数 React Hooks的应⽤场景如下

1. 函数组件 
2. ⾃定义hooks 

函数组件 ⾃定义hooks 在未来的版本React Hooks会扩展到class组件，但是现阶段不能 再class⾥使⽤
1. 旧版上下文 
import React, { Component } from 'react'
import PropTypes from 'prop-types';
class Header extends Component {
    //定义子上下文对象的属性和类型
    static childContextTypes = {
        name:PropTypes.string,
        age:PropTypes.number
    }
    //返回或者说定义真正的子上下文
    getChildContext(){
        return {
            age:10,
            name:'Header' 
        }
    }
    render() {
        console.log(this.context)
        return <div style={{ border: '5px solid green', padding: '5px' }}>
            
            <Title></Title>
        </div>
    }
}
class Title extends Component {
    //表示或者 说指定我要获取哪些上下文对象
    static contextTypes = {
        color: PropTypes.string,
        name:PropTypes.string,
        age:PropTypes.number
    }
    render() {
        console.log(this.context)
        return <div style={{ border: '5px solid orange', padding: '5px',color:this.context.color }}>
            Title
        </div>
    }
}
class Main extends Component {
    render() {
        return <div style={{ border: '5px solid blue', padding: '5px' }}>
            Main
            <Content></Content>
        </div>
    }
}
class Content extends Component {
    static contextTypes = {
        color: PropTypes.string,
        name:PropTypes.string,
        age:PropTypes.number,
        setColor:PropTypes.func
    }
    render() {
        return <div style={{ border: '5px solid pink', padding: '5px',color:this.context.color }}>
            Content
            <button onClick={()=>this.context.setColor('red')}>变红</button>
            <button onClick={()=>this.context.setColor('green')}>变绿</button>
        </div>
    }
}
export default class Page extends Component {

    constructor() {
        super();
        this.state = { color: 'gray'};
    }
    //定义子上下文对象的属性和类型
    static childContextTypes = {
        name:PropTypes.string,
        color:PropTypes.string,
        setColor: PropTypes.func
    }
    //返回或者说定义真正的子上下文
    getChildContext(){
        return {
            color:this.state.color,
            setColor:this.setColor,
            name:'Page' 
        }
    }
    setColor = (color)=>{
        this.setState({color});
    }
    render() {
        return (
            <div style={{ border: '5px solid red', padding: '5px' }}>
                Page
                <Header>
                    <Title>

                    </Title>
                </Header>
                <Main>
                    <Content>

                    </Content>
                </Main>
            </div>
        )
    }
}

新版上下文 
static contextType = ThemeContex

## 使用hooks实现异步请求  

```js
import React, { Fragment, useState, useEffect } from 'react';
import axios from 'axios';
function App1() {
  const [data, setData] = useState({ hits: [] });
  const [query, setQuery] = useState('redux');
  const [url, setUrl] = useState(
    'https://hn.algolia.com/api/v1/search?query=redux',
  );
  const [isLoading, setIsLoading] = useState(false);
  useEffect(() => {
    const fetchData = async () => {
      setIsLoading(true);
      const result = await axios(url);
      setData(result.data);
      setIsLoading(false);
    };
    fetchData();
  }, [url]);
  return (
    <Fragment>
      <input
        type="text"
        value={query}
        onChange={event => setQuery(event.target.value)}
      />
      <button
        type="button"
        onClick={() =>
          setUrl(`http://hn.algolia.com/api/v1/search?query=${query}`)
        }
      >
        Search
      </button>
      {isLoading ? (
        <div>Loading ...</div>
      ) : (
        <ul>
          {data.hits.map(item => (
            <li key={item.objectID}>
              <a href={item.url}>{item.title}</a>
            </li>
          ))}
        </ul>
      )}
    </Fragment>
  );
}
export default App1;
```
## react 路由
1 下载  `npm install react-router-dom`


# 基于create-react-app引入redux的脚手架搭建 


## 一、背景说明
`Redux` 是 `JavaScript` 的状态容器，提供可预测化的状态管理，它将所有的 `state` 以对象树的形式存储于 `store` 中，用以统一管理 `state`，提供全局的作用域，使得所有的组件都可以访问 `store`并且方面的拿到 `state`,解决了多组件间层层嵌套的数据传递问题。由于 `create-react-app` 没有集成 `redux`，因此想要在项目中使用 `redux` ，就要在项目中引入有关 `redux` 的插件，具体操作如下：

## 二、操作步骤

### 1、创建项目
(1)、进入到你想创建项目存放的磁盘文件，比如 `D:\test` 文件，双击进入 `test` 文件后，在空白的地方按住 `shift` 键再用鼠标点击右键，选择在**此处打开命令窗口（w)**,此时会弹出一个命令行窗口。

(2)、我们需要使用create-react-app脚手架创建一个项目，此时在命令行窗口中输入

`create-react-app react-redux-demo`

回车后等待项目安装完成，此时项目名称为 `react-redux-demo`。

(3)、安装完成后，我们会在 `D:\test` 中看到新增的 `react-redux-demo` 文件，文件结构如下：

	├── node_modules                  // 模块安装依赖包
	├── public                        //公共文件，可以不用管
	│   ├── favicon.ico               //图标
	│   ├── index.html                //入口html
	│   ├── manifest.json             //manifest配置文件，指定应用名称、图标等信息
	├── src 						  //编写自己代码的存放文件
	│   ├── App.css                   //app的引用css文件
	│   ├── App.js					  //组件js文件
	│   ├── App.test.js               //测试文件
	│   ├── index.css                 //idnex引用的css文件
	│   └── index.js				  //页面入口文件
	│   ├── logo.svg                  //页面图片
	│   ├── serviceWorker.js          //加速程序运行文件
	├──.gitignore                     //提交到远程代码库时要忽略的文件
	├──package.json                   //用来声明项目的各种模块安装信息，脚本信息等
	├──package-lock.json              //用来锁定模块安装版本的，确保安装的模块版本一致
	├──README.md					  //盛放关于这个项目的说明文件 


### 2、启动项目
此时我们要验证一下项目能否运行成功，则在命令行窗口中输入

`cd react-redux-demo`

进入到项目里面，再输入命令

`npm star`

后，在弹出的浏览器 `localhost://3000` 页面下能看到页面展示出来，如下图：
![react](https://github.com/lijinping2017/react-redux-demo/raw/master/redux-images/react.jpg)

就说明项目能正常运行成功了。

### 3、安装Redux插件
(1)、首先我们要安装有关redux的插件，这里需要安装两个插件，分别是redux、react-redux，由于刚刚启动了项目，服务器在运行中，因此在安装插件之前先结束进程，具体操作是：在命令行窗口中按住ctrl+c键结束运行，然后在命令行中输入

`npm install redux react-redux --save-dev`

回车等待安装完成。

(2)安装完成后，打开 `package.json` 文件，我们会看到 `devDependencies` 多 `redux` 和
`rect-redux` 两个插件以及插件的版本号信息，如图所示：
![package-json](https://github.com/lijinping2017/react-redux-demo/raw/master/redux-images/package-json.jpg)

此时项目就可以使用 `redux`、`react-redux` 这两个插件了。

### 4、使用Redux的步骤
下面我们以一个计数器为例，实现react结合redux完成计数器的效果。

##### (1)、创建组件

在 `src` 文件夹下创建一个名为 `components` 的文件夹，用来存放项目的自定义组件，在`components` 文件夹下创建一个 `Counter.js` 文件，代码如下：

Counter.js

	import React, { Component } from 'react';

	import { connect } from 'react-redux';
	import { increaseAction, decreaseAction } from '../actions.js';
	
	class Counter extends Component {
		constructor(props) {
			super(props);
		}
	
		componentDidMount() {
		    console.log(this.props)
		} 
	
		render() {
			return(
				<div>
					<h2>{this.props.counter}</h2>
					<button onClick={this.props.increase}>增加</button>
					<button onClick={this.props.decrease}>减少</button>
				</div>
			) 
	    
	  	}
	}
	
	const mapStateToProps = (state) => {
		return {
			counter: state
		}
	}
	
	const mapDispatchToProps = (dispatch) => {
		return {
			increase: ()=>dispatch(increaseAction),
			decrease: ()=>dispatch(decreaseAction)
		}
	}
	
	export default connect(mapStateToProps,mapDispatchToProps)(Counter);


因为要使用 `redux`，因此在 `Counter.js` 文件头部引入counter方法。此时在 `Counter.js` 文件的最后通过使用 `connect(mapStateToProps,mapDispatchToProps)(Counter)` 方法，将store和 `Counter` 组件连接起来，并传入两个参数 `mapDispatchToProps` 和 `mapDispatchToProps`，使 `Counter` 组件可以拿到 `store` 树上的 `state` 值以及 `dispath` 方法。

`Counter.js` 的头部通过 `import { increaseAction, decreaseAction } from '../actions.js`语句引入 `increaseAction` 和 `decreaseAction`，`actions.js` 文件代码具体见下一步操作。


##### (2)、编写actions.js、reducer.js

在 `src` 文件夹下创建两个 `js` 文件，分别为 `actions.js` 和 `reducer.js`,具体代码如下：

actions.js
	
	export const increaseAction  = {
		type: "Add",
		data: 1
	}
	
	export const decreaseAction  = {
		type: "Sub",
		data: 1
	}




reducer.js

	import { increaseAction, decreaseAction } from './actions.js';

	const  counterReducer = (state = 0,action) => {
		switch(action.type) {
			case 'Add':
				return state + action.data;
			case 'Sub':
				return state - action.data;
			default:
				return state;
		}
	}
	
	export default counterReducer;

##### (3)、修改index.js、App.js

此时要将 `reducer` 处理返回的 `state` 值放入 `store` 树上，并且达到渲染 `Counter` 组件，因此要修改 `index.js` 和 `App.js` 文件代码，具体代码如下：

index.js

	import React from 'react';
	import ReactDOM from 'react-dom';
	import './index.css';
	import App from './App';
	import * as serviceWorker from './serviceWorker';
	
	import reducer from './reducer';
	import { createStore } from 'redux';
	import { Provider } from 'react-redux';
	
	const store = createStore(reducer);
	
	ReactDOM.render(
		<Provider store={store}>
			<App />
		</Provider>, 
		document.getElementById('root')
	);
	
	serviceWorker.unregister();	


App.js
	
	import React from 'react';
	import './App.css';
	import Counter from "./components/Counter";
	
	function App() {
	    return (
	        <div className="App">
	          <Counter />
	        </div>
	    );
	}
	
	export default App;

### 5、测试redux项目是否成功

到这里使用 `redux` 的步骤基本完成了，这时我们需要在浏览器中查看我们编写的代码是否在页面上生效，因为需要重新启动项目，在命令行窗口中输入

 `npm start`

启动项目后，会弹出新的浏览器 `localhost://3000` 页面，此时页面是这样的：
![redux](https://github.com/lijinping2017/react-redux-demo/raw/master/redux-images/redux.jpg)

出现这样的页面就说明项目没有错误，达到了我们想要的效果，这时要验证一下点击**增加**按钮数值是否**加1**，点击**减少**按钮时数值是否**减1**，测试结果如下：

点击**增加**按钮：
![increase](https://github.com/lijinping2017/react-redux-demo/raw/master/redux-images/add.jpg)

点击**减少**按钮：

![decrease](https://github.com/lijinping2017/react-redux-demo/raw/master/redux-images/sub.jpg)


好了，此时的页面效果可以说明了我们已经完成了基于 `create-react-app` 引入 `redux` 来实现计数器的效果了。





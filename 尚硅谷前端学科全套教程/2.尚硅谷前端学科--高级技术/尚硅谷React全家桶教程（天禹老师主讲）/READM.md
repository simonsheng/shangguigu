# [尚硅谷React教程（已加更新版内容，B站最火）](https://www.bilibili.com/video/BV1wy4y1D7JT)

- [x] 001_尚硅谷react教程_react简介 27:16
  - DOM Diffing
- [x] 002_尚硅谷react教程_hello_react案例 25:55
  - babel ES6->ES5
  - hoisting: var, let, const
  - let 和 const 声明的变量虽然也被“记录”在内存中，但在初始化前不能访问，这就是所谓的 Temporal Dead Zone（TDZ，暂时性死区）
  - JavaScript 声明方式支持版本

|声明方式|引入版本|描述|
|-------|------|----|
|var|ES1 (1997)|最早期就存在，函数作用域，有变量提升。|
|function|ES1 (1997)|传统的函数声明方式，支持函数提升。|
|let|ES6 (2015)|块作用域，不能重复声明，有 TDZ（暂时性死区）。|
|const|ES6 (2015)|块作用域，不可重新赋值，有 TDZ。|

  - 浏览器支持（主流现代浏览器中都已全面支持 let 和 const）：
    - Chrome: 支持 let 和 const 从版本 49（2016年）开始全面稳定。
    - Firefox: 从版本 44（2016）全面支持。
    - Edge: 旧版从 EdgeHTML 13 起支持（2015 年）；新版基于 Chromium 全面支持。
    - Safari: 从 10（2016年）全面支持。
    - Node.js: 从 v6+ 开始良好支持 ES6。

  - 兼容注意：
    - 如果你的代码需要兼容旧浏览器（如 IE11），需要使用 Babel 等工具将 let/const 转换为 var。
    - var 与 function 可被安全使用于所有 JavaScript 环境。

- [x] 003_尚硅谷react教程_虚拟DOM的两种创建方式 12:10
  - React.createElement();
- [x] 004_尚硅谷react教程_虚拟DOM与真实DOM 06:21
- [x] 005_尚硅谷react教程_jsx语法规则 26:33
  - JavaScript XML
  - {表达式}
  - className
  - style={{}}
- [x] 006_尚硅谷react教程_jsx小练习 22:13
- [x] 007_尚硅谷react教程_组件与模块 08:04
- [x] 008_尚硅谷react教程_开发者工具的安装 06:44
- [x] 009_尚硅谷react教程_函数式组件 17:3
  - function foo(){}
  - this is undefined
  - babel open 'use strict'，它不许this指向任何对象
- [x] 010_尚硅谷react教程_复习类相关知识 27:45

在 JavaScript 中，call() 方法可以显式地更改函数执行时的 this 指向（即函数内部的上下文对象）。这是由 JavaScript 的 执行上下文（Execution Context） 机制决定的。以下是详细解释：

1. this 的默认绑定规则

JavaScript 函数的 this 默认由 调用方式 决定：

```javascript
function sayHello() {
  console.log(this.name);
}

const obj = { name: "Alice" };

// 默认调用：this 指向全局对象（非严格模式）或 undefined（严格模式）
sayHello(); // 输出：undefined（浏览器中可能是 window.name）

// 方法调用：this 指向调用它的对象
obj.sayHello = sayHello;
obj.sayHello(); // 输出："Alice"
```
2. call() 如何强制更改 this？
call() 是函数原型（Function.prototype）上的方法，它的作用是：

- 立即调用函数，并显式指定函数内部的 this 值。
- 语法：func.call(thisArg, arg1, arg2...)

```javascript
function greet() {
  console.log(`Hello, ${this.name}!`);
}

const person1 = { name: "Alice" };
const person2 = { name: "Bob" };

// 用 call() 强制将 this 指向 person1
greet.call(person1); // 输出："Hello, Alice!"

// 用 call() 将 this 指向 person2
greet.call(person2); // 输出："Hello, Bob!"
```

3. 底层原理

当调用 call() 时，JavaScript 引擎会：
1. 绑定 this：将函数的内部 [[ThisValue]] 设置为 call() 的第一个参数（thisArg）。
2. 执行函数：以指定的 this 值运行函数代码。
伪代码模拟 call() 的实现：

```javascript
Function.prototype.myCall = function(thisArg, ...args) {
  // 1. 将函数设为 thisArg 的一个临时方法
  thisArg._tempFunc = this; // this 是原函数
  // 2. 调用方法（此时 this 自然指向 thisArg）
  const result = thisArg._tempFunc(...args);
  // 3. 删除临时方法
  delete thisArg._tempFunc;
  return result;
};

// 使用示例
greet.myCall(person1); // 效果与 call() 相同
```

4. 对比 apply() 和 bind()
- apply()：与 call() 类似，但参数以数组形式传递。
```javascript
greet.apply(person1, []); // 同 greet.call(person1)
```

- bind()：返回一个新函数，永久绑定 this，但不立即执行。
```javascript
const boundGreet = greet.bind(person1);
boundGreet(); // 输出："Hello, Alice!"
```

5. 使用场景
- 明确指定上下文：如借用其他对象的方法。
```javascript
const car = { brand: "Toyota" };

function showBrand() {
  console.log(this.brand);
}

showBrand.call(car); // 输出："Toyota"
```
- 类数组操作：如将 arguments 转为数组。
```javascript
function convertArgs() {
  const arr = Array.prototype.slice.call(arguments);
  console.log(arr);
}
convertArgs(1, 2, 3); // 输出：[1, 2, 3]
```
6. 注意事项
- 非严格模式：若 thisArg 为 null 或 undefined，this 会指向全局对象（如 window）。
- 箭头函数：call() 对箭头函数无效（因其 this 由词法作用域决定）。
```javascript
const arrowFunc = () => console.log(this);
arrowFunc.call({ name: "Alice" }); // this 仍是外层作用域的 this
```
- [x] 011_尚硅谷react教程_类式组件 17:41
  - onClick="demo()" call demo() then return the result. for this case it's undefined
  - onClick="demo", pass the function name as onclick="demo()"
- [x] 012_尚硅谷react教程_对state的理解 06:43
- [x] 013_尚硅谷react教程_初始化state 13:03
- [x] 014_尚硅谷react教程_react中的事件绑定 12:54
- [x] 015_尚硅谷react教程_类中方法中的this 23:41
- [x] 016_尚硅谷react教程_解决类中this指向问题 08:08
- [x] 017_尚硅谷react教程_setState的使用 19:43
  - render 1+n 次
- [x] 018_尚硅谷react教程_state的简写方式 18:04
  - ()=>{}，自动找外层函数的this
- [x] 019_尚硅谷react教程_总结state 04:22
- [ ] 020_尚硅谷react教程_props的基本使用 10:21
- [ ] 021_尚硅谷react教程_批量传递props 17:38
- [ ] 022_尚硅谷react教程_对props进行限制 23:20
- [ ] 023_尚硅谷react教程_props的简写方式 08:51
- [ ] 024_尚硅谷react教程_类式组件中的构造器与props 10:47
- [ ] 025_尚硅谷react教程_函数式组件使用props 08:05
- [ ] 026_尚硅谷react教程_总结props 04:39
- [ ] 027_尚硅谷react教程_字符串形式的ref 15:51
- [ ] 028_尚硅谷react教程_回调形式的ref 14:06
- [ ] 029_尚硅谷react教程_回调ref中调用次数的问题 18:43
- [ ] 030_尚硅谷react教程_createRef的使用 08:51
- [ ] 031_尚硅谷react教程_总结ref 03:15
- [ ] 032_尚硅谷react教程_react中的事件处理 08:38
- [ ] 033_尚硅谷react教程_非受控组件 13:37
- [ ] 034_尚硅谷react教程_受控组件 10:35
- [ ] 035_尚硅谷react教程_高阶函数_函数柯里化 26:29
- [ ] 036_尚硅谷react教程_不用柯里化的写法 06:15
- [ ] 037_尚硅谷react教程_引出生命周期 39:02
- [ ] 038_尚硅谷react教程_生命周期(旧)_组件挂载流程 14:12
- [ ] 039_尚硅谷react教程_生命周期(旧)_setState流程 13:00
- [ ] 040_尚硅谷react教程_生命周期(旧)_forceUpdate流程 05:27
- [ ] 041_尚硅谷react教程_生命周期(旧)_父组件render流程 14:14
- [ ] 042_尚硅谷react教程_总结生命周期(旧) 09:07
- [ ] 043_尚硅谷react教程_对比新旧生命周期 31:28
- [ ] 044_尚硅谷react教程_getDerivedStateFromProps 16:41
- [ ] 045_尚硅谷react教程_getSnapshotBeforeUpdate 16:36
- [ ] 046_尚硅谷react教程_getSnapshotBeforeUpdate举例 19:53
- [ ] 047_尚硅谷react教程_总结生命周期(新) 04:26
- [ ] 048_尚硅谷react教程_DOM的diffing算法 45:39
- [ ] 049_尚硅谷react教程_初始化react脚手架 23:38
- [ ] 050_尚硅谷react教程_脚手架文件介绍_public 30:37
- [ ] 051_尚硅谷react教程_脚手架文件介绍_src 18:25
- [ ] 052_尚硅谷react教程_一个简单的Hello组件 38:01
- [ ] 053_尚硅谷react教程_样式的模块化 05:07
- [ ] 054_尚硅谷react教程_vscode中react插件的安装 06:38
- [ ] 055_尚硅谷_react教程_组件化编码流程 13:15
- [ ] 056_尚硅谷_react教程_TodoList案例_静态组件 24:08
- [ ] 057_尚硅谷_react教程_TodoList案例_动态初始化列表 20:50
- [ ] 058_尚硅谷_react教程_TodoList案例_添加todo 24:23
- [ ] 059_尚硅谷_react教程_TodoList案例_鼠标移入效果 08:40
- [ ] 060_尚硅谷_react教程_TodoList案例_添加一个todo 15:53
- [ ] 061_尚硅谷_react教程_TodoList案例_对props进行限制 05:34
- [ ] 062_尚硅谷_react教程_TodoList案例_删除一个todo 12:15
- [ ] 063_尚硅谷_react教程_TodoList案例_实现底部功能 25:07
- [ ] 064_尚硅谷_react教程_TodoList案例_总结TodoList案例 05:44
- [ ] 065_尚硅谷_react教程_脚手架配置代理_方法1 27:29
- [ ] 066_尚硅谷_react教程_脚手架配置代理_方法2 26:11
- [ ] 067_尚硅谷_react教程_github搜索案例_静态组件 19:20
- [ ] 068_尚硅谷_react教程_github搜索案例_axios发送请求 26:04
- [ ] 069_尚硅谷_react教程_github搜索案例_展示数据 12:53
- [ ] 070_尚硅谷_react教程_github搜索案例_完成案例 20:37
- [ ] 071_尚硅谷_react教程_消息订阅与发布技_pubsub 28:26
- [ ] 072_尚硅谷_react教程_fetch发送请求 47:02
- [ ] 073_尚硅谷_react教程_总结github搜索案例 03:48
- [ ] 074_尚硅谷_react教程_对SPA应用的理解 13:23
- [ ] 075_尚硅谷_react教程_对路由的理解 11:22
- [ ] 076_尚硅谷_react教程_前端路由原理 23:01
- [ ] 077_尚硅谷_react教程_路由的基本使用 44:04
- [ ] 078_尚硅谷_react教程_路由组件与一般组件 20:20
- [ ] 079_尚硅谷_react教程_NavLink的使用 06:55
- [ ] 080_尚硅谷_react教程_封装NavLink组件 19:44
- [ ] 081_尚硅谷_react教程_Switch的使用 08:39
- [ ] 082_尚硅谷_react教程_解决样式丢失问题 25:07
- [ ] 083_尚硅谷_react教程_路由的模糊匹配与严格匹配 11:54
- [ ] 084_尚硅谷_react教程_Redirect的使用 07:25
- [ ] 085_尚硅谷_react教程_嵌套路由 28:19
- [ ] 086_尚硅谷_react教程_向路由组件传递params参数 28:20
- [ ] 087_尚硅谷_react教程_向路由组件传递search参数 16:18
- [ ] 088_尚硅谷_react教程_向路由组件传递state参数 18:04
- [ ] 089_尚硅谷_react教程_总结路由参数 02:48
- [ ] 090_尚硅谷_react教程_push与repalce 07:42
- [ ] 091_尚硅谷_react教程_编程式路由导航 24:27
- [ ] 092_尚硅谷_react教程_withRouter的使用 11:52
- [ ] 093_尚硅谷_react教程_BrowserRouter与HashRouter 07:59
- [ ] 094_尚硅谷_react教程_antd的基本使用 31:44
- [ ] 095_尚硅谷_react教程_antd样式的按需引入 22:01
- [ ] 096_尚硅谷_react教程_antd自定义主题 16:06
- [ ] 097_尚硅谷_react教程_redux简介 16:48
- [ ] 098_尚硅谷_react教程_redux工作流程 36:11
- [ ] 099_尚硅谷_react教程_求和案例_纯react版 19:05
- [ ] 100_尚硅谷_react教程_求和案例_redux精简版 53:19
- [ ] 101_尚硅谷_react教程_求和案例_redux完整版 20:32
- [ ] 102_尚硅谷_react教程_求和案例_异步action版 35:56
- [ ] 103_尚硅谷_react教程_对react-redux的理解 08:58
- [ ] 104_尚硅谷_react教程_连接容器组件与UI组件 22:27
- [ ] 105_尚硅谷_react教程_react-redux基本使用 46:04
- [ ] 106_尚硅谷_react教程_优化1_简写mapDispatch 20:15
- [ ] 107_尚硅谷_react教程_优化2_Provider组件的使用 13:45
- [ ] 108_尚硅谷_react教程_优化3_整合UI组件与容器组件 27:00
- [ ] 109_尚硅谷_react教程_数据共享_编写Person组件 14:43
- [ ] 110_尚硅谷_react教程_数据共享_编写Person组件的reducer 13:37
- [ ] 111_尚硅谷_react教程_数据共享_完成数据共享 33:23
- [ ] 112_尚硅谷_react教程_纯函数 18:18
- [ ] 113_尚硅谷_react教程_redux开发者工具 14:43
- [ ] 114_尚硅谷_react教程_最终版 23:29
- [ ] 115_尚硅谷_react教程_项目打包运行 07:43
- [ ] 116_尚硅谷_react教程_扩展1_setState 26:34
- [ ] 117_尚硅谷_react教程_扩展2_lazyLoad 20:23
- [ ] 118_尚硅谷_react教程_扩展3_stateHook 18:19
- [ ] 119_尚硅谷_react教程_扩展4_EffectHook 21:50
- [ ] 120_尚硅谷_react教程_扩展5_RefHook 04:39
- [ ] 121_尚硅谷_react教程_扩展6_Fragment 06:54
- [ ] 122_尚硅谷_react教程_扩展7_Context 26:08
- [ ] 123_尚硅谷_react教程_扩展8_PureComponent 44:35
- [ ] 124_尚硅谷_react教程_扩展9_renderProps 25:36
- [ ] 125_尚硅谷_react教程_扩展10_ErrorBoundary 31:42
- [ ] 126_尚硅谷_react教程_组件间通信方式总结 06:11
- [ ] 127_尚硅谷_ReactRouter6教程_课程说明 04:22
- [ ] 128_尚硅谷_ReactRouter6教程_一级路由 14:46
- [ ] 129_尚硅谷_ReactRouter6教程_重定向 10:07
- [ ] 130_尚硅谷_ReactRouter6教程_NavLink高亮 06:09
- [ ] 131_尚硅谷_ReactRouter6教程_useRoutes路由表 06:12
- [ ] 132_尚硅谷_ReactRouter6教程_嵌套路由 12:15
- [ ] 133_尚硅谷_ReactRouter6教程_路由的params参数 12:20
- [ ] 134_尚硅谷_ReactRouter6教程_路由的search参数 09:00
- [ ] 135_尚硅谷_ReactRouter6教程_路由的state参数 04:03
- [ ] 136_尚硅谷_ReactRouter6教程_编程式路由导航 12:47
- [ ] 137_尚硅谷_ReactRouter6教程_useInRouterContext 03:56
- [ ] 138_尚硅谷_ReactRouter6教程_useNavigationType 02:30
- [ ] 139_尚硅谷_ReactRouter6教程_useOutlet 01:44
- [ ] 140_尚硅谷_ReactRouter6教程_useOutletuseResolvedPath 01:12
- [ ] 141_尚硅谷_ReactRouter6教程_总结 06:21
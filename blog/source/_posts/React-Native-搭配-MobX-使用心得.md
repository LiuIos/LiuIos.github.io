---
title: React Native 搭配 MobX 使用心得
date: 2021-09-09 10:56:39
tags: react-native
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed/20210909105918.png
toc: true
---

## React Native 使用 MobX

[MobX](https://zh.mobx.js.org/README.html) 是一个用于状态管理的[Javascript](https://codingislove.com/tag/javascript/)库，它通过运用透明的函数式响应编程（Transparent Functional Reactive Programming，TFRP）使状态管理变得简单和可扩展，与React Native一起使用效果很好。

### 安装 Mobx 和 Mobx-react

Mobx 是主要的库，而 mobx-react 具有用于 react 的 mobx 绑定。使用以下命令安装 Mobx 和 Mobx-react：

```
yarn add mobx mobx-react
```

### 启用 Mobx 装饰器语法

你也可以在没有装饰器语法的情况下使用 Mobx，但是使用装饰器可以简化代码，所以让我们启用它。

使用以下命令为装饰器安装 babel 插件：

```
yarn add --dev @babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties
```

并在 `.babelrc`文件中启用（注意，插件的顺序很重要）：

```
{
    "plugins": [
        ["@babel/plugin-proposal-decorators", { "legacy": true }],
        ["@babel/plugin-proposal-class-properties", { "loose": false }]
        // 与MobX 4/5不同的是, "loose" 必须为 false!    ^
    ]
}
```



## 如何使用React上下文设置 Mobx

让我们看看如何使用 react 和 react 上下文来设置 Mobx。

### 什么是react上下文？

Context 提供了一种通过组件树传递数据的方法，而无需在每个级别手动向下传递 props。简单来说，React 上下文用于将一些数据存储在一个地方并在整个应用程序中使用它。每次修改上下文中的数据时，组件也会重新渲染。如果我们不使用上下文，那么我们将使用 props 手动传递数据。

从技术上讲，Mobx 和其他状态管理库也做同样的事情，但功能更多

### 设置一个基本的 Mobx store

转到react native程序中的 src 文件夹，创建一个名为的文件夹`services`并创建一个名为的文件`store.js`

转到新创建的`store.js`文件并粘贴以下代码

```js
import React from "react";
import { action, observable } from "mobx";
 
/* Store start */
export default class Store {
  @observable title = "Coding is Love";
 
  @observable user = {
    userId: 1,
    name: "Ranjith kumar V",
    website: "https://codingislove.com",
    email: "ranjith@codingislove.com",
  };
 
  @action
  setUser(user) {
    this.user = user;
  }
 
  @action
  updateUser(data) {
    this.user = { ...this.user, ...data };
  }
 
  @action
  clearUser() {
    this.user = undefined;
  }
 
  @action
  setTitle(title) {
    this.title = title;
  }
}
/* Store end */
 
/* Store helpers */
const StoreContext = React.createContext();
 
export const StoreProvider = ({ children, store }) => {
  return (
    <StoreContext.Provider value={store}>{children}</StoreContext.Provider>
  );
};
 
/* Hook to use store in any functional component */
export const useStore = () => React.useContext(StoreContext);
 
/* HOC to inject store to any functional or class component */
export const withStore = (Component) => (props) => {
  return <Component {...props} store={useStore()} />;
};
```

### store说明

它是一个非常简单的存储，带有一个用于存储用户数据的用户对象、一个标题字符串、一些修改用户和标题的函数。`@observable`**用于告诉 mobx 在修改 observable 属性时重新渲染组件。**

`@action`是一个用于修改 observable 的函数。运行一个`@actions`也会触发`autoRun`如果您设置了其中任何一个，函数。

useStore 是我们的自定义钩子，用于在任何功能组件中使用 mobx 存储

withStore 是一个自定义 HOC（高阶组件），可以在任何类组件中使用 mobx 存储。

### Mobx 使用

转到App.js

```js
import React from "react";
import Home from "./screens/Home";
import Store, { StoreProvider } from "./services/store";
 
const store = new Store();
/* Create a new store */
 
function App() {
  return (
    <StoreProvider store={store}>
      <Home />
    </StoreProvider>
  );
}
 
export default App;
```

Home.js

```js
import React, { Component } from "react";
import logo from "../logo.svg";
import "../App.css";
import { observer } from "mobx-react";
import { withStore } from "../services/store";
 
@withStore
@observer
class Home extends Component {
  toggleTitle = () => {
    const { store } = this.props;
    if (store.title === "Coding is Love") {
      store.setTitle("Mobx React Context");
    } else {
      store.setTitle("Coding is Love");
    }
  };
 
  render() {
    const { store } = this.props;
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <a
            className="App-link"
            href="https://codingislove.com"
            target="_blank"
            rel="noopener noreferrer"
          >
            {store.title}
          </a>
          <button onClick={this.toggleTitle} style={{ margin: 20 }}>
            Toggle title
          </button>
        </header>
      </div>
    );
  }
}
 
export default Home;
```

### 功能组件使用 useStore 钩子

```js
import React from "react";
import { observer } from "mobx-react";
import { useStore } from "../services/store";
 
const Username = observer(() => {
  const store = useStore();
  return <div style={{ fontSize: 14 }}>- By {store.user.name}</div>;
});
 
export default Username;
```


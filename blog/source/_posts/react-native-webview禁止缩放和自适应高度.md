---
title:
  - react-native-webview禁止缩放和自适应高度
date: 2021-09-07 16:04:33
tags: react-native
categories: 技术
---

```js
const BaseScript = `
    (function () {
        var height = null;
        function changeHeight() {
          if (document.body.scrollHeight != height) {
            height = document.body.scrollHeight;
            if (window.ReactNativeWebView.postMessage) {
              window.ReactNativeWebView.postMessage(JSON.stringify({
                type: 'setHeight',
                height: height,
              }))
            }
          }
        }
        setTimeout(changeHeight, 100);
        const meta = document.createElement('meta'); 
        meta.setAttribute('content', 'initial-scale=1, maximum-scale=1, user-scalable=0'); 
        meta.setAttribute('name', 'viewport'); 
        document.getElementsByTagName('head')[0].appendChild(meta);
    } ())
    `;
/**
   * web端发送过来的交互消息
   */
  onMessage(event) {
    try {
      const action = JSON.parse(event.nativeEvent.data);
      console.log('====================================');
      console.log(action);
      console.log('====================================');
      if (action.type === 'setHeight' && action.height > 0) {
        this.setState({height: action.height});
      }
    } catch (error) {
      // pass
      console.log('====================================');
      console.log(error);
      console.log('====================================');
    }
  }
<WebView ref={e => (this.webView = e)} 
injectedJavaScript={BaseScript}/>
```


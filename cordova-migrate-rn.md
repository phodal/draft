Cordova 演进到 React Native
===

Cordova + Ionic + React Native


处理 Cordova 插件
---

### 集成原生插件

### 没有 UI 和 React Tag

最麻烦的部分是 Cordova 插件

原始代码：

```
$rootScope.$on('RNBridge.datePicker', function(event, data) {

});
RNBridgeHelper.datePicker(options);
```

插件桥

```
function datePicker(options) {
  function handler(event) {
    event.target.removeEventListener('message', handler);
    var data = JSON.parse(event.data);
    $rootScope.$broadcast('RNBridge.datePicker', data);
  }
  window.document.addEventListener('message', handler);
  window.postMessage(JSON.stringify({
    action: action,
    payload: payload
  }));
}
```

WebView

```
<WebView
    source={source}
    ...
    onMessage={this.handleMessage}
  />
```          

```
WebViewHandler.handleWebViewMessage = (evt, webView) => {
  const event = JSON.parse(evt.nativeEvent.data);
  const action = event.action;
  const payload = event.payload
  ...
  switch (action) {
    case 'DATE_PICKER': {
      return DatePickerHandler.showDatePicker(payload, webView);
    }
  }
  ...
}
```

```
DatePickerHandler.showDatePicker = (payload, webView) => {
  const showPicker = async (options, webView) => {
    try {
      const { action, year, month, day } = await DatePickerAndroid.open(options);
      if (action === DatePickerAndroid.dismissedAction) {
        webView.postMessage(JSON.stringify({
          type: 'DATE_PICKER',
          success: false,
        }));
      }
    } catch ({ code, message }) {
      console.warn('Cannot open date picker', message);
    }
  };

  showPicker({
    date: new Date(payload.date),
    maxDate: new Date(payload.maxDate),
  }, webView);
};
```


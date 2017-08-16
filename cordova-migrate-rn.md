Cordova 演进到 React Native
===

Cordova + Ionic + React Native


处理 WebView
---

```
let source;
if (__DEV__) {
  source = require(`./www/index.html`);
} else {
  source = Platform.OS === 'ios' ? require(`./www/index.html`) : { uri: `file:///android_asset/www/index.html#${baseUrl}` };
}
```

WebView复制：

```
rm -rf ios/assets/src/components/ui/www
mkdir -p ios/assets/src/components/ui/www
cp -a src/components/ui/www ios/assets/src/components/ui/
```

处理 Cordova 插件
---

WebView <-> React Native <-> Native

RN: injectJavaScript/PostMessage
WebView: PostMessage
Native: RCTBridge
RN:

原始的 Cordova Native 和 WebView 的通访方式 

```
let js = 'var event = new CustomEvent("' + action + '", {detail: ' + JSON.stringify(detail) + '});';
js += 'window.document.dispatchEvent(event);';
webView.injectJavaScript(js);
```    

```
dispatch_async(dispatch_get_main_queue(), ^{
    [self sendEventWithName:@"NATIVE_INVOKE" body: @{@"injectedJS": javascript}];
});
```    

```
private void executeJS(String injectedJS) {
    WritableMap params = Arguments.createMap();
    params.putString("injectedJS", injectedJS);
    sendEvent(Zhaohu.getInstance().getReactContext(), NATIVE_INVOKE, params);
}
```    

### 集成原生插件

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

### 没有 UI 和 React Tag


处理 Tabbar
---

第三方封装的 TabBar 都会绑定 View，所以只能自己去实现

```
<TabBar>
    <TabBar.Item
        ...
        onPress={() => {

        }}
        title='首页'>
    </TabBar.Item>
</TabBar>
```

RN 返回 WebView
---

```
window.postMessage(JSON.stringify({
  action: 'RN_BACK'
}));
```

```
RNBackHandler.handleRNBack = () => {
  Actions.pop();
};
```

WebView 打开 RN

```
let js = 'var event = new CustomEvent("' + action + '", {detail: ' + JSON.stringify(detail) + '});';
js += 'window.document.dispatchEvent(event);';
webView.injectJavaScript(js);
```






URL Scheme
---

```
<activity
  android:name=".MainActivity"
  android:launchMode="singleTask">
```

```
// iOS 9.x or newer
#import <React/RCTLinkingManager.h>

- (BOOL)application:(UIApplication *)application
   openURL:(NSURL *)url
   options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
  return [RCTLinkingManager application:app openURL:url options:options];
}
```

```
- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity
 restorationHandler:(void (^)(NSArray * _Nullable))restorationHandler
{
 return [RCTLinkingManager application:application
                  continueUserActivity:userActivity
                    restorationHandler:restorationHandler];
}
```

```

  componentWillUnmount() {
    RCTZhaohuEvent.removeAllListeners('NATIVE_INVOKE');
    Linking.removeEventListener('url', this._handleOpenURL.bind(this));
  }

  componentDidMount() {
    //   PushNotificationIOS.requestPermissions();
    Linking.addEventListener('url', this._handleOpenURL.bind(this));
  }

  _handleOpenURL(event) {
    let script = 'var event = new CustomEvent(\'LaunchUrl\', {detail: {\'url\': \'' + event.url + '\'}});window.dispatchEvent(event);';
    this.refs.webview.injectJavaScript(script);
  }
  ```
  
  ```
  
  _handleAppStateChange(currentAppState) {
    // this.setState({currentAppState});
    if (Platform.OS === 'android') {
      this._checkDeepLink();
    }
  }

  _checkDeepLink() {
    Linking.getInitialURL().then(url => {
      if (url) {
        let script = 'var event = new CustomEvent(\'LaunchUrl\', {detail: {\'url\': \'' + url + '\'}});window.dispatchEvent(event);';
        this.refs.webview.injectJavaScript(script);
      }
    });
  }

  componentDidMount() {
    AppState.addEventListener('change', this._handleAppStateChange);
    this._checkDeepLink();
  }
  ```
  

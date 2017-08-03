

iOS 封装问题
---

### 原生模块里

//:configuration = Debug
HEADER_SEARCH_PATHS = $(SRCROOT)/** $(SRCROOT)/node_modules/react-native/React $(SRCROOT)/../../../node_modules/react-native/React/** $(SRCROOT)/../react-native/React/**

//:configuration = Release
HEADER_SEARCH_PATHS = $(SRCROOT)/** $(SRCROOT)/node_modules/react-native/React $(SRCROOT)/../../../node_modules/react-native/React/** $(SRCROOT)/../react-native/React/**

//:completeSettings = some
HEADER_SEARCH_PATHS

### 其它 

//:configuration = Debug
FRAMEWORK_SEARCH_PATHS = $(SRCROOT)/../node_modules/react-native-xxx/ios/RNxxx/**

//:configuration = Release
FRAMEWORK_SEARCH_PATHS = $(SRCROOT)/../node_modules/react-native-xxx/ios/RNZxxx/**

//:completeSettings = some
FRAMEWORK_SEARCH_PATHS



### 编译选项


Link Binary with Libraries

添加： CrashReporter.framework, CoreTelephony.framework

编译：的

//:configuration = Debug
OTHER_LDFLAGS = -ObjC -lsqlite3 -lz -liconv -lstdc++ -lc++ -lstdc++.6

//:configuration = Release
OTHER_LDFLAGS = -ObjC -lsqlite3 -lz -liconv -lstdc++ -lc++ -lstdc++.6

//:completeSettings = some
OTHER_LDFLAGS


插件 
---

RN 的 JS 代码 

```
import { NativeModules } from 'react-native';

module.exports = NativeModules.SDK;
```

Android 示例

```
@ReactMethod
public void login(int userid, String password, Promise promise) {
    try {
        sdkLogin(userid, _signature);
        promise.resolve(SUCCEED_INFO);
    } catch (Exception e) {
        Log.e(TAG, e.getMessage(), e);
        promise.reject(e);
    }
}

@ReactMethod
public void isLogined(Promise promise) {
    try {
        promise.resolve(SDK.app().is_logined());
    } catch (Exception e) {
        Log.e(TAG, e.getMessage(), e);
        promise.reject(e);
    }
}
```

iOS 示例：

头文件

```
#import "RCTBridgeModule.h"

@interface SDK : NSObject <RCTBridgeModule>


@end
```

代码文件

```
@implementation SDK

RCT_EXPORT_MODULE();
```

```objc
RCT_EXPORT_METHOD(isLogined:(RCTPromiseResolveBlock)resolve
                  rejecter:(RCTPromiseRejectBlock)reject) {
    BOOL isLogin = [[SDKHelper sharedHelper] isLogin];
    resolve(@[[NSNull null], @{@"isLogined": @(isLogin)}]);
}
```

为了往 JS 中发送代码

js 代码端


```
    webviewOnLoad = () => {
      if(Platform.OS === 'android') {
        DeviceEventEmitter.addListener('NATIVE_INVOKE', this.handleNativeInvoke);
      } else {
        var RCTZhaohu = new NativeEventEmitter(NativeModules.SDK);
        RCTSDK.addListener('NATIVE_INVOKE', this.handleNativeInvoke)
      }
    }
```      

原生代码:

头文件：

```
#import <React/RCTBridgeModule.h>
#import <React/RCTEventEmitter.h>


@interface SDK : RCTEventEmitter <RCTBridgeModule>

@end
```

代码文件：

```
#import <React/RCTBridge.h>

@implementation SDK

@synthesize bridge = _bridge;

- (NSArray<NSString *> *) supportedEvents
{
    return @[@"SDK_EVENT"];
}

RCT_EXPORT_MODULE();


- (void)pushEvent:(NSString *)formatString eventName:(NSString *)eventName JSONObject:(id)JSONObject {
    NSData* JSONObjectData = [NSJSONSerialization dataWithJSONObject:[JSONObject copy]
                                                             options:NSJSONWritingPrettyPrinted
                                                               error:nil];
    NSString* JSONObjectString = [[NSString alloc] initWithData:JSONObjectData
                                                       encoding:NSUTF8StringEncoding];
    NSString* javascript = [NSString stringWithFormat:formatString, eventName, JSONObjectString];
    dispatch_async(dispatch_get_main_queue(), ^{
        [self sendEventWithName:@"SDK_EVENT" body: @{@"injectedJS": javascript}];
    });
}

@end
```

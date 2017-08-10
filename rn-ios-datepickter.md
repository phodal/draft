```
#import <React/RCTBridgeModule.h>
#import <UIKit/UIKit.h>

@interface Datepicker : NSObject <RCTBridgeModule>

@end
```

```
#import "Datepicker.h"

@implementation Datepicker
RCT_EXPORT_MODULE()


RCT_EXPORT_METHOD(options:(NSMutableDictionary *) options
                  callbacker:(RCTResponseSenderBlock)callback) {


    callback(@[]);
}

@end
```

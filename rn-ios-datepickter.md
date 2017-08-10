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

测试

```

#import "RNCMBDatepicker.h"

@implementation RNCMBDatepicker
RCT_EXPORT_MODULE()

- (UIView *)view
{
    return [UIDatePicker new];
}

RCT_EXPORT_METHOD(show:(NSDictionary *) options
                  callbacker:(RCTResponseSenderBlock)callback) {

    UIDatePicker  *picker = [[UIDatePicker alloc] initWithFrame:CGRectMake(0,0, self.view.frame.size.width, self.view.frame.size.height)];

    picker.datePickerMode = UIDatePickerModeDate;
    NSDate *maxDate = [[NSDate alloc]initWithTimeIntervalSinceNow:24*60*60];
    picker.maximumDate = maxDate;
    
    [self.view addSubview:picker];
    callback(@[]);
}

@end
```

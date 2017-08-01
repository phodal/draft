

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

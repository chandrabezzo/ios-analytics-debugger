# AnalyticsDebugger

[![Version](https://img.shields.io/cocoapods/v/IosAnalyticsDebugger.svg?style=flat)](https://cocoapods.org/pods/AnalyticsDebugger)
[![License](https://img.shields.io/cocoapods/l/IosAnalyticsDebugger.svg?style=flat)](https://cocoapods.org/pods/AnalyticsDebugger)
[![Platform](https://img.shields.io/cocoapods/p/IosAnalyticsDebugger.svg?style=flat)](https://cocoapods.org/pods/AnalyticsDebugger)

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Requirements

## Installation

### Swift Package Manager

You can include this debugger with SPM [from this repo](https://github.com/avohq/ios-analytics-debugger-spm)

### Cocoapods

IosAnalyticsDebugger is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'AnalyticsDebugger'
```

# Create the debugger instance

Obj-C

    AnalyticsDebugger * debugger = [AnalyticsDebugger new];

Swift

    let debugger = AnalyticsDebugger()
    
# Debugger instance management

1. The debugger object maintains the history of your analytics events, so to share the history between different screens you should use the same debugger instance. We recommend to have a singleton that you provide or inject to your screens. A simple example can be found [here](https://github.com/avohq/ios-analytics-debugger/blob/master/Example/IosAnalyticsDebugger/AVOAppDelegate.m#L12).

2. Even if your app only has one screen (common case in PoC apps), remember to keep a reference to the `AnalyticsDebugger` instance somewhere. Otherwise it will become unresponsive to touches.

# Show the debugger

Obj-C

    [debugger showBubbleDebugger];
    
or

    [debugger showBarDebugger];

Swift
  
    debugger.showBubble()
    
or

    debugger.showBarDebugger()

# Hide the debugger

Obj-C

    [debugger hideDebugger];
    
Swift

    debugger.hide()
    
# Post an event

Obj-C

    NSMutableArray * props = [NSMutableArray new];

    [props addObject:[[DebuggerProp alloc] initWithId:@"id0" withName:@"id0 event" withValue:@"value 0"]];
    [props addObject:[[DebuggerProp alloc] initWithId:@"id1" withName:@"id1 event" withValue:@"value 1"]];

    NSMutableArray * errors = [NSMutableArray new];

    [errors addObject:[[DebuggerPropError alloc] initWithPropertyId:@"id0" withMessage:@"error in event id0"]];

    [debugger publishEvent:@"Test Event" withTimestamp:[NSNumber numberWithDouble:[[NSDate date] timeIntervalSince1970]]
        withProperties:props withErrors:errors];
        
Swift

    debugger.publishEvent("Test Event", withTimestamp: NSNumber(value: NSDate().timeIntervalSince1970),
        withProperties: [DebuggerProp(id: "id0", withName: "prop 0", withValue: "value 0"), 
                         DebuggerProp(id: "id1", withName: "prop 1", withValue: "value 1")], 
        withErrors: [DebuggerPropError(propertyId: "id0", withMessage: "error in event with id0")]);

# Using with Avo

When using Avo generated code you'll be calling the `init` methods. Actual interface of the methods depends on your schema setup, but there will be init methods with `debugger` parameter, where you can pass an instance of `AnalyticsDebugger`.

Obj-C

    [Avo initAvoWithEnv:AVOEnvDev ... debugger:debugger];

Swift

    Avo.initAvo(env: AvoEnv.dev, ..., debugger: debugger)

After that all events from Avo function calls will be automatically accessable in the debugger UI.

## Author

Avo (https://www.avo.app), friends@avo.app

## License

IosAnalyticsDebugger is available under the MIT license. See the LICENSE file for more info.

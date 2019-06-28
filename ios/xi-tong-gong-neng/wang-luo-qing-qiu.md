# 网络请求

## NetworkManagement

### Reachability\(可达性判断\)

#### [RealReachability](https://github.com/dustturtle/RealReachability)

To integrate RealReachability into your Xcode project using CocoaPods, specify it in your `Podfile`:

```text
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '6.0'

pod 'RealReachability', '~> 1.1.1'
```

Then, run the following command:

```text
$ pod install
```

**Start to notify**

```text
    [GLobalRealReachability startNotifier];
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(networkChanged:)
                                                 name:kRealReachabilityChangedNotification
                                               object:nil];
```

**Observer like below**

```text
- (void)networkChanged:(NSNotification *)notification
{
    RealReachability *reachability = (RealReachability *)notification.object;
    ReachabilityStatus status = [reachability currentReachabilityStatus];
    NSLog(@"currentStatus:%@",@(status));
}
```

**Trigger realtime Reachability like below**

```text
[GLobalRealReachability reachabilityWithBlock:^(ReachabilityStatus status) {
        switch (status)
        {
            case RealStatusNotReachable:
            {
            //  case NotReachable handler
                break;
            }

            case RealStatusViaWiFi:
            {
            //  case WiFi handler
                break;
            }

            case RealStatusViaWWAN:
            {
            //  case WWAN handler
                break;
            }

            default:
                break;
        }
    }];
```

**Query currentStatus**

```text
ReachabilityStatus status = [reachability currentReachabilityStatus];
```

## HTTP Client

### Alamofire

> [Alamofire Github 主页](https://github.com/Alamofire/Alamofire)

## HTTP Stubs

### [OHHTTPStubs](https://github.com/AliSoftware/OHHTTPStubs/)

#### Basic example

**In Objective-C**

```text
[OHHTTPStubs stubRequestsPassingTest:^BOOL(NSURLRequest *request) {
  return [request.URL.host isEqualToString:@"mywebservice.com"];
} withStubResponse:^OHHTTPStubsResponse*(NSURLRequest *request) {
  // Stub it with our "wsresponse.json" stub file (which is in same bundle as self)
  NSString* fixture = OHPathForFile(@"wsresponse.json", self.class);
  return [OHHTTPStubsResponse responseWithFileAtPath:fixture
            statusCode:200 headers:@{@"Content-Type":@"application/json"}];
}];
```

**In Swift**

This example is using the Swift helpers found in `OHHTTPStubsSwift.swift` provided by the `OHHTTPStubs/Swift` subspec

```text
stub(isHost("mywebservice.com")) { _ in
  // Stub it with our "wsresponse.json" stub file (which is in same bundle as self)
  let stubPath = OHPathForFile("wsresponse.json", self.dynamicType)
  return fixture(stubPath!, headers: ["Content-Type":"application/json"])
}
```


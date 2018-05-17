# Prerequisite(预准备)

## Enable HealthKit

如果希望在应用程序中使用HealthKit，首先需要在生成证书的时候勾选HealthKit选项。

![](http://jademind.com/wp-content/uploads/2014/11/healthkit_enable_capability.png)

## Check availability(检查HealthKit可用性)

考虑到目前HealthKit仅仅可以在iPhone设备上使用，不能在iPad或者iPod中使用，所以在接入HealthKit代码之前最好检验下可用性：

``` objective-c
if(NSClassFromString(@"HKHealthStore") && [HKHealthStore isHealthDataAvailable])
{
   // Add your HealthKit code here
}
```

## Request authorization(请求授权)

由于HealthKit存储了大量的用户敏感信息，App如果需要访问HealthKit中的数据，首先需要请求用户权限。权限分为读取与读写权限(苹果将读写权限称为share)。请求权限还是比较简单的，可以直接使用[`requestAuthorizationToShareTypes: readTypes: completion:`](https://developer.apple.com/library/ios/DOCUMENTATION/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/occ/instm/HKHealthStore/requestAuthorizationToShareTypes:readTypes:completion:) 方法。

``` objective-c
HKHealthStore *healthStore = [[HKHealthStore alloc] init];

// Share body mass, height and body mass index
NSSet *shareObjectTypes = [NSSet setWithObjects:
                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMass],
                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierHeight],
                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMassIndex],
                           nil];

// Read date of birth, biological sex and step count
NSSet *readObjectTypes  = [NSSet setWithObjects:
                           [HKObjectType characteristicTypeForIdentifier:HKCharacteristicTypeIdentifierDateOfBirth],
                           [HKObjectType characteristicTypeForIdentifier:HKCharacteristicTypeIdentifierBiologicalSex],
                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierStepCount],
                           nil];

// Request access
[healthStore requestAuthorizationToShareTypes:shareObjectTypes
                                    readTypes:readObjectTypes
                                   completion:^(BOOL success, NSError *error) {

                                       if(success == YES)
                                       {
                                           // ...
                                       }
                                       else
                                       {
                                           // Determine if it was an error or if the
                                           // user just canceld the authorization request
                                       }

                                   }];
```

如上代码会调用下图这样的权限请求界面：

![](http://jademind.com/wp-content/uploads/2014/11/healthkit_request_auth_dialog-575x1024.png)

用户在该界面上可以选择接受或者拒绝某些对于读写健康数据的请求。在确定或者关闭请求界面之后，回调会被自动调用。



# 读写数据

从Health Store中读写数据的方法比较直接，HKHealthStore类是提供了很多便捷的方法读取基本的属性。不过如果需要以更多复杂的方式进行查询，可以使用相关的子类：HKQuery。

## 生理数据

### 性别与年龄

``` objective-c
NSError *error;
HKBiologicalSexObject *bioSex = [healthStore biologicalSexWithError:&error];

switch (bioSex.biologicalSex) {
    case HKBiologicalSexNotSet:
        // undefined
        break;
    case HKBiologicalSexFemale:
        // ...
        break;
    case HKBiologicalSexMale:
        // ...
        break;
}
```

### 体重

``` objective-c
// Some weight in gram
double weightInGram = 83400.f;

// Create an instance of HKQuantityType and
// HKQuantity to specify the data type and value
// you want to update
NSDate          *now = [NSDate date];
HKQuantityType  *hkQuantityType = [HKQuantityType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMass];
HKQuantity      *hkQuantity = [HKQuantity quantityWithUnit:[HKUnit gramUnit] doubleValue:weightInGram];

// Create the concrete sample
HKQuantitySample *weightSample = [HKQuantitySample quantitySampleWithType:hkQuantityType
                                                                 quantity:hkQuantity
                                                                startDate:now
                                                                  endDate:now];

// Update the weight in the health store
[healthStore saveObject:weightSample withCompletion:^(BOOL success, NSError *error) {
    // ..
}];
```



## 运动数据

运动数据查询时往往进行的是统计类型的查询，即查询某几个小时或者某几天的运动数据情况，此时最常用的工具类即是HKStatisticsQuery。要使用该类，首先是调用```initWithQuantityType:quantitySamplePredicate:options:completionHandler:```方法进行初始化，然后使用[executeQuery:](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/occ/instm/HKHealthStore/executeQuery:)方法进行查询。

### 步数与卡路里

- Objective-C

``` objective-c
- (void)fetchTotalJoulesConsumedWithCompletionHandler:(void (^)(double, NSError *))completionHandler {
    NSCalendar *calendar = [NSCalendar currentCalendar];

    NSDate *now = [NSDate date];

    NSDateComponents *components = [calendar components:NSCalendarUnitYear|NSCalendarUnitMonth|NSCalendarUnitDay fromDate:now];

    NSDate *startDate = [calendar dateFromComponents:components];

    NSDate *endDate = [calendar dateByAddingUnit:NSCalendarUnitDay value:1 toDate:startDate options:0];

    HKQuantityType *sampleType = [HKQuantityType quantityTypeForIdentifier:HKQuantityTypeIdentifierDietaryEnergyConsumed];
    NSPredicate *predicate = [HKQuery predicateForSamplesWithStartDate:startDate endDate:endDate options:HKQueryOptionStrictStartDate];

    HKStatisticsQuery *query = [[HKStatisticsQuery alloc] initWithQuantityType:sampleType quantitySamplePredicate:predicate options:HKStatisticsOptionCumulativeSum completionHandler:^(HKStatisticsQuery *query, HKStatistics *result, NSError *error) {
        if (!result) {
            if (completionHandler) {
                completionHandler(0.0f, error);
            }
            return;
        }

        double totalCalories = [result.sumQuantity doubleValueForUnit:[HKUnit jouleUnit]];
        if (completionHandler) {
            completionHandler(totalCalories, error);
        }
    }];

    [self.healthStore executeQuery:query];
}
```

- Swift

``` swift
func fetchTotalJoulesConsumedWithCompletionHandler(
    completionHandler:(Double?, NSError?)->()) {

        let calendar = NSCalendar.currentCalendar()
        let now = NSDate()
        let components = calendar.components(.YearCalendarUnit |
            .MonthCalendarUnit | .DayCalendarUnit, fromDate: now)

        let startDate = calendar.dateFromComponents(components)

        let endDate = calendar.dateByAddingUnit(.DayCalendarUnit,
            value: 1, toDate: startDate, options: NSCalendarOptions(nil))

        let sampleType = HKQuantityType.quantityTypeForIdentifier(
            HKQuantityTypeIdentifierDietaryEnergyConsumed)

        let predicate = HKQuery.predicateForSamplesWithStartDate(startDate,
            endDate: endDate, options: .StrictStartDate)

        let query = HKStatisticsQuery(quantityType: sampleType,
            quantitySamplePredicate: predicate,
            options: .CumulativeSum) { query, result, error in

                if result != nil {
                    completionHandler(nil, error)
                    return
                }

                var totalCalories = 0.0

                if let quantity = result.sumQuantity() {
                    let unit = HKUnit.jouleUnit()
                    totalCalories = quantity.doubleValueForUnit(unit)
                }

                completionHandler(totalCalories, error)
        }

        healthStore.executeQuery(query)
}
```

注意，在根据时间来获取HealthKit的记录值时，往往存在着一个时区的问题。即HealthKit默认的是用的UTC时间来进行计数的，虽然它显示的是本地时间。而我们一般对时区进行本地化时，只是将NSDate的数值变化了，并不一定会修正它所在的时区，笔者用的方法所在时区还是UTC。所以，可能需要先求出起始时间的TimeInterval，然后统一用UTC时间计算：

``` objective-c
    startDate = [[NSDate alloc] initWithTimeIntervalSinceNow:- [endDate timeIntervalSinceDate:startDate]];
    
    endDate = [[NSDate alloc] init];
```

# [CareKit](https://github.com/carekit-apple/CareKit)

![](https://github.com/carekit-apple/CareKit/wiki/AddedBinaries.png)



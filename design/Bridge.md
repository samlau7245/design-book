# 介绍

<!-- ## 前言
## 优点
## 缺点
## 注意 -->
## 场景

* 不希望使用继承或因为多层次继承导致系统类的个数急剧增加的系统，桥接模式尤为适用。
* 一个类存在两个独立变化的维度，且这两个维度都需要进行扩展。

<img src="/assets/images/design/Bridge_1.JPG"/>

<img src="/assets/images/design/Bridge_2.JPG"/>

# 示例

<img src="/assets/images/design/Bridge_3.png"/>

接口定义：

```objc
@protocol SoundInterface <NSObject>
-(NSString*)getSound;
@end

@interface SystemMac : NSObject<SoundInterface>
@end

@interface SystemWindows : NSObject<SoundInterface>
@end

@interface SystemLinux : NSObject<SoundInterface>
@end

@interface SoftWareA : NSObject
@property(nonatomic,strong) id<SoundInterface> sound;
-(void)play;
- (instancetype)initWithSound:(id<SoundInterface>)sound;
@end

@interface SoftWareA_A : SoftWareA
-(void)playVIP;
@end
```

实现：

```objc
@implementation SystemMac
-(NSString *)getSound {
    return @"Mac Sound";
}
@end

@implementation SystemWindows
-(NSString *)getSound {
    return @"Windows Sound";
}
@end

@implementation SystemLinux
-(NSString *)getSound {
    return @"Linux Sound";
}
@end

@implementation SoftWareA
- (instancetype)initWithSound:(id<SoundInterface>)sound
{
    self = [super init];
    if (self) {
        _sound = sound;
    }
    return self;
}
-(void)play {
    NSLog(@"%@",[self.sound getSound]);
}
@end

@implementation SoftWareA_A
-(void)playVIP {
    NSLog(@"VIP:%@",[self.sound getSound]);
}
@end
```

使用：

```objc
+(void)run {
    [[[SoftWareA alloc] initWithSound:[SystemMac new]] play];
    [[[SoftWareA alloc] initWithSound:[SystemWindows new]] play];
    
    [[[SoftWareA_A alloc] initWithSound:[SystemLinux new]] play];
    [[[SoftWareA_A alloc] initWithSound:[SystemWindows new]] playVIP];
}

// 打印结果：
// Mac Sound
// Windows Sound
// Linux Sound
// VIP:Windows Sound
```


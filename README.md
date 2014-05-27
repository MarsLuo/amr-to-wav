amr-to-wav
==========

a library that can convert amr to wav


##useage

### 1.drag arm folder to your project.

### 2.include "amrFileCodec.h"

```obj-c
#import "amrFileCodec.h"
```

### 3.convert and  play

```obj-c
    NSString *mediaPath = [NSString stringWithFormat:@"%@%@",HOST_VOICE,order.sound];
    
    NSURL *url = [[NSURL alloc]initWithString:mediaPath];
    NSData * audioData = [NSData dataWithContentsOfURL:url];
    
    
    //将数据保存到本地指定位置
    NSString *docDirPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
    NSString *filePath = [NSString stringWithFormat:@"%@/%@.amr", docDirPath , @"temp"];
    [audioData writeToFile:filePath atomically:YES];
    
    //播放本地音乐
    NSURL *fileURL = [NSURL fileURLWithPath:filePath];
    //amr 转 wav
    [Tool initPlayer];
    NSData *data = [NSData dataWithContentsOfURL:fileURL];
    
    NSError *error;
    player = [[AVAudioPlayer alloc]initWithData:DecodeAMRToWAVE(data) error:&error];
    NSLog(@"error:%@",error);
    
    [player play];

```

```obj-c
+ (void)initPlayer{
    //初始化播放器的时候如下设置
    UInt32 sessionCategory = kAudioSessionCategory_MediaPlayback;
    AudioSessionSetProperty(kAudioSessionProperty_AudioCategory,
                            sizeof(sessionCategory),
                            &sessionCategory);
    
    UInt32 audioRouteOverride = kAudioSessionOverrideAudioRoute_Speaker;
    AudioSessionSetProperty (kAudioSessionProperty_OverrideAudioRoute,
                             sizeof (audioRouteOverride),
                             &audioRouteOverride);
    
    AVAudioSession *audioSession = [AVAudioSession sharedInstance];
    //默认情况下扬声器播放
    [audioSession setCategory:AVAudioSessionCategoryPlayback error:nil];
    [audioSession setActive:YES error:nil];
    audioSession = nil;
}
```

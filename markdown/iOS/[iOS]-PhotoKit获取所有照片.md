####代码:
```
- (NSMutableArray *)getAllPhoto{
    NSMutableArray *arr = [NSMutableArray array];
        // 所有智能相册
    PHFetchResult *smartAlbums = [PHAssetCollection fetchAssetCollectionsWithType:PHAssetCollectionTypeSmartAlbum subtype:PHAssetCollectionSubtypeAlbumRegular options:nil];
    for (NSInteger i = 0; i < smartAlbums.count; i++) {
        PHCollection *collection = smartAlbums[i];
        //遍历获取相册
        if ([collection isKindOfClass:[PHAssetCollection class]]) {
            PHAssetCollection *assetCollection = (PHAssetCollection *)collection;
            PHFetchResult *fetchResult = [PHAsset fetchAssetsInAssetCollection:assetCollection options:nil];
            PHAsset *asset = nil;
            if (fetchResult.count != 0) {
                for (NSInteger j = 0; j < fetchResult.count; j++) {
                    //从相册中取出照片
                    asset = fetchResult[j];
                    PHImageRequestOptions *opt = [[PHImageRequestOptions alloc]init];
                    opt.synchronous = YES;
                    PHImageManager *imageManager = [[PHImageManager alloc] init];
                    [imageManager requestImageForAsset:asset targetSize:PHImageManagerMaximumSize contentMode:PHImageContentModeAspectFill options:opt resultHandler:^(UIImage * _Nullable result, NSDictionary * _Nullable info) {
                        if (result) {
                            [arr addObject:result];
                        }
                    }];
        }
            }
    }
        }
    
    //返回所有照片
    return arr;
}
```
####由于此方法为同步方法 所以需要放在子线程中去执行 例如
```
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        NSMutableArray *arr = [self getAllPhoto];
        NSLog(@"完成%@ \n照片总数%ld", arr, arr.count);
    });﻿​
```

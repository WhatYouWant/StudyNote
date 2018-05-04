#SDWebImage学习笔记

##NSData+ImageContentType文件分析

```
+ (SDImageFormat)sd_imageFormatForImageData:(nullable NSData *)data {
	if (!data) {
		return SDImageFormatUndefined;
	}
	
	uint8_t c;
	[data getBytes:&c length:1]; //将NSData转换成十六进制数据，第一个字节的数据
	switch(c) {
		case 0xFF:
			return SDImageFormatJPEG;
		case 0x89:
			return SDImageFormatPNG;
		case 0x47:
		 	return SDImageFormatGIF;
		case 0x49:
		case 0x4D:
			return SDImageFormatTIFF;
		case 0x52: {
			if (data.length>=12){
			//NSData可以进行截取，转换成字符串
				NSString *testString = [[NSString alloc] initWithData:[data subdataWithRange:NSMakeRange(0,12)] encoding:NSASCIIStringEncoding];
				if([testString hasPrefix:@"RIFF"] && [testString hasSuffix:@"WEBP"]) {
					return SDImageFormatWebP;
				}
			}
			break;
		}
		case 0x00: {
			if (data.length>=12) {
				//....ftypheic ....ftypheix ....ftyphevc ....ftyphevx
                NSString *testString = [[NSString alloc] initWithData:[data subdataWithRange:NSMakeRange(4, 8)] encoding:NSASCIIStringEncoding];
                if ([testString isEqualToString:@"ftypheic"]
                    || [testString isEqualToString:@"ftypheix"]
                    || [testString isEqualToString:@"ftyphevc"]
                    || [testString isEqualToString:@"ftyphevx"]) {
                    return SDImageFormatHEIC;
                }
			}
			break;
		}
	}
	return SDImageFormatUndefined;
}
```

### __deprecated_msg 

```
+ (NSString *)contentTypeForImageData:(NSData *)data __deprecated_msg("Use 'sd_contentTypeForImageData:'") 
```
	__deprecated_msg 可以告诉开发者该方法不建议使用。在开发新功能时，如果功能相同，但是想使用新的方法名的时候，使用 __deprecated_msg 给予开发者一个提示。

###UIImage解压缩
参考地址<http://www.cocoachina.com/ios/20170227/18784.html>
####图片加载的工作流
	概括来说，从磁盘中加载一张图片，并将它显示到屏幕上，中间的主要工作流如下：
	1.假设我们使用 +imageWithContentsOfFile: 方法从磁盘中加载一张图片，这个时候的图片并没有解压缩；
	2.然后将生成的 UIImage 赋值给 UIImageView ；
	3.接着一个隐式的 CATransaction 捕获到了 UIImageView 图层树的变化；
	4.在主线程的下一个 run loop 到来时，Core Animation 提交了这个隐式的 transaction ，这个过程可能会对图片进行 copy 操作，而受图片是否字节对齐等因素的影响，这个 copy 操作可能会涉及以下部分或全部步骤：
	•	分配内存缓冲区用于管理文件 IO 和解压缩操作；
	•	将文件数据从磁盘读到内存中；
	•	*将压缩的图片数据解码成未压缩的位图形式*，这是一个非常耗时的 CPU 操作；
	•	最后 Core Animation 使用未压缩的位图数据渲染 UIImageView 的图层。

###为什么需要解压缩
	位图就是一个像素数组，数组中的每个像素就代表着图片中的一个点。我们经常用到的JPEG和PNG图片就是位图。JPEG和PNG图片都是一种压缩的位图图形格式，不过PNG图片是无损压缩，并且支持alpha通道，而JPEG图片则是有损压缩。将磁盘中的图片渲染到屏幕之前，必须先要得到图片的原始像素数据，才能执行后续的绘制操作，所以需要对图片进行解压缩。
###强制解压缩原理
 我们前面已经提到了，当未解压缩的图片将要渲染到屏幕时，系统会在**主线程**对图片进行解压缩，而如果图片已经解压缩了，系统就不会再对图片进行解压缩。因此，也就有了业内的解决方案，在**子线程**提前对图片进行强制解压缩。
 而强制解压缩的原理就是对图片进行**重新绘制**，得到一张新的解压缩后的位图。其中，用到的最核心的函数是 CGBitmapContextCreate(创建位图上下文) ：
 
```
CG_EXTERN CGContextRef __nullable CGBitmapContextCreate(void * __nullable data,
    size_t width, size_t height, size_t bitsPerComponent, size_t bytesPerRow,
    CGColorSpaceRef cg_nullable space, uint32_t bitmapInfo)
    CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);
    
    ***************参数含义**********************
    * data: 如果不为 NULL ，那么它应该指向一块大小至少为 bytesPerRow * height 字节的内存；如果 为 NULL ，那么系统就会为我们自动分配和释放所需的内存，所以一般指定 NULL 即可；
    * width和height:位图的宽度和高度，分别赋值为图片的像素宽度和像素高度即可;
    * bitsPerComponent:像素的每个颜色分量使用的 bit 数，在 RGB 颜色空间下指定 8 即可;
    * bytesPerRow:位图的每一行使用的字节数，大小至少为 width * bytes per pixel 字节。有意思的是，当我们指定 0 时，系统不仅会为我们自动计算，而且还会进行 cache line alignment 的优化。
    * space: 颜色空间，一般使用RGB即可。
    * bitmapInfo:位图的布局信息。
```

###Pixel Format(像素格式)
像素格式用来描述每个像素的组成格式。

```
* Bits per component: 一个像素中每个独立的颜色分量使用的bit数；
* Bits per pixel: 一个像素使用的总bit数；
* Bytes per row: 位图中的每一行使用的字节数。
```

###图片强制解压缩的实现

```
CGContextRef context = CGBitmapContextCreate(NULL, width, height, 8, 0, <RGB>, bitmapInfo);
if (!context) return NULL;

CGContextDrawImage(context, CGRectMake(0, 0, width, height), imageRef);
CGImageRef newImage = CGBitmapContextCreateImage(context);
CFRelease(context);
```
接收一个原始的位图参数imageRef, 最终返回一个新的解压缩后的位图newImage,中间主要经过以下三个步骤:

```
* 使用CGBitmapContextCreate函数创建一个位图上下文;
* 使用CGContextDrawImage函数将原始位图绘制到上下文中;
* 使用CGBitmapContextCreateImage函数创建一张新的解压缩后的位图。
```

##数组(字典)中添加弱引用；NSPointerArray, NSHashTable, NSMapTable
NSPointerArray,可以创建强引用，弱引用对象的数组
```
+ (NSPointerArray *)strongObjectsPointerArray NS_AVAILABLE(10_8, 6_0);
+ (NSPointerArray *)weakObjectsPointerArray NS_AVAILABLE(10_8, 6_0);
```
当对象被释放后，数组中同时会置为NULL;
 NSHashTable类似于NSSet;
 NSMapTable类似于NSDictionary;
 
##定义常量字符串

```
.h
FOUNDATION_EXPORT NSString * _Nonnull SDWebImageInternalSetImageGroupKey;

.m
NSString * const SDWebImageInternalSetImageGroupKey = @"xxx";

``` 

##定义取地址的字符串

```
static char imageURLKey;

- (nullable NSURL *)sd_imageURL {
	return objc_getAssociatedObject(self, &imageURLKey);
}

```





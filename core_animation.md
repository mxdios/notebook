# Core Animation 开发笔记

## CAlayer

1. CALayer 和UIView类似，同样是一些被层级关系树管理的矩形块。包括图片、文本、背景色等。和UIView最大不同是 不能处理用户交互

2. CALayer添加图层，需求是在UIView上添加一个图层，处理方式不是添加一个UIView(这样当然可以实现)，是创建一个layer，这是layer的背景色，添加到View的layer上

		UIView *bgview = [[UIView alloc] initWithFrame:CGRectMake(100, 100, 200, 200)];
	    bgview.backgroundColor = [UIColor grayColor];
	    [self.view addSubview:bgview];
	    
	    CALayer *layer = [CALayer layer];
		layer.frame = CGRectMake(30, 30, 100, 100);
		layer.backgroundColor = [UIColor blueColor].CGColor;
		[bgview.layer addSublayer:layer];


## layer的contents

1. 在UIView上显示一张图片。一般的处理方式是在view上添加一个UIImageView ，现在可以用layer的contents

		UIView *bgview = [[UIView alloc] initWithFrame:CGRectMake(100, 100, 200, 100)];
	    bgview.backgroundColor = [UIColor grayColor];
	    [self.view addSubview:bgview];

		bgview.layer.contents = (__bridge id _Nullable)([UIImage imageNamed:@"Icon-Small-50"].CGImage);


2. 为什么要用关键字 \__bridge 修饰UIImage？ 因为，contents的类型是id，之所以被定义为id，是因为在MacOS系统上，这个属性对CGImage和NSImage都有作用。在iOS上，如果赋值UIImage，只能得到一个空白图层。其实真正赋值的是CGImageRef类型，它是指向CGImage结构的指针，.CGImage返回了“CGImageRef”，把它直接赋值给contents，会报编译错误，因为CGImageRef不是一个Cocoa对象，而是一个Core Foundation类型，所以要通过__bridge 修饰。（如果用的非arc则不需要）
	 
3. 这时image会根据bgview的大小形变缩放了。 在UIImageView里有一个contentMode的属性来设置image的缩放

		imageview.contentMode = UIViewContentModeScaleAspectFit;
	
4. 在layer里有 contentsGravity

		bgview.layer.contentsGravity = kCAGravityResizeAspect;

## contentsScale

1. 这个属性很少用到。这个属性顶一个图片的像素尺寸和视图比例

2. 举个例子，将图片设置为不能拉伸， kCAGravityCenter这个属性值，会将图设置为不拉伸，图有多大就显示多大，不会局限于view的大小
		
		bgview.layer.contentsGravity = kCAGravityCenter;
	
3. 图片为@2x图片，这时图片会放大了，会有明显的颗粒点。因为这时每个点1个像素绘制了图片，retina屏幕是每个点绘制2个像素点。CGImage没有拉伸的概念，如果用UIImage去拉伸图片没有问题。在用CGImage时拉伸时会丢失，所以模糊了。这时需要用contentsScale来修复这个问题
	
		bgview.layer.contentsScale = image.scale;
		
		//image.scale 在@2x时 = 2， @1x时 = 1


## maskToBounds

1. 图片超出view的范围要切掉。在UIView中用 clispToBounds ，在CALayer中 使用 maskToBounds

	    bgview.layer.masksToBounds = YES;


## contentsRect

1. 切割图片，设置添加到layer.contents上的图片的子域。就是只显示某一部分

2. contentsRect 的取值是相对值。相对于图片的尺寸。默认 {0,0,1,1}， 如果取值 {0, 0, 0.5, 0.5} 则显示图片的左上角整个图片的1/4处
		
		//显示图片的上面1/2处
	    bgview.layer.contentsRect = CGRectMake(0, 0, 1, 0.5);

3. 使用场景：载入多张图拼合一起的整张图，切割了放在不同地方。单张大图比多张小图载入速度快。缺点就是某个小图尺寸改变时会很麻烦。

4. 有一些软件可以生成拼合图片，并生成拼合坐标的xml或plist文件，并给每个拼合图层设置contentsRect。这样就方便多了。我也在找这个软件

## contentsCenter

1. 可拉伸区域, contentsCenter的值也是相对值，默认 {0,0,1,1}，图片将会均匀拉伸。contentsCenter可以指定拉伸区域

    	bgview.layer.contentsCenter = CGRectMake(0.25, 0.25, 0.5, 0.5);
    	//中间一部分可以拉伸

2. 类似UIImage的不形变拉伸方法，指定拉伸位置

		- (UIImage *)stretchableImageWithLeftCapWidth:(NSInteger)leftCapWidth topCapHeight:(NSInteger)topCapHeight
		

## drawing

1. 自定义view的时候，会有-drawRect方法，在这里可以绘制。这个方法不会默认实现，如果用不到寄宿图的话，就不要创建这个方法。没有自定义的绘制任务时，不要写一个空的 -drawRect方法，会造成资源浪费

2. 使用layer的代理方法，绘制寄宿图

		CALayer *layer = [CALayer layer];
	    layer.frame = CGRectMake(30, 30, 100, 100);
	    layer.backgroundColor = [UIColor blueColor].CGColor;
	    layer.delegate = self;
	    layer.contentsScale = [UIScreen mainScreen].scale;
	    [bgview.layer addSublayer:layer];
	    
	    // 需要手动调用，因为CALayer不会自动重绘内容。
	    [layer display];
	    
	    //代理方法
	    - (void)drawLayer:(CALayer *)layer inContext:(CGContextRef)ctx
		{
		    CGContextSetLineWidth(ctx, 10);
		    CGContextSetStrokeColorWithColor(ctx, [UIColor redColor].CGColor);
		    CGContextStrokeEllipseInRect(ctx, layer.bounds);
		}
		// 不会对超出layer存放边界以外的地方有绘制作用，所有超出的地方就像被切掉了一样，类似masksToBounds一样
		


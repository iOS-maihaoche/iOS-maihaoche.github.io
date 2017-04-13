---
title: "iOS 代码规范"
category: group
author: "ajie"
description: 卖好车 iOS 团队代码规范
layout: post
header: iOS code guideline
---

# 卖好车 iOS 开发规范

*version 1.1*

## 开发规范介绍
本文是卖好车 iOS 开发规范说明，该规范非单纯的代码规范，是集成了公司项目管理规范、iOS 代码规范、iOS 项目开发规范以及苹果证书管理相关等内容，主要分为以下几个章节：

* Git flow
* 代码规范
* 模块化开发
* 代码复审
* 证书管理
* JSPatch
* Cocoapods


## Git flow
Git flow 是项目开发过程中的一套标准 git 使用流程，它作为卖好车技术部基础技能，在日常开发过程中必须严格遵循 Git flow 流程，对代码进行合理的管理。
更多详细的使用教程可参考欧雷编写的这篇文章 [Git flow in maihaoche](http://cf.dawanju.net/pages/viewpage.action?pageId=3244594)，因为是基础技能，在此不再赘述。

## 代码规范
代码规范的作用，就是让项目代码的可读性、可维护性大大提高，并且这也是对自我要求的一种体现，一套好的编码规范也会让你在之后的开发工作中节省大量的时间。

目前网络上可以搜到的 iOS 代码规范可谓聆郎满目，有苹果官方推出的 [Coding Guidelines for Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)、Google 推出的 [Google Objective-C Style Guide](https://google.github.io/styleguide/objcguide.xml)、 Github 总结的 [objective-c-style-guide](https://github.com/github/objective-c-style-guide)，以及这本讲述提高 Objective-C 开发效率的书籍 [Effective Objective-C 2.0(Chinese Edition)](https://www.amazon.com/Effective-Objective-C-Chinese-YING-Galloway/dp/B00IDSGY06/ref=sr_1_3?ie=UTF8&qid=1490855446&sr=8-3&keywords=Effective+Objective-C+2.0)。

你可以参照上述的编码规范，因为这些都是非常不错的规范，下面的规范是对以上的精炼总结以及根据日常项目开发习惯进行定制。

> 注：这将会花费你一些时间来浏览所有代码规范，不过强烈建议能够完整看完这份规范。规范中会标注遵守等级其中`必要`等级的规范是必须要遵守的，`建议`等级的规范是强烈建议遵守，但根据个人编码习惯可进行取舍。

### 一、命名规范

Quora 上曾今有一个热门问题叫做，[What is the hardest thing you do as a software engineer](https://www.quora.com/Software-Engineering/What-is-the-hardest-thing-you-do-as-a-software-engineer)，其中有个答案获得了很多人的赞同，那就是 `naming the variables`。很多人在开发过程中就会因为一个变量名、一个方法名该如何命名而困扰，因为一个变量或方法的名称不仅需要简洁，而且还需要易理解。在下面的各个类别中，介绍了如何完成一个名称的创建。

#### 类/ 类目/ 代理

文件中的类名必须与文件的名称相同，类的命名规范如下：

* 不同的项目设置对应的 ClassPrefix，类名的前缀必须是项目对应的类前缀（如：**MHCB2B**）
* 不同的业务模块，类名的前缀后添加对应的模块前缀（如：MHCB2B**DMS**）
* 类名后缀不要采用缩写，保持单词拼写完整，保证能够清晰表述该类的作用
	* 正例：MHCB2BDMS**HomeViewController**，MHCB2BSeekCar**AllQuoteViewController**
	* 反例：MHCB2B**NoteGroupSendVC**，MHCB2B**PVModel**

#### 变量

变量的名称采用驼峰式命名法，一个完整的例子如下：

```
// in .m file
@interface MHCB2BSeekCarHomeController() {
    NSInteger _myClueNewNumber;     // 我的寻车线索数
    BOOL _bMyQuoteNewPoint;         // 是否需要显示红点
}

@property (nonatomic, strong) UICollectionView *collectionView;
@property (nonatomic, assign) BOOL bDisplayTab; // 是否要显示 Tab
@property (nonatomic, weak) IBOutlet 

@end

@implementation MHCB2BSeekCarHomeController 

/** Cell 的 ReusedId（需要定义成 static，以避免 duplicate symbols）*/
static NSString *const kReusedIdCommentCell = @"CommentCell";  

...

@end

```

* 成员变量前添加下划线，统一与属性名称调用方式相同，如：`_myClueNewNumber`
* 布尔型变量前添加类型标识，如：`_bMyQuoteNewPoint`，`bDisplayTab`
* 常量前添加类型标识，如：`kReuseIdCommentCell`


#### 方法

方法的名称采用驼峰式命名法

```
- (CGFloat)cellHeightWithContent:(NSString *)content {
    ...
}
```

```
- (UIView *)_generalHeaderWithTitle:(NSString *)title {
    ...
}
```

#### 枚举

根据枚举值的使用情况不同，可以分为两种类型定义：

第一种，枚举值只能单独使用，每个枚举值需要定义它所对应的值

```
/** 分享渠道枚举*/
typedef NS_ENUM(NSInteger, MHCB2BShareChannel) {
    MHCB2BShareChannelSession  = 0, // 微信消息列表
    MHCB2BShareChannelTimeLine = 1, // 微信朋友圈
    MHCB2BShareChannelMessage  = 2  // 短信
};
```

第二种，枚举值可以多值同时使用

```
/** 客户筛选状态枚举*/
typedef NS_OPTIONS(NSUInteger, MHCB2BFilterClientStatus) {
    MHCB2BFilterClientStatusUnhandle   = 1 << 0,  // 未处理
    MHCB2BFilterClientStatusFollowing  = 1 << 1,  // 跟进中
    MHCB2BFilterClientStatusOrder      = 1 << 2,  // 下订未交车
    MHCB2BFilterClientStatusDeal       = 1 << 3,  // 已交车
    MHCB2BFilterClientStatusLost       = 1 << 4,  // 战败
    MHCB2BFilterClientStatusUnorder    = 1 << 5,  // 退订
    MHCB2BFilterClientStatusInvalid    = 1 << 6   // 无效
};
```

> 注：不论哪种类型枚举，其名称建议以`MHCB2B`作为前缀。若是较容易重复的名称，最好在前缀后再拼接对应模块名称。

#### Block

Block 在代码中可以作为变量，也可以作为参数。在使用 block 之前建议先定义好名称

```objc
typedef void (^ResultHandler)(BOOL success, NSDictionary *result, NSError *error);
typedef void (^FinishAction)(void);
```

当 block 作为参数时，不预先定义 block 而是直接在参数上定义

```objc
- (instancetype)initWithName:(NSString *)name foldAction:(void (^)(void))foldAction andSpreadAction:(void (^)(BOOL finished))spreadAction;
```

#### 宏

宏的名称中全部使用大写字母，多个单词之间使用下划线连接

```objc
#define ONE_PIXEL (1.0f / [UIScreen mainScreen].scale)
#define STRENGTHEN(self) __strong __typeof(self) strongSelf = self
```

#### 通知

为了防止项目中不同模块的项目单独定义，引起通知名称重名导致的 bug，项目中的通知对应的 Key 值都定义在 `MHCB2BNotificationKey` 文件中，使用或新增新的通知先查看该文件。

通知 key 名称的定义规范：`前缀 + 发起者动作`

```
kNotification + Assign a client = kNotificationClientAssigned
```

#### UserDefaultsKey

项目使用到的 UserDefaults key 都定义在 `MHCB2BUserDefaultsKey` 文件中，使用或新增通知先查看该文件。

UserDefaults key 名称定义规范：`前缀 + key 值意义`

```
kUserDefaults + Whether the DMS guide view was shown before = kUserDefaultsHasShownDMSGuide
```

#### 图片素材

图片素材的文件按照自己的模块分成组，在自己的模块内，图片素材以模块缩写作为前缀。如`dms_no_clue_icon`，`profile_combined_shape`等。

反例：   
 
```
| Lotics
    | combinedShape
| profile
    | combinedShape
```  

* 文件夹名称要使用大写：`Profile`
* 不要使用错误的英文单词：`Logistics`
* 避免图片名称重复，名称要添加前缀：`profile_combined_shape`，`logistics_combined_shape`


### 二、注释规范

一段优秀的代码不需要添加任何注释，因为当阅读到它的时候就明白它的用途，但理想和现实总是有出入。项目中更多的是业务需求代码，当你写好代码后没有任何注释，不了解业务需求的后人看这块代码时是非常吃力的，因此在协同开发的团队中，注释是必不可少的。

知道注释是不可或缺的之后，如何写出完整的文档就非常重要了。这里有一份他人总结的 Objective-C 注释规范 [Objective-C Comment Style](http://nshipster.com/documentation/) 可以作为参考，更多的注释规范请继续往下看。

iOS 开发中常见的注释有以下几种：

```objc
// 方法中的简短注释，用于说明该代码块的作用

/** 属性、方法实现体、枚举值的含义解释*/

/**
 头文件中方法含义的说明
 
 @param arg  参数意义
 @return ret 返回值意义
 */
 
 /**
  头文件中对类名的说明（阐述这个类的用途）
  */
```

基本上这几种注释就可以涵盖我们日常开发中所需要用到的注释类型，每种注释的用途已经在上面解释过了，不同的地方使用对应的注释方法。

> 提示：这里推荐使用 Xcode 8 之后自带的注释快捷键 `Command` + `Option` + `/` 生成方法注释

#### 类注释

业务模块中，现有的类已经多到数不清，除了该类的创建者外，其余开发者无法第一时间了解到该类的用途，需要花大量时间阅读代码或断点调试。因此，在类创建后添加类注释的意义就非常明显，每个类有注释后会便于他人排查问题以及阅读代码。一个正常的类注释如下：

```objc
//
//  MHCB2BClientListViewController.h
//  MHCB2BApp
//
//  Created by xujie on 16/9/2.
//  Copyright © 2016年 maihaoche. All rights reserved.
//
//  DMS - 客户列表页面
//
```

### 三、弃用 API

项目中存在一些方法，因为其功能不满足新需求或性能损耗过高，但仍然被一些老模块代码调用，这些方法就被标记为弃用 API。

弃用 API 会在项目中存在一段时间，在这段时间内方法会被标记为 `NS_DEPRECATED_IOS`，新代码不建议在使用。等到老代码重构后，去掉该方法的调用，这些 API 就会被删除。

标记弃用 API，必须说明在哪个版本启用，哪个版本弃用，并且现应该调用哪个接口去完成此功能。

```objc
/**
 通过forwardUrl + urlType(2拼接userId) + function(title) 快速初始化一个H5Web JumpModel
 */
+ (instancetype)modelWithWebForwardUrl:(NSString *)forwardUrl
                               urlType:(NSInteger)urlType
                              function:(NSString *)function NS_DEPRECATED_IOS(2_0, 2_0, "由于拼接userId和title目前已在webview里做了,请改用modelWithWebForwardUrl:");
```

### 四、项目代码要求

#### Storyboard & Xib

使用过 storyboard 的人都应该经历过合并代码冲突后的抓狂，因为 storyboard 文件是 xml 结构，而在 Xcode 中以可视化图形展示，因此冲突极难解决。项目现在已经禁止新建 storyboard，现在项目中几个老模块的 storyboard 也会逐渐地移除掉。

由于 Xib/Nib 的颗粒度更高，冲突产生的几率小，因此 Xib/Nib 是支持使用的。但是现在发现很多 Xib 中仅仅只有一个 UITableView 并且也没有做过多的设置，这不经增加了项目静态资源文件的大小，也增加了代码的可读性。因此，项目中禁止新增的 Xib 中仅有一个单一的控件，这种情况下就不要创建 Xib，而选择使用代码来构建 UI。

#### 公共基类

项目中的每个 ViewController 都需要去继承项目的公共基类，来实现一些通用的功能，如断网页面显示、阿里云埋点、页面进入打印等。现有的公共基类有：

*MHCB2BBaseDataListViewController*

用于带有网络请求的列表页面，支持断网显示无网页面，点击重新加载继续网络请求

*MHCB2BBaseDataViewController*

用于带有网络请求的非列表页面，支持断网显示无网页面，点击重新加载刷新网络请求

*MHCB2BBaseTableView*

用于 Table View，现已弃用

*MHCB2BBaseTableViewController*

用于列表页面，现已弃用

*MHCB2BBaseViewController*

所有页面的基类，提供阿里云埋点，页面进入打印功能

*MHCB2BBaseWebViewController*

Web View 的基类

#### 其他

下面是一个符合代码规范的 Demo 文件：

**MHCB2BDMSHomeViewController.h**

```objc
#import "MHCB2BBaseDataViewController.h"

/** DMS 首页来源页面类型*/
typedef NS_ENUM(NSInteger, MHCB2BDMSSourceVCType) {
    MHCB2BDMSSourceVCTypeHome       = 0,    // 来源是首页
    MHCB2BDMSSourceVCTypeAllMessage = 1,    // 来源是全部消息
}

@class MHCB2BDMSSalerTool;
@interface MHCB2BDMSHomeViewController : MHCB2BBaseDataViewController

/** 来源页面类型*/
@property (nonatomic, assign) MHCB2BDMSSourceVCType sourceVCType;

/**
 DMS 首页添加销售工具
 
 @note 在销售工具栏中添加自定义的销售工具

 @param sellerTool 销售工具
 */
- (void)addSellerTool:(MHCB2BDMSSalerTool * _Nonnull)sellerTool;

@end
```

**MHCB2BDMSHomeViewController.m**

```objc
#import "MHCB2BDMSHomeViewController.h"

#import "MHCB2BDMSSalerTool.h"

@interface MHCB2BDMSHomeViewController()

@end

@implementation MHCB2BDMSHomeViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    [self _initData];
    [self _initUI];
    [self _registerNotifications];
    [self _requestHomeData];
}

- (void)addSellerTool:(MHCB2BDMSSalerTool *)sellerTool {
    // some code here...
}

- (void)refreshData {
    [self _requestHomeData];
}

- (void)_initData {
    
    // 在这里初始化一些变量属性
    // some code here...
}

- (void)_initUI {

    // 在这里初始化 UI
    // some code here...
}

- (void)_registerNotifications {
    
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(_refreshHome) name:kNotificationClientFollowed object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(_refreshHome) name:kNotificationClientAssigned object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(_refreshHome) name:kNotificationClientsBatchAssigned object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(_refreshHome) name:kNotificationClientTransfer object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(_refreshHome) name:kNotificationModifyClientBaseInfo object:nil];
}

- (void)_requestHomeData {
    
    MHCB2BProgressHUD *hud = [MHCB2BProgressHUD createHUDWithText:kHUDLoadingText];
    [hud show];
    [[MHCB2BDMSService service] dmsHomePageIndexWithToolVersion:4 andResultHandler:^(BOOL success, NSDictionary *result, NSError *error) {
        if (success) {
            [hud hide];
            // some code here...
            [_tableView reloadData];
        } else {
            [hud changeType:HUDTypeError andText:error.localizedDescription thenHide:YES];
        }
    }];
}

@end
```

## B2B 项目结构

目前 B2B 项目已经完成了组件化重构，需要了解更多项目结构相关内容，请查看下一章内容`模块化开发`。

## 模块化开发

经过一个月左右的全项目模块化重构，现今后的开发流程与之前已有不同。目前 B2B 项目（Bugatti）的结构及各模块说明参照 [B2B 组件化实践](http://cf.dawanju.net/pages/viewpage.action?pageId=12453578)。

### 开发流程及规范

B2B 项目拆分为主工程、业务层、基础层。在开发过程中，我们只需要引入主工程与对应的模块即可。    

基础层现已整理出部分文档，具体参见：[B2B 基础层项目文档](http://172.21.1.250:8080)

#### 1. 初始化项目环境

[下载](http://cf.dawanju.net/download/attachments/12459647/envinit.sh) 并执行初始化脚本 

```
// 使脚本可执行
chmod +x envinit.sh
// 执行脚本
./envinit.sh
```

#### 2. 添加项目到 SourceTree（若使用其他 git 管理工具则添加至相关工具）

#### 3. 设置 Podfile 引用路径

不同的开发人员开发不同的项目模块，因此需要在 Podfile 中将对应的 pod 指引到本地路径。如，DMS 模块开发对应的 Podfile：

```ruby
# Category
pod 'CTMediator', :git => 'https://git.dawanju.net/iOSMilestone/CTMediator'
pod 'DMS_Category', :path => '../BusinessLayer/DMS_Category/'
pod 'CarSource_Category', :path => '../BusinessLayer/CarSource_Category/'
pod 'StaffManage_Category', '0.1.0'
pod 'Loan_Category', '0.1.5'
pod 'AllMsg_Category', '0.1.1'
pod 'My_Category', '0.1.9'
pod 'Seek_Category', '0.1.7'
pod 'Trade_Category', '0.1.3'
pod 'Logistics_Category', '0.1.3'
pod 'Pay_Category', '0.1.5'
pod 'Service_Category', '0.1.1'
pod 'WebView_Category', :path => '../BusinessLayer/WebView_Category/'

# Modules
pod 'MHCB2BHomeModule', '0.2.1'
pod 'MHCB2BBusiness', '0.3.1'
pod 'MHCB2BAllMsgModule', '0.1.3'
pod 'MHCB2BPayModule', '0.2.0'
pod 'MHCB2BTradeModule', '0.2.5'
pod 'MHCB2BDMSModule', :path => '../BusinessLayer/MHCB2BDMSModule/'
pod 'MHCB2BSeekModule', '0.3.8'
pod 'MHCB2BLogisticsModule', '0.2.1'
pod 'MHCB2BStaffManageModule', '0.1.5'
pod 'MHCB2BWebModule', '0.2.1'
pod 'MHCB2BFinancialModule', '0.2.3'
pod 'MHCB2BMyModule', '0.2.2'
pod 'MHCB2BHomeJump', '0.2.2'
pod 'MHCB2BCarSourceModule', '0.2.4'
    
# Service
pod 'MHCB2BCarSourceService', :path => '../BusinessLayer/MHCB2BCarSourceService/'
pod 'MHCB2BSeekCarService', '0.1.3'
pod 'MHCB2BLogisticsService', '0.1.4'
pod 'MHCB2BProfileService', '0.1.8'
pod 'MHCB2BFinanceService', :path => '../BusinessLayer/MHCB2BFinanceService/'
pod 'MHCB2BDMSService', :path => '../BusinessLayer/MHCB2BDMSService/'
    
# Base
pod 'MHCThirdWidget', '0.2.1'
pod 'Alipay', '0.1.0'
pod 'MHCThirdUtility', '0.1.2'
pod 'MHCB2BEntity', '0.3.6'
pod 'MHCUtility', '0.2.8'
pod 'MHCHttpManager', '0.2.7'
pod 'MHCWidget', '0.1.7'
pod 'MHCB2BHttpManager', '0.2.9'
pod 'MHCB2BUtility', '0.1.5'
pod 'MHCB2BWidget', '0.4.6'
...
```

> 注：正在开发的几个库需要分别拉取 feature 分支，不可以在 master 上进行开发，若已经在 master 上进行了开发，则用 `git stash` 将未暂存代码迁移至 feature 分支上。

#### 4. 开发规范

##### 主工程

主工程的职责有 `图片资源存放`、`证书存放`、`项目配置`、`AppDelegate` 以及 `Podfile 管理`。

Podfile 中定义了所有依赖库及其对应的版本号。

##### Service 模块

添加的新接口注释规范及命名方式与原有接口要保持一致，可参考 `MHCB2BCommonService.h`

```objc
/**
 登录接口
 
 @param userName      手机号
 @param password      密码
 @param resultHandler resultHandler
 */
- (void)loginWithUserName:(NSString *)userName password:(NSString *)password andResultHandler:(ResultHandler)resultHandler;
```

Service 中的接口要保持干净，不允许掺和业务逻辑代码，若接口请求参数较多，则在业务代码中用 `NSDictionary` 组装再传入。

```
- (void)dmsAddAnnotateWithParameters:(NSMutableDictionary *)parameters andResultHandler:(ResultHandler)resultHandler {
    [self postWithUrl:DMSAddAnnotate parameters:parameters resultHandler:resultHandler];
}
```

接口对应的请求地址以宏定义的方式定义在对应 Service 的实现文件中。

```
#define DMSSmsTemplateTypeList  @"v2/dms/msg/model/list.json"  // DMS 短信模板类型列表
#define DMSSmsTemplateList      @"v2/dms/msg/model/type.json"  // DMS 某一类型的短信模板列表
#define DMSSmsTemplateAdd       @"v2/dms/msg/model/add.json"   // DMS 添加短信模板
#define DMSSmsTemplateModify    @"v2/dms/msg/model/edit.json"  // DMS 编辑短信模板
#define DMSSmsTemplateDelete    @"v2/dms/msg/model/del.json"   // 删除短信模板
```

若接口升级改动较大，影响到方法的参数修改（增加、减少或改变类型），则需要将使用到该接口的所有模块一并同时进行修改，避免自己模块能够正常运行而其他模块出现问题的情况。

##### Module 模块

在往 Category 中添加新的接口时，要遵守以下几点规范：

* 接口的参数要明确到类型，尽量不要使用 `NSDictionary` 作为参数
* 若参数为模块中的 Model，则将该 Model 下沉到公共库（`MHCB2BEntity`），并在 Category 中引用
* 注释规范参照原有接口的格式

```objc
/**
 批注回复

 @param clueLogId               线索跟进记录 Id
 @param showClientDetailItem    是否显示跳转客户详情页按钮
 @param userName                当前登录用户姓名
 @param needKeyboard            是否需要显示键盘
 @return MHCB2BAnnotateDetailViewController
 */
- (UIViewController *)MHCB2BAnnotateDetailViewControllerWithClueLogId:(NSNumber *)clueLogId showClientDetailItem:(BOOL)showClientDetailItem userName:(NSString * _Nullable)userName andNeedKeyboard:(BOOL)needKeyboard;
```

向 Target 中添加新接口时，要遵守以下几点规范：

* 接口的返回值不可以为 `void`，因为 CTMediator 在运行时调用 Target 中的接口，并默认返回方法的返回值
* Target 中的接口参数均使用 `NSDictionary`
* 注释规范参照原有接口的格式

##### 基础层

基础层是提供稳定功能的代码层，日常的业务开发一般不会涉及到该层的代码修改，几种需要修改基础层代码的情况如下：

* 多个模块共用到同一个类（控件、Model）需要下沉到项目对应的底层库中
* 项目工具库或 UI 库中的功能不能满足当前需求，需要进行功能的新增

切记！基础层不得修改已有方法的 `方法名`、`参数`、`返回值`，若原方法已弃用，则要在方法后添加 `NS_DEPRECATED_IOS` 宏；若原方法不满足新功能需求，则自行新增新方法；若原方法存在 bug，先确认该方法被调用的模块是否有问题，若无问题则另行新增方法，若有问题则将原方法修正后一并修改所有调用处。

#### 5. 开发结束

组件化的开发过程中，每个 pod 库也需要严格按照 Git flow 要求进行。

Release 开发结束，主工程合并代码到 Master 之前，需要在 Pofile 里把本地引用的依赖库修改为此次发布的版本号。

## 代码复审

## 证书管理

## JSPatch

## Cocoapods


热更新流程

使用途径：
    1、修复bug
    2、小的功能添加

开发流程：
    1、建立新的分支，版本号以要修复的版本或者以当前app发布版本的版本号为基础在其后添加
    2、开发完毕合并至develop分支提测，更新内容上传至codepush开发环境
    3、测试人员使用特定版本app安装更新进行测试
    4、测试通过后将更新内容发布到codepush生产环境，更新上线
    5、develop分支合并至master分支

可能出现的问题和解决：
    1、用户忽略更新，多次未更新后点击更新
        用户有两次或以上未更新，codepush会全量进行更新

    2、针对部分版本更新
        更新推送命令可以指定一个确切的版本号或是一个版本范围
        1.2.3	        Only devices running the specific binary app store version 1.2.3 of your app
        *	            Any device configured to consume updates from your CodePush app
        1.2.x	        Devices running major version 1, minor version 2 and any patch version of your app
        1.2.3 - 1.2.7	Devices running any binary version between 1.2.3 (inclusive) and 1.2.7 (inclusive)
        >=1.2.3 <1.2.7	Devices running any binary version between 1.2.3 (inclusive) and 1.2.7 (exclusive)
        ~1.2.3	        Equivalent to >=1.2.3 <1.3.0
        ^1.2.3	        Equivalent to >=1.2.3 <2.0.0
### cordova 使用 codepush 热更新

1. 安装 codepush cli

        npm install -g code-push-cli

2. 创建 codepush 账号（github 账号或者 Microsoft 账号）

        code-push register

    >  使用github绑定的，会生成一个key，复制后在控制台里粘贴，如果需要推出可以使用

        cod-push logout

3. 注册应用服务

        code-push app add [APPNAME]

    >  会生成两个值

4. 安装 cordova 插件

        cordova plugin add cordova-plugin-code-push@latest

5. 修改config.xml, 注册应用服务生成的key添加到里面

        <platform name="android">
        <preference name="CodePushDeploymentKey" value="YOUR-ANDROID-DEPLOYMENT-KEY" />
        </platform>
        <platform name="ios">
        <preference name="CodePushDeploymentKey" value="YOUR-IOS-DEPLOYMENT-KEY" />
        </platform>

6. 添加<access /> 标签 至config.xml

        <access origin="https://codepush.azurewebsites.net" />
        <access origin="https://codepush.blob.core.windows.net" />

    >  如果config.xml 里设置了 ``` <access origin="*" /> ``` 可以忽略此步骤

7. 确保app可以访问 codepush 服务，在index.html 里添加meta

        <meta http-equiv="Content-Security-Policy" content="default-src https://codepush.azurewebsites.net 'self' data: gap: https://ssl.gstatic.com 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src *" />

8. 检查app是否安装了 ``` cordova-plugin-whitelist ``` 插件，如果没有安装

        cordova plugin add cordova-plugin-whitelist

9. 调用插件

        codePush.sync();

or

        document.addEventListener("resume", function () {
            codePush.sync();
        });

10. code-push 相关命令

        列出 登陆的token    
        code-push access-key ls
        删除某个access-key
        code-push access-key rm <accessKey>

        code-push app ls

        更名
        code-push app rename 旧名字 新名字
        删除
        code-push app rm 名字

        部署管理
        上面的部署类型 Production  Staging，还可以自己加例如dev  alpha beta等，
        code-push deployment add <app名字> <部署名字>
        还可以重命名部署名字：
        code-push deployment rename <app名字> <旧部署名字> <新部署名字>
        删除部署名字  
        code-push deployment rm <app名字> <部署名字>
        列表部署名字
        code-push deployment ls <app名字>

        code-push release-cordova <appName> <platform>
        eg.
        code-push release-cordova MyApp-ios ios
        code-push release-cordova MyApp-Android android

        # Release a mandatory update with a changelog
        code-push release-cordova MyApp-ios ios -m --description "Modified the header color"

        # Release a dev Android build to just 1/4 of your end users
        code-push release-cordova MyApp-Android android --rollout 25%

        # Release an update that targets users running any 1.1.* binary, as opposed to
        # limiting the update to exact version name in the config.xml file
        code-push release-cordova MyApp-Android android --targetBinaryVersion "~1.1.0"

        # Release the update now but mark it as disabled
        # so that no users can download it yet
        code-push release-cordova MyApp-ios ios -x

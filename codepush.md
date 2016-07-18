### cordova 使用 codepush 热更新

1. 安装 codepush cli

        npm install -g code-push-cli

2. 创建 codepush 账号（github 账号或者 Microsoft 账号）

        code-push register

    >  使用github绑定的，会生成一个key，复制后在控制台里粘贴，如果需要推出可以使用

        cod-push logout

    如果已经有账号了可以使用以下命令绑定其他账号
        
        code-push link
  
    或者使用以下命令进行登录
        
        code-push login

    验证是否登录
        
        code-push whoami

    查看/删除会话记录
        
        code-push session ls
        code-push session rm <machineName>

3. 注册应用服务

        code-push app add [APPNAME]

    >  会生成两个值  `Staging` -- (开发环境)  `Production`-- （生产环境）

    修改名字

        code-push app rename <appName> <newAppName>

    删除应用

        code-push app rm <appName>

   查看应用列表

        code-push app ls

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

        添加协作开发人员，相关人员需要用邮箱注册过codepush（协作人员只拥有部分权限）
        code-push collaborator add <appName> <collaboratorEmail>

        删除协作人员
        code-push collaborator rm <appName> <collaboratorEmail>

        查看协作列表
        code-push collaborator ls <appName>

        项目所有者权限转移
        code-push app transfer <appName> <newOwnerEmail>

        列出 登陆的token    
        code-push access-key ls
        删除某个access-key
        code-push access-key rm <accessKey>


        部署管理
        上面的部署类型 Production  Staging，还可以自己加例如dev  alpha beta等，
        code-push deployment add <app名字> <部署名字>
        还可以重命名部署名字：
        code-push deployment rename <app名字> <旧部署名字> <新部署名字>
        删除部署名字  
        code-push deployment rm <app名字> <部署名字>
        列表部署名字
        code-push deployment ls <app名字> [--displayKeys|-k]


        上传更新
        code-push release-cordova <appName> <platform>
        [--deploymentName <deploymentName>]
        [--description <description>]
        [--mandatory]
        [--targetBinaryVersion <targetBinaryVersion>]
        [--rollout <rolloutPercentage>]
        [--build]
        
        
        code-push release-cordova MyApp-ios ios
        code-push release-cordova MyApp-Android android

        #指定发布环境
        code-push release-cordova MyApp-ios ios -deploymentName/-d Staging/Production

        # 更改日志
        code-push release-cordova MyApp-ios ios -m --description/-desc "Modified the header color"

        # 只有四分之一的用户会接收到这次更新
        code-push release-cordova MyApp-Android android --rollout 25%

        # 强制更新
        code-push release-cordova MyApp-Android android --mandatory/-m true

        # Release an update that targets users running any 1.1.* binary, as opposed to
        # limiting the update to exact version name in the config.xml file
        code-push release-cordova MyApp-Android android --targetBinaryVersion "~1.1.0"

        # Release the update now but mark it as disabled
        # so that no users can download it yet
        code-push release-cordova MyApp-ios ios -x

        # 调试
        code-push debug <platform>

        # 补救更新参数
        code-push patch <appName> <deploymentName>
        [--label <releaseLabel>] //部署版本，默认最新的版本
        [--mandatory <isMandatory>]
        [--description <description>]
        [--rollout <rolloutPercentage>]
        [--disabled <isDisabled>]
        [--targetBinaryVersion <targetBinaryVersion>]

        # 切换部署环境
        code-push promote <appName> <sourceDeploymentName> <destDeploymentName>
        [--description <description>]
        [--disabled <disabled>]
        [--mandatory]
        [--rollout <rolloutPercentage>]
        [--targetBinaryVersion <targetBinaryVersion]

        # 查看发布历史
        code-push deployment history <appName> <deploymentName>

        # 清除历史记录
        code-push deployment clear <appName> <deploymentName>
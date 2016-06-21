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

# QQBOT备忘

注意：部署bot为公主连结机器人。
采用`NTQQ`+`Trss-Yunzai`+`ws-plugin`来代替`go-cqhttp`
由于`qsign`已经快失效，使用`go-cqhttp`有很大概率出现冻结或永久风控。本篇记录采用`NTQQ`+`Liteloader`（利用Chronocat插件作为服务端）的方式在Windows服务器上成功部署可同时运行`yunzai`&` hoshino`& `yobot`。
本次部署使用目前最新可用`Liteloader`的`QQNT（16183版本）`以便使用Chronocat。

**第一步：安装环境**

在Releases下载`resources.zip`。

安装下面的软件/工具
   - Node.js (版本至少v18以上)
   - Redis
   - Git

**第二步：安装NTQQ**

下载并解压文件后，安装`QQ9.9.2.16183_x64.exe`，登录成功后，退出QQ。

**第三步：安装Liteloader。**

解压`Liteloader.zip`，打开NTQQ安装目录，将LiteLoader文件夹放入\resources\app 文件夹内。然后任意位置打开`LiteLoaderPatchNFixer.exe`，进行修补。如果QQ程序自动运行，需要退出。

**第四步：安装Chronocat插件。**

解压`LiteLoaderQQNT-Plugin-Chronocat-master.zip`，将`LiteLoaderQQNT-Plugin-Chronocat`文件夹放入C:\Users\Administrator\Documents\LiteLoaderQQNT\plugins 文件夹内。然后启动NTQQ，查看 设置-LiteLoader 配置界面-扩展中Chronocat已安装并已启动。

**第五步：安装TRSS-Yunzai**

1.根据偏好选择使用 GitHub 或 Gitee 安装下载

GitHub

```
git clone --depth 1 https://github.com/TimeRainStarSky/Yunzai
cd Yunzai
git clone --depth 1 https://github.com/TimeRainStarSky/Yunzai-genshin plugins/genshin
git clone --depth 1 https://github.com/yoimiya-kokomi/miao-plugin plugins/miao-plugin
git clone --depth 1 https://github.com/TimeRainStarSky/TRSS-Plugin plugins/TRSS-Plugin
```

Gitee

```
git clone --depth 1 https://gitee.com/TimeRainStarSky/Yunzai
cd Yunzai
git clone --depth 1 https://gitee.com/TimeRainStarSky/Yunzai-genshin plugins/genshin
git clone --depth 1 https://gitee.com/yoimiya-kokomi/miao-plugin plugins/miao-plugin
git clone --depth 1 https://Yunzai.TRSS.me plugins/TRSS-Plugin
```

2.安装pnpm

```
npm install -g pnpm
```

3.安装依赖

```
pnpm i
```

4.运行

```
node app
```

5.关闭yunzai

**第六步：安装ws-plugin**

在 Yunzai-Bot 根目录夹打开终端并运行下述指令

由于需要RedProtocol，ws-plugin的安装指令为：
```
#gitee
git clone --depth=1 -b red https://gitee.com/xiaoye12123/ws-plugin.git ./plugins/ws-plugin/
pnpm install --filter=ws-plugin
```

**第五步：配置ws-plugin以便连接NTQQ和其他BOT。**

在 C:\Users\Administrator\\.chronocat\config\chronocat.yml 中可以找到Chronocat的red协议端口（默认端口16530）和token，将其记住。然后打开 Yunzai目录/plugins/ws-plugin/config/default_config/ws-config.yaml，新增如下配置并保存:
```
 - name: NTQQ
    address: 服务器ip:red协议端口
    accessToken: 'token'
    type: 4
    reconnectInterval: 5
    maxReconnectAttempts: 0
    uin: botQQ账号
```
此时Yunzai可以正常获取NTQQ的消息

以下是反向连接HoshinoBot的示例：

```
 - name: hoshino
    address: ws://127.0.0.1:8080/ws/
    type: 1
    reconnectInterval: 5
    maxReconnectAttempts: 0
    uin: botQQ账号
```

以下是反向连接Yobot的示例：

```
 - name: yobot
    address: ws://127.0.0.1:9222/ws/
    type: 1
    reconnectInterval: 5
    maxReconnectAttempts: 0
    uin: botQQ账号
```

至此，即可使用

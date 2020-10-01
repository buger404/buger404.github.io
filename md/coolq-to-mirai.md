# 从酷Q直接迁移到Mirai

通过该操作，可以不需要重构代码“复活”你的酷Q机器人。

> 请注意：cqp格式的插件是不受支持的，需要同时提供.dll和.json

# 准备工作

安装运行环境（两者都要安装）

* jdk 8 x86（**必须是x86**）

* jdk 11

# 下载Mirai相关包

Github：https://github.com/iTXTech/mirai-console-loader/releases

双击启动下载的jar，目录结构将自动创建，同时下载必要的Mirai包。

# 下载Mirai Native

Github：https://github.com/iTXTech/mirai-native/releases

1.将下载好的jar放入刚才目录的`plugins`文件夹中（请留意带”*“号的目录）。

```
├─config
│  └─Console
├─*data
│  ├─image
│  │      MIRAI_NATIVE_IMAGE_DATA
│  ├─*MiraiNative
│  │      CQP.dll（不要使用酷Q本体的CQP.dll替换它！）
│  └─record
│          MIRAI_NATIVE_RECORD_DATA
├─libs
├─*plugins
│      *mirai-native-1.9.3.jar
└─scripts
```

2.获取CQHTTP

* Github：https://github.com/richardchien/coolq-http-api/releases
* 将 `libiconv.dll` 和 `sqlite3.dll` 放入 `data\MiraiNative\libraries` 文件夹（这两个dll可以从酷Q本体中找到）
* 将 `http.dll`和`http.json` 放入 `data\MiraiNative\plugins` 文件夹（刚才下载的CQHTTP的dll和json）

# 检查你的插件

请确保：

* 你使用的Native C# SDK的版本为4.5
* 编译条件为Release
* app.json中不含有注释语句

> 若在之后的步骤中发生无法找到插件配置的情况，尝试使用记事本将app.json的编码转换为ANSI。

# 启动Mirai

```powershell
@echo off
cd "[Mirai所在的绝对路径]"
Title Intallk
java -cp "./libs/*" net.mamoe.mirai.console.terminal.MiraiConsoleTerminalLoader %*
Pause ""
```

1.将上述代码写入文件`launch.bat`中，右键以管理员身份运行。

2.等待加载完毕后，输入`login [QQ] [密码]`回车登录机器人账号。

> 如何自动登陆？
>
> 修改`config\Console\AutoLogin.yml`，修改为：
>
> ```yaml
> plainPasswords: 
> 
> QQ: 明文密码
> ```

3.大功告成

# 我遇到的问题

* CQHTTP不能正常加载

  尝试将CQHTTP的`http.json`中的注释语句删除，然后将其编码转换为`ANSI`

* 插件载入时程序停止工作

  请按照上述的”插件检查“排查问题

* 未能创建数据目录

  请确认`启动Mirai`步骤中的绝对路径是否填写正确，并且使用管理员身份运行此`.bat`

* 插件没有被载入

  官方文档存在问题，插件应该被存放在`data\MiraiNative\plugins`而不是`plugins\MiraiNative\plugins`
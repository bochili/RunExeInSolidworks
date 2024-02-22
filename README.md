# RunExeInSolidworks

本项目是一个基于Solidworks API开发的Solidworks工具栏插件，用于在Solidworks工具栏中生成一个程序列表，打开外部程序。并且可以通过文本文件灵活地编辑程序列表和添加分类菜单。

![运行示例](https://cdn.jsdelivr.net/gh/bochili/cdn3/202402222158031.png)

## 开发环境

.NET 4.0 x64

Visual Studio 2022

## 使用说明

编译后，在`项目路径/bin/Debug/` 下拷贝 `BigEngineerIntegrate.dll` 和 `SolidWorksTools.dll` 到一个新目录，在该目录下创建 `categories.txt` 、 `plugins.txt` 、`register.bat` ，和 `plugins` 目录，结构如下图：

![目录结构](https://cdn.jsdelivr.net/gh/bochili/cdn3/202402222211295.png)

### plugins.txt内容格式

plugins.txt为插件列表，格式如下：

```
名称,提示文本,可执行文件路径,分类名称
```

可执行文件若放在plugins目录中，则可执行文件路径填写如下：

```
\plugins\executable.exe
```

若程序需要直接显示在工具栏中，则无需添加分类名称，格式如下：

```
名称,提示文本,可执行文件路径
```

以下为示例：

```
BOM,BOM,\plugins\bom\Bom.exe
结构细部设计,Detail(结构细部设计),\plugins\Detail\Detail.exe,设计工具
```

### categories.txt内容格式

存放分类列表，每行一个：

```
计算工具
设计工具
库工具
批量工具
查询工具
```

### register.bat内容

该文件为注册插件到Solidworks的脚本，调用.NET的regasm程序：

```powershell
C:\WINDOWS\Microsoft.NET\Framework64\v4.0.30319\regasm.exe  /codebase %~dp0/BigEngineerIntegrate.dll
 pause
```

其中 `%~dp0` 为bat文件路径。

### 注册

内容准备完成后，以管理员权限运行 `register.bat` 即可。

### 清除插件注册信息

插件在注册时会向注册表新建 `HKEY_LOCAL_MACHINE\SOFTWARE\SolidWorks\AddIns\{GUID}` 和 `HKEY_CURRENT_USER\Software\SolidWorks\AddInsStartup\{GUID}` 两个条目，将这两条删除即可清理。

本代码中默认GUID为 `43320236-ec4d-4da6-a851-97d911ee9998`。
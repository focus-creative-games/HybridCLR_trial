# HybridCLR 体验项目

一个示例热更新项目。

想了解更多，请加 QQ群: 

- HybridCLR c#热更新 开发交流群：651188171
- HybridCLR使用疑难咨询：651188171

你可以使用发布的包来体验HybridCLR热更新功能。

**示例项目使用 Unity 2020.3.33(任意后缀子版本如f1、f1c1、f1c2都可以) 版本**，需要精确匹配。

## 目录介绍

- Assets Unity项目目录
  - Main AOT主包模块
  - Hotfix 热更新模块
- HybridCLRData 包含HybridCLR的il2cpp本地安装目录

## 使用介绍

HybridCLR为c++实现，只有打包后才可使用。日常开发在编辑器下，无需打包。

如何打包出一个可热更新的包，请先参阅 [快速开始](https://focus-creative-games.github.io/hybridclr/start_up/)。

### 运行流程

本示例演示了如下几部分内容

- 将dll和资源打包成ab
- 多热更新dll，并且按依赖顺序加载它们
- 热更新脚本挂载到热更新资源中，并且正常运行
- 直接反射运行普通热更新函数App::Main

进入场景后，Main场景中的LoadDll会按顺序加载StreamingAssets目录下common AssetBundle里的HotFix.dll和HotFix2.dll，其中HotFix2.dll依赖HotFix.dll。
接着运行HotFix2.dll里的App::Main函数。

注意！多热更新dll不是必须的！大多数项目完全可以只有HotFix.dll这一个热更新模块。纯粹出于演示才故意设计了两个热更新模块。

### 体验热更新

以Win64为例，其他平台同理。

- 安装HybridCLR （安装HybridCLR的原理请看 [快速上手](https://focus-creative-games.github.io/hybridclr/start_up/)）
  - 进入HybridCLRData目录
  - 酌情修改 init_local_il2cpp_data.bat(或.sh)文件中代码
    - `set IL2CPP_BRANCH=2020.3.33` 改成你的版本（目前只有2020.3.33或2021.3.1）
    - `set IL2CPP_PATH=<你的Unity editor的il2cpp目录的路径>` 改成你的Unity安装目录
  - 运行 init_local_il2cpp_data.bat 或.sh 文件 创建本地il2cpp目录，即 LocalIl2CppData 目录。
  - 至此完成包含HybridCLR的本地il2cpp安装
- 打包主工程
  
  - 由于ab包依赖裁剪后的dll，因此首先需要build工程
  - build ab包，点击菜单`HybridCLR/BuildBundles/Win64`，生成Win64目标的AssetBundle，生成的AssetBundle文件会自动复制到StreamingAssets目录下
  - 运行，会看到打出 common, scene等AssetBundle包
- 更新ab包
  - 修改HotFix项目的PrintHello代码，比如改成打印 "hello,world"。
  - 运行菜单 HybridCLR/BuildBundles/Win64，重新生成ab
  - 将StreamingAssets下的ab包同步到打包主工程时Build目标的StreamingAsset目录，在{BuildDir}\build\bin\hybridclr_Data\StreamingAssets
- 再将运行，屏幕上会打印"hello,world"。

剩下的体验之旅，比如各种c#特性，自己体验吧。

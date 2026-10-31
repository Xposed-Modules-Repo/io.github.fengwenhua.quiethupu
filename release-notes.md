## 0.1.1 - 2026-10-31

真机验证修复版（OnePlus PKH110 / Android 16 / 虎扑 8.2.66.10232）。

### 修复

- 网易易盾 NIS 加固下，`onPackageReady` 时真实 `com.hupu.*` DEX 尚未解密加载，
  原实现全部 `class not found`。新增 `DeferredInstall`：hook 壳 Application 的
  `attachBaseContext/onCreate`，在原方法执行完后安装；另有 100ms 轮询
  （壳 ClassLoader / 当前 Activity ClassLoader）兜底，最多 30 秒。
- 只在主进程安装 hook；`:pushcore`、`:privileged_process0` 不再刷 ClassNotFound。
- 适配版本更新为 **8.2.66.10232（12320）**。

### 验证

- 30 个锚点全部解析并注册。
- 实测命中：AD-01（后台预加载）、AD-02（热启动开屏）、AD-03 信息流（数十次）、
  AD-04 下拉刷新、AD-05 Banner、AD-06 浮层、AD-07 弹窗、AD-08 搜索框、AD-10 摇一摇、
  AD-11 冷启动免广告弹窗；无 FATAL。
- 冷启动未再出现开屏广告。

### 界面与默认值

- 关于页删除「模块互链」，新增「参考项目：QuietBox」与「源码」链接（fengwenhua/quiethupu）及
  https://github.com/yigenhuobah/quietbox 链接。
- PV-01 剪贴板广告默认开启；AD-11 激励视频/短剧默认拦截。


### 补充（发布前实测）

- R8 release 包中 `kotlin.Unit` 在 hook 安装阶段不可见，UI-01 改为调用时延迟解析，
  找不到时返回 null（调用点丢弃返回值）；真机命中 20 次，更新弹窗被拦截且无崩溃。
- 最终 APK 由 GitHub Actions release 环境签名，证书 SHA-256
  `4545b55bac4d79ac16769e5ba0f45efa4f080ad98ad66c0e2e942064dcbced1c`。
- APK SHA-256 `bf5950c870b2a028cec5947b530417efb8639e68181f31a770f700af5b34bbcd`。

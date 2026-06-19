# Yorumi

一款基于 Flutter 构建的 Emby 客户端，采用 mpv 播放内核。

## 功能

- **Emby 媒体播放** — 浏览、搜索、收藏、播放
- **mpv 播放内核** — 通过 Dart FFI 直调 mpv-android 的 `libmpv.so`，支持 GPU 渲染
- **ASS 字幕渲染** — mpv + libass 内嵌渲染，无需第三方组件
- **硬件解码** — 支持 MediaCodec 硬解与软解回退
- **续播功能** — 记录播放进度，自动跳到上次位置
- **播放设置** — 解码器切换、比例、YUV420、缓存大小、字幕样式等
- **手势操控** — 左右滑动快进/快退，上下滑调节亮度/音量
- **播放报告** — 遵循 Emby Sessions API 上报播放进度、心跳、暂停、结束事件
- **多用户** — Emby 账号登录 + nodemby 注册/登录系统
- **个人中心** — 邮箱/QQ/密码修改，QQ 头像展示

## 截图

（待添加）

## 构建要求

| 工具 | 版本 |
|------|------|
| Flutter | 3.44+ |
| Dart | 3.12+ |
| Android SDK | 34+ |
| NDK | 随 Flutter 自动 |
| Java | 17+ |


[GPL v2](LICENSE)

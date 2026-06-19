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

## 快速开始

```bash
# 克隆项目
git clone https://github.com/whohh/Yorumi.git
cd Yorumi/miru_flutter

# 获取依赖
flutter pub get

# 构建 debug APK
flutter build apk --debug

# 或按架构拆分 release APK
flutter build apk --split-per-abi
```

## 技术栈

| 层 | 技术 |
|----|------|
| UI | Flutter 3.44 + Material 3 |
| 路由 | flutter_modular |
| 播放器 | mpv-android (Dart FFI) |
| 渲染 | EGL + SurfaceView + mpv_render_context |
| 视频解码 | MediaCodec (mediacodec-copy) |
| 字幕渲染 | libass (mpv 内置) |
| 网络 | Dio + Emby REST API |
| 用户系统 | nodemby (Node.js + MySQL + JWT) |
| 状态管理 | Provider + ValueNotifier |
| 持久化 | hive_ce |
| 图片缓存 | flutter_cache_manager |

## 项目结构

```
miru_flutter/
├── android/
│   ├── app/src/main/
│   │   ├── cpp/mpv_native.c       # JNI C 桥接：EGL + 渲染线程
│   │   ├── kotlin/.../
│   │   │   ├── MainActivity.kt     # 平台通道、PIP、亮度
│   │   │   └── player/
│   │   │       ├── MpvNative.kt    # JNI 声明
│   │   │       ├── MpvPlugin.kt    # Flutter 插件 + MethodChannel
│   │   │       └── MpvSurfaceView.kt # PlatformView
│   │   └── jniLibs/               # mpv-android 预编译 .so
│   └── CMakeLists.txt
├── lib/
│   ├── main.dart
│   ├── player/
│   │   ├── mpv_player.dart         # Dart FFI 封装
│   │   ├── player_screen.dart      # 播放器 UI
│   │   └── reporter.dart           # Emby 播放上报
│   ├── pages/
│   │   ├── detail/                 # 剧集/电影详情页
│   │   ├── home/                   # 首页
│   │   ├── search/                 # 搜索
│   │   ├── library/                # 媒体库
│   │   ├── my/                     # 个人中心
│   │   └── settings/               # 设置
│   └── services/
│       ├── auth_api.dart           # nodemby API
│       ├── update_checker.dart     # 更新检查
│       └── storage.dart            # Hive 配置
└── pubspec.yaml
```

## 播放器架构

```
用户操作 (Dart)
    │
    ▼
MethodChannel('com.miru/mpv')  →  MpvPlugin.kt
    │
    ▼
MpvNative.nativeCommand()  →  mpv_native.c (JNI)  →  libmpv.so
    │
    ▼
render_thread  →  mpv_render_context  →  eglSwapBuffers  →  SurfaceView
```

## 致谢

- [mpv-android](https://github.com/mpv-android/mpv-android) — mpv 预编译库
- [nodemby](https://github.com/whohh/nodemby) — 用户注册/登录后端
- [flutter_mpv](https://github.com/example/flutter_mpv) — 参考架构

## 许可证

[GPL v2](LICENSE)

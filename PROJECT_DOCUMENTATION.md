# Fideo直播录制软件项目文档

## 1. 项目概述

基于Electron的跨平台直播录制软件，可以帮助用户简单便捷地对多个平台的直播进行监控和录制，并保存为MP4格式的视频。

### 技术栈
- **前端**：React、Tailwind CSS、Shadcn UI
- **后端**：Electron、Node.js
- **视频处理**：FFmpeg
- **网络穿透**：FRP (Fast Reverse Proxy)
- **其他**：TypeScript、Zustand(状态管理)、i18next(国际化)

## 2. 项目结构

```
fideo-live-record/
├── src/                   # 源代码目录
│   ├── main/              # Electron主进程代码
│   │   ├── crawler/       # 直播平台爬虫模块
│   │   ├── ffmpeg/        # FFmpeg视频处理模块
│   │   ├── frpc/          # FRP网络穿透模块
│   │   ├── log/           # 日志模块
│   │   ├── download-dep/  # 依赖下载模块
│   │   └── index.ts       # 主进程入口文件
│   ├── preload/           # Electron预加载脚本
│   ├── renderer/          # 渲染进程代码(前端)
│   │   ├── src/           # 前端源代码
│   │   │   ├── assets/    # 静态资源
│   │   │   ├── components/# React组件
│   │   │   ├── hooks/     # React自定义钩子
│   │   │   ├── lib/       # 工具库
│   │   │   ├── locales/   # 国际化语言包
│   │   │   ├── store/     # Zustand状态管理
│   │   │   ├── shadcn/    # UI组件库
│   │   │   ├── App.tsx    # 主应用组件
│   │   │   └── main.tsx   # 前端入口文件
│   │   └── index.html     # HTML入口文件
│   ├── const.ts           # 全局常量
│   └── code.ts            # 状态码定义
├── resources/             # 资源文件目录
├── build/                 # 构建相关配置
├── out/                   # 构建输出目录
├── electron-builder.yml   # Electron打包配置
├── electron.vite.config.ts# Electron Vite配置
├── tsconfig.json          # TypeScript配置
├── tailwind.config.js     # Tailwind CSS配置
└── package.json           # 项目依赖和脚本
```

## 3. 核心功能模块

### 3.1 直播抓取模块 (src/main/crawler/)

负责从各个直播平台获取直播流URL和房间信息。支持的平台包括：YouTube、Twitch、TikTok、抖音、快手、B站、网易CC、花椒、微博、斗鱼、淘宝、Bigo、YY、虎牙、京东、时光、陌陌、17LIVE、小红书、AcFun、畅聊、vv直播、克拉克拉等。

### 3.2 视频录制模块 (src/main/ffmpeg/)

使用FFmpeg进行直播流的录制和处理，将直播流保存为MP4视频文件。包含以下主要功能：
- 开始/停止录制
- 录制进度监控
- 视频格式处理

### 3.3 依赖下载模块 (src/main/download-dep/)

负责下载和管理应用所需的外部依赖，如FFmpeg和FFprobe。确保用户首次启动应用时能够自动下载所需的依赖。

### 3.4 网络穿透模块 (src/main/frpc/)

使用FRP(Fast Reverse Proxy)实现网络穿透，允许用户通过手机或其他设备远程控制应用。

### 3.5 状态管理 (src/renderer/src/store/)

使用Zustand管理应用状态，包括：
- 直播配置存储
- 默认设置存储
- 导航状态存储
- 网页控制设置存储
- 依赖下载信息存储

### 3.6 用户界面 (src/renderer/src/components/)

基于React和Shadcn UI构建的用户界面，包含：
- 标题栏
- 导航栏
- 直播配置列表
- 设置对话框
- 依赖下载进度显示

## 4. 主要流程

### 4.1 应用启动流程
1. 检查并下载必要依赖(FFmpeg等)
2. 初始化主窗口和托盘图标
3. 加载用户配置和设置
4. 渲染用户界面

### 4.2 直播录制流程
1. 用户添加直播地址和配置
2. 系统爬取直播流信息和URL
3. 使用FFmpeg开始录制直播
4. 监控录制进度并显示给用户
5. 完成录制并保存为MP4文件

### 4.3 远程控制流程
1. 用户启用网页控制功能
2. 系统启动FRP服务
3. 生成远程访问链接
4. 用户通过手机或其他设备访问链接进行远程操作

## 5. 构建和打包

项目使用electron-builder进行打包，支持Windows和macOS平台。构建配置在electron-builder.yml文件中定义，包括：
- 应用ID和产品名称
- 打包目标平台和架构
- 安装程序配置
- 资源文件处理

## 6. 开发指南

### 6.1 本地开发
```shell
# 安装依赖
pnpm install

# 开发模式启动
pnpm debug

# 构建应用
pnpm build

# 打包Windows版本
pnpm build:win

# 打包macOS版本
pnpm build:mac
```

### 6.2 项目扩展
- 添加新的直播平台支持：在src/main/crawler/目录中添加新平台的爬虫模块
- 修改UI：在src/renderer/src/components/目录中修改相应组件
- 添加新功能：根据功能类型在相应目录添加代码，并在主进程中注册相关IPC处理函数

## 7. 已支持平台
- YouTube
- Twitch
- TikTok
- 抖音
- 快手
- B站
- 网易CC
- 花椒
- 微博
- 斗鱼
- 淘宝
- Bigo
- YY
- 虎牙
- 京东
- 时光
- 陌陌
- 17LIVE
- 小红书
- AcFun
- 畅聊
- vv直播
- 克拉克拉

## 8. 注意事项

- 项目使用了FFmpeg和FRP等外部依赖，需要确保它们能够正确下载和运行
- 不同平台的直播源获取方式可能会随着平台更新而变化，需要及时更新爬虫模块
- 应用遵循开源协议，但使用时需遵守相关平台的服务条款和所在国家/地区的法律法规

## 9. 国际化支持

项目支持多语言，使用i18next实现国际化。语言文件位于src/renderer/src/locales/目录。
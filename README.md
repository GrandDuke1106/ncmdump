# ncmdump-mobile

这是一个基于 Flutter 开发的小玩具，为 [ncmdump-go](https://git.taurusxin.com/taurusxin/ncmdump-go) 提供了现代化的移动端图形界面。

Windows 端也可以使用[ncmdump-gui](https://git.taurusxin.com/taurusxin/ncmdump-gui)。本程序的 ui 完全为手机设计，桌面端（Windows, Linux）可能用得不顺手，不过相同的后端功能都一样。

它可以帮助你在各种设备上直接将网易云音乐的 `.ncm` 文件转换为普通的 `.mp3` 或 `.flac` 格式，并自动补全专辑封面等元数据。

## ✨ 功能特性

* **批量转换**：支持添加单个文件或扫描整个目录下的 `.ncm` 文件。
* **元数据修复**：转换同时自动下载并修复歌曲的专辑封面、歌手、专辑名等信息。
* **智能分组**：自动按目录分组显示文件，清晰直观。
* **目录历史**：记住你添加过的目录，支持下拉刷新，一键重新扫描所有历史目录。

## 🚀 编译指南

如果你想自己编译这个项目，请按照以下步骤操作。

### 1. 环境准备

确保你已经安装了以下环境：
* Flutter SDK
* Go 1.24+
* Android Studio & Android SDK
* `gomobile` 工具:
    ```bash
    cd ext
    go install golang.org/x/mobile/cmd/gomobile@latest
    gomobile init
    ```

### 2. 编译 Go 核心库

本项目依赖 `ncmdump-go` 的核心逻辑。你需要先将 Go 代码编译为 Android, Windows 或者 Linux 的 `.aar`, `dll` 或者 `so` 库。

在项目根目录执行：

```bash
# 如果遇到 javac 报错，请检查 JAVA_HOME 环境变量
cd ext
gomobile bind -target=android -androidapi 21 -o ../ncmdump_mobile/android/app/libs/ncmdump.aar ./mobile
go build -buildmode=c-shared -o ../ncmdump_mobile/windows/runner/ncmdump.dll export.go ./desktop
go build -buildmode=c-shared -o ../ncmdump_mobile/linux/libncmdump.so export.go ./desktop
````

### 3. 安装 Flutter 依赖

```bash
cd ncmdump_mobile
flutter pub get
```

### 4. 运行或打包

连接你的需要调试的设备：

```bash
# 调试运行
flutter run

# 打包各个平台的软件
flutter build apk --release --split-per-abi
flutter build windows --release
flutter build linux --release
```

## 📦 主要依赖库

* **`ffi`**: 外部函数接口，用于调用 Go 编写的 `ncmdump` 核心动态库。
* **`file_picker`**: 跨平台的文件与目录选择器。
* **`permission_handler`**: 处理 Android 和 iOS 的运行时权限请求。
* **`provider`**: 简单高效的应用状态管理。
* **`shared_preferences`**: 本地持久化存储，用于保存目录扫描历史。
* **`device_info_plus`**: 获取设备信息，用于适配不同 Android 版本的权限策略。
* **`path_provider`**: 获取各平台的常用文件路径（如文档目录、临时目录）。
* **`flutter_launcher_icons`** (Dev): 自动化生成和配置所有平台的应用启动图标。

## ⚖️ 免责声明

本项目基于[MIT](LICENSE)开源，不提供担保。

本项目仅供学习和技术研究使用。请勿将本软件用于任何商业用途或侵犯第三方版权的行为。使用本软件产生的任何法律后果由使用者自行承担。

## 🙏 致谢

后端来自于：[ncmdump-go](https://git.taurusxin.com/taurusxin/ncmdump-go)


# M7H Pro HDMI Camera Lab（诊断版源码）

## 重要说明
当前交付的是 Android Studio 工程源码，不是已编译 APK。当前构建环境没有 Android SDK/Gradle，因此无法在这里可靠地产生可安装 APK。请用 Android Studio 在 Windows 11 上打开本目录并构建。

## 当前版本能力
- 读取 Android Build / Android 版本信息
- 使用 Camera2 枚举系统已经注册的摄像头及输出尺寸、FPS 范围
- 检查普通 APK 对 `/dev/video0..31` 和 `/sys/class/video4linux` 的可见性
- 对系统已注册的摄像头尝试 Camera2 预览
- 分享纯文本诊断报告

## 当前版本不做的事
- 不 Root、不修改系统分区、不刷固件
- 不绕过 Linux 设备节点权限
- 不会把 HDMI RX 自动注册为系统摄像头
- 如果 HDMI 输入尚未由厂商 Camera HAL 暴露，普通 APK 无法单独让直播软件识别它

## 在 Windows 11 编译
1. 安装 Android Studio（建议稳定版）及 Android SDK Platform 35、Build Tools。
2. 解压工程 ZIP。
3. Android Studio -> Open -> 选择 `M7HPro_HDMI_Camera_Lab` 文件夹。
4. 等待 Gradle Sync 完成。
5. 选择 Build -> Build Bundle(s) / APK(s) -> Build APK(s)。
6. APK 通常位于 `app/build/outputs/apk/debug/app-debug.apk`。
7. 将 APK 安装到 M7H Pro，授予 Camera 权限，运行并导出诊断报告。

## 下一步
将 APK 报告发回开发对话。需要据此确认 HDMI RX 是否有 V4L2 节点、普通 APK 是否可访问、当前 Camera Provider 实现和是否必须进行系统级 HAL 集成。

## 云端编译（GitHub Actions）
本工程已附带 `.github/workflows/android.yml`。将整个工程上传到你自己的 GitHub 仓库（建议设为 Private），推送到 `main` 分支后会自动在 GitHub 托管的 Ubuntu runner 上编译；也可以在仓库 Actions 页面手动运行 `Build M7H Pro HDMI Camera Lab APK`。成功后打开对应运行记录，在 Artifacts 下载 `M7HPro-HDMI-Camera-Lab-debug`，解压得到 `app-debug.apk`。
注意：这不是 ChatGPT 直接访问你的 GitHub 账户；仓库创建、上传和下载 Artifact 需要你在 GitHub 网页上完成。Debug APK 仅用于测试，不是正式发布签名版本。

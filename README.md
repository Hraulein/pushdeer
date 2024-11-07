# PushDeer For Android 自架版打包指南

此仓库旨在指导用户如何打包一个包含自己服务器地址的 PushDeer For Android 应用。

## 特性

- 自定义服务器地址配置
- 简易化的打包流程
- 去除微信登录，仅支持 Apple 账号登录

## 开发环境

- **IDE**: IntelliJ IDEA 社区版
- **JDK**: 1.8.0_361
- **Android SDK**: 确保安装了必要的 Android SDK 组件

开发环境的资源包可以在此处下载（是个小水管）: [下载资源包](https://alist.hraulein.com/resources/PushDeer-Android-ENV)

## 安装依赖

在开始之前，请确保你的开发环境已经设置好，包括安装适当的 IDE 和 JDK。

## 步骤

### 1. 克隆仓库

使用 Git 克隆该仓库：

```bash
git clone https://github.com/Hraulein/pushdeer.git
```

### 2. 配置服务器地址

找到以下两个文件并将 `baseUrl` 修改为你的服务器地址：

1. **文件一**: `android/app/src/main/java/com/pushdeer/os/data/api/PushDeerApi.kt`
2. **文件二**: `android/pushdeercommon/src/main/java/com/pushdeer/common/api/PushDeerApi.kt`

在这两个文件中，找到如下代码行：

```kotlin
val baseUrl = "https://api2.pushdeer.com"
```

将其修改为：

```kotlin
val baseUrl = "https://your-server-address.com"
```

### 3. 生成 Android 签名

生成 Android 签名，请按照以下步骤操作：

1. 打开终端或命令提示符。
2. 使用以下命令生成一个新的签名密钥库（keystore）：

   ```bash
   keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
   ```

   - `my-release-key.keystore` 是文件名，可以自定义。
   - `my-key-alias` 是你在 keystore 中的别名，可以自定义。
   - 在命令提示符中，会要求你输入一些信息以生成签名，包括密码、姓名等。

3. 生成密钥后，将生成的 `my-release-key.keystore` 文件放到项目的 `android/app` 目录下。

### 4. 复制并配置 key_debug.properties

1. 在项目目录中，找到 `android/key_debug.sample.properties` 文件。
2. 复制该文件，并将其命名为 `key_debug.properties`。
3. 打开 `key_debug.properties` 文件，并填入刚生成的签名文件的信息，例如：

   ```
   storeFile=../app/my-release-key.keystore
   storePassword=你的keystore密码
   keyAlias=my-key-alias
   keyPassword=你的key别名密码
   ```

   - 请根据实际情况替换文件路径和密码。

### 5. 同步 Gradle

在 IntelliJ IDEA 中点击 "Sync Now" 来同步项目。

### 6. 构建 APK

在 IntelliJ IDEA 中，点击 `Build` > `Build Bundle(s) / APK(s)` > `Build APK(s)`。

在构建过程中，系统会提示你选择并填写签名文件的信息。请按照以下提示操作：

- **选择签名文件**: 在弹出的对话框中，选择你之前生成的 `my-release-key.keystore` 文件。
- **输入密码**: 填写 keystore 密码、key 别名、key 密码等信息。

构建过程完成后，你会看到一个提示，包含你的 APK 文件的路径。

### 7. 安装 APK

将生成的 APK 文件安装到你的设备上，可以直接用 USB 连接设备，或通过其他方式进行安装。

## 使用说明

使用 Apple 方式登录，登录成功后的相关操作与官版大差不差。

- 当前分支 APK 的使用情况说明：
  - 经过 HTTP 请求后，应用内会保存消息记录。
  - 应用不会发送通知到手机的通知栏目。
  - 此分支去除了微信登录功能，目前仅支持使用 Apple 账号进行登录。
  - 主打一个能用就行.
  
如果对我的分支删减内容有异议，可自行复制官方 Android 源文件，修改那两个 `.kt` 文件即可。

## 许可证

该项目遵循 [MIT License](LICENSE)。

## 联系

如果在打包过程中有任何问题或建议，请联系:

- [Ali](mailto:solitude@hraulein.com)

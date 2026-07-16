# 心迹 MindTrace

> 记录此刻，遇见懂你的 AI。

**心迹**是一款移动优先、隐私至上的个人感悟记录与 AI 陪伴工具。它既是一个精致的本地日记本，也是一位随着时间推移越来越了解你的 AI 伙伴。

---

## 功能概览

### 📝 随笔记录
- 快速发布与富文本编辑器，支持多行输入
- 按日期倒序排列的时间线视图，每日自动分组
- 全文搜索，按关键词快速检索历史记录
- **语音转文字**输入（系统原生语音识别）
- **笔记自动分类**：AI 自动为笔记打上标签（心情、工作、生活、思考、目标、关系、学习、健康、其他）
- **分类筛选**：点击标签可筛选查看对应分类的笔记

### 🤖 AI 伴侣
- 基于 DeepSeek / OpenAI 兼容 API 的智能对话
- 自动注入最近笔记作为上下文（可配置条数），AI 真正了解你
- **用户画像分析**：AI 从最近 50 条笔记和 7 天内聊天记录中分析 10 个维度——性格特征、心理状态、情绪倾向、兴趣爱好、思维模式、价值取向、能量规律、核心矛盾、人生阶段、相处建议，以及**动态素描**（与前次分析进行动态对比）
- **主动发起话题**：每天首次打开 AI 分享自己的感想生活（情绪随机变化）；写下新笔记后 AI 根据笔记内容回应
- 聊天记录持久化，重启 App 不丢失
- **可自定义 AI 人设**：性别、年龄、身份、性格、爱好、语言风格（理性/感性拉条）
- 支持三种语言：简体中文、繁體中文、English

### 🔐 隐私安全
- 所有数据存储在本地（IndexedDB + localStorage）
- API Key 本地保存，不上传任何服务器
- 支持一键导出/导入所有数据（JSON 格式），可选择保存到 Documents
- 支持云存档（Firebase + Google 登录），可备份与恢复

### 🎨 个性化设置
- 浅色/深色主题（跟随系统或手动切换）
- 三语言界面（简体中文、繁體中文、English）
- 可配置 API 地址、模型、上下文条数
- **昵称设置**：设置你的昵称，AI 主动聊天时用昵称呼唤你
- **生日设置**：设置生日后，生日当天 ≥ 8:00 AM AI 自动送上定制祝福
- 长按笔记一键复制全文

---

## 技术栈

| 层级 | 技术选型 | 说明 |
|------|---------|------|
| 前端 | 原生 HTML5 + CSS3 + JavaScript (ES6+) | 零框架依赖，极致轻量 |
| 移动端 | Capacitor 6 | 打包为 Android/iOS 原生应用 |
| 本地存储 | IndexedDB + localStorage | 完全离线 |
| 加密 | 无（密码功能已移除） | 数据存在本地 |
| AI 接入 | Fetch API (RESTful) | 直连 OpenAI 兼容接口 |
| 语音输入 | @capacitor-community/speech-recognition | Android 原生语音识别 |
| 云存档 | Firebase Auth + Firestore REST API | 可选的云端备份 |
| 文件操作 | @capacitor/filesystem | 写入 Documents 目录 |

---

## 项目结构

```
mindtrace-app/
├── www/                          # Web 源码
│   ├── index.html                # 主入口
│   ├── css/
│   │   ├── style.css             # 主体样式
│   │   └── dark.css              # 深色模式
│   ├── js/
│   │   ├── app.js                # 应用初始化与路由
│   │   ├── ui.js                 # 视图渲染
│   │   ├── auth.js               # 配置管理
│   │   ├── storage.js            # IndexedDB 封装
│   │   ├── notes.js              # 笔记 CRUD
│   │   ├── ai.js                 # AI API 调用
│   │   ├── settings.js           # 设置页逻辑
│   │   ├── cloud.js              # 云存档功能
│   │   ├── speech.js             # 语音输入
│   │   ├── i18n.js               # 多语言引擎
│   │   └── firebase-config.js    # Firebase 配置（需用户填写）
│   │
├── android/                      # Android 原生项目
├── capacitor.config.json         # Capacitor 配置
├── package.json                  # npm 依赖
├── mindtrace.jks                 # 签名密钥库
└── README.md
```

---

## 开发环境

- **Node.js** v18+
- **VS Code**（推荐）
- **Android Studio**（打包 Android APK）
- **Xcode**（打包 iOS IPA，仅 macOS）

---

## 快速开始

### 安装依赖

```bash
cd ~/Desktop/mindtrace-app
npm install
```

### 本地预览

```bash
npm run dev
```

浏览器打开 `http://localhost:3000`

### 打包 Android APK

```bash
export JAVA_HOME=$(pwd)/jdk17/Contents/Home
npx cap sync android
cd android && ./gradlew assembleRelease
```

APK 位置：`android/app/build/outputs/apk/release/app-release.apk`

### 正式签名

APK 使用项目根目录的 `mindtrace.jks` 签名：

| 项目 | 值 |
|------|-----|
| storePassword | `a0WGHKsG4XCg1Op` |
| keyAlias | `mindtrace` |
| 证书有效期 | 2053-11-12 |

---

## 配置 Firebase（云存档）

1. 在 Firebase Console 创建项目
2. **Authentication → Sign-in method → 启用 Google 登录**
3. **项目设置 → 添加 Android 应用**（包名 `com.mindtrace.app`），下载 `google-services.json` 放入 `android/app/`
4. **项目设置 → 添加 Web 应用**，复制 `firebaseConfig` 填入 `www/js/firebase-config.js`
5. **Firestore Database → 创建数据库 → 规则设为以下 UID 规则：**
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /mindtrace_users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```
6. 在 Firebase 控制台添加 SHA-1 指纹（从 `mindtrace.jks` 获取）

---

## 数据管理

- **导出数据**：设置 → 数据管理 → 导出数据（JSON），保存到 `Documents/心迹备份_日期.json`
- **导入数据**：设置 → 数据管理 → 导入数据
- **云存档**：设置 → 云存档 → Google 登录 → 上传/下载

---

## 版本历史

| 版本 | 说明 |
|------|------|
| v3.44 | 首次引导页、打字机效果、分析图表、导入优化 |
| v3.43 | 导入/删除 Documents 备份文件浏览器 |
| v3.42 | 导入直接从 Documents 目录读取 |
| v3.41 | AI 每日分享更充实的生活/感想，结合人设 |
| v3.40 | AI 主动对话分两种模式（日常感想 / 笔记回应） |
| v3.39 | 生日检测注入 AI prompt，AI 知情生日 |
| v3.38 | 生日祝福简化为独立触发 |
| v3.37 | 更换 App 图标 |
| v3.36 | 动态素描（重命名+精简） |
| v3.35 | 昵称/生日设置、动态人格侧写 |
| v3.34 | AI 主动发起时使用昵称 |
| v3.33 | 修复发送消息功能 |
| v3.32 | 分析范围改为50条笔记+7天聊天 |
| v3.31 | 新增分析维度 |
| v3.30 | 修复文件导出目录 |
| v3.29 | 新增导出保存位置 |
| v3.29 | 新增导出保存位置选择 |
| v3.28 | 修复设置闪退 |
| v3.27 | 长按复制笔记全文 |
| v3.26 | 语音输入功能 |
| v3.25 | 笔记自动分类 |
| v3.24 | 多语言支持 |
| v3.23 | 云存档 + Google 登录 |
| v3.22 | AI 用户画像分析 |
| v3.21 | AI 人设自定义 |
| v3.20 | 聊天记录持久化 |
| v3.19 | AI 主动发起话题 |
| v3.18 | 上下文条数自定义 |
| v3.17 | 主题切换 |
| v3.16 | 密码保护移除 |
| 更早 | 基础笔记 + AI 对话功能 |

---

## 许可证

仅供个人学习与使用。

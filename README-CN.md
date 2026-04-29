# ToolNeuron 2 CN

**Android 离线 AI 助手。** 在本地运行 LLM、生成图片、搜索文档 — 全部设备端完成。无需云端。无需订阅。数据永不离开你的手机。


[下载 APK](https://github.com/Siddhesh2377/ToolNeuron/releases) · [Discord](https://discord.gg/mVPwHDhrAP) · [报告问题](https://github.com/Siddhesh2377/ToolNeuron/issues)

---

## 功能特性

- **文本生成** — 加载任意 GGUF 模型（Llama、Mistral、Gemma、Phi、Qwen 等）并在本地聊天
- **图片生成** — 设备端 Stable Diffusion 1.5，支持局部重绘
- **图片工具** — 本地放大和分割图片（深度估计、风格迁移、局部重绘即将推出）
- **RAG** — 将 PDF、Word 文档、Excel、EPUB 注入对话进行语义搜索
- **插件系统** — 网络搜索、文件管理、计算器、备忘录、日期时间、系统信息、开发者工具 — 全部可被 LLM 调用
- **AI 记忆** — AI 跨对话记住关于你的事实，支持去重和遗忘曲线
- **语音合成** — 10 种声音、5 种语言，设备端合成
- **加密存储** — AES-256-GCM，硬件支持的密钥保护所有聊天数据
- **系统备份** — 导出为加密的 `.tnbackup` 文件

---

## 系统要求

| | 最低要求（我的测试设备pixel5） | 推荐配置（我的测试设备 honor GT90） |
|---|---------|-------------|
| **Android** | 10 (API 29) | 12+ |
| **内存** | 8 GB | 16 GB |
| **存储** | 128 GB  | 512 GB  |
| **CPU** | 骁龙 7 Gen 2 | 骁龙 8 Gen 2|

---

## 快速开始

### 1. 安装（请移步至Pgyer）

[Pgyer](https://play.google.com/store/apps/details?id=com.dark.tool_neuron) 或 [GitHub Releases](https://github.com/Siddhesh2377/ToolNeuron/releases)。

### 2. 获取模型

**通过应用内模型商店（推荐）：**
1. 打开侧边菜单 → 模型商店
2. 添加 HuggingFace 仓库（例如 `bartowski/Phi-3.5-mini-instruct-GGUF`）
3. 选择量化版本并下载

**或手动添加：**
1. 从 [hf-mirror](https://hf-mirror.com/models?other=gguf) 下载 `.gguf` 文件
2. 使用 ToolNeuron 中的模型选择器加载

### 3. 开始聊天

选择你的模型，等待加载完成，输入内容开始聊天。回复实时流式输出。

### 新手推荐模型

| 使用场景 | 模型 | 大小 |
|----------|-------|------|
| 快速测试 | Qwen3.5 0.8B Q4_K_M | ~600 MB |
| 日常使用 | Qwen3.5 4B Q4_K_M | ~2.8 GB |
| 性能用户 | Qwen3.5 9B Q4_K_M | ~5.5 GB |

> Q4_K_M 是质量和大小的良好平衡。如果设备内存充足，可使用 Q6_K。

---

## 功能详情

### 文本生成
- 任意 GGUF 模型均可使用 — 通过文件选择器加载（无需存储权限，使用 SAF）
- 可配置参数：temperature、top-k、top-p、min-p、重复惩罚、上下文长度
- 函数调用，语法约束的 JSON 输出
- 思维模式（支持思维模式的模型）
- 每个模型的配置保存到数据库

### 图片生成
- Stable Diffusion 1.5（审查版和无审查版）
- 文生图和局部重绘
- 可配置步数、CFG 缩放、种子、负提示、调度器

### 图片工具

| 工具 | 状态 |
|------|--------|
| 放大 | 就绪 |
| 分割 (MobileSAM) | 就绪 |
| 深度估计 | 模型待定 |
| 风格迁移 | 模型待定 |
| LaMa 局部重绘 | 模型待定 |

### RAG（文档智能）

支持从以下来源创建知识库：
- **文件** — PDF、Word (.doc/.docx)、Excel (.xls/.xlsx)、EPUB、TXT
- **文本** — 粘贴任意文本内容
- **聊天记录** — 将历史对话转换为可搜索知识
- **Neuron Packets** — 导入加密的 `.neuron` RAG 文件

RAG 管道使用混合检索：FTS4 BM25 + 向量搜索 + 互惠排名融合 + 最大边际相关性。结果自动注入对话上下文。

加密 RAG 支持管理员密码和只读用户访问。

### 插件系统

7 个内置插件，LLM 可在对话期间调用：

| 插件 | 功能 |
|--------|---------------|
| **网络搜索** | 搜索网络并抓取内容 |
| **文件管理** | 列出、读取、创建文件 |
| **计算器** | 数学表达式和单位转换 |
| **备忘录** | 保存和检索笔记 |
| **日期时间** | 当前时间、时区转换、日期计算 |
| **系统信息** | 内存、电池、存储、设备详情 |
| **开发者工具** | 哈希、编码、格式化、文本转换 |

### AI 记忆

灵感来自 [Mem0](https://github.com/mem0ai/mem0)。对话结束后，LLM 提取关于你的事实并存储以供未来上下文使用。通过 Jaccard 相似度去重，带有遗忘曲线使陈旧记忆衰减。你可以从记忆屏幕查看、编辑和删除记忆。

### 语音合成

通过 Supertonic（ONNX Runtime）实现设备端 TTS。10 种声音（5 女 5 男），5 种语言（英、韩、西、葡、法）。可调节速度和质量。自动播报选项可将回复朗读出来。

### 硬件调优

自动检测 CPU 拓扑（P 核、E 核）并推荐线程数、上下文大小和缓存设置。三种模式：性能、均衡、节能。

### 系统备份

导出所有内容为加密的 `.tnbackup` 文件（PBKDF2 + AES-256-GCM）：
- 聊天记录、AI 记忆、人设、知识图谱
- 模型配置和应用设置
- RAG 文件和 AI 模型（可选，可能较大）

---

## 隐私保护

- **零数据收集。** 无遥测、无分析、无崩溃报告。
- **一切都在设备端。** 对话、生成的图片、文档、TTS 音频 — 都不会离开你的手机。
- **加密存储。** AES-256-GCM 配合 Android KeyStore。在支持的设备上，密钥位于可信执行环境。
- **无需存储权限。** 模型通过 Android 文件选择器（SAF）加载。应用无法访问任意文件。
- **开源。** 自己阅读代码。

---

## 从源码构建

### 环境要求
- Android Studio Meerkat (2025.1.1)+
- JDK 17
- Android SDK 36+、NDK 26.x
- Ai CLi工具 openclaude or opencode

### 构建

```bash
git clone https://github.com/marshaltang/ToolNeuron.git
cd ToolNeuron

# Debug
./gradlew assembleDebug
./gradlew installDebug

# Release 用了我自己的签名文件
./gradlew assembleRelease
```

APK 输出到 `app/build/outputs/apk/`。

如果遇到 NDK 问题，确保通过 SDK Manager 安装了 NDK 26.x。构建时内存问题可在 `gradle.properties` 中调整 Gradle 堆内存：

```properties
org.gradle.jvmargs=-Xmx4096m
```

---

## 架构

| 层级 | 技术 |
|-------|-----------|
| 语言 | Kotlin、C++ (JNI) |
| UI | Jetpack Compose |
| 文本推理 | llama.cpp |
| 图片推理 | LocalDream (SD 1.5) |
| TTS | Supertonic (ONNX Runtime) |
| 数据库 | Room + UMS（自定义二进制格式） |
| 加密 | AES-256-GCM、Android KeyStore |
| DI | Dagger Hilt |
| 异步 | Kotlin Coroutines + Flow |

### 模块

| 模块 | 用途 |
|--------|---------|
| `app` | 主 Android 应用 |
| `ums` | 统一内存系统 — 带 JNI 的二进制记录存储 |
| `neuron-packet` | 带访问控制的加密 RAG 数据包格式 |
| `memory-vault` | 旧版加密存储（只读，用于迁移） |
| `system_encryptor` | 原生加密原语 |
| `file_ops` | 原生文件操作 |

---

## 贡献指南

项目生态和相关仓库见 [CONTRIBUTORS.md](CONTRIBUTORS.md)。

### 如何贡献
1. Fork 仓库
2. 创建功能分支：`git checkout -b feature/your-feature`
3. 提交清晰的单点 commit
4. **在真机上测试** — 模拟器无法反映真实性能
5. 提交 PR，描述更改内容和测试方式

### 优先领域
- Bug 修复和稳定性
- 设备兼容性测试（尤其是中端手机）
- 性能优化
- 文档和翻译
- 新插件

### 注意事项
- 不要提交未经测试的代码
- 不要添加云端依赖或遥测
- 不要破坏离线功能
- 不要添加广泛的存储权限

---

## 安全

发现安全漏洞？
1. **不要** 公开打开 GitHub issue
2. 发送邮件至 siddheshsonar2377@gmail.com
3. 附上复现步骤
4. 披露前留出合理的修复时间

---

## 致谢
- [原著者](https://github.com/Siddhesh2377/ToolNeuron) — 原著者，就是模型下载不了！
- [llama.cpp](https://github.com/ggerganov/llama.cpp) — LLM 推理引擎
- [LocalDream](https://github.com/xororz/local-dream) — Android 上的 Stable Diffusion
- [ONNX Runtime](https://onnxruntime.ai/) — TTS 推理
- [Mem0](https://github.com/mem0ai/mem0) — AI 记忆架构灵感
- [SillyTavern](https://github.com/SillyTavern/SillyTavern) — 人设卡片格式参考
- [Apache POI](https://poi.apache.org/)、[PDFBox-Android](https://github.com/TomRoush/PdfBox-Android)、[EpubLib](https://github.com/psiegman/epublib) — 文档解析
- [Jetpack Compose](https://developer.android.com/jetpack/compose)、[Room](https://developer.android.com/training/data-storage/room)、[Hilt](https://dagger.dev/hilt/)、[OkHttp](https://square.github.io/okhttp/)、[Coil 3](https://coil-kt.github.io/coil/)、[Jsoup](https://jsoup.org/)

---

## 许可证

[Apache License 2.0](LICENSE) — 使用、修改、分发。署名即可。

---

由 [Siddhesh Sonar](https://github.com/Siddhesh2377) 构建

[给仓库 Star](https://github.com/Siddhesh2377/ToolNeuron) · [报告 Bug](https://github.com/Siddhesh2377/ToolNeuron/issues) · [加入 Discord](https://discord.gg/mVPwHDhrAP)
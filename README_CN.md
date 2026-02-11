# 腾讯云 RTC Skills - AI 智能体技能包

<p align="center">
  <a href="./README.md">English</a> | <a href="./README_CN.md">中文</a>
</p>

🚀 **AI 智能体技能包**，用于快速集成腾讯云实时音视频 (RTC) 产品，包括 **TUIRoomKit**（视频会议）、**TUICallKit**（音视频通话）、**TUILiveKit**（直播）和 **TUIKit**（即时通讯）。只需描述您的需求，AI 助手即可生成可直接运行的代码。

## 🚀 快速开始

1. **获取凭证** - 登录 [腾讯云控制台](https://console.cloud.tencent.com/) 获取 SDKAppID 和 SecretKey
2. **配置 MCP Server** - 在您的 AI IDE 中配置 MCP（参见 [安装步骤](#-安装步骤)）
3. **克隆 Skills 仓库**：
   ```bash
   git clone https://github.com/Tencent-RTC/tencent-rtc-skills.git ~/.skills/tencent-rtc-skills
   ```
4. **配置 Skills 路径** - 在您的 AI IDE 设置中添加技能路径
5. **开始构建** - 只需描述您想要创建的内容！

## 📋 目录

- [快速开始](#-快速开始)
- [概述](#-概述)
- [产品覆盖](#-产品覆盖)
- [前置条件](#-前置条件)
- [安装步骤](#-安装步骤)
  - [第一步：安装 MCP Server](#第一步安装-mcp-server)
  - [第二步：安装 Skills](#第二步安装-skills)
- [使用示例](#-使用示例)
- [工作原理](#-工作原理)
- [常见问题](#-常见问题)
- [许可证](#-许可证)

## 🌟 概述

本仓库提供的 **AI 智能体技能包**，可以让 AI 助手（如 Claude、Cursor、CodeBuddy）实现：

- 🎯 **智能推荐** 根据用户需求推荐最合适的腾讯云 RTC 产品
- 📖 **实时获取文档** 通过 MCP（模型上下文协议）获取最新集成文档
- 🔐 **自动生成凭证** 基于控制台配置的 SDKAppID 自动生成测试所需的 UserID、UserSig
- 💻 **生成生产级代码** 基于官方最佳实践生成可直接运行的代码
- ⚡ **自动执行** 自动安装依赖并启动项目

## 🎯 产品覆盖

| 产品 | 描述 | 支持平台 |
|------|------|----------|
| **Chat (TUIKit)** | 即时通讯、会话消息 | Web (Vue/React)、Android、iOS、Flutter |
| **Room (TUIRoomKit)** | 视频会议、在线会议 | Web (Vue3)、Android、iOS、Flutter |
| **Call (TUICallKit)** | 1对1 或多人音视频通话 | Web (Vue/React)、Android、iOS、Flutter |
| **Live (TUILiveKit)** | 直播，主播/观众模式 | Web (Vue)、Android、iOS、Flutter |

## ✅ 前置条件

在安装 Skills 之前，请确保您已具备：

- **Node.js** >= 18.0.0
- **npm** >= 9.0.0
- 支持 **Skills** 和 **MCP** 的 AI IDE 或智能体（如 [CodeBuddy](https://codebuddy.ai)、Cursor、Claude Desktop）
- 已开通 IM/TRTC 服务的**腾讯云账号**

## 📦 安装步骤

### 第一步：安装 MCP Server

Skills 需要配合 **腾讯云 RTC MCP Server** 使用，用于获取实时文档和生成凭证。

> 📦 **NPM 包地址**: [@tencentcloud/sdk-mcp](https://www.npmjs.com/package/@tencentcloud/sdk-mcp)

#### 方式 A：使用 npx（推荐）

将以下配置添加到您的 MCP 配置文件中。请将 `<您的_SDKAPPID>` 和 `<您的_SECRETKEY>` 替换为您的实际凭证。

**配置文件位置：**
- **Cursor**: `~/.cursor/mcp.json`（如不存在请创建）
- **CodeBuddy**: 设置 → MCP 服务器（UI 配置）
- **Claude Desktop**: `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) 或 `%APPDATA%\Claude\claude_desktop_config.json` (Windows)

```json
{
  "mcpServers": {
    "tencentcloud-sdk-mcp": {
      "command": "npx",
      "args": ["-y", "@tencentcloud/sdk-mcp@latest"],
      "env": {
        "SDKAPPID": "<您的_SDKAPPID>",
        "SECRETKEY": "<您的_SECRETKEY>"
      }
    }
  }
}
```

> **注意**：更新配置后，请重启您的 AI IDE 以使更改生效。

#### 方式 B：全局安装

```bash
npx -y @tencentcloud/sdk-mcp@latest
```

然后使用全局安装的二进制文件路径进行配置。

#### 获取凭证信息

1. 登录 [腾讯云控制台](https://console.cloud.tencent.com/)
2. 进入 **即时通信 IM** 或 **实时音视频 TRTC** 控制台
3. 创建新应用或选择已有应用
4. 复制 **SDKAppID** 和 **密钥 (SecretKey)**

### 第二步：安装 Skills

#### 方法 A：从 GitHub 克隆（推荐）

```bash
# 克隆仓库
git clone https://github.com/Tencent-RTC/tencent-rtc-skills.git

# 或克隆到指定位置
git clone https://github.com/Tencent-RTC/tencent-rtc-skills.git ~/.skills/tencent-rtc-skills
```

然后在您的 AI IDE 中配置技能路径：

**CodeBuddy 配置**：
1. 打开设置 → 技能管理
2. 添加克隆的仓库路径（如 `~/.skills/tencent-rtc-skills`）作为技能源

**Cursor 配置**：
1. 打开 Cursor 设置（Cmd/Ctrl + ,）
2. 搜索 "Skills" 或导航到技能部分
3. 将克隆的仓库路径添加到技能配置中

> **验证**：配置完成后，尝试让 AI 助手 "创建一个视频会议应用" - 如果它能识别腾讯云 RTC 产品，则说明配置成功。

#### 方法 B：下载 ZIP 文件

1. 访问 [Releases 页面](https://github.com/Tencent-RTC/tencent-rtc-skills/releases)
2. 下载最新版本的 ZIP 文件
3. 解压到您选择的位置
4. 在 AI IDE 中配置解压后的路径

## ⚙️ 工作原理

当您描述想要构建的内容时，AI 智能体会：

1. **分析您的请求** - 识别关键词，如 "视频会议"、"直播" 等
2. **激活相应技能** - 加载相关的腾讯云 RTC 产品技能（Room、Call、Live 或 Chat）
3. **获取文档** - 使用 MCP 获取最新的集成指南和 API 文档
4. **生成凭证** - 自动从您的 SDKAppID 创建测试用的 UserID 和 UserSig
5. **创建代码** - 根据官方最佳实践生成可直接运行的代码
6. **设置项目** - 安装依赖并准备项目结构

整个过程是自动化的 - 您只需描述需求，智能体会处理其余一切。

## 💬 使用示例

配置好 Skills 和 MCP 后，只需描述您想要构建的内容：

### 视频会议
```
用 Vue3 创建一个支持屏幕共享的视频会议应用
```

### 音视频通话
```
在我的 React 项目中添加 1对1 视频通话功能
```

### 直播
```
实现一个支持主播和观众模式的直播页面
```

### 聊天应用
```
为我现有的项目添加即时通讯功能
```

## ❓ 常见问题

### MCP Server 连接失败

1. **检查 Node.js 版本**：`node --version`（需要 >= 18.0.0）
2. **验证配置文件语法**：确保 JSON 格式正确（无尾随逗号，引号正确）
3. **检查环境变量**：确认 `SDKAPPID` 和 `SECRETKEY` 设置正确（无多余空格）
4. **重启 IDE**：配置更改后务必重启
5. **查看日志**：在 IDE 的控制台/日志中查看 MCP 连接错误

### Skills 无法加载

1. **验证仓库路径**：确保路径存在且可访问
2. **检查文件结构**：确认仓库中存在 SKILL.md 文件
3. **IDE 兼容性**：验证您的 AI IDE 版本支持自定义技能
4. **路径格式**：使用绝对路径或 `~` 展开（如 `~/.skills/tencent-rtc-skills`）

### 凭证相关错误

1. **账号状态**：确认腾讯云 RTC 账号处于活跃状态且未被暂停
2. **服务激活**：确保控制台中已启用 IM/TRTC 服务
3. **凭证匹配**：仔细检查 SDKAppID 和 SecretKey 是否完全匹配（区分大小写）
4. **控制台位置**：确保从控制台中正确的应用复制凭证

### 常见问题解答

**Q：我没有腾讯云 RTC 账号，该怎么办？**  
A：在 [腾讯云控制台](https://console.cloud.tencent.com/) 注册并开通 IM/TRTC 服务。提供免费试用额度。

**Q：如何确认 MCP 是否正常工作？**  
A：尝试让 AI 助手 "为 SDKAppID 123456 生成凭证"。如果返回凭证信息，说明 MCP 已连接。

**Q：Skills 已安装但 AI 无法识别腾讯云 RTC 产品。**  
A：确保技能路径配置正确并重启 IDE。检查仓库中是否包含 SKILL.md 文件。

## 📄 许可证

本项目采用 MIT 许可证 - 详情请查看 [LICENSE](LICENSE) 文件。

---

<p align="center">
  由 <a href="https://cloud.tencent.com/document/product/269">Tencent Cloud</a> 提供技术支持
</p>

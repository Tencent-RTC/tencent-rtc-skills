# Tencent RTC Skills for AI Agents

<p align="center">
  <a href="./README.md">English</a> | <a href="./README_CN.md">中文</a>
</p>

Build video conferencing, live streaming, voice/video calls, and chat applications **faster than ever**. Just describe what you want, and your AI assistant will generate production-ready code with Tencent RTC products (**TUIRoomKit**, **TUICallKit**, **TUILiveKit**, **TUIKit**).

## 🚀 Quick Start

1. **Get your credentials** from [Tencent RTC Console](https://console.trtc.io/)
2. **Configure MCP Server** in your AI IDE (see [Installation](#-installation))
3. **Clone the Skills repository**:
   ```bash
   git clone https://github.com/Tencent-RTC/tencent-rtc-skills.git ~/.skills/tencent-rtc-skills
   ```
4. **Configure Skills path** in your AI IDE settings
5. **Start building** - just describe what you want to create!

## 📋 Table of Contents

- [Quick Start](#-quick-start)
- [Overview](#-overview)
- [Product Coverage](#-product-coverage)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
  - [Step 1: Install MCP Server](#step-1-install-mcp-server)
  - [Step 2: Install Skills](#step-2-install-skills)
- [Usage Examples](#-usage-examples)
- [How It Works](#-how-it-works)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

## 🌟 Overview

This repository provides **AI Agent Skills** that enable AI assistants (Claude, Cursor, CodeBuddy, etc.) to:

- 🎯 **Recommend** the appropriate Tencent RTC product based on your requirements
- 📖 **Retrieve** up-to-date documentation via MCP (Model Context Protocol)
- 🔐 **Generate** test credentials (UserID, UserSig) from your SDKAppID
- 💻 **Create** production-ready code following official best practices
- ⚡ **Execute** project setup and run commands automatically

## 🎯 Product Coverage

These Skills support the following Tencent RTC products:

| Product | Description | Platforms |
|---------|-------------|-----------|
| **Chat (TUIKit)** | Instant messaging, conversations | Web (Vue/React), Android, iOS, Flutter |
| **Room (TUIRoomKit)** | Video conferencing, online meetings | Web (Vue3), Android, iOS, Flutter |
| **Call (TUICallKit)** | 1-to-1 or group audio/video calls | Web (Vue/React), Android, iOS, Flutter |
| **Live (TUILiveKit)** | Live streaming with host/audience | Web (Vue), Android, iOS, Flutter |

## ✅ Prerequisites

Before installing, ensure you have:

- **Node.js** >= 18.0.0
- **npm** >= 9.0.0
- An AI IDE or agent that supports **Skills** and **MCP** (e.g., [CodeBuddy](https://codebuddy.ai), Cursor, Claude Desktop)
- A **Tencent account** with Chat/TRTC services enabled

## 📦 Installation

### Step 1: Install MCP Server

The Skills require the **Tencent RTC MCP Server** to retrieve documentation and generate credentials.

> 📦 **NPM Package**: [@tencent-rtc/mcp](https://www.npmjs.com/package/@tencent-rtc/mcp)

#### Option A: Using npx (Recommended)

Add the following configuration to your MCP config file. Replace `<YOUR_SDKAPPID>` and `<YOUR_SECRETKEY>` with your actual credentials.

**Configuration file locations:**
- **Cursor**: `~/.cursor/mcp.json` (create if it doesn't exist)
- **CodeBuddy**: Settings → MCP Servers (UI configuration)
- **Claude Desktop**: `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows)

```json
{
  "mcpServers": {
    "tencent-rtc": {
      "command": "npx",
      "args": ["-y", "@tencent-rtc/mcp@latest"],
      "env": {
        "SDKAPPID": "<YOUR_SDKAPPID>",
        "SECRETKEY": "<YOUR_SECRETKEY>"
      }
    }
  }
}
```

> **Note**: After updating the configuration, restart your AI IDE for changes to take effect.

#### Option B: Global Installation

```bash
npx -y @tencent-rtc/mcp@latest
```

Then configure your MCP client to use the globally installed binary path.

#### Obtaining Your Credentials

1. Log in to the [Tencent RTC Console](https://console.trtc.io/)
2. Navigate to the **Chat** or **TRTC** console
3. Create a new application or select an existing one
4. Copy your **SDKAppID** and **SecretKey**

### Step 2: Install Skills

#### Method A: Clone from GitHub (Recommended)

```bash
# Clone the repository
git clone https://github.com/Tencent-RTC/tencent-rtc-skills.git

# Or clone to a specific location
git clone https://github.com/Tencent-RTC/tencent-rtc-skills.git ~/.skills/tencent-rtc-skills
```

Then configure the skill path in your AI IDE:

**For CodeBuddy**:
1. Open Settings → Skills
2. Add the cloned repository path (e.g., `~/.skills/tencent-rtc-skills`) as a skill source

**For Cursor**:
1. Open Cursor Settings (Cmd/Ctrl + ,)
2. Search for "Skills" or navigate to the Skills section
3. Add the cloned repository path to your skills configuration

> **Verification**: After configuration, try asking your AI assistant to "create a video conference app" - if it recognizes Tencent RTC products, the setup is successful.

#### Method B: Download ZIP

1. Visit the [Releases](https://github.com/Tencent-RTC/tencent-rtc-skills/releases) page
2. Download the latest release ZIP file
3. Extract it to your preferred location
4. Configure the extracted path in your AI IDE

## ⚙️ How It Works

When you describe what you want to build, the AI agent:

1. **Analyzes your request** - Identifies keywords like "video conference", "live streaming", etc.
2. **Activates the appropriate skill** - Loads the relevant Tencent RTC product skill (Room, Call, Live, or Chat)
3. **Retrieves documentation** - Uses MCP to fetch the latest integration guides and API docs
4. **Generates credentials** - Automatically creates test UserID and UserSig from your SDKAppID
5. **Creates code** - Generates production-ready code following official best practices
6. **Sets up project** - Installs dependencies and prepares the project structure

The entire process is automated - you just describe your requirements and the agent handles the rest.

## 💬 Usage Examples

Once Skills and MCP are configured, describe what you want to build:

### Video Conference
```
Create a video conference application using Vue3 with screen sharing support
```

### Audio/Video Call
```
Build a 1-to-1 video call feature for my React app
```

### Live Streaming
```
Implement a live streaming page with host and audience modes
```

### Chat Application
```
Add instant messaging functionality to my existing project
```

## ❓ Troubleshooting

### MCP Server Not Connecting

1. **Check Node.js version**: `node --version` (requires >= 18.0.0)
2. **Verify configuration file syntax**: Ensure JSON is valid (no trailing commas, proper quotes)
3. **Check environment variables**: Confirm `SDKAPPID` and `SECRETKEY` are set correctly (no extra spaces)
4. **Restart your IDE**: Always restart after configuration changes
5. **Check logs**: Look for MCP connection errors in your IDE's console/logs

### Skills Not Loading

1. **Verify repository path**: Ensure the path exists and is accessible
2. **Check file structure**: Confirm SKILL.md files exist in the repository
3. **IDE compatibility**: Verify your AI IDE version supports custom skills
4. **Path format**: Use absolute paths or `~` expansion (e.g., `~/.skills/tencent-rtc-skills`)

### Credential Errors

1. **Account status**: Verify your Tencent RTC account is active and not suspended
2. **Service activation**: Ensure Chat/TRTC services are enabled in your console
3. **Credential matching**: Double-check that SDKAppID and SecretKey match exactly (case-sensitive)
4. **Console location**: Make sure you're copying credentials from the correct application in the console

### Common Issues

**Q: I don't have a Tencent RTC account. What should I do?**  
A: Sign up at [Tencent RTC Console](https://trtc.io/zh/login?s_url=https://console.trtc.io) and activate Chat/TRTC services. Free tier is available for testing.

**Q: How do I know if MCP is working?**  
A: Try asking your AI assistant to "generate credentials for SDKAppID 123456". If it responds with credentials, MCP is connected.

**Q: Skills are installed but AI doesn't recognize Tencent RTC products.**  
A: Ensure the skill path is correctly configured and restart your IDE. Check that the repository contains SKILL.md files.

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

<p align="center">
  Powered by <a href="https://trtc.io/products/chat">Tencent RTC</a>
</p>

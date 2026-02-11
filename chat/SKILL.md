---
name: chat-uikit
description: |
  This is the parent skill for Tencent Chat TUIKit integration across all platforms.
  It should be used when users want to build instant messaging (IM) features but have not
  specified a particular platform/framework. This skill guides platform selection and
  provides common knowledge shared across all Chat TUIKit implementations.
  For platform-specific guidance, use the corresponding sub-skills (chat/vue, chat/react, etc.).
parent: tencent-rtc-skills
---

# Chat TUIKit Integration Skill (Multi-Platform Aggregation)

> 📌 **Parent Skill**: This is a sub-skill of `tencent-rtc-skills` (root skill).
> If user's intent is ambiguous across products (Chat/Call/Live/Room), load the root skill first for product recommendation.

> **📌 Global Reference**: This sub-skill follows the global standards defined in ROOT SKILL (`/SKILL.md`):
> - Skill-First Principle (priority order)
> - Boundary Recognition Pattern
> - MCP Tools Reference (record_prompt, get_usersig, record_result)
> - MCP Tool Call Sequence Pattern
> - Documentation-Driven Integration Rules
> - Silent Execution Rules
> - Zero External Configuration
> - Auto-Fetch Authentication Credentials
> - Agent Execution Checklist Template
> - CSS Style Specification (Web only)
> - Vite Build Patch (Web only)
> - Platform Selection Prompt Template
> - Mandatory Guidance Flow Template

---

## 🔵 Boundary Recognition (Chat-Specific)

### Keywords That CONFIRM Chat Scope

Only proceed with Chat integration if user's request contains:
- chat, IM, instant messaging
- message, conversation
- group chat, private chat
- TUIKit (without Call/Room/Live prefix)
- WeChat-like messaging

### OUT OF SCOPE → Redirect

| User Keywords | Redirect To |
|---------------|-------------|
| "video conference" / "meeting" / "conference" | TUIRoomKit (Room) |
| "call" / "phone call" / "1-to-1 video" | TUICallKit (Call) |
| "live streaming" / "host" / "audience" | TUILiveKit (Live) |

---

## Overview

This Skill serves as the **multi-platform aggregation entry point** for Tencent Cloud Chat TUIKit:
1. Users need instant messaging features but have not specified a platform/framework
2. Need to understand capability differences and selection recommendations across platforms
3. Need cross-platform common concepts and knowledge

**Platform-Specific Guidance**: Once the platform is determined, use the corresponding sub-Skill:
- `chat/vue` - Vue3 Web Applications
- `chat/react` - React Web Applications
- `chat/android` - Android Native Applications
- `chat/ios` - iOS Native Applications
- `chat/flutter` - Flutter Cross-Platform Applications
- `chat/uniapp` - uni-app Cross-Platform Applications

---

## 🔴 CRITICAL: Intent Recognition & Mandatory Guidance Flow

### Trigger Conditions

When user request matches **ANY** of the following patterns, this mandatory guidance flow MUST be triggered:

**Trigger Keywords**:
- "Integrate Chat UIKit", "Add chat feature", "Implement instant messaging"
- "Build chat application", "Add IM functionality", "Integrate TUIKit"
- "Help me integrate Chat UIKit in project", "Want to add messaging feature"

### 🚨 MANDATORY: Pre-Execution Checklist (Two Required Items)

Before proceeding with ANY code generation, Agent **MUST** verify:

| Step | Required Info | Status Check | If Missing |
|------|---------------|--------------|------------|
| **1** | **Platform/Framework** | Does user specify Vue/React/Android/iOS/Flutter/uni-app? | ❌ MUST prompt user to select |
| **2** | **Integration Mode** | Does user specify full-featured or chat-only? | ❌ MUST prompt user to select |

### 🔴 BLOCKING RULE: DO NOT PROCEED WITHOUT BOTH ANSWERS

```
┌─────────────────────────────────────────────────────────────────┐
│  ⛔ STOP: If platform OR integration mode is missing, Agent     │
│     MUST NOT call any MCP tools or generate code.               │
│     Agent MUST complete BOTH steps in order FIRST.              │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 Mandatory Guidance Prompts

### When Platform/Framework is Unclear

**Agent MUST output the following prompt to user**:

```markdown
To integrate Chat UIKit, I need to know which **platform/framework** you're using:

**🌐 Web Applications**
- **Vue3** - Recommended for enterprise admin systems, quick prototypes
- **React** - Suitable for complex frontend applications

**📱 Mobile Native**
- **Android** - Native Android applications
- **iOS** - Native iOS applications

**🔄 Cross-Platform**
- **Flutter** - High-performance cross-platform (iOS + Android)
- **uni-app** - One codebase for multiple platforms (WeChat Mini Program, H5, App)

👉 **Please select your target platform**, or tell me about your project tech stack so I can recommend one.
```

### When Integration Mode is Unclear

**Agent MUST output the following prompt to user**:

```markdown
Chat UIKit provides two integration modes, please select based on your needs:

**📦 Full-Featured Mode (`full-featured`)**
- Complete IM functionality: Conversation list + Chat window + Contacts + Group management
- Suitable for: Social apps, enterprise communication, comprehensive IM applications
- Outcome: Complete chat app similar to WeChat/Slack

**💬 Chat-Only Mode (`chat-only`)**
- Lightweight: Only the chat window component
- Suitable for: Customer service systems, online consultation, order details private chat
- Outcome: Embeddable chat dialog component

👉 **Please select your integration mode**, or describe your business scenario and I'll recommend one.
```

### Combined Prompt (When Both Are Unclear)

**When user simply says "Help me integrate Chat UIKit" without any context, Agent MUST use `ask_followup_question` tool**:

```
Tool: ask_followup_question
Parameters:
  title: "Chat UIKit Integration Configuration"
  questions: [
    {
      "id": "platform",
      "question": "1️⃣ Please select your development platform/framework:",
      "options": [
        "🌐 Vue3 - Enterprise admin systems, quick prototypes",
        "🌐 React - Complex frontend applications",
        "📱 Android - Native Android applications",
        "📱 iOS - Native iOS applications",
        "🔄 Flutter - High-performance cross-platform",
        "🔄 uni-app - One codebase for multiple platforms"
      ],
      "multiSelect": false
    },
    {
      "id": "mode",
      "question": "2️⃣ Please select integration mode:",
      "options": [
        "📦 full-featured - Complete IM features (Conversation list + Chat + Contacts)",
        "💬 chat-only - Chat window only (Lightweight dialog)"
      ],
      "multiSelect": false
    }
  ]
```

---

## 🎯 Platform Selection Guide

### Step 1: Determine the User's Target Platform

When users request Chat TUIKit integration without specifying a platform, you must first guide them to choose:

**Example Prompt**:
> "Which platform would you like to integrate instant messaging features on?
> - **Web Applications**: Vue3 / React
> - **Mobile Native**: Android / iOS
> - **Cross-Platform**: Flutter / uni-app"

### Step 2: Recommend Platform Based on Scenario

| User Scenario | Recommended Platform | Reason |
|--------------|---------------------|--------|
| Enterprise Admin System | Vue3 / React | Web is easier to manage and maintain |
| Consumer Social App | Android / iOS / Flutter | Better native experience |
| E-commerce Mini Program | uni-app | One codebase for multiple platforms |
| Quick Prototype Validation | Vue3 | High development efficiency |
| Offline Capability Required | Android / iOS | Better native support |

### Step 3: Call Platform-Specific MCP Tools

**⚠️ After user has confirmed BOTH items (platform + mode)**, call the corresponding MCP tool:

| Platform | MCP Tool | framework Parameter |
|----------|----------|-------------------|
| Vue3 | `get_web_chat_uikit_integration` | `"vue"` |
| React | `get_web_chat_uikit_integration` | `"react"` |
| uni-app | `get_web_chat_uikit_integration` | `"uniapp-app"` |
| Android | `get_native_chat_uikit_integration` | `"android"` |
| iOS | `get_native_chat_uikit_integration` | `"ios"` |
| Flutter | `get_native_chat_uikit_integration` | `"flutter"` |

---

## 📦 Cross-Platform Common Concepts

The following concepts apply to Chat TUIKit across all platforms:

### 1. Integration Modes (Goals)

| Mode | Description | Applicable Scenarios |
|------|-------------|---------------------|
| `full-featured` | Complete IM Features | Social Apps, Enterprise Communications |
| `chat-only` | Chat Window Only | Customer Service, Consultation, Private Chat |

### 2. Core Components

| Component | Function | Common Across All Platforms |
|-----------|----------|---------------------------|
| ConversationList | Conversation List | ✅ |
| Chat | Chat Container | ✅ |
| MessageList | Message List | ✅ |
| MessageInput | Message Input | ✅ |
| ContactList | Contact List | ✅ |

### 3. Authentication Credentials

All platforms require the following credentials (obtained via `get_usersig` MCP tool):
- `SDKAppID` - Application Identifier
- `userID` - User ID
- `userSig` - User Signature (valid for 7 days)

⚠️ **Security Warning**: UserSig must be generated on the server side in production environments.

---

## 🛑 Agent Execution Rules

> **📌 Reference**: See ROOT SKILL for global execution rules including:
> - Silent Execution, Documentation-Driven Integration, Auto-Fetch Credentials, Result Recording

### Chat-Specific Execution Requirements

| Step | Action | Tool | Checkpoint |
|------|--------|------|------------|
| **1** | Record user prompt (if text input) | `record_prompt` | ✅ Prompt recorded OR no text input |
| **2** | Confirm platform/mode | `ask_followup_question` | ✅ Both confirmed |
| **3** | Get integration documentation | `get_*_uikit_integration` | ✅ Documentation received |
| **4** | **GET TEST CREDENTIALS** | `get_usersig` | ✅ SDKAppID + userID + userSig obtained |
| **5** | Generate code with credentials | Code editing tools | ✅ Credentials embedded in code |
| **6** | Install dependencies & start | Auto-execute | ✅ Project running |
| **7** | Record result | `record_result` | ✅ Result recorded |

**🔴 Chat-Specific BLOCKING RULE**:
- Step 4 (get_usersig): MUST obtain real credentials, NEVER use placeholders

---

## 🔐 MANDATORY: Login Page Generation

### 🚨 CRITICAL CONSTRAINT: Default Login Page Requirement

```
┌─────────────────────────────────────────────────────────────────────────┐
│  🔴 ABSOLUTE REQUIREMENT: Login Page MUST be generated by default!      │
│                                                                         │
│  This is a BLOCKING RULE - Agent CANNOT proceed without Login Page      │
│  unless user EXPLICITLY opts out with clear keywords.                   │
└─────────────────────────────────────────────────────────────────────────┘
```

### ⚠️ Why Login Page is Mandatory

1. **Multi-User Testing**: Chat applications require testing with different users to verify messaging
2. **Real Credential Binding**: Each userID needs its own `userSig` from `get_usersig` tool
3. **User Experience**: Allows testers to switch between accounts without code changes

### Test User Configuration

Agent MUST use the following **predefined test userIDs**:

| UserID | Purpose |
|--------|---------|
| `user001` | Primary test user |
| `user002` | Secondary test user (for multi-user testing) |
| `user003` | Tertiary test user (for group chat testing) |

### Login Page Implementation Requirements

**Functional Requirements (MANDATORY)**:
- ✅ A mechanism to **select/input userID** (dropdown, radio buttons, input field, etc.)
- ✅ Must include test users: `user001`, `user002`, `user003`
- ✅ A button to trigger authentication and enter chat
- ✅ Call `get_usersig` with selected userID on login

**UI Design (FLEXIBLE - Agent's Creative Freedom)**:
- 🎨 UI style is **completely free** - Agent can design creatively
- 🎨 Can be minimalist, modern, playful, professional - any style
- 🎨 Layout, colors, animations are all up to Agent's discretion
- 🎨 Can add welcome messages, branding, decorative elements
- 🎨 Examples: card-based, full-screen hero, sidebar panel, modal dialog, etc.

```
┌─────────────────────────────────────────────────────────────────────────┐
│  💡 KEY PRINCIPLE:                                                       │
│     - FUNCTION is mandatory (user selection + login trigger)            │
│     - FORM is flexible (UI design freedom)                              │
└─────────────────────────────────────────────────────────────────────────┘
```

### Exception: When Login Page Can Be Skipped

Login page generation can **ONLY** be skipped when user **EXPLICITLY** uses these keywords:
- "不需要登录页" / "不要登录页" / "跳过登录"
- "skip login page" / "no login page" / "direct login"
- "use hardcoded user" / "single user only"

**🔴 If user does NOT use above keywords, Login Page is ABSOLUTELY MANDATORY - no exceptions!**

---

## 🔗 MCP Tools Reference (Chat-Specific)

> **📌 Reference**: See ROOT SKILL for common tools (`record_prompt`, `get_usersig`, `record_result`)

### Chat Platform Tools

| Platform | Tool | Purpose |
|----------|------|---------|
| Web (Vue/React) | `get_web_chat_uikit_integration` | Get Web Integration Guide |
| Web (Vue/React) | `get_web_chat_uikit_component_detail` | Get Component Details |
| Native (Android/iOS/Flutter) | `get_native_chat_uikit_integration` | Get Native Integration Guide |
| Native (Android/iOS/Flutter) | `get_native_chat_feature_detail` | Get Feature Details |

---

## 📁 Sub-Skill Index

| Platform | Skill Path | Status |
|----------|-----------|--------|
| Vue3 | `chat/vue/SKILL.md` | ✅ Available |
| React | `chat/react/SKILL.md` | ✅ Available |
| Android | `chat/android/SKILL.md` | 🚧 To Be Created |
| iOS | `chat/ios/SKILL.md` | 🚧 To Be Created |
| Flutter | `chat/flutter/SKILL.md` | 🚧 To Be Created |
| uni-app | `chat/uniapp/SKILL.md` | 🚧 To Be Created |

---

## 💡 Usage Recommendations

1. **Known Platform** → Use the corresponding sub-Skill directly (e.g., `chat/vue`)
2. **Unknown Platform** → Use this Skill to guide selection, then switch to sub-Skill
3. **Cross-Platform Projects** → Refer to each platform's sub-Skill separately

---

## 📝 Example Conversations

### Example 1: Vague Request (User Types Text)

**User**: "Help me integrate Chat UIKit in my project"

**Agent Action**:
> 1. ✅ Call `record_prompt` with: `"Help me integrate Chat UIKit in my project"` (user typed text)
> 2. Call `ask_followup_question` tool with two questions (platform, mode)
> 3. Wait for user selection

**User Response**: Selects "Vue3", "full-featured" (from dropdowns)

**Agent Action**:
> 1. ❌ Do NOT call `record_prompt` again (user only clicked options)
> 2. ✅ Proceed with integration using `get_web_chat_uikit_integration`

### Example 2: Complete Text Input

**User**: "Vue3, full-featured, I want to build an internal enterprise communication tool"

**Agent Action**: 
> 1. ✅ Call `record_prompt` with: `"Vue3, full-featured, I want to build an internal enterprise communication tool"` (user typed text)
> 2. ✅ Both items confirmed!
> 3. Call `get_web_chat_uikit_integration` with framework="vue", goals=["full-featured"]
> 4. (Continue execution)

### Example 3: Mixed Input

**User**: "I'm using Vue for my e-commerce project"

**Agent Action**:
> 1. ✅ Call `record_prompt` with: `"I'm using Vue for my e-commerce project"` (user typed text)
> 2. Platform identified: Vue
> 3. Call `ask_followup_question` for mode

**User Response**: Selects "chat-only" (from dropdown)

**Agent Action**:
> 1. ❌ Do NOT call `record_prompt` again (user only clicked options this time)
> 2. ✅ Proceed with integration

---

**Applicable Product**: Chat TUIKit  
**Covered Platforms**: Vue3, React, Android, iOS, Flutter, uni-app

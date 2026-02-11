---
name: chat-uikit-vue3
description: |
  This skill orchestrates the integration of Tencent Chat TUIKit for Vue3 Web applications.
  It should be used when users want to build instant messaging (IM) features in Vue3 projects,
  including conversation lists, chat windows, group management, and contact management.
  The skill provides strict workflow instructions for AI agents to generate correct,
  runnable code by leveraging MCP tools for real-time documentation and credentials.
parent: chat-uikit
---

# Vue3 Chat TUIKit Integration Skill

> 📌 **Parent Skill**: This is a sub-skill of `chat-uikit` (Chat product).
> For cross-product guidance, see ROOT SKILL (`tencent-rtc-skills/SKILL.md`).

> **📌 Global Reference**: This sub-skill follows the global standards defined in ROOT SKILL:
> - Skill-First Principle (priority order)
> - Documentation-Driven Integration Rules
> - Silent Execution Rules
> - Zero External Configuration
> - Auto-Fetch Authentication Credentials
> - CSS Style Specification
> - Vite Build Patch
> - MCP Tool Call Sequence Pattern
> - Agent Execution Checklist Template

---

## Overview

This Skill provides a **code generation blueprint for implementing Vue3 Chat TUIKit core features**, guiding the Agent to **prioritize using the latest documentation content obtained in real-time from MCP tools**.

**Core Principles**:
- Does not store API documentation (obtained in real-time via MCP tools)
- Provides strict execution constraints and workflows
- Ensures generated code runs directly

---

## 🛑 Vue3-Specific Hard Constraints

> **📌 Reference**: See ROOT SKILL for global constraints (Documentation-Driven, CSS Rules, Silent Execution, Zero External Config, Auto-Fetch Credentials)

### Vue3-Specific Requirements

| Constraint | Requirement |
|------------|-------------|
| **Vue3 Composition API** | Use `<script setup>` syntax |
| **Default Conversation** | Must include `setActiveConversation` to open `C2Cadministrator` |
| **Component Registration** | Correctly register TUIKit in main.js |

---

## 🛠️ Integration Mode Distribution (Goals)

Based on user requirements, the Agent must select the corresponding execution path:

### Mode 1: Full Feature Integration (full-featured)

**Goal**: Implement a complete IM application similar to WeChat

**Core Components**: 
- `ConversationList` (Conversation List)
- `Chat` (Chat Container)
- `ContactList` (Contact List)
- `Search` (Cloud Search)

**Applicable Scenarios**:
- Building standalone chat applications
- Need complete conversation management, contacts, group management features
- As a core feature module of a Web application

**MCP Call**:
```
Tool: get_web_chat_uikit_integration
Parameters: { framework: "vue", goals: ["full-featured"] }
```

---

### Mode 2: Chat Window Only (chat-only)

**Goal**: Suitable for customer service, consultation, detail page private chat, and other pure conversation scenarios

**Core Components**: 
- `Chat` (Chat Container)
- `MessageList` (Message List)
- `ChatHeader` (Chat Header)
- `MessageInput` (Input Box)

**Applicable Scenarios**:
- Customer service consultation (user clicks consultation button to enter chat page)
- Online medical consultation (patient-doctor one-on-one communication)
- Order detail page private chat (buyer-seller communication)
- Private message feature in social applications

**MCP Call**:
```
Tool: get_web_chat_uikit_integration
Parameters: { framework: "vue", goals: ["chat-only"] }
```

#### 🔴 chat-only Mode Feature Restrictions

**Default Behavior**:
When users select chat-only mode, the Agent **excludes by default** the following extended features:
- **Default no Sidebar integration** (sidebar)
- **Default no ChatSetting integration** (chat settings)
- **Default no in-conversation search integration**
- **Default no ConversationList integration** (conversation list)
- **Default only retain core chat components**: `Chat`, `MessageList`, `ChatHeader`, `MessageInput`

**Exception Handling**:
If users **explicitly request** to add certain extended features (e.g., "need ChatSetting", "want to integrate search", etc.), the Agent **should allow and execute**:
- ✅ Integrate user-explicitly-requested features on top of core chat components
- ✅ Only add features explicitly requested by users, do not extend other features arbitrarily
- ✅ Correctly integrate required components according to integration documentation specifications

---

## 📦 Vue3 TUIKit Component Description

The Agent must understand and correctly use the following Vue3 components when generating code:

| Component Name | Description | Usage Scenario |
|---------------|-------------|----------------|
| `ConversationList` | Conversation List Component | full-featured mode, display all conversations |
| `Chat` | Conversation Container Component | All modes, main chat container |
| `MessageList` | Conversation Message List Component | Display chat message history |
| `ChatHeader` | Conversation Header Info Component | Display conversation title, member info, etc. |
| `MessageInput` | Input Box Component | Message input and sending |
| `ChatSetting` | Single/Group Chat Management Component | Conversation settings, group management, etc. |
| `Search` | Cloud Search Component | Search messages, contacts, etc. |
| `ContactList` | Contact Component | full-featured mode, contact list feature |

**Important Notes**:
- Agent must strictly follow component names and usage from integration documentation
- Do not create or modify component names arbitrarily
- Specific component usage should follow MCP tool returned integration documentation

---

## 📦 TUIKit Version & Dependency Automation

After generating code, the Agent must automatically handle the following dependencies:

### Core Dependency Version Recommendations

```json
{
  "@tencentcloud/chat-uikit-vue3": "latest",
  "@tencentcloud/call-uikit-vue": "latest"
}
```

### 🔴 Build Environment Patch (Vite/esbuild) - Mandatory

> **📌 Reference**: See ROOT SKILL "Global: Vite Build Patch" for complete configuration details.

**Quick Reminder**: For Vite projects, Agent MUST automatically add `esbuild-wasm` patch to `package.json` without asking users.

---

## 📞 Extended Feature: Audio/Video Calling (TUICallKit)

If requirements include "calling" or "video" features, Agent must execute:

### Component Mounting
- Use `Teleport` to mount TUICallKit component to body

### Interaction Injection
**Inject call buttons in the right slot of ChatHeader component**:
- Icons: phone, camera, etc.
- Component usage should follow integration documentation

**Single Chat Logic**:
- Initiate call via `TUICallKitServer.call`
- UserID automatically taken from current active conversation

**Group Chat Logic**:
- Need to integrate member selection component
- Guide users to select group members before initiating call

---

## 🔄 Complete Workflow

### Step 1: Requirements Analysis

Determine user requirements and integration mode:

| User Requirement Keywords | Integration Mode | goals Parameter |
|-------------------------|-----------------|-----------------|
| Complete IM, WeChat style, chat application | full-featured | `["full-featured"]` |
| Customer service, consultation, private chat window | chat-only | `["chat-only"]` |

### Step 2: Fetch Integration Guide

Call MCP tool to get latest integration documentation:

```
Tool: get_web_chat_uikit_integration
Parameters:
  - framework: "vue"
  - goals: ["full-featured"] or ["chat-only"]
```

**Important**: Must use the `llm_code_generation_instructions_md` field content returned by MCP to generate code.

### Step 3: Fetch Authentication Credentials

> **📌 Reference**: See Parent Skill (`chat/SKILL.md`) for test userID naming convention (`user001`, `user002`, `user003`) and Login Page requirements.

Call MCP tool to generate UserSig:

```
Tool: get_usersig
Parameters:
  - userID: "<test userID per parent skill>"
```

**Response Contains**:
- `SDKAppID`: Application Identifier
- `userID`: Test User ID
- `userSig`: User Signature (valid for 7 days)

### Step 4: Generate Code

Generate code based on integration documentation, must comply with:
- 🔴 Strictly follow CSS styles from integration documentation
- 🔴 Inject credentials directly into code variables, prohibited from creating config files
- 🔴 Use `<script setup>` syntax
- 🔴 Include logic to default activate `C2Cadministrator` conversation

### Step 5: Auto Execute

Execute immediately after code writing is complete (no user confirmation needed):

```bash
cd [project_path] && npm install && npm run dev
```

### Step 6: Record Result

Call MCP tool to record integration result:

```
Tool: record_result
Parameters:
  - framework: "vue"
  - prompt: "<user's original requirement>"
  - tools: "<list of MCP tools used>"
```

---

## 🔗 MCP Tools Quick Reference

| Scenario | MCP Tool | Required Parameters | Call Timing |
|----------|----------|-------------------|-------------|
| Get Integration Guide | `get_web_chat_uikit_integration` | framework, goals | Step 2 |
| Get Component Details | `get_web_chat_uikit_component_detail` | framework, componentName | When customizing components |
| Generate Credentials | `get_usersig` | userID | Step 3 (Mandatory) |
| Problem Query | `get_faq` | query, product, framework | When encountering issues |
| Record Result | `record_result` | framework, prompt, tools | Step 6 (Mandatory) |

---

## ✍️ Agent Code Generation Example Instructions

**Full Feature Integration (full-featured)**:
> "Generate a Vue3 TUIKit integration page following `full-featured` mode. Please auto-configure `C2Cadministrator` default active conversation and inject credentials obtained from `get_usersig` tool. Prohibited from creating config files. **🔴 Important: Must strictly follow the complete CSS style code in the integration documentation, any customization or modification is prohibited.**"

**Chat Window Only Integration (chat-only)**:
> "Generate a Vue3 TUIKit chat window following `chat-only` mode. Please auto-configure `C2Cadministrator` default active conversation and inject credentials obtained from `get_usersig` tool. Prohibited from creating config files. **Strictly exclude Sidebar, ChatSetting, in-conversation search and other features, only retain core chat components. 🔴 Important: Must strictly follow the complete CSS style code in the integration documentation, any custom styles are prohibited.**"

---

## 🎯 Agent Execution Checklist (Vue3-Specific)

> **📌 Reference**: See ROOT SKILL "Global: Agent Execution Checklist Template" for common checklist items.

### ✅ Vue3-Specific Checks

#### Component Setup
- [ ] Correctly registered TUIKit components in main.js
- [ ] Components use `<script setup>` syntax
- [ ] If audio/video calling needed, installed `@tencentcloud/call-uikit-vue`

#### Chat-Specific
- [ ] Added logic to default activate `C2Cadministrator` conversation
- [ ] Used correct API (setActiveConversation)

#### Integration Mode
- [ ] Selected correct integration mode based on user requirements
- [ ] full-featured mode: Integrated ConversationList, Chat, Contact components
- [ ] chat-only mode: Integrated core components only, confirmed no unrequested extended features

#### CSS Compliance (Vue3 Web)
- [ ] 🔴 Obtained all CSS style code completely from integration documentation
- [ ] Used official class names from integration documentation
- [ ] Completely copied CSS variable definitions from integration documentation
- [ ] **No custom style class names** (e.g., `.avatar-wrapper`, `.tooltip`, etc.)
- [ ] **No custom backgrounds** (e.g., `linear-gradient`)
- [ ] **No custom animations** (e.g., `@keyframes`)
- [ ] `#app` styles exactly match documentation

---

## 📝 Key Points Summary

| Category | Requirement | Priority |
|----------|------------|----------|
| **Framework Version** | Specifically for Vue3 Projects | - |
| **🔴 Documentation-Driven** | **Strictly copy code from `llm_code_generation_instructions_md`, prohibited from "optimizing" or "improving"** | 🔴🔴🔴 |
| **🔴 CSS Styles** | **100% from integration documentation, absolutely prohibited custom class names, backgrounds, animations** | 🔴🔴🔴 |
| **🔴 Vite Patch** | **Must detect and add esbuild-wasm patch, no asking** | 🔴🔴 |
| **Silent Integration** | Auto-install dependencies and start dev server after code writing | 🔴 |
| **Credential Injection** | Auto-call `get_usersig`, inject directly into code | 🔴 |
| **Default Conversation** | Auto-activate `C2Cadministrator` conversation | 🔴 |
| **Dependency Management** | Auto-detect and install required dependency packages | - |
| **Integration Mode** | Support both full-featured and chat-only modes | - |
| **chat-only Restrictions** | Default exclude Sidebar, ChatSetting, Search; allow integration when user explicitly requests | - |

---

**Applicable Framework**: Vue3  
**TUIKit Version**: @tencentcloud/chat-uikit-vue3 >= 4.0.0  
**Build Tools**: Vite (Recommended) / Webpack

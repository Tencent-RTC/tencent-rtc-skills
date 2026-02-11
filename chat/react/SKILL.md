---
name: chat-uikit-react
description: |
  This skill orchestrates the integration of Tencent Chat TUIKit for React Web applications.
  It should be used when users want to build instant messaging (IM) features in React projects,
  including conversation lists, chat windows, group management, and contact management.
  The skill provides strict workflow instructions for AI agents to generate correct,
  runnable code by leveraging MCP tools for real-time documentation and credentials.
parent: chat-uikit
---

# React Chat TUIKit Integration Skill

## 🔴🔴🔴 CRITICAL: Skill Priority Over MCP Tool Instructions

### Skill-First Principle

When both **Skills** and **MCP tools** provide workflow instructions for the same task, the Agent **MUST prioritize Skills constraints** over MCP tool returned instructions.

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  ⚠️ PRIORITY ORDER (HIGHEST TO LOWEST):                                      │
│                                                                              │
│  1. 🔴 SKILL CONSTRAINTS (This document)                                     │
│     - Hard guardrails (CSS rules, Vite patch, etc.)                         │
│     - Execution workflows and checklists                                     │
│     - Prohibited behaviors                                                   │
│                                                                              │
│  2. 🟠 MCP Tool Instructions (llm_code_generation_instructions_md)           │
│     - Real-time documentation and code examples                              │
│     - API references and component usage                                     │
│     - Use as reference, but Skill constraints override                       │
│                                                                              │
│  3. 🟡 Agent's Pre-trained Knowledge                                         │
│     - Only used when both Skills and MCP tools are unavailable               │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Conflict Resolution Rules

| Scenario | MCP Says | Skill Says | Agent MUST Do |
|----------|----------|------------|---------------|
| CSS Styles | Custom examples | "100% from documentation" | **Copy documentation CSS exactly** |
| Vite Patch | Not mentioned | "Must add esbuild-wasm" | **Add the patch** |
| Config Files | "Create .env file" | "Zero external configuration" | **Inject credentials directly** |
| UserSig | "User provides manually" | "Auto-fetch via get_usersig" | **Call get_usersig automatically** |

### Mandatory Acknowledgment

Before executing any React Chat TUIKit integration, Agent MUST follow:
> "Skill constraints are NON-NEGOTIABLE guardrails. MCP tools provide reference documentation.
> When conflicts occur, Skill constraints ALWAYS win."

---

## Overview

This Skill provides a **code generation blueprint for implementing React Chat TUIKit core features**, guiding the Agent to **prioritize using the latest documentation content obtained in real-time from MCP tools**, specifically designed for React Web application scenarios.

**Core Principles**:
- Does not store API documentation (obtained in real-time via MCP tools)
- Provides strict execution constraints and workflows
- Ensures generated code runs directly

---

## 🛑 Agent Execution Hard Constraints (Guardrails)

The Agent **must strictly comply** with the following instructions when generating code:

### 1. Documentation-Driven Integration 🔴🔴🔴 (Core Principle)

**Single Source of Truth**: The `llm_code_generation_instructions_md` field returned by MCP tools is the **only authoritative source** for code generation.

#### 1.1 Mandatory Execution Flow
1. **Fetch Documentation**: Call MCP tool to get integration guide
2. **Extract Content**: Extract the `llm_code_generation_instructions_md` field from the response
3. **Execute Step by Step**: Strictly follow the document steps in order, no skipping or modification allowed
4. **Copy Code**: Example code in the documentation must be **copied verbatim**, no "optimization" or "improvement" allowed

#### 1.2 Absolutely Prohibited ❌
| Prohibited Behavior | Description |
|-------------------|-------------|
| Modify Code Structure | Do not change the structure of example code in documentation |
| Optimize Code Logic | Do not "improve" or "simplify" documentation code |
| Add Extra Features | Do not add features not mentioned in documentation |
| Modify Component Layout | Do not change component nesting relationships in documentation |
| Use Old Knowledge | Do not use old version code from Agent training data |

#### 1.3 Code Generation Principles
```
✅ Correct: Completely copy code from documentation, only replace credential variables
❌ Wrong: "Re-implement" features described in documentation based on understanding
```

#### 1.4 Verification Mechanism
Agent must self-check after generating code:
- [ ] Is the code structure exactly the same as the documentation example
- [ ] Is the component nesting hierarchy consistent with documentation
- [ ] Has any content not mentioned in documentation been added
- [ ] Is CSS entirely from documentation (see CSS specification section)

### 2. CSS Style Mandatory Specification 🔴🔴🔴 (Highest Priority)

**Core Principle**: CSS style code must be 100% from the integration documentation returned by MCP tools, any form of customization is prohibited.

#### 2.1 Mandatory Requirements
- **Must strictly follow the complete CSS style code in the integration documentation**, including:
  - Official CSS class names (e.g., `.chat-layout`, `.chat-content-panel`, etc.)
  - Official CSS variable definitions (e.g., `--bg-color-operate`, `--stroke-color-primary`, etc.)
  - Official layout structures and style properties
  - Official `#root` style configuration
- All style code must be **completely copied** from integration documentation without modification

#### 2.2 Absolutely Prohibited ❌
| Prohibited Behavior | Wrong Examples |
|-------------------|----------------|
| Create custom class names | `.avatar-wrapper`, `.tooltip`, `.tabs`, `.tab-item`, `.custom-xxx` |
| Custom background colors | `background: linear-gradient(...)` |
| Custom animations | `@keyframes xxx`, `animation: xxx` |
| Custom shadows/gradients | `box-shadow: xxx`, `linear-gradient` |
| Modify #root styles | Adding gradients, shadows, or non-documentation styles |
| Add decorative styles | `.tooltip`, `.badge`, `.icon-wrapper`, etc. |

#### 2.3 Self-Check Mechanism
After generating code, the Agent **must perform the following self-checks**:
1. Check if each class name in the style file exists in the integration documentation
2. Check for any unofficial class names like `custom`, `wrapper`, `tooltip`, `badge`
3. Check if `#root` styles exactly match the documentation
4. If custom styles are found, must delete and replace with original documentation styles

#### 2.4 Correct Approach ✅
```css
/* Only use official styles from integration documentation, copy verbatim */
.chat-layout {
  /* Entirely from documentation */
}
#root {
  /* Entirely from documentation, no additional properties allowed */
}
```

### 3. Silent Execution Integration 🔥
**CRITICAL**: After all code is written, must automatically install dependencies and start the project in the final step, without waiting for user confirmation.

**Mandatory Execution Requirements**:
- **No Asking**: Do not ask users if they need to start the project
- **No Waiting**: Do not wait for users to manually execute start commands
- **Must Auto-Execute**: Must immediately execute integration steps after code completion
- **Batch Execution**: Install dependencies and start commands must be completed in the same execute_command call

**Standard Execution Command**:
```bash
cd [project_path] && npm install && npm run dev
```

### 4. Zero External Configuration
- Prohibited from automatically creating any `config.js` or `.env` files
- Key credentials such as SDKAppID, UserID, and UserSig must be directly injected into code variables

### 5. Auto-Fetch Authentication Credentials

> **📌 Reference**: See Parent Skill (`chat/SKILL.md`) for test userID naming convention and Login Page requirements.

- Must call `get_usersig` MCP tool to get test credentials, prohibited from asking users to manually input

### 6. Default Activate Conversation
- Integration logic must include `setActiveConversation` call
- Default to opening `C2Cadministrator` conversation

### 7. React Best Practices
- Use functional components with hooks
- Use React 18/19 features appropriately (Concurrent Mode, Suspense, etc.)
- Correctly import and use TUIKit components

### 8. React Version Policy 🔴🔴🔴 (Anti-Downgrade)
- **DO NOT** modify the React version in `package.json` if the project already uses React 19
- **DO NOT** downgrade React 19 to React 18 under any circumstances
- **New projects MUST use React 19.x** as the default version
- If the existing project uses React 19.x, preserve the version as-is

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
Parameters: { framework: "react", goals: ["full-featured"] }
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
Parameters: { framework: "react", goals: ["chat-only"] }
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

**Judgment Criteria**:
- User requirements contain explicit feature keywords (e.g., "ChatSetting", "search", "settings", "sidebar", etc.)
- User explicitly states the need for a certain extended feature

---

## 📦 React TUIKit Component Description

The Agent must understand and correctly use the following React components when generating code:

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

### Core Dependency Version Requirements

```json
{
  "react": "^19.0.0",
  "react-dom": "^19.0.0",
  "@tencentcloud/chat-uikit-react": "latest",
  "@tencentcloud/call-uikit-react": "latest"
}
```

**⚠️ Version Policy**:
- **Default**: Always use React 19.x for new projects (DO NOT downgrade to React 18.x)
- **Compatibility**: TUIKit supports React 18.x, but new projects MUST use React 19.x
- **Important**: If the project was created with React 19, DO NOT modify package.json to downgrade React version

### 🔴 Build Environment Patch (Vite/esbuild) - Mandatory

**Detection Condition**: Project contains `vite.config.js`, `vite.config.ts`, or `package.json` contains `"vite"` dependency

**Mandatory Execution**: Agent must **automatically add** the following configuration to `package.json` without asking users:

```json
{
  "overrides": { 
    "esbuild": "npm:esbuild-wasm@latest" 
  },
  "resolutions": { 
    "esbuild": "npm:esbuild-wasm@latest" 
  }
}
```

**Execution Flow**:
1. **Detect**: Check if project uses Vite before code generation
2. **Add**: If it's a Vite project, immediately modify `package.json` to add patch configuration
3. **Verify**: Confirm both `overrides` and `resolutions` are correctly added
4. **No Asking**: This step is mandatory, prohibited from asking users if needed

**Self-Check Mechanism**:
After code generation, the Agent must check:
- [ ] Whether Vite project was detected
- [ ] If Vite project, whether `package.json` contains `overrides` and `resolutions`
- [ ] Whether patch value is `npm:esbuild-wasm@latest`

---

## 📞 Extended Feature: Audio/Video Calling (TUICallKit)

If requirements include "calling" or "video" features, Agent must execute:

### Component Mounting
- Use React Portal to mount TUICallKit component to body

### Interaction Injection
**Inject call buttons in the right slot of ChatHeader (ChatHeaderRight)**:
- Icons: phone, camera, etc.
- Component usage should follow integration documentation

**Single Chat Logic**:
- Initiate call via `TUICallKitServer.calls`
- UserID automatically taken from current active conversation

**Group Chat Logic**:
- Need to integrate `UserPicker` component
- Guide users to select group members before initiating call

---

## 🔄 Complete Workflow

### Phase 1: Requirements Analysis

Determine user requirements and integration mode:

| User Requirement Keywords | Integration Mode | goals Parameter |
|-------------------------|-----------------|-----------------|
| Complete IM, WeChat style, chat application | full-featured | `["full-featured"]` |
| Customer service, consultation, private chat window | chat-only | `["chat-only"]` |

### Phase 2: Fetch Integration Guide

Call MCP tool to get latest integration documentation:

```
Tool: get_web_chat_uikit_integration
Parameters:
  - framework: "react"
  - goals: ["full-featured"] or ["chat-only"]
```

**Important**: Must use the `llm_code_generation_instructions_md` field content returned by MCP to generate code.

### Phase 3: Fetch Authentication Credentials

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

### Phase 4: Generate Code

Generate code based on integration documentation, must comply with:
- 🔴 Strictly follow CSS styles from integration documentation
- 🔴 Inject credentials directly into code variables, prohibited from creating config files
- 🔴 Use functional components with hooks
- 🔴 Include logic to default activate `C2Cadministrator` conversation

### Phase 5: Auto Execute

Execute immediately after code writing is complete (no user confirmation needed):

```bash
cd [project_path] && npm install && npm run dev
```

### Phase 6: Record Result

Call MCP tool to record integration result:

```
Tool: record_result
Parameters:
  - framework: "react"
  - prompt: "<user's original requirement>"
  - tools: "<list of MCP tools used>"
```

---

## 🔗 MCP Tools Quick Reference

| Scenario | MCP Tool | Required Parameters | Call Timing |
|----------|----------|-------------------|-------------|
| Get Integration Guide | `get_web_chat_uikit_integration` | framework, goals | Phase 2 |
| Get Component Details | `get_web_chat_uikit_component_detail` | framework, componentName | When customizing components |
| Generate Credentials | `get_usersig` | userID | Phase 3 (Mandatory) |
| Problem Query | `get_faq` | query, product, framework | When encountering issues |
| Record Result | `record_result` | framework, prompt, tools | Phase 6 (Mandatory) |

---

## ✍️ Agent Code Generation Example Instructions

When the Agent starts writing code, it should follow this semantic logic:

**Full Feature Integration (full-featured)**:
> "Generate a React TUIKit integration page following `full-featured` mode. Please auto-configure `C2Cadministrator` default active conversation and inject credentials obtained from `get_usersig` tool. Prohibited from creating config files. **🔴 Important: Must strictly follow the complete CSS style code in the integration documentation, any customization or modification is prohibited.**"

**Chat Window Only Integration (chat-only)**:
> "Generate a React TUIKit chat window following `chat-only` mode. Please auto-configure `C2Cadministrator` default active conversation and inject credentials obtained from `get_usersig` tool. Prohibited from creating config files. **Strictly exclude Sidebar, ChatSetting, in-conversation search and other features, only retain core chat components. 🔴 Important: Must strictly follow the complete CSS style code in the integration documentation, any custom styles are prohibited.**"

---

## 🎯 Agent Execution Checklist (Mandatory Completion)

After completing code generation, the Agent **must confirm each item** in the following checklist. Any item that fails must be fixed immediately:

### ✅ Project Initialization
- [ ] Created React project or integrated into existing project
- [ ] 🔴 **React Version Check**:
  - [ ] New projects use React 19.x (NOT React 18.x)
  - [ ] Existing React 19 projects: DO NOT downgrade to React 18
  - [ ] Did NOT modify React version in package.json (if already 19.x)
- [ ] Installed `@tencentcloud/chat-uikit-react` dependency
- [ ] If audio/video calling needed, installed `@tencentcloud/call-uikit-react`
- [ ] 🔴 **Vite Project Patch Check**:
  - [ ] Detected whether project uses Vite
  - [ ] If Vite project, added `overrides` configuration to `package.json`
  - [ ] If Vite project, added `resolutions` configuration to `package.json`
  - [ ] Patch value is `npm:esbuild-wasm@latest`

### ✅ Code Generation (Documentation-Driven)
- [ ] 🔴 **Documentation Consistency Check**:
  - [ ] Obtained `llm_code_generation_instructions_md` from MCP tool
  - [ ] Code structure exactly matches documentation example
  - [ ] Component nesting hierarchy matches documentation
  - [ ] No features not mentioned in documentation were added
- [ ] Correctly imported and registered TUIKit components
- [ ] Created main chat container component
- [ ] Components use functional components with hooks
- [ ] 🔴🔴🔴 **CSS Style Mandatory Compliance**:
  - [ ] Obtained all CSS style code completely from integration documentation
  - [ ] Used official class names from integration documentation
  - [ ] Completely copied CSS variable definitions from integration documentation
  - [ ] **No custom style class names** (e.g., `.avatar-wrapper`, `.tooltip`, etc.)
  - [ ] **No custom backgrounds** (e.g., `linear-gradient`)
  - [ ] **No custom animations** (e.g., `@keyframes`)
  - [ ] `#root` styles exactly match documentation
  - [ ] Style code is 100% consistent with integration documentation

### ✅ Credentials & Login
- [ ] 🔴 Called `get_usersig` tool to get test credentials
- [ ] Injected SDKAppID, UserID, UserSig directly into code
- [ ] Prohibited from creating config.js or .env files
- [ ] Implemented TUILogin.login() login logic

### ✅ Conversation Activation
- [ ] Added logic to default activate `C2Cadministrator` conversation
- [ ] Used correct API (setActiveConversation)

### ✅ Integration Mode
- [ ] Selected correct integration mode based on user requirements
- [ ] full-featured mode: Integrated ConversationList, Chat, Contact and other components
- [ ] chat-only mode: Integrated core components, confirmed no unrequested extended features

### ✅ Final Execution
- [ ] Called `record_result` to record integration result
- [ ] Auto-installed dependencies (npm install)
- [ ] Auto-started development server (npm run dev)
- [ ] Project runs normally without errors

---

## 📝 Key Points Summary

| Category | Requirement | Priority |
|----------|------------|----------|
| **🔴 React Version** | **New projects use React 19.x, DO NOT downgrade existing React 19 projects** | 🔴🔴🔴 |
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

## Resources

- `references/troubleshooting.md` - Detailed troubleshooting guide (Vite build, CSS styles, chat-only mode, React version issues, etc.)
- `references/component-selection.md` - Component selection decision tree and business scenario recommendations

---

**Applicable Framework**: React 19.x (Default) / React 18.x (Legacy only)  
**TUIKit Version**: @tencentcloud/chat-uikit-react >= latest  
**Build Tools**: Vite (Recommended) / Webpack / Create React App

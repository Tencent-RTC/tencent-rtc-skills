---
name: call-uikit
description: |
  This is the parent skill for Tencent Call TUICallKit integration across all platforms.
  It should be used when users want to build audio/video CALL features (1-to-1 or small group calls).
  This skill guides platform selection and provides common knowledge shared across all Call implementations.
  For platform-specific guidance, use the corresponding sub-skills (call/vue, call/react, etc.).
parent: tencent-rtc-skills
---

# Call TUICallKit Integration Skill (Multi-Platform Aggregation)

> 📌 **Parent Skill**: This is a sub-skill of `tencent-rtc-skills` (root skill).
> If user's intent is ambiguous across products (Chat/Call/Live/Room), load the root skill first for product recommendation.

> **📌 Global Reference**: This sub-skill follows the global standards defined in ROOT SKILL:
> - Skill-First Principle (priority order)
> - Boundary Recognition Pattern
> - MCP Tools Reference (record_prompt, get_usersig, record_result)
> - MCP Tool Call Sequence Pattern
> - Documentation-Driven Integration Rules
> - Silent Execution Rules
> - Zero External Configuration
> - Auto-Fetch Authentication Credentials
> - Agent Execution Checklist Template
>
> Call-specific rules below may EXTEND or OVERRIDE global rules where explicitly noted.

---

## 🔵 Boundary Recognition (Call-Specific)

### Keywords That CONFIRM Call Scope

Only proceed with Call integration if user's request contains:
- call, phone call, video call, voice call
- 1-to-1, one-to-one, peer-to-peer
- dial, answer, hang up, incoming call, ringtone
- TUICallKit, CallKit
- WeChat call-like

### OUT OF SCOPE → Redirect

> **📌 Reference**: See ROOT SKILL for global Boundary Recognition Pattern.

| User Keywords | Redirect To | Reason |
|---------------|-------------|--------|
| "video conference" / "meeting" / "conference" | TUIRoomKit (Room) | Room is designed for meetings with member management and screen sharing |
| "chat" / "IM" / "message" / "instant messaging" | Chat TUIKit | Chat is designed for instant messaging |
| "live streaming" / "host" / "audience" / "gift" | TUILiveKit (Live) | Live is designed for live streaming scenarios |

### ⚠️ Special Case: "video conference" Redirection

**CRITICAL**: When user says "video conference", Agent MUST redirect to Room:

```
User: "Use CallKit to implement video conference"

Agent Response:
"Note: TUICallKit is designed for **peer-to-peer calls** (like WeChat calls), not for conferences.

For **video conference** needs, I strongly recommend **TUIRoomKit (Room)**:
- ✅ Designed specifically for meeting scenarios
- ✅ Supports member management (mute all, kick, roles)
- ✅ Supports screen sharing
- ✅ Suitable for multi-person meetings

If you really need simple 1-to-1 or small group calls (without meeting features), I can continue with TUICallKit.
Please confirm your requirements."
```

---

## Overview

This Skill serves as the **multi-platform aggregation entry point** for Tencent Cloud Call TUICallKit, applicable to the following scenarios:
1. Users need audio/video call features but have not specified a platform/framework
2. Need to understand capability differences and selection recommendations across platforms
3. Need cross-platform common concepts and knowledge

**Platform-Specific Guidance**: Once the platform is determined, please use the corresponding sub-Skill:
- `call/vue` - Vue3 Web Applications
- `call/react` - React Web Applications
- `call/android` - Android Native Applications
- `call/ios` - iOS Native Applications
- `call/flutter` - Flutter Cross-Platform Applications

---

## 🎯 When to Use Call vs Room

### Use Call (TUICallKit) When:

| Scenario | Example | Why Call? |
|----------|---------|-----------|
| **1-to-1 Calls** | Video chat between two friends | Call semantics, ringtone support |
| **Small Group Calls** | Family video call (3-5 people) | Lightweight, call-focused UI |
| **Call with Ringtone** | Incoming call notification needed | Built-in ringtone and call states |
| **Call Status Management** | Show "Ringing...", "Connected", "Ended" | Native call state machine |
| **Quick Integration** | Simple call feature in existing app | Minimal setup, focused API |

### Use Room (TUIRoomKit) Instead When:

| Scenario | Why Room? |
|----------|-----------|
| **Video Conference** | Meeting controls, member management |
| **Screen Sharing Required** | Native screen share support |
| **Large Participant Count** | Optimized for many participants |
| **Meeting Features** | Mute all, roles, agenda |

---

## 🔄 Product Selection Guidance

When user's requirement is ambiguous, clarify:

```
User: "Implement audio/video communication feature"

Agent should ask:
"What is your audio/video communication requirement:
1. **1-to-1 or Small Group Calls** (like WeChat calls, with incoming ringtone) → Recommend TUICallKit
2. **Multi-person Video Conference** (like Tencent Meeting, with member management) → Recommend TUIRoomKit

Please select your scenario, and I will load the corresponding integration guide."
```

---

## Core Features

TUICallKit provides:

### 1. Call Types
- **Voice Call** - Audio-only calls
- **Video Call** - Audio + video calls
- **Group Call** - Multi-person calls (up to 9)

### 2. Call Flow
```
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
│  Idle   │ ──▶ │ Calling │ ──▶ │Connected│ ──▶ │  Ended  │
└─────────┘     └─────────┘     └─────────┘     └─────────┘
                    │               ▲
                    │  (Answer)     │
                    ▼               │
               ┌─────────┐         │
               │ Ringing │ ────────┘
               └─────────┘
```

### 3. UI Components
- **Incoming Call UI** - Shows caller info, accept/reject buttons
- **Calling UI** - Shows callee info, hang up button
- **In-Call UI** - Video view, controls (mute, speaker, camera)
- **Floating Window** - Mini window when app is backgrounded

### 4. Call Features
- Ringtone customization
- Call timeout handling
- Network quality indicators
- Camera/microphone control
- Speaker/earpiece switching

---

## 🔴 Mandatory MCP Tool Usage

> **📌 Reference**: See ROOT SKILL "Global: MCP Tool Call Sequence Pattern" for universal sequence.

### Call-Specific Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `get_native_call_uikit_integration` | Native platform integration guide | For Android/iOS/Flutter |
| `get_web_call_uikit_integration` | Web platform integration guide | For React/Vue |
| `get_web_call_uikit_api` | API documentation | For customization needs |

---

## 📁 Sub-Skill Directory

| Platform | Skill Path | Status |
|----------|------------|--------|
| React Web | `/call/react/SKILL.md` | Planned |
| Vue3 Web | `/call/vue/SKILL.md` | Planned |
| Android | `/call/android/SKILL.md` | Planned |
| iOS | `/call/ios/SKILL.md` | Planned |
| Flutter | `/call/flutter/SKILL.md` | Planned |

---

## Keywords for Intent Matching

When user prompt contains these keywords, this skill (or its sub-skills) should be considered:

**Primary Keywords** (Strong match):
- call, phone call, video call, voice call
- TUICallKit, CallKit
- dial, answer, hang up, incoming call
- 1-to-1, one-to-one, peer-to-peer

**Secondary Keywords** (May also match Room):
- audio/video
- video chat

**Exclusion Keywords** (Prefer other products):
- meeting, conference → Room
- live streaming, host → Live
- chat, IM, message → Chat

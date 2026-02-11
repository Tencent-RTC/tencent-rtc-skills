---
name: live-uikit
description: |
  This is the parent skill for Tencent Live TUILiveKit integration across all platforms.
  It should be used when users want to build LIVE STREAMING features (host broadcasting, audience viewing).
  This skill guides platform selection and provides common knowledge shared across all Live implementations.
  For platform-specific guidance, use the corresponding sub-skills (live/vue, live/android, etc.).
parent: tencent-rtc-skills
---

# Live TUILiveKit Integration Skill (Multi-Platform Aggregation)

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
> Live-specific rules below may EXTEND or OVERRIDE global rules where explicitly noted.

---

## 🔵 Boundary Recognition (Live-Specific)

### Keywords That CONFIRM Live Scope

Only proceed with Live integration if user's request contains:
- live streaming, broadcast
- host, audience
- co-host, take seat, leave seat
- gift, tip
- barrage, comment
- PK, battle
- voice chat room, voice room, seat management
- TUILiveKit, LiveKit

### OUT OF SCOPE → Redirect

> **📌 Reference**: See ROOT SKILL for global Boundary Recognition Pattern.

| User Keywords | Redirect To | Reason |
|---------------|-------------|--------|
| "video conference" / "meeting" / "conference" | TUIRoomKit (Room) | Room is designed for meeting scenarios |
| "call" / "phone call" / "1-to-1" | TUICallKit (Call) | Call is designed for call scenarios |
| "chat" / "IM" / "message" (without live context) | Chat TUIKit | Chat is designed for instant messaging |

---

## Overview

This Skill serves as the **multi-platform aggregation entry point** for Tencent Cloud Live TUILiveKit, applicable to the following scenarios:
1. Users need live streaming features but have not specified a platform/framework
2. Need to understand capability differences and selection recommendations across platforms
3. Need cross-platform common concepts and knowledge

**Platform-Specific Guidance**: Once the platform is determined, please use the corresponding sub-Skill:
- `live/vue` - Vue3 Web Applications
- `live/android` - Android Native Applications
- `live/ios` - iOS Native Applications
- `live/flutter` - Flutter Cross-Platform Applications

---

## 🎯 When to Use Live vs Room

### Use Live (TUILiveKit) When:

| Scenario | Example | Why Live? |
|----------|---------|-----------|
| **Live Streaming** | Influencer broadcasting | Host/audience role separation |
| **Interactive Live** | Gaming live stream | Gifts, barrage, reactions |
| **E-commerce Live** | Product showcase | Co-hosting, product links |
| **Voice Chat Room** | Audio-only live room | Seat management, audio effects |
| **Co-hosting** | Guest joins host | Built-in co-host flow |
| **PK Battle** | Two hosts compete | Cross-room PK support |

### Use Room (TUIRoomKit) Instead When:

| Scenario | Why Room? |
|----------|-----------|
| **Webinar** | Presenter + many attendees, Q&A focus |
| **Video Conference** | Peer-to-peer collaboration |
| **Online Classroom** | Teacher-student interaction, screen share |

---

## Core Features

TUILiveKit provides two main scenarios:

### 1. Video Live
```
┌─────────────────────────────────────────────────────────────┐
│                        VIDEO LIVE                           │
│  ┌─────────────┐              ┌─────────────────────────┐  │
│  │    HOST     │    ──▶      │       AUDIENCE          │  │
│  │  (1 host)   │   Stream    │    (Many viewers)       │  │
│  └─────────────┘              └─────────────────────────┘  │
│         │                              │                    │
│         │    Co-host                   │                    │
│         ▼                              ▼                    │
│  ┌─────────────┐              ┌─────────────────────────┐  │
│  │   Gifts     │              │   Barrage/Comments      │  │
│  │   Beauty    │              │   Like/Reactions        │  │
│  │   PK Battle │              │   Follow                │  │
│  └─────────────┘              └─────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### 2. Voice Chat Room
```
┌─────────────────────────────────────────────────────────────┐
│                     VOICE CHAT ROOM                         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                    SEATS                             │   │
│  │  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐  │   │
│  │  │ 🎤 │ │ 🎤 │ │ 🎤 │ │ 🎤 │ │ 🎤 │ │ 🎤 │ │ 🎤 │  │   │
│  │  │Host│ │Guest│ │Guest│ │Empty│ │Empty│ │Empty│ │Empty│  │   │
│  │  └────┘ └────┘ └────┘ └────┘ └────┘ └────┘ └────┘  │   │
│  └─────────────────────────────────────────────────────┘   │
│                           │                                 │
│                           ▼                                 │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              AUDIENCE (listeners)                    │   │
│  │  - Request to take seat                             │   │
│  │  - Send gifts, barrage                              │   │
│  │  - Audio effects, voice changer                     │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔄 Scenario Selection

When user's live requirement is ambiguous, clarify:

```
User: "Implement live streaming feature"

Agent should ask:
"Your live streaming requirement is:
1. **Video Live** (host on camera, audience watching) → Video Live
2. **Voice Chat Room** (multi-person voice interaction, seat management) → Voice Chat Room

Please select your scenario, and I will load the corresponding integration guide."
```

---

## Core Capabilities

### Basic Features
| Feature | Video Live | Voice Room |
|---------|-----------|------------|
| Host streaming | ✅ | ✅ |
| Audience viewing | ✅ | ✅ |
| Room list | ✅ | ✅ |
| Login/Auth | ✅ | ✅ |

### Interactive Features
| Feature | Video Live | Voice Room |
|---------|-----------|------------|
| Co-hosting | ✅ Video | ✅ Audio seats |
| PK Battle | ✅ | ✅ |
| Audience management | ✅ | ✅ |

### Extra Features
| Feature | Video Live | Voice Room |
|---------|-----------|------------|
| Beauty filters | ✅ | - |
| Audio effects | ✅ | ✅ |
| Barrage/Comments | ✅ | ✅ |
| Gifts | ✅ | ✅ |

---

## 🔴 Mandatory MCP Tool Usage

> **📌 Reference**: See ROOT SKILL "Global: MCP Tool Call Sequence Pattern" for universal sequence.

### Live-Specific Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `present_scenario_choice` | Clarify video live vs voice room | When scenario unclear |
| `get_native_live_uikit_integration` | Basic features (host/audience/list) | For Android/iOS/Flutter |
| `get_native_live_uikit_interaction` | Interactive features (co-host/PK) | For advanced interactions |
| `get_native_live_uikit_extra_features` | Extra features (beauty/gifts) | For enhanced UX |
| `get_web_live_uikit_integration` | Web platform integration | For Vue web |

### Live-Specific Sequence Note

Live has an **additional step** in the sequence for scenario selection:

```
Standard Sequence with Live-Specific Step:
1. record_prompt
2. present_framework_choice (if platform unclear)
3. 🔴 present_scenario_choice (video live vs voice room) ← LIVE-SPECIFIC
4. get_usersig
5. get_*_live_uikit_integration → interaction → extra_features
6. [Generate code]
7. record_result
```

---

## 📁 Sub-Skill Directory

| Platform | Skill Path | Status |
|----------|------------|--------|
| Vue3 Web | `/live/vue/SKILL.md` | Planned |
| Android | `/live/android/SKILL.md` | Planned |
| iOS | `/live/ios/SKILL.md` | Planned |
| Flutter | `/live/flutter/SKILL.md` | Planned |

---

## Keywords for Intent Matching

When user prompt contains these keywords, this skill (or its sub-skills) should be considered:

**Primary Keywords** (Strong match):
- live streaming, broadcast
- TUILiveKit, LiveKit
- host, audience
- co-host, take seat
- gift, tip
- barrage, comment
- PK, battle
- voice chat room, voice room, seat management

**Secondary Keywords** (May also match Room):
- interactive
- real-time

**Exclusion Keywords** (Prefer other products):
- meeting, conference → Room
- call, phone call, 1-to-1 → Call
- chat, IM, message → Chat

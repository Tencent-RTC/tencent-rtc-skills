---
name: room-uikit
description: |
  This is the parent skill for Tencent Room TUIRoomKit integration across all platforms.
  It should be used when users want to build VIDEO CONFERENCE or MEETING features.
  This skill guides platform selection and provides common knowledge shared across all Room implementations.
  For platform-specific guidance, use the corresponding sub-skills (room/vue, room/react, etc.).
parent: tencent-rtc-skills
---

# Room TUIRoomKit Integration Skill (Multi-Platform Aggregation)

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
> Room-specific rules below may EXTEND or OVERRIDE global rules where explicitly noted.

---

## 🔵 Boundary Recognition (Room-Specific)

### Keywords That CONFIRM Room Scope

Only proceed with Room integration if user's request contains:
- video conference, meeting
- multi-party meeting, conference room
- screen sharing
- TUIRoomKit, RoomKit
- Tencent Meeting-like
- online classroom

### OUT OF SCOPE → Redirect

> **📌 Reference**: See ROOT SKILL for global Boundary Recognition Pattern.

| User Keywords | Redirect To | Reason |
|---------------|-------------|--------|
| "chat" / "IM" / "message" / "instant messaging" | Chat TUIKit | Chat is designed for instant messaging |
| "1-to-1 call" / "phone call" / "incoming call ringtone" | TUICallKit (Call) | Call is designed for peer-to-peer calls |
| "live streaming" / "host" / "gift" / "barrage" | TUILiveKit (Live) | Live is designed for live streaming scenarios |

---

## 🔴 KEY POSITIONING: Room is the FIRST CHOICE for Video Conferences

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  ⚠️ IMPORTANT: PRODUCT RECOMMENDATION PRIORITY                               │
│                                                                              │
│  When user says "video conference" / "meeting" / "conference call":          │
│                                                                              │
│  🥇 FIRST CHOICE: Room (TUIRoomKit) ← THIS PRODUCT                           │
│  🥈 SECOND CHOICE: Call (TUICallKit) - Only if Room doesn't fit              │
│                                                                              │
│  Room is PURPOSE-BUILT for conference scenarios:                             │
│  - Complete meeting UI out of the box                                        │
│  - Member management (mute all, kick, roles)                                 │
│  - Screen sharing natively supported                                         │
│  - Handles large participant counts                                          │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## Overview

This Skill serves as the **multi-platform aggregation entry point** for Tencent Cloud Room TUIRoomKit, applicable to the following scenarios:
1. Users need video conference/meeting features but have not specified a platform/framework
2. Need to understand capability differences and selection recommendations across platforms
3. Need cross-platform common concepts and knowledge

**Platform-Specific Guidance**: Once the platform is determined, please use the corresponding sub-Skill:
- `room/vue` - Vue3 Web Applications (PC + H5)
- `room/react` - React Web Applications
- `room/android` - Android Native Applications
- `room/ios` - iOS Native Applications
- `room/flutter` - Flutter Cross-Platform Applications

---

## 🎯 When to Use Room vs Other Products

### Use Room (TUIRoomKit) When:

| Scenario | Example | Why Room? |
|----------|---------|-----------|
| **Video Conference** | Team meeting, board meeting | Full meeting UI, controls |
| **Online Meeting** | Client meeting, interview | Screen sharing, member mgmt |
| **Webinar** | Company all-hands, training | Presenter mode, Q&A |
| **Online Classroom** | Virtual class, tutoring | Teacher controls, hand raise |
| **Multi-person Collaboration** | Design review, code review | Screen share, annotations |

### Use Call (TUICallKit) Instead When:

| Scenario | Why Call? |
|----------|-----------|
| **1-to-1 Call** | Simpler, call-focused UI |
| **Simple Video Chat** | Ringtone, call states |
| **Quick Call Feature** | Minimal integration |

### Use Live (TUILiveKit) Instead When:

| Scenario | Why Live? |
|----------|-----------|
| **Live Streaming** | Host/audience separation |
| **Interactive Live** | Gifts, barrage, reactions |
| **Voice Chat Room** | Seat management |

---

## Core Features

TUIRoomKit provides a complete video conference solution:

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           TUIRoomKit Architecture                               │
│                                                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐   │
│  │                         CONFERENCE ROOM                                  │   │
│  │  ┌───────────────────────────────────────────────────────────────────┐  │   │
│  │  │                        TOP BAR                                     │  │   │
│  │  │  Room Name | Duration | Network | Settings                        │  │   │
│  │  └───────────────────────────────────────────────────────────────────┘  │   │
│  │                                                                          │   │
│  │  ┌─────────────────────────────────┐ ┌──────────────────────────────┐  │   │
│  │  │         VIDEO AREA              │ │       SIDE PANEL             │  │   │
│  │  │  ┌─────┐ ┌─────┐ ┌─────┐       │ │  - Member List               │  │   │
│  │  │  │ 👤  │ │ 👤  │ │ 👤  │       │ │  - Chat Messages             │  │   │
│  │  │  └─────┘ └─────┘ └─────┘       │ │  - Settings                  │  │   │
│  │  │  ┌─────┐ ┌─────┐ ┌─────┐       │ │                              │  │   │
│  │  │  │ 👤  │ │ 👤  │ │ 👤  │       │ │                              │  │   │
│  │  │  └─────┘ └─────┘ └─────┘       │ │                              │  │   │
│  │  │                                 │ │                              │  │   │
│  │  │  Grid / Sidebar / Speaker View  │ │                              │  │   │
│  │  └─────────────────────────────────┘ └──────────────────────────────┘  │   │
│  │                                                                          │   │
│  │  ┌───────────────────────────────────────────────────────────────────┐  │   │
│  │  │                       CONTROL BAR                                  │  │   │
│  │  │  🎤 Mic | 📷 Camera | 🖥️ Share | 👥 Members | 💬 Chat | ❌ Leave   │  │   │
│  │  └───────────────────────────────────────────────────────────────────┘  │   │
│  └──────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Feature Categories

#### 1. Pre-Meeting Features
| Feature | Description |
|---------|-------------|
| **Device Detection** | Test camera, mic, speaker before joining |
| **Room Creation** | Create room with settings |
| **Room Joining** | Join existing room by ID |
| **Scheduled Meeting** | Schedule future meetings |

#### 2. In-Meeting Features
| Feature | Description |
|---------|-------------|
| **Video Views** | Grid, sidebar, speaker layouts |
| **Screen Sharing** | Share screen/window/tab |
| **Member Management** | Mute, kick, promote to host |
| **In-Meeting Chat** | Text messages during meeting |
| **Virtual Background** | Blur or replace background |
| **Beauty Filters** | Skin smooth, whitening |

#### 3. Meeting Controls
| Feature | Description |
|---------|-------------|
| **Mute All** | Host mutes all participants |
| **Disable Video All** | Host disables all cameras |
| **Lock Room** | Prevent new participants |
| **Hand Raise** | Request to speak |
| **Recording** | Record meeting (if enabled) |

---

## 🔄 Integration Approaches

TUIRoomKit offers two integration approaches:

### 1. Pre-built UI (Recommended for Quick Start)
- Use complete conference pages directly
- Minimal customization needed
- Fastest time to production

### 2. Custom UI (For Advanced Needs)
- Use atomic components and State APIs
- Full control over UI/UX
- Requires more development effort

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│  Integration Approach Decision:                                                 │
│                                                                                 │
│  User says: "quickly implement meeting feature" → Pre-built UI (get_web_room_uikit_integration)  │
│  User says: "customize meeting interface" → Custom UI (get_web_room_feature_detail)          │
│                                                                                 │
│  DEFAULT: Pre-built UI (unless user explicitly requests customization)          │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔴 Mandatory MCP Tool Usage

> **📌 Reference**: See ROOT SKILL "Global: MCP Tool Call Sequence Pattern" for universal sequence.

### Room-Specific Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `get_web_room_uikit_integration` | Pre-built UI integration | For quick start (DEFAULT) |
| `get_web_room_feature_detail` | Custom UI with atomic APIs | For customization needs |
| `get_faq` | Troubleshooting | When errors occur |

### Tool Selection Logic

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│  MCP Tool Selection for Room:                                                   │
│                                                                                 │
│  IF user wants:                                                                 │
│    - "video conference", "meeting", "conference" (general)                      │
│    - "quick integration", "quick start"                                         │
│    - "full meeting features"                                                    │
│  THEN: Use get_web_room_uikit_integration (Pre-built UI)                        │
│                                                                                 │
│  IF user wants:                                                                 │
│    - "custom UI", "custom interface"                                            │
│    - "individual feature module" (screen share only, device detection only)     │
│    - "deep customization"                                                       │
│  THEN: Use get_web_room_feature_detail (Custom UI)                              │
│                                                                                 │
│  DEFAULT: get_web_room_uikit_integration                                        │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📁 Sub-Skill Directory

| Platform | Skill Path | Status |
|----------|------------|--------|
| Vue3 Web | `/room/vue/SKILL.md` | Planned |
| React Web | `/room/react/SKILL.md` | Planned |
| Android | `/room/android/SKILL.md` | Planned |
| iOS | `/room/ios/SKILL.md` | Planned |
| Flutter | `/room/flutter/SKILL.md` | Planned |

---

## Keywords for Intent Matching

When user prompt contains these keywords, this skill (or its sub-skills) should be considered:

**Primary Keywords** (Strong match - ROOM FIRST):
- video conference, meeting, conference
- TUIRoomKit, RoomKit
- meeting room, conference room
- multi-person meeting, group meeting
- online meeting, virtual meeting
- screen sharing, screen share
- member management, participant management
- similar to Tencent Meeting, Zoom-like

**Secondary Keywords** (Room is strong candidate):
- collaboration, collaborative
- classroom, online education, e-learning
- training, webinar, workshop

**Exclusion Keywords** (Prefer other products):
- live, streaming, broadcaster, host → Live
- call, phone call, 1-to-1, one-on-one → Call
- chat, IM, messaging, conversation → Chat
- gift, barrage, comment → Live

---

## 🎯 Example: Why Room for "video conference"

```
User: "Help me implement a video conference feature"

Why Room (TUIRoomKit) is the RIGHT choice:

✅ Meeting UI: Complete conference interface with all controls
✅ Screen Share: Native support for sharing screen/window
✅ Member Mgmt: Mute all, kick, assign roles
✅ Large Meetings: Optimized for many participants
✅ Meeting Features: Hand raise, chat, recording

Why Call (TUICallKit) is NOT the right choice:

❌ Call is for 1-to-1 or small group CALLS
❌ Call focuses on ringtone, call states
❌ Call doesn't have meeting controls
❌ Call is not optimized for conferences
```

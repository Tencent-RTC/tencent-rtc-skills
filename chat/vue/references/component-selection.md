# Vue Chat TUIKit Component Selection Guide

This document helps developers choose the correct components and integration methods based on business requirements.

## Decision Flow Chart

```
                    ┌─────────────────────────────┐
                    │  What type of chat feature  │
                    │        do you need?         │
                    └─────────────────────────────┘
                                  │
                ┌─────────────────┼─────────────────┐
                ▼                 ▼                 ▼
        ┌───────────┐     ┌───────────┐     ┌───────────┐
        │ Complete  │     │ Embedded  │     │  Highly   │
        │  IM App   │     │   Chat    │     │Customized │
        └───────────┘     └───────────┘     └───────────┘
                │                 │                 │
                ▼                 ▼                 ▼
        ┌───────────┐     ┌───────────┐     ┌───────────┐
        │ full-     │     │ chat-only │     │ Composite │
        │ featured  │     │           │     │Integration│
        └───────────┘     └───────────┘     └───────────┘
                │                 │                 │
                ▼                 ▼                 ▼
        ┌───────────┐     ┌───────────┐     ┌───────────┐
        │ <TUIKit/> │     │ <Chat/>   │     │ Individual│
        │Full Suite │     │Chat Window│     │ Components│
        └───────────┘     └───────────┘     └───────────┘
```

## Scenario Comparison Table

### Scenario 1: Complete Instant Messaging Application

**Characteristics**:
- Need conversation list + chat window
- Need contacts/friend management
- Standalone chat module or Tab

**Recommended Solution**: `full-featured` mode

**MCP Call**:
```
Tool: get_web_chat_uikit_integration
Parameters: { framework: "vue", goals: ["full-featured"] }
```

**Applicable Cases**:
- Social Apps
- Enterprise Internal Communication Tools
- WeChat/WeCom-like Applications
- Online Education Platforms (Teacher-Student Communication)

---

### Scenario 2: Embedded Customer Service/Consultation Window

**Characteristics**:
- Only need chat window
- Embed into existing pages
- No conversation list needed

**Recommended Solution**: `chat-only` mode

**MCP Call**:
```
Tool: get_web_chat_uikit_integration
Parameters: { framework: "vue", goals: ["chat-only"] }
```

**Applicable Cases**:
- E-commerce Customer Service Window
- Order Detail Page Private Chat
- Online Medical Consultation
- Technical Support Dialog

---

### Scenario 3: Highly Customized Interface

**Characteristics**:
- Need complete control over UI styles
- Need custom interaction logic
- Only use partial features

**Recommended Solution**: Composite Integration (use individual components)

**MCP Call**:
```
Tool: get_web_chat_uikit_component_detail
Parameters: { framework: "vue", componentName: "<specific_component>" }
```

**Component Combination Examples**:

| Requirement | Component Combination |
|-------------|----------------------|
| Custom message list style | `message-list` + `message-input` |
| Custom conversation list entry | `conversation-list` + custom chat component |
| Embedded search feature | `search` |
| Custom avatar display | `avatar` |

---

## Component Feature Comparison

### Core Components

| Component | Function | When to Use |
|-----------|----------|-------------|
| `tuikit` | Complete UIKit Suite | Quick integration of full features |
| `chat` | Complete Chat Window | Need standard chat interface |
| `conversation-list` | Conversation List | Need to display all conversations |

### Fine-Grained Components

| Component | Function | When to Use |
|-----------|----------|-------------|
| `chat-header` | Chat Header Navigation | Customize header style/features |
| `message-list` | Message List Area | Customize message display |
| `message-input` | Message Input Box | Customize input area |
| `contact-list` | Contact List | Friend/Group Management |
| `search` | Search Component | Global search feature |
| `chat-setting` | Conversation Settings | DND/Pin settings, etc. |
| `avatar` | Avatar Component | Customize avatar display |

---

## Recommendations by Business Scenario

### E-commerce Platform

```
Recommended Components:
├── Buyer Side: chat-only mode
│   └── Embed TUIChat component in order page
│
└── Seller Side: full-featured mode
    └── Standalone customer service workstation
```

### Online Education

```
Recommended Components:
├── Student Side: 
│   ├── conversation-list (course group list)
│   └── chat (course group chat)
│
└── Teacher Side:
    ├── conversation-list (class group list)
    ├── chat (group chat/private chat)
    └── contact-list (student management)
```

### Medical Consultation

```
Recommended Components:
├── Patient Side: chat-only mode
│   └── Consultation window
│
└── Doctor Side: 
    ├── conversation-list (patient list)
    ├── chat (consultation dialogue)
    └── chat-setting (conversation management)
```

### Enterprise Internal Communication

```
Recommended Components:
└── full-featured mode
    ├── conversation-list (conversation list)
    ├── chat (chat window)
    ├── contact-list (contacts)
    └── search (global search)
```

---

## Component Dependency Relationships

```
TUIKit (Complete Suite)
├── ConversationList (Conversation List)
│   └── Avatar
├── Chat (Chat Window)
│   ├── ChatHeader (Header)
│   │   └── Avatar
│   ├── MessageList (Message List)
│   │   └── Avatar
│   └── MessageInput (Input Box)
├── ContactList (Contacts)
│   └── Avatar
├── Search
└── ChatSetting (Settings)
```

**Dependency Notes**:
- When using `Chat` component, it automatically includes `ChatHeader`, `MessageList`, `MessageInput`
- If individual customization is needed, you can import sub-components only

---

## Get Component Details

After determining needed components, get detailed documentation via MCP tool:

```
Tool: get_web_chat_uikit_component_detail
Parameters: 
  - framework: "vue"
  - componentName: "conversation-list" (or other component name)
```

**componentName Options**:
- `component-list` - Component Overview
- `tuikit` - Complete Suite
- `chat` - Chat Window
- `chat-header` - Chat Header
- `conversation-list` - Conversation List
- `message-list` - Message List
- `message-input` - Message Input
- `contact-list` - Contacts
- `search` - Search
- `chat-setting` - Chat Settings
- `avatar` - Avatar

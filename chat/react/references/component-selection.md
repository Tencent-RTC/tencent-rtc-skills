# React Chat TUIKit Component Selection Guide

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
Parameters: { framework: "react", goals: ["full-featured"] }
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
Parameters: { framework: "react", goals: ["chat-only"] }
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
Parameters: { framework: "react", componentName: "<specific_component>" }
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
│   └── Embed Chat component in order page
│
└── Seller Side: full-featured mode
    └── Standalone customer service workstation
```

### Online Education

```
Recommended Components:
├── Student Side: 
│   ├── ConversationList (course group list)
│   └── Chat (course group chat)
│
└── Teacher Side:
    ├── ConversationList (class group list)
    ├── Chat (group chat/private chat)
    └── ContactList (student management)
```

### Medical Consultation

```
Recommended Components:
├── Patient Side: chat-only mode
│   └── Consultation window
│
└── Doctor Side: 
    ├── ConversationList (patient list)
    ├── Chat (consultation dialogue)
    └── ChatSetting (conversation management)
```

### Enterprise Internal Communication

```
Recommended Components:
└── full-featured mode
    ├── ConversationList (conversation list)
    ├── Chat (chat window)
    ├── ContactList (contacts)
    └── Search (global search)
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

## React-Specific Considerations

### Hooks Usage

When using TUIKit components with React hooks:

```typescript
import { useState, useEffect } from 'react';
import { TUIConversationService } from '@tencentcloud/chat-uikit-engine';

function ChatPage() {
  const [isReady, setIsReady] = useState(false);
  
  useEffect(() => {
    // Set up conversation after login
    TUIConversationService.switchConversation('C2Cadministrator')
      .then(() => setIsReady(true));
  }, []);
  
  if (!isReady) return <div>Loading...</div>;
  
  return <Chat />;
}
```

### Context Providers

TUIKit uses React Context for state management. Ensure components are wrapped properly:

```typescript
import { TUIKit } from '@tencentcloud/chat-uikit-react';

function App() {
  return (
    <TUIKit SDKAppID={SDKAppID} userID={userID} userSig={userSig}>
      {/* All chat components must be inside TUIKit */}
      <YourChatComponents />
    </TUIKit>
  );
}
```

### Performance Optimization

For large conversation lists, consider:

```typescript
import { memo } from 'react';

// Memoize custom components to prevent unnecessary re-renders
const MemoizedConversationItem = memo(({ conversation }) => {
  return <div>{conversation.conversationID}</div>;
});
```

---

## Get Component Details

After determining needed components, get detailed documentation via MCP tool:

```
Tool: get_web_chat_uikit_component_detail
Parameters: 
  - framework: "react"
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

---

## Version Requirements

| Dependency | Required Version | Notes |
|------------|-----------------|-------|
| React | 18.2.0 | ❌ React 19 not supported |
| react-dom | 18.2.0 | Must match React version |
| Node.js | >= 16 | LTS version recommended |
| @tencentcloud/chat-uikit-react | latest | Use latest stable version |

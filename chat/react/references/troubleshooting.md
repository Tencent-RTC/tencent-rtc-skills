# React Chat TUIKit Troubleshooting Guide

This document collects in-depth troubleshooting experience beyond the MCP FAQ tool for Agent and developer reference.

---

## 1. React Version Issues 🔴

### 1.1 React 19 Compatibility Error

**Symptoms**:
```
Error: React 19 is not supported
```
or
```
Warning: Invalid hook call. Hooks can only be called inside of the body of a function component.
```

**Cause**: TUIKit currently does not support React 19

**Solution**:
When creating a new project, ensure React version is 18.2.0:

```bash
# Using Vite
npm create vite@latest my-chat-app -- --template react
cd my-chat-app

# Modify package.json to use React 18.2.0
```

Modify `package.json`:
```json
{
  "dependencies": {
    "react": "18.2.0",
    "react-dom": "18.2.0"
  }
}
```

Then reinstall dependencies:
```bash
rm -rf node_modules package-lock.json
npm install
```

### 1.2 Multiple React Instances

**Symptoms**:
```
Warning: Invalid hook call. Hooks can only be called inside of the body of a function component.
```

**Cause**: Multiple React instances in the project

**Solution**:
```bash
# Check for duplicate React installations
npm ls react

# If multiple versions exist, ensure only one version
npm dedupe
```

---

## 2. Vite Build Errors

### 2.1 esbuild Related Errors

**Symptoms**:
```
Error: The esbuild binary for the current platform is not installed
```
or
```
Error: Cannot find module 'esbuild'
```

**Cause**: Some dependency packages are incompatible with esbuild

**Solution**:
Add patch configuration to `package.json`:
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

Then reinstall dependencies:
```bash
rm -rf node_modules package-lock.json
npm install
```

### 2.2 Dependency Resolution Errors

**Symptoms**:
```
[vite] Internal server error: Failed to resolve import
```

**Solution**:
Add configuration to `vite.config.ts`:
```typescript
export default defineConfig({
  optimizeDeps: {
    include: ['@tencentcloud/chat-uikit-react']
  }
});
```

### 2.3 React Refresh Plugin Error

**Symptoms**:
```
Error: @vitejs/plugin-react cannot be used with React 19
```

**Solution**:
Ensure using React 18.2.0 and update vite plugin:
```bash
npm install @vitejs/plugin-react@latest
```

---

## 3. CSS Style Issues

### 3.1 Styles Not Matching Integration Documentation 🔴

**Symptoms**: Generated code uses custom style class names or modifies official styles

**Cause**: Agent did not strictly follow CSS styles in integration documentation when generating code

**Solution**:
1. Re-call `get_web_chat_uikit_integration` MCP tool to get latest integration documentation
2. Completely copy all CSS style code from integration documentation
3. Strictly use class names, variable names, and style properties from integration documentation
4. Prohibit any form of modification or customization

**CSS Specifications Agent Must Follow**:
- ✅ Use official class names (e.g., `.chat-layout`, `.chat-content-panel`)
- ✅ Use official CSS variables (e.g., `--bg-color-operate`, `--stroke-color-primary`)
- ✅ Completely copy official `#root` style configuration
- ❌ Prohibited from creating custom style class names
- ❌ Prohibited from modifying official style properties

### 3.2 Component Styles Not Taking Effect

**Checklist**:
1. Confirm style files have been imported
2. Check import order (TUIKit styles should come before custom styles)
3. Check if CSS Reset is overriding

**Correct Import Method**:
```typescript
// index.tsx or App.tsx - Import in order following integration documentation
import '@tencentcloud/chat-uikit-react/styles/index.css';
```

### 3.3 Style Conflicts with Other UI Frameworks

**Symptoms**: Style disorder when using Material UI / Ant Design and other frameworks

**Solution**:
```css
/* Add scope isolation on chat container */
.chat-container {
  box-sizing: border-box;
}

.chat-container * {
  box-sizing: border-box;
}
```

---

## 4. chat-only Mode Issues

### 4.1 Contains Features It Shouldn't Have

**Symptoms**: Code generated in chat-only mode contains Sidebar, ChatSetting, or in-conversation search features that user didn't request

**Cause**: Agent didn't correctly judge user requirements and arbitrarily added extended features

**Judgment Criteria**:
- ✅ **Should Add**: User explicitly states "need ChatSetting", "want to integrate search", "add settings feature", etc.
- ❌ **Should Not Add**: User only says "build chat-only chat window" without mentioning extended features

**Features Excluded by Default**:
1. Sidebar
2. ChatSetting (Chat Settings)
3. Search (In-conversation Search)
4. ConversationList (Conversation List)

**Solution**: Regenerate code, ensure extended features are not included by default

### 4.2 Conversation Activation Failed

**Symptoms**: Empty chat window in chat-only mode

**Cause**: `setActiveConversation` not set correctly

**Solution**:
Ensure code contains default conversation activation logic:
```typescript
import { TUIConversationService } from '@tencentcloud/chat-uikit-engine';

// Activate default conversation after successful login
TUIConversationService.switchConversation('C2Cadministrator');
```

---

## 5. Login & Authentication Issues

### 5.1 Login Timeout

**Symptoms**:
```
Error: Login timeout
```

**Troubleshooting Steps**:
1. Check network connection
2. Check if SDKAppID is correct
3. Check if UserSig has expired
4. Check firewall/proxy settings

### 5.2 Invalid UserSig

**Common Causes**:
- UserSig has expired (default validity is 7 days)
- UserID doesn't match the UserID used when generating UserSig
- SDKAppID mismatch

**Solution**:
Re-call `get_usersig` MCP tool to generate new credentials.

### 5.3 Credential Configuration Error

**Symptoms**: Agent created config.js or .env file

**Cause**: Violated "Zero External Configuration" rule

**Correct Approach**:
- ❌ Prohibited from creating `config.js`, `.env` and other configuration files
- ✅ Inject SDKAppID, UserID, UserSig directly into code variables

```typescript
// Correct: Use credentials directly in code
const SDKAppID = 1400000000; // Obtained from get_usersig
const userID = 'user001';
const userSig = 'eJw...'; // Obtained from get_usersig
```

---

## 6. Dependency Installation Issues

### 6.1 Node.js Version Incompatible

**Symptoms**:
```
Error: The engine "node" is incompatible with this module
```

**Solution**:
```bash
# Check Node version
node -v

# Use nvm to switch to compatible version (requires 16+)
nvm install 18
nvm use 18
```

### 6.2 Dependency Installation Failed

**Symptoms**:
```
npm ERR! peer dep missing: react@^18.0.0
```

**Solution**:
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm cache clean --force
npm install
```

### 6.3 TypeScript Type Errors

**Symptoms**:
```
Cannot find module '@tencentcloud/chat-uikit-react' or its corresponding type declarations
```

**Solution**:
Add to `tsconfig.json`:
```json
{
  "compilerOptions": {
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true
  }
}
```

---

## 7. Message Sending Issues

### 7.1 Message Send Failed

**Troubleshooting Steps**:
1. Confirm successful login
2. Check if target user/group exists
3. Check network connection
4. View console error messages

**Common Error Codes**:
| Error Code | Meaning | Solution |
|-----------|---------|----------|
| 10002 | Not Logged In | Call TUILogin.login first |
| 10007 | Rate Limit Exceeded | Reduce sending frequency |
| 20003 | Friend Relationship Verification Failed | Check friend settings |

### 7.2 Image/File Send Failed

**Common Causes**:
- File size exceeds limit (images 20MB, files 100MB)
- File format not supported
- Upload service configuration issue

---

## 8. Project Startup Issues

### 8.1 Auto-Start Failed

**Symptoms**: Project didn't start normally after executing `npm install && npm run dev`

**Troubleshooting Steps**:
1. Check scripts configuration in `package.json`
2. Check if port is occupied
3. Check if dependencies are fully installed

**Standard Start Command**:
```bash
cd [project_path] && npm install && npm run dev
```

### 8.2 Component Not Found

**Symptoms**: Console error showing component is undefined or cannot find module

**Solution**:
Ensure TUIKit is correctly imported in your App component:
```typescript
import React from 'react';
import { TUIKit } from '@tencentcloud/chat-uikit-react';
import '@tencentcloud/chat-uikit-react/styles/index.css';

function App() {
  return (
    <TUIKit SDKAppID={SDKAppID} userID={userID} userSig={userSig}>
      {/* Chat components */}
    </TUIKit>
  );
}

export default App;
```

### 8.3 Hooks Error in Class Components

**Symptoms**:
```
Error: Invalid hook call. Hooks can only be called inside of the body of a function component.
```

**Cause**: Attempting to use TUIKit hooks in class components

**Solution**:
Convert class components to functional components or create wrapper components:
```typescript
// Wrong: Class component
class ChatPage extends React.Component {
  // Cannot use hooks here
}

// Correct: Functional component
function ChatPage() {
  // Can use hooks here
  return <Chat />;
}
```

---

## 9. Debugging Tips

### 9.1 Enable SDK Logs

```typescript
import TIM from '@tencentcloud/chat';

// Enable Debug mode
TIM.setLogLevel(0); // 0: Debug, 1: Log, 2: Warn, 3: Error
```

### 9.2 Listen to Global Events

```typescript
import { TUIStore, StoreName } from '@tencentcloud/chat-uikit-engine';

// Listen to conversation list changes
TUIStore.watch(StoreName.CONV, {
  conversationList: (list) => {
    console.log('Conversation list updated:', list);
  }
});
```

### 9.3 React Developer Tools

Use React Developer Tools browser extension to:
- Inspect component hierarchy
- Check component props and state
- Profile component performance

### 9.4 Network Request Analysis

In browser developer tools Network panel:
- Filter `wss://` to view WebSocket connections
- Filter `XHR` to view HTTP requests
- Check error messages in request responses

---

## 10. Common React-Specific Issues

### 10.1 State Update After Unmount

**Symptoms**:
```
Warning: Can't perform a React state update on an unmounted component
```

**Solution**:
Clean up subscriptions and async operations in useEffect cleanup:
```typescript
useEffect(() => {
  let mounted = true;
  
  // Async operation
  fetchData().then(data => {
    if (mounted) {
      setData(data);
    }
  });
  
  return () => {
    mounted = false;
  };
}, []);
```

### 10.2 StrictMode Double Rendering

**Symptoms**: Components render twice in development

**Cause**: React StrictMode intentionally double-invokes certain functions

**Solution**:
This is expected behavior in development. It will not happen in production. If it causes issues:
```typescript
// In index.tsx, you can remove StrictMode for testing
// But keep it for better error detection
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

## Related Resources

- Official Documentation: Obtain via MCP tool `get_web_chat_uikit_integration`
- FAQ Query: Query via MCP tool `get_faq`
- Component API: Obtain via MCP tool `get_web_chat_uikit_component_detail`

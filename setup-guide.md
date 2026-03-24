# Step-by-Step Setup Guide

This guide walks you through setting up and using the agent orchestration system in VS Code.

---

## Prerequisites

- **VS Code** (or VS Code Insiders)
- **GitHub Copilot subscription** (with access to custom agents)
- Basic understanding of VS Code and GitHub Copilot

---

## 📖 Step 1: Understanding Custom Agents

### What are Custom Agents?

Custom agents are specialized AI assistants in VS Code that:
- Have specific roles and capabilities
- Follow predefined instructions
- Can be invoked by other agents
- Work together through orchestration

### The Four Agent Types:

1. **Orchestrator** - Coordinates all work (never implements)
2. **Planner** - Creates implementation plans
3. **Coder** - Writes actual code
4. **Designer** - Handles UI/UX design

---

## 📖 Step 2: Install the Agents

### Option A: Using the .agents folder (Automatic)

The `.agents` folder in this project contains all agent definitions:

```
.agents/
├── orchestrator.agent.md
├── planner.agent.md
├── coder.agent.md
└── designer.agent.md
```

VS Code automatically detects these files and registers the agents!

### Option B: Manual Installation

1. Open VS Code
2. Press `Ctrl+Shift+P` (Windows) or `Cmd+Shift+P` (Mac)
3. Type "GitHub Copilot: Manage Agents"
4. Click "New Agent"
5. Copy the content from each `.agent.md` file
6. Save with the appropriate name

---

## 📖 Step 3: Enable Required Settings

You MUST enable these settings for agent orchestration to work:

### Via Settings UI:

1. Press `Ctrl+,` (or `Cmd+,` on Mac) to open Settings
2. Search for: `custom agent in subagent`
3. Enable: ✅ **Chat: Custom Agent In Subagent**
4. Search for: `memory`
5. Enable: ✅ **Chat: Use Memory** (if available)

### Via settings.json:

Press `Ctrl+Shift+P` → "Preferences: Open User Settings (JSON)"

Add these settings:

```json
{
  "chat.customAgentInSubagent.enabled": true,
  "chat.agent.enabled": true,
  "chat.agent.maxRequests": 64,
  "chat.useNestedAgentsMdFiles": true,
  "chat.viewSessions.enabled": true
}
```

**Critical:** Without `chat.customAgentInSubagent.enabled`, your Orchestrator cannot call other agents!

---

## 📖 Step 4: Verify Agent Installation

### Check Available Agents:

1. Open GitHub Copilot Chat (`Ctrl+Alt+I` or sidebar icon)
2. Click the agent picker (dropdown at bottom of chat)
3. You should see:
   - 🤖 Orchestrator
   - 📋 Planner
   - 💻 Coder
   - 🎨 Designer

### Verify Tools Access:

1. Select an agent (e.g., Coder)
2. Click the "toolbox" icon
3. Verify it has appropriate tools:
   - Coder: file editing, search, read
   - Orchestrator: runSubagent, search, read (NO file editing)
   - Planner: search, read (NO file editing)

---

## 📖 Step 5: Your First Orchestrated Task

### Start Simple:

1. Open GitHub Copilot Chat
2. Select **@Orchestrator** from the agent picker
3. Type a request:

```
Add a simple counter component to this app
```

### What Should Happen:

1. **Orchestrator** receives your request
2. **Orchestrator** calls **@Planner**: "Create a plan for adding a counter component"
3. **Planner** returns a detailed plan with steps
4. **Orchestrator** parses plan into phases
5. **Orchestrator** calls **@Coder** to implement each step
6. **Orchestrator** reports completion

### Expected Output:

```markdown
## Execution Plan

### Phase 1: Implementation
- Task 1.1: Create counter component → Coder
  Files: src/components/Counter.tsx

### Phase 2: Integration
- Task 2.1: Add counter to app → Coder
  Files: src/App.tsx

[Phase 1 executes...]
✓ Counter component created

[Phase 2 executes...]
✓ Counter integrated into app

## Summary
✅ Counter component successfully added with increment/decrement functionality!
```

---

## 📖 Step 6: Troubleshooting Common Issues

### Issue 1: "I don't have file editing tools available"

**Cause:** Agent is not running with correct permissions

**Solution:**
- Ensure `chat.customAgentInSubagent.enabled` is `true`
- Make sure you're in "Agent" mode, not "Ask" or "Plan" mode
- Check agent's toolbox to verify tools are available

### Issue 2: Orchestrator is writing code instead of delegating

**Cause:** The Orchestrator agent has file editing tools enabled

**Solution:**
- Review the orchestrator.agent.md file
- Ensure it's configured WITHOUT file editing tools
- The Orchestrator should ONLY have: search, read, runSubagent

### Issue 3: No agents showing in the picker

**Cause:** Agents not detected or settings not enabled

**Solution:**
- Verify `.agents` folder contains `.agent.md` files
- Restart VS Code
- Check `chat.useNestedAgentsMdFiles` is enabled

### Issue 4: Agents can't call other agents

**Cause:** Subagent feature not enabled

**Solution:**
- Verify `chat.customAgentInSubagent.enabled: true`
- Restart VS Code
- Check VS Code version (needs recent version)

---

## 📖 Step 7: Debug Logs

To see what's happening behind the scenes:

1. Press `F1` or `Ctrl+Shift+P`
2. Run: `GitHub Copilot: Show Chat Log View`
3. This shows:
   - Which agent is being invoked
   - What model it's using
   - Tool calls being made
   - Subagent invocations

Look for entries like:
```
tool/runSubagent-Planner
tool/runSubagent-Coder
```

---

## 📖 Step 8: Practice Workflows

Try these increasingly complex tasks:

### Beginner:
```
Add a button that changes color when clicked
```

### Intermediate:
```
Create a todo list with add, delete, and mark complete functionality
```

### Advanced:
```
Add dark mode support with a toggle, localStorage persistence,
and system preference detection
```

### Expert:
```
Implement user authentication with login form, protected routes,
and session management
```

---

## 📖 Step 9: Understanding the Workflow

### The Standard Pattern:

```
User Request
    ↓
Orchestrator (receives)
    ↓
Planner (creates plan)
    ↓
Orchestrator (parses into phases)
    ↓
Phase 1: Coder + Designer (parallel)
    ↓
Phase 2: Coder (sequential)
    ↓
Phase 3: Coder (sequential)
    ↓
Orchestrator (verifies & reports)
    ↓
User gets result
```

### Key Principles:

1. **Separation of Concerns**: Each agent has ONE job
2. **Plan First**: Always get a plan before implementing
3. **Parallel When Possible**: Different files = run simultaneously
4. **Sequential When Needed**: Dependencies = wait for completion
5. **Clear Communication**: WHAT not HOW

---

## 📖 Step 10: Next Steps

### Learn More:
- Read [example-workflow.md](./example-workflow.md) for detailed walkthrough
- Study the agent definition files in `.agents/`
- Experiment with different types of requests

### Customize:
- Modify agent instructions in `.agents/*.agent.md`
- Add your own coding principles to coder.agent.md
- Adjust orchestration logic in orchestrator.agent.md

### Extend:
- Create additional specialist agents (Testing, Documentation, etc.)
- Add more sophisticated parallelization rules
- Implement review/approval gates

---

## 🎯 Success Indicators

You'll know the system is working when:

✅ Orchestrator delegates instead of implementing  
✅ Planner provides detailed, file-specific plans  
✅ Coder follows existing project patterns  
✅ Tasks run in parallel when files don't overlap  
✅ Progress updates appear after each phase  
✅ Final verification confirms everything works  

---

## 🆘 Getting Help

If you're stuck:

1. Check the debug logs (`GitHub Copilot: Show Chat Log View`)
2. Verify all settings in Step 3 are enabled
3. Review your agent definition files
4. Try a simpler task to isolate the issue
5. Restart VS Code

---

## 📚 Additional Resources

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Custom Agents Guide](https://code.visualstudio.com/docs/copilot/copilot-chat)
- [Original Ultralight Orchestration Gist](https://gist.github.com/burkeholland/0e68481f96e94bbb98134fa6efd00436)

---

Happy orchestrating! 🚀

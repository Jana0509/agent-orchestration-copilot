# Quick Reference Guide

A cheat sheet for agent orchestration concepts and patterns.

---

## 🤖 The Four Agents

| Agent | Role | Does | Doesn't | Model |
|-------|------|------|---------|-------|
| **Orchestrator** | Coordinator | Delegates, manages workflow, reports progress | Write code, implement | Claude Sonnet 4.5 |
| **Planner** | Architect | Creates plans, identifies dependencies | Write code | Claude Sonnet 4.5 |
| **Coder** | Developer | Writes code, fixes bugs | Design UI, coordinate | GPT-5.3-Codex |
| **Designer** | UX Specialist | Designs UI/UX, creates specs | Write implementation | Gemini 3.1 Pro |

---

## 🔄 Standard Workflow

```
User → Orchestrator → Planner → Orchestrator → [Phases] → Orchestrator → User
         (receives)     (plans)    (parses)     (executes)   (reports)
```

---

## ⚡ Parallel vs Sequential

### ✅ Parallel When:
- Different files
- Different domains
- No dependencies

### ❌ Sequential When:
- Same file
- Task B needs Task A
- Approval required

---

## 🎯 Delegation Rules

### DO: Tell WHAT (Outcome)
```
✅ "Add search functionality"
✅ "Fix the login bug"
✅ "Design a modern toggle"
```

### DON'T: Tell HOW (Implementation)
```
❌ "Use useState for search"
❌ "Add useCallback to prevent re-render"
❌ "Use Tailwind class bg-blue-500"
```

---

## 📋 Plan Format

```markdown
### Summary
Brief overview

### Implementation Steps
1. **Step Name** (Agent: Coder/Designer)
   - What to do
   - Files: file1.ts, file2.tsx
   - Dependencies: None / Step X

### Edge Cases
- Case 1
- Case 2

### Open Questions
- Question if any
```

---

## 🚀 Phase Structure

```markdown
## Execution Plan

### Phase 1: [Name] (Parallel OK)
- Task 1.1 → Agent
  Files: a.ts
- Task 1.2 → Agent
  Files: b.ts

### Phase 2: [Name] (Sequential - depends on Phase 1)
- Task 2.1 → Agent
  Files: c.ts
```

---

## 🛠️ Required VS Code Settings

```json
{
  "chat.customAgentInSubagent.enabled": true,
  "chat.useNestedAgentsMdFiles": true,
  "chat.agent.enabled": true
}
```

---

## 🔍 Debug Commands

- `F1` → `GitHub Copilot: Show Chat Log View`
- Look for: `tool/runSubagent-AgentName`

---

## 📁 File Structure

```
.agents/
├── orchestrator.agent.md  ← Coordinates
├── planner.agent.md       ← Plans
├── coder.agent.md         ← Codes
└── designer.agent.md      ← Designs
```

---

## ❌ Common Mistakes

| Mistake | Fix |
|---------|-----|
| Orchestrator writing code | Remove file edit tools |
| No plan before implementing | Always call Planner first |
| Parallel tasks, same file | Run sequentially |
| Telling HOW instead of WHAT | Describe outcome only |
| Skipping dependencies | Parse plan carefully |

---

## ✅ Success Checklist

- [ ] Orchestrator delegates, doesn't implement
- [ ] Planner creates detailed plans
- [ ] File assignments are explicit
- [ ] Parallel tasks don't conflict
- [ ] Dependencies are respected
- [ ] Progress reported after phases
- [ ] Final verification performed

---

## 🎓 Practice Tasks

### Beginner
- Add a button
- Create a greeting component
- Add a footer

### Intermediate  
- Todo list (add, delete, complete)
- Search filter
- Modal dialog

### Advanced
- Dark mode (toggle, persist, detect)
- Authentication (login, routes, session)
- Dashboard (charts, filters, data)

---

## 🆘 Troubleshooting Quick Fixes

| Problem | Solution |
|---------|----------|
| "No file editing tools" | Enable `customAgentInSubagent` |
| Orchestrator codes | Check agent toolbox |
| Agents not visible | Restart VS Code |
| Can't call subagents | Verify settings |

---

## 🔗 Quick Links

- [Setup Guide](./setup-guide.md)
- [Example Workflow](./example-workflow.md)
- [Core Concepts](./concepts.md)
- [Agent Definitions](../.agents/)

---

**Print this for quick reference while learning! 📋**

# Agent Orchestration - Example Workflow

This document walks through a complete example of how the agent orchestration system works.

---

## 📝 Example Task: "Add Dark Mode to the Application"

Let's see how the Orchestrator coordinates this work across the specialist agents.

---

## Step 1: User Request

**User says:**
> "Add dark mode support to our application with a toggle button"

---

## Step 2: Orchestrator Receives Request

The **Orchestrator** analyzes the request and recognizes it needs a plan.

**Orchestrator delegates to Planner:**
> "Create an implementation plan for adding dark mode support with a toggle button"

---

## Step 3: Planner Creates Plan

**Planner Agent** researches the codebase and creates a detailed plan:

```markdown
### Summary
Implement dark mode by creating a theme context with localStorage persistence,
designing a toggle UI component, and applying theme tokens across all components.

### Implementation Steps

1. **Create Theme Context and Hook** (Agent: Coder)
   - Implement ThemeContext with Provider
   - Create useTheme custom hook
   - Add localStorage persistence
   - Detect system color scheme preference
   - Files: `src/contexts/ThemeContext.tsx`, `src/hooks/useTheme.ts`
   - Dependencies: None

2. **Design Theme Toggle Component** (Agent: Designer)
   - Create visual design for toggle switch
   - Define colors, sizes, animations
   - Spec out hover/active states
   - Files: Design specs for `src/components/ThemeToggle.tsx`
   - Dependencies: None

3. **Implement Toggle Component** (Agent: Coder)
   - Build the ThemeToggle component based on design specs
   - Connect to theme context
   - Files: `src/components/ThemeToggle.tsx`
   - Dependencies: Steps 1, 2

4. **Integrate Theme Provider** (Agent: Coder)
   - Wrap application with ThemeProvider
   - Add toggle to header/nav
   - Files: `src/App.tsx`, `src/components/Header.tsx`
   - Dependencies: Steps 1, 3

5. **Define Theme Tokens** (Agent: Designer)
   - Create CSS variables for light/dark themes
   - Define color palette
   - Files: `src/styles/themes.css`
   - Dependencies: None

6. **Apply Theme Styles** (Agent: Coder)
   - Update components to use theme CSS variables
   - Files: Multiple component CSS files
   - Dependencies: Steps 4, 5

### Edge Cases
- User has system dark mode preference → Auto-detect on first load
- Theme persistence → Use localStorage
- Flash of wrong theme on load → Apply theme class before React renders

### Open Questions
- None
```

---

## Step 4: Orchestrator Parses into Phases

The **Orchestrator** analyzes the plan and identifies parallel opportunities:

```markdown
## Execution Plan

### Phase 1: Foundation (Parallel - No file overlap)
- Task 1.1: Create theme context and hook → Coder
  Files: src/contexts/ThemeContext.tsx, src/hooks/useTheme.ts
  
- Task 1.2: Design toggle component → Designer
  Files: Design specifications
  
- Task 1.3: Define theme tokens → Designer
  Files: src/styles/themes.css

### Phase 2: Component Implementation (Sequential - Depends on Phase 1)
- Task 2.1: Implement toggle component → Coder
  Files: src/components/ThemeToggle.tsx
  Dependencies: Phase 1 (context + design)

### Phase 3: Integration (Sequential - Depends on Phase 2)
- Task 3.1: Integrate theme provider → Coder
  Files: src/App.tsx, src/components/Header.tsx
  Dependencies: Phase 2 (toggle component)

### Phase 4: Styling (Sequential - Depends on Phase 3)
- Task 4.1: Apply theme styles → Coder
  Files: Multiple CSS files
  Dependencies: Phase 3 (theme provider active)
```

---

## Step 5: Orchestrator Executes Each Phase

### 🚀 Phase 1: Foundation (Tasks run in parallel)

**Orchestrator → Coder:**
> "Create a theme context and custom hook for managing light/dark mode.
> Include localStorage persistence and system preference detection.
> Create these files: src/contexts/ThemeContext.tsx, src/hooks/useTheme.ts"

**Orchestrator → Designer (parallel):**
> "Design a toggle component for switching between light and dark modes.
> Provide specifications for the visual design, sizes, colors, and animations."

**Orchestrator → Designer (parallel):**
> "Define CSS theme tokens for light and dark modes.
> Create a complete color palette. 
> Create file: src/styles/themes.css"

**Result:** All three tasks complete. Orchestrator waits for all before proceeding.

---

### 🚀 Phase 2: Component Implementation

**Orchestrator → Coder:**
> "Implement the ThemeToggle component using the design specifications
> and connecting to the theme context created in Phase 1.
> Create file: src/components/ThemeToggle.tsx"

**Result:** Toggle component is implemented.

---

### 🚀 Phase 3: Integration

**Orchestrator → Coder:**
> "Integrate the theme provider into the application and add the toggle
> to the header. Modify files: src/App.tsx, src/components/Header.tsx"

**Result:** Theme system is wired up and visible to users.

---

### 🚀 Phase 4: Styling

**Orchestrator → Coder:**
> "Update all component styles to use the theme CSS variables.
> Ensure all colors, backgrounds, and text use theme tokens."

**Result:** All components respect the selected theme.

---

## Step 6: Orchestrator Verifies and Reports

**Orchestrator performs final checks:**
- ✅ Theme context is working
- ✅ Toggle component functions correctly
- ✅ Theme persists across page reloads
- ✅ All components use theme tokens
- ✅ System preference is detected

**Orchestrator reports to user:**

```markdown
✅ Dark mode implementation complete!

## What Was Done:

### Phase 1 - Foundation
- Created ThemeContext with localStorage persistence
- Designed toggle component specifications
- Defined CSS theme tokens

### Phase 2 - Component
- Implemented ThemeToggle component with animations

### Phase 3 - Integration
- Integrated ThemeProvider into app
- Added toggle to header

### Phase 4 - Styling
- Applied theme variables to all components

## Features:
- ✅ Smooth light/dark mode toggle
- ✅ Persists user preference
- ✅ Detects system color scheme
- ✅ All components themed
- ✅ Accessible (keyboard navigation)

You can test it by clicking the theme toggle in the header!
```

---

## 🎯 Key Takeaways

1. **Orchestrator NEVER codes** - Only delegates and coordinates
2. **Planner provides the roadmap** - Detailed steps with file assignments
3. **Parallel execution** - Tasks in Phase 1 run simultaneously (different files)
4. **Sequential when needed** - Later phases wait for dependencies
5. **Clear delegation** - Each agent gets specific WHAT, not HOW
6. **File conflict prevention** - Explicit file assignments prevent overlaps
7. **Progress reporting** - Updates after each phase

---

## 🚫 Common Mistakes to Avoid

### ❌ Orchestrator Writing Code
```markdown
Bad: Orchestrator says "I'll create the ThemeContext file with this code..."
Good: Orchestrator says "→ Coder: Create the theme context and hook"
```

### ❌ Telling Agents HOW
```markdown
Bad: "Add a useState hook called theme and update it with setTheme"
Good: "Implement theme state management in the context"
```

### ❌ Parallel Tasks with File Overlap
```markdown
Bad: 
  Task A → Coder: Update App.tsx (add provider)
  Task B → Coder: Update App.tsx (add routing) [PARALLEL]
  
Good:
  Phase 1: Task A → Update App.tsx (add provider)
  Phase 2: Task B → Update App.tsx (add routing) [AFTER Phase 1]
```

### ❌ Skipping the Plan
```markdown
Bad: Orchestrator immediately starts delegating implementation tasks
Good: Orchestrator first calls Planner, then parses plan into phases
```

---

This workflow demonstrates the power of agent orchestration: clear roles, efficient parallelization, and coordinated execution!

# Agent Orchestration - Core Concepts

This document explains the fundamental concepts behind agent orchestration.

---

## 🧠 What is Agent Orchestration?

**Agent Orchestration** is a design pattern where multiple specialized AI agents work together under coordination to accomplish complex tasks.

Think of it like a software development team:
- **Project Manager** (Orchestrator) - Coordinates work
- **Architect** (Planner) - Designs the solution
- **Developer** (Coder) - Implements features
- **Designer** (Designer) - Creates UI/UX

Each role is essential, and they work together efficiently.

---

## 🎭 The Four Agent Roles

### 1. Orchestrator (The Coordinator)

**Purpose**: Manages the workflow, delegates tasks, ensures completion

**Key Responsibilities**:
- Receives user requests
- Calls Planner for implementation strategy
- Parses plans into executable phases
- Delegates work to appropriate agents
- Reports progress and final results

**What It NEVER Does**:
- ❌ Write code
- ❌ Create designs
- ❌ Implement features directly

**Think of it as**: A project manager who coordinates but doesn't write code

---

### 2. Planner (The Architect)

**Purpose**: Creates detailed implementation plans

**Key Responsibilities**:
- Researches the existing codebase
- Identifies patterns and structures
- Creates step-by-step implementation plans
- Specifies which files will be affected
- Notes dependencies between steps
- Identifies edge cases

**What It NEVER Does**:
- ❌ Write actual code
- ❌ Implement solutions
- ❌ Execute plans

**Think of it as**: A software architect who designs but doesn't code

---

### 3. Coder (The Implementer)

**Purpose**: Writes code following best practices

**Key Responsibilities**:
- Implements features based on specifications
- Fixes bugs
- Follows existing code patterns
- Writes clean, maintainable code
- Handles errors properly
- Makes code changes

**What It NEVER Does**:
- ❌ Create overall architecture plans
- ❌ Design UI/UX
- ❌ Coordinate other agents

**Think of it as**: A software developer who implements solutions

---

### 4. Designer (The UX Specialist)

**Purpose**: Creates user interfaces and experiences

**Key Responsibilities**:
- Designs UI components
- Defines color schemes and layouts
- Ensures accessibility
- Specifies animations and interactions
- Creates design specifications
- Focuses on user experience

**What It NEVER Does**:
- ❌ Implement the actual code
- ❌ Create business logic
- ❌ Coordinate other agents

**Think of it as**: A UI/UX designer who creates specifications

---

## 🔄 The Standard Workflow

```
1. User Request
      ↓
2. Orchestrator receives request
      ↓
3. Orchestrator → Planner: "Create a plan"
      ↓
4. Planner analyzes and returns plan
      ↓
5. Orchestrator parses plan into phases
      ↓
6. Phase 1 execution (possibly parallel)
      ↓
7. Phase 2 execution (depends on Phase 1)
      ↓
8. Phase N execution
      ↓
9. Orchestrator verifies completion
      ↓
10. Orchestrator reports to user
```

---

## 🚀 Key Principle: Phased Execution

### Why Phases?

Tasks are organized into **phases** based on:
1. **File dependencies** - Can't modify same file simultaneously
2. **Logic dependencies** - Task B needs Task A's output
3. **Efficiency** - Maximize parallel work

### Example: Adding Dark Mode

```
Phase 1: Foundation (CAN RUN IN PARALLEL)
├─ Task A: Create theme context (files: ThemeContext.tsx)
├─ Task B: Design toggle UI (files: design specs)
└─ Task C: Create CSS tokens (files: theme.css)

Phase 2: Implementation (MUST BE SEQUENTIAL)
└─ Task D: Build toggle component (files: ThemeToggle.tsx)
   ↑ Depends on Phase 1 (needs context + design)

Phase 3: Integration (MUST BE SEQUENTIAL)
└─ Task E: Wire up in App (files: App.tsx)
   ↑ Depends on Phase 2 (needs toggle component)

Phase 4: Styling (MUST BE SEQUENTIAL)
└─ Task F: Apply theme everywhere (files: multiple CSS)
   ↑ Depends on Phase 3 (theme must be active)
```

---

## ⚡ Parallelization Rules

### ✅ Run in Parallel When:

1. **Different files**
   ```
   Task A: Create UserContext.tsx
   Task B: Create ProductContext.tsx
   → Different files, no conflict, RUN PARALLEL
   ```

2. **Different domains**
   ```
   Task A: Implement business logic (Coder)
   Task B: Design button styles (Designer)
   → Different concerns, RUN PARALLEL
   ```

3. **No data dependencies**
   ```
   Task A: Add header component
   Task B: Add footer component
   → Independent features, RUN PARALLEL
   ```

### ❌ Run Sequentially When:

1. **Same file**
   ```
   Task A: Add theme provider to App.tsx
   Task B: Add router to App.tsx
   → Same file, RUN SEQUENTIAL
   ```

2. **Task B needs Task A's output**
   ```
   Task A: Create context
   Task B: Use context in component
   → Dependency, RUN SEQUENTIAL
   ```

3. **Design must be approved first**
   ```
   Task A: Design component (Designer)
   Task B: Implement design (Coder)
   → Approval needed, RUN SEQUENTIAL
   ```

---

## 🎯 Delegation: What vs How

### The Golden Rule

Tell agents **WHAT** to do (the outcome), not **HOW** to do it (the implementation).

### ✅ Good Delegation (WHAT)

```markdown
"Add a search feature to filter the product list"
→ Describes the outcome

"Fix the infinite loop error in the sidebar"
→ Describes the problem to solve

"Create a visually appealing toggle for dark mode"
→ Describes the desired result
```

### ❌ Bad Delegation (HOW)

```markdown
"Add a useState hook called searchTerm and use filter() on the array"
→ Prescribes the implementation

"Add useShallow to the selector to prevent re-renders"
→ Tells them exactly how to fix it

"Use a 56px toggle with a blue background and Tailwind classes"
→ Micromanages the design
```

### Why This Matters

- Agents are **experts** in their domain
- They know **better patterns** for the codebase
- Over-specifying **limits their effectiveness**
- Trust their **specialized knowledge**

---

## 🚫 File Conflict Prevention

### The Problem

If two agents modify the same file simultaneously:
- Changes can conflict
- One agent's work might overwrite the other's
- Results in broken code

### The Solution: Explicit File Assignment

```markdown
Phase 1 (Parallel):
  Task 1.1 → Coder
    Files: src/contexts/AuthContext.tsx ✓
  
  Task 1.2 → Coder  
    Files: src/contexts/ThemeContext.tsx ✓
  
  ↑ Different files = Safe to run in parallel

Phase 2 (Sequential):
  Task 2.1 → Coder
    Files: src/App.tsx
  
  Then Task 2.2 → Coder
    Files: src/App.tsx
  
  ↑ Same file = Must run sequentially
```

---

## 🧩 Separation of Concerns

Each agent has **ONE clear responsibility**:

| Agent | Responsibility | Tools |
|-------|----------------|-------|
| Orchestrator | Coordinate | runSubagent, search, read |
| Planner | Strategize | search, read |
| Coder | Implement | All file editing tools |
| Designer | Design | read, search (for design creation) |

### Why Separation Matters

- **Clarity**: Each agent knows its role
- **Quality**: Specialists produce better work
- **Debugging**: Easy to identify where issues occurred
- **Maintainability**: Clear boundaries make changes easier

---

## 🎓 Mental Models

### Model 1: Software Team

```
User Request = Product Requirement
Orchestrator = Project Manager
Planner = Software Architect
Coder = Software Developer
Designer = UI/UX Designer
```

### Model 2: Construction Project

```
User Request = Building Plans
Orchestrator = General Contractor
Planner = Structural Engineer
Coder = Construction Crew
Designer = Interior Designer
```

### Model 3: Restaurant Kitchen

```
User Request = Customer Order
Orchestrator = Head Chef (coordinates)
Planner = Sous Chef (prepares mise en place)
Coder = Line Cook (executes dishes)
Designer = Pastry Chef (creates presentation)
```

---

## 📊 Benefits of Orchestration

### 1. Better Quality
- Specialists focus on their expertise
- Code follows best practices
- Designs are more thoughtful

### 2. Efficiency
- Parallel execution when possible
- No duplicate work
- Clear workflow reduces confusion

### 3. Maintainability
- Clear separation of concerns
- Predictable structure
- Easy to modify or extend

### 4. Scalability
- Easy to add new specialist agents
- Can handle complex multi-step tasks
- Workflow adapts to task complexity

---

## 🎯 Success Criteria

You understand orchestration when you can:

✅ Explain why the Orchestrator doesn't write code  
✅ Identify when tasks can run in parallel  
✅ Recognize file conflicts before they happen  
✅ Delegate using WHAT not HOW  
✅ Break complex tasks into phases  
✅ Choose the right agent for each task  

---

## 🚀 Next Steps

Now that you understand the concepts:

1. **Read**: [example-workflow.md](./example-workflow.md) - See it in action
2. **Setup**: [setup-guide.md](./setup-guide.md) - Get it running
3. **Practice**: Try the example tasks in README.md
4. **Experiment**: Modify agent definitions
5. **Build**: Use it in real projects

---

**Remember**: Agent orchestration isn't about replacing developers—it's about **coordinating AI specialists** to work together efficiently, just like a great software team! 🎯

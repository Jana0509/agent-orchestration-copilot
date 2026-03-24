# Agent Orchestration Learning Project

A hands-on project to learn **agent orchestration** using GitHub Copilot custom agents. This implements the "Ultralight Orchestration" pattern where specialist AI agents coordinate to complete complex tasks.

---

## 🎯 What is Agent Orchestration?

Agent orchestration is a pattern where:
- **Multiple AI agents** work together, each with specific expertise
- An **Orchestrator agent** coordinates the work (but never implements)
- A **Planner agent** creates detailed implementation plans
- **Specialist agents** (Coder, Designer) execute specific tasks
- Tasks are **parallelized** when they don't conflict
- Work is **phased** based on dependencies

### Why Use This Pattern?

✅ **Better results**: Specialists focus on what they do best  
✅ **Efficient execution**: Parallel processing when possible  
✅ **Clear structure**: Phased approach prevents conflicts  
✅ **Maintainable**: Each agent has a single, clear responsibility  
✅ **Scalable**: Easy to add more specialist agents  

---

## 🏗️ Project Structure

```
agent-poc/
├── .agents/                      # Custom agent definitions
│   ├── orchestrator.agent.md    # Coordinates all work
│   ├── planner.agent.md          # Creates implementation plans
│   ├── coder.agent.md            # Writes code
│   └── designer.agent.md         # Handles UI/UX
│
├── docs/                         # Learning materials
│   ├── setup-guide.md            # Step-by-step setup instructions
│   ├── example-workflow.md       # Complete example walkthrough
│   └── concepts.md               # Key concepts explained
│
└── README.md                     # This file
```

---

## 🚀 Quick Start

### 1. Prerequisites

- VS Code (or VS Code Insiders)
- GitHub Copilot subscription
- Basic understanding of VS Code

### 2. Enable Required Settings

Open VS Code Settings (Ctrl+,) and enable:

- ✅ `Chat: Custom Agent In Subagent`
- ✅ `Chat: Use Nested Agents Md Files`

Or add to `settings.json`:

```json
{
  "chat.customAgentInSubagent.enabled": true,
  "chat.useNestedAgentsMdFiles": true,
  "chat.agent.enabled": true
}
```

### 3. Verify Agents Are Loaded

1. Open GitHub Copilot Chat (Ctrl+Alt+I)
2. Click the agent picker dropdown
3. You should see: Orchestrator, Planner, Coder, Designer

### 4. Try Your First Task

Select `@Orchestrator` and type:

```
Create a simple counter component with increment and decrement buttons
```

Watch as:
1. Orchestrator calls Planner for a plan
2. Planner creates implementation steps
3. Orchestrator delegates to Coder
4. Coder implements the solution
5. Orchestrator reports completion

---

## 📚 Learning Path

### Level 1: Understanding the Basics ⭐

**Read:**
- [docs/concepts.md](docs/concepts.md) - Core concepts
- [.agents/orchestrator.agent.md](.agents/orchestrator.agent.md) - How orchestration works

**Try:**
- Simple component creation
- Basic feature additions

### Level 2: Workflow Patterns ⭐⭐

**Read:**
- [docs/example-workflow.md](docs/example-workflow.md) - Complete walkthrough
- [.agents/planner.agent.md](.agents/planner.agent.md) - Planning strategies

**Try:**
- Multi-step features (e.g., form with validation)
- Features requiring design + code

### Level 3: Advanced Orchestration ⭐⭐⭐

**Read:**
- [docs/setup-guide.md](docs/setup-guide.md) - Advanced configurations
- All agent definitions for customization

**Try:**
- Complex features with dependencies
- Parallel task execution
- Custom agent modifications

---

## 🎓 Example Tasks to Practice

### Beginner Tasks

```
1. Add a button that shows an alert when clicked
2. Create a greeting component that displays a personalized message
3. Add a footer with copyright text to the application
```

### Intermediate Tasks

```
1. Build a todo list with add, delete, and toggle complete
2. Create a search box that filters a list
3. Add a modal dialog with open/close functionality
```

### Advanced Tasks

```
1. Implement dark mode with toggle, persistence, and system detection
2. Add user authentication with login form and protected routes
3. Create a data dashboard with charts and filtering
```

---

## 🤖 The Four Agents

### Orchestrator
- **Role**: Project coordinator
- **Does**: Receives requests, calls other agents, manages workflow
- **Doesn't**: Write any code or implement features
- **Model**: Claude Sonnet 4.5

### Planner
- **Role**: Implementation strategist
- **Does**: Researches codebase, creates detailed plans
- **Doesn't**: Write code or designs
- **Model**: Claude Sonnet 4.5

### Coder
- **Role**: Software engineer
- **Does**: Writes code, fixes bugs, implements features
- **Doesn't**: Create designs or plan architecture
- **Model**: GPT-5.3-Codex

### Designer
- **Role**: UI/UX specialist
- **Does**: Creates designs, defines styles, ensures accessibility
- **Doesn't**: Write implementation code
- **Model**: Gemini 3.1 Pro

---

## 🔑 Key Concepts

### 1. Delegation Over Implementation
- Orchestrator **never writes code**
- It only **coordinates** other agents
- Think: project manager, not developer

### 2. Plan Before Execute
- Always call Planner first
- Parse plan into phases
- Execute phases in order

### 3. Parallelization
- Tasks with **different files** → run in parallel
- Tasks with **same files** → run sequentially
- Maximizes efficiency, prevents conflicts

### 4. What, Not How
- Tell agents **WHAT** to do (outcome)
- Never tell them **HOW** to do it (implementation)
- Trust their expertise

### 5. File Conflict Prevention
- Explicitly assign files to each task
- Ensure no overlap in parallel tasks
- Use sequential phases when files must be shared

---

## 🧪 Testing Your Understanding

After learning, you should be able to answer:

1. **Why doesn't the Orchestrator write code?**
   - Separation of concerns, prevents doing too much

2. **When should tasks run in parallel?**
   - When they touch different files and have no dependencies

3. **Why do we call the Planner first?**
   - To get a structured plan before implementing

4. **What's wrong with "Add a button that calls handleClick"?**
   - Tells HOW, not WHAT. Should be "Add a save button"

---

## 🛠️ Troubleshooting

### Problem: Orchestrator is writing code

**Solution**: Check orchestrator.agent.md doesn't have file editing tools

### Problem: Agents aren't calling each other

**Solution**: Verify `chat.customAgentInSubagent.enabled: true`

### Problem: Can't see custom agents

**Solution**: Ensure `.agents` folder has `.agent.md` files and restart VS Code

More help: See [docs/setup-guide.md](docs/setup-guide.md#troubleshooting-common-issues)

---

## 📖 Documentation

- **[Setup Guide](docs/setup-guide.md)** - Detailed setup instructions
- **[Example Workflow](docs/example-workflow.md)** - Complete example walkthrough
- **[Concepts](docs/concepts.md)** - Core concepts explained
- **[Agent Definitions](.agents/)** - Each agent's instructions

---

## 🎯 Learning Objectives

By the end of this project, you'll understand:

✅ How agent orchestration works  
✅ When to use different specialist agents  
✅ How to structure work into phases  
✅ How to maximize parallel execution  
✅ How to prevent file conflicts  
✅ How to customize agents for your needs  

---

## 🌟 Next Steps

### After Mastering the Basics:

1. **Customize Agents**
   - Add your team's coding standards to coder.agent.md
   - Enhance planner with your workflow preferences
   - Create specialized agents (Testing, Documentation, etc.)

2. **Build Real Features**
   - Use this pattern in your actual projects
   - Experience the efficiency gains
   - Refine the workflow based on your needs

3. **Share & Contribute**
   - Share your agent customizations
   - Document patterns you discover
   - Help others learn orchestration

---

## 📚 Additional Resources

- **Original Pattern**: [Ultralight Orchestration by Burke Holland](https://gist.github.com/burkeholland/0e68481f96e94bbb98134fa6efd00436)
- **GitHub Copilot Docs**: [Official Documentation](https://docs.github.com/en/copilot)
- **Custom Agents**: [VS Code Guide](https://code.visualstudio.com/docs/copilot/copilot-chat)

---

## 🤝 Contributing

Found a better way to explain something? Got an awesome example? 

Feel free to:
- Improve documentation
- Add more examples
- Share your customizations
- Report issues

---

## 📝 License

This is a learning project. Use it however helps you learn!

---

## 🎉 Ready to Start?

1. Read [docs/setup-guide.md](docs/setup-guide.md) to get everything configured
2. Study [docs/example-workflow.md](docs/example-workflow.md) to see it in action
3. Try the practice tasks above
4. Build something amazing! 🚀

---

**Happy Learning! 💡**

Remember: The goal isn't just to use AI agents, but to understand **how to coordinate them effectively** for complex tasks. That's a valuable skill in modern AI-assisted development!

---

## 🌤️ WeatherNow — Weather Web Application

A modern, glassmorphism-styled weather app with animated gradients that change based on weather conditions.

### Features
- Search any city worldwide for real-time weather
- Current weather: temperature, humidity, wind speed, pressure
- 5-slot upcoming forecast display
- Dynamic background gradients that shift based on weather conditions (sunny, cloudy, rainy, snowy, etc.)
- Fully responsive design with glassmorphism UI

### Project Structure
```
agent-poc/
├── backend/
│   ├── package.json       # Dependencies (Express, CORS, node-fetch)
│   └── server.js          # API server — proxies OpenWeatherMap
├── frontend/
│   ├── index.html         # App layout
│   ├── styles.css         # Glassmorphism + animated gradients
│   └── app.js             # Client-side logic
```

### Setup & Run

**1. Get an API Key**
Sign up at [OpenWeatherMap](https://openweathermap.org/api) and get a free API key.

**2. Install Dependencies**
```bash
cd backend
npm install
```

**3. Run the Server (PowerShell)**
```powershell
$env:OPENWEATHER_API_KEY="your_api_key_here"
npm start
```

**4. Open the App**
Navigate to [http://localhost:3000](http://localhost:3000) in your browser.

### Tech Stack
- **Backend:** Node.js, Express, node-fetch
- **Frontend:** Vanilla HTML5, CSS3, JavaScript (no frameworks)
- **API:** OpenWeatherMap (free tier — 60 calls/min)
"# copilot-agent-orchestration" 

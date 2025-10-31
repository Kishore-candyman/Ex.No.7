# Exno.7-Develop a prompt-based application tailored to their personal needs, fostering creativity and practical problem-solving skills while leveraging the capabilities of large language models.

# Date: 31/10/2025
# Register no.: 212224040161
# Aim: To develop a prompt-based application using ChatGPT - To demonstrate how to create a prompt-based application to organize daily tasks, showing the progression from simple to more advanced prompt designs and their corresponding outputs.

# AI Tools Required: ChatGPT,VScode/pycharm


# Explanation: 
Prompt:
"Design a personal productivity assistant that can help manage daily tasks, schedule reminders, suggest wellness tips, and answer general queries. The assistant should interact using natural language and be adaptable to the user’s changing preferences over time."
Procedure:
1. Define the core requirements of a personal productivity assistant.
2. Identify and construct appropriate prompts for each task using an LLM (e.g., ChatGPT).
3. Simulate natural user interaction through a simple interface or command-line system.
4. Collect feedback or inputs from users and adapt responses accordingly.
5. (Optional) Integrate basic memory to simulate preference adaptation.
EXPECTED OUTPUT: - (attached the drive link)
Output (Example Response by LLM):
Personal Productivity Assistant Features:
1. Daily Task Manager:
o Accept tasks via natural language (e.g., "Remind me to call mom at 6 PM").
o Organize tasks by priority and deadline.
o Provide daily summaries and pending items.
2. Smart Scheduler:
o Schedule events and set reminders using contextual understanding.
o Notify user of overlapping appointments or free time slots.
3. Wellness Tips Generator:
o Suggest daily wellness advice (hydration, exercise, screen-time breaks).
o Adapt suggestions based on past user preferences and responses.

# OUTPUT:
### **1. Core Requirements**

The personal productivity assistant should:

* **Understand and respond in natural language**, enabling users to interact conversationally.
* **Manage daily tasks**, including creating, updating, deleting, and summarizing them.
* **Schedule reminders** and **set events** contextually (time, date, recurrence).
* **Suggest wellness tips** (hydration, posture, mindfulness, exercise) based on usage patterns.
* **Answer general queries** (knowledge, productivity tips, or contextual help).
* **Adapt to user preferences over time**, learning from user behavior, frequency, and responses.

---

### **2. Tasks & LLM Prompt Design**

| **Functionality**          | **Example LLM Prompt / Instruction**                                                 | **Expected LLM Behavior**                                         |
| -------------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| **Daily Task Manager**     | “Add a task to finish my physics assignment by 5 PM.”                                | Parses task, extracts title and deadline, adds to daily list.     |
| **Task Summary**           | “What do I need to do today?”                                                        | Returns a list of tasks due today with priorities.                |
| **Reminder Creation**      | “Remind me to submit the report every Friday at 3 PM.”                               | Identifies recurrence and schedules reminder.                     |
| **Wellness Tip Generator** | “Give me a wellness tip for today.”                                                  | Suggests contextually relevant tip (e.g., hydration, stretching). |
| **General Query Handler**  | “What’s the best way to improve focus?”                                              | Provides short, evidence-based productivity advice.               |
| **Preference Adaptation**  | (After repeated interactions) “I usually prefer reminders 15 minutes before events.” | Stores user preference and adjusts default reminder time.         |

---

### **3. Simulated User Interaction (CLI-based Example)**

```
> Hello, I’m your productivity assistant. How can I help today?

User: Add “Study Quantum Computing” every weekday at 7 PM.
Assistant: Task added: "Study Quantum Computing" scheduled for weekdays at 7 PM.

User: Show my tasks for tomorrow.
Assistant: Tomorrow’s tasks:
1. Study Quantum Computing – 7:00 PM
2. Submit assignment – 5:00 PM

User: Remind me to drink water every 2 hours.
Assistant: Hydration reminder set for every 2 hours.

User: I’m feeling tired.
Assistant: Take a short 5-minute break and stretch your legs. Would you like me to remind you again in 30 minutes?

User: Yes, please.
Assistant: Got it! I’ll check in after 30 minutes.
```

---

### **4. Adaptation and Feedback Simulation**

* After repeated “yes” responses to stretch reminders → the assistant increases wellness prompts.
* If the user ignores reminders after 9 PM → assistant stops sending night-time notifications.
* If user keeps rescheduling morning tasks → assistant learns to default morning tasks to later times.

**Example Adaptation Log:**

```
Preference Update:
- Preferred reminder lead time: 15 minutes
- Active hours: 9 AM – 9 PM
- Favorite wellness type: short breathing exercises
```

---

### **5. (Optional) Basic Memory Integration**

A simple key-value memory store (JSON / local file) keeps:

```json
{
  "preferred_reminder_time": "15 minutes before",
  "work_hours": "9:00 - 21:00",
  "favorite_wellness_tip": "breathing exercise",
  "ignored_time_slots": ["after 22:00"]
}
```

* On startup, the assistant loads this memory.
* Future responses respect stored preferences automatically.

---

### **6. Summary of Features**

#### **Daily Task Manager**

* Accepts natural language task input (e.g., “Remind me to call mom at 6 PM”).
* Organizes tasks by **priority**, **deadline**, and **recurrence**.
* Provides daily and weekly **task summaries**.

#### **Smart Scheduler**

* Schedules events and reminders contextually.
* Detects conflicts (overlapping events) and suggests free slots.
* Suggests optimal scheduling based on previous behavior.

#### **Wellness Tips Generator**

* Provides daily wellness suggestions (hydration, mindfulness, posture, exercise).
* Adapts tips to user’s feedback and engagement history.

#### **Query Responder**

* Answers general knowledge or productivity-related questions.
* Maintains conversational flow using natural language.

#### **Adaptive Personalization**

* Learns user habits and modifies responses accordingly.
* Adjusts reminder times, notification tone, and tip frequency.

---

### **7. Example Architecture (Conceptual)**

```
User Input → NLP Processor → Intent Recognition
       ↓
 Task/Reminder Manager   Wellness Engine   Query Module
       ↓                         ↓             ↓
  Local Memory & Preference Store (Adaptive Layer)
       ↓
     Natural Language Generator → User Response
```

---


## 🧩 **STEP 1 — Interactive REPL (Command-Line Version)**

Below is the improved **Python interactive version** of your *Personal Productivity Assistant*.
You can copy and run this as `assistant_repl.py`.

It lets you type commands like:

* `add study quantum computing every weekday at 7pm`
* `show tasks`
* `remind 15`
* `wellness`
* `exit`

---

### 🧠 Code: `assistant_repl.py`

```python
import json, os
from datetime import datetime, timedelta

MEMORY_PATH = "assistant_memory.json"

class Assistant:
    def __init__(self):
        self.tasks = []
        self.memory = self.load_memory()
        self.now = datetime.now().replace(second=0, microsecond=0)

    def load_memory(self):
        if os.path.exists(MEMORY_PATH):
            with open(MEMORY_PATH, "r") as f:
                return json.load(f)
        return {
            "preferred_reminder_lead_min": 15,
            "active_hours": ["09:00", "21:00"],
            "wellness_pref": "breathing",
            "wellness_prompt_count": 0
        }

    def save_memory(self):
        with open(MEMORY_PATH, "w") as f:
            json.dump(self.memory, f, indent=2)

    def add_task(self, text):
        parts = text.split(" at ")
        title = parts[0].replace("add", "").strip().capitalize()
        when_text = parts[1] if len(parts) > 1 else ""
        due, recur = None, None
        wt = when_text.lower()
        if "every" in wt:
            recur = wt
        elif "tomorrow" in wt:
            due = (self.now + timedelta(days=1)).replace(hour=17, minute=0)
        else:
            wt2 = wt.replace(" ", "")
            if wt2.endswith("pm") or wt2.endswith("am"):
                suffix = wt2[-2:]
                hh = int(wt2[:-2])
                if suffix == "pm" and hh != 12: hh += 12
                if suffix == "am" and hh == 12: hh = 0
                due = self.now.replace(hour=hh, minute=0)
        task = {"title": title, "due": due.isoformat() if due else None, "recur": recur, "status": "todo"}
        self.tasks.append(task)
        print(f"✅ Task added: '{title}' at {due.strftime('%Y-%m-%d %H:%M') if due else 'recurring'}")

    def show_tasks(self):
        if not self.tasks:
            print("No tasks yet.")
            return
        print("\n📋 Your Tasks:")
        for i, t in enumerate(self.tasks, 1):
            due = t["due"][:16].replace("T", " ") if t["due"] else "Recurring"
            print(f" {i}. {t['title']} — {due} — {t['status']}")

    def wellness(self):
        self.memory["wellness_prompt_count"] += 1
        pref = self.memory.get("wellness_pref", "breathing")
        tip = "Take 3 deep breaths and stretch." if pref == "breathing" else "Take a short 5-minute walk."
        print("💡 Wellness Tip:", tip)
        if self.memory["wellness_prompt_count"] >= 3:
            self.memory["wellness_pref"] = "stretch"
            print("🧠 Adapting: I’ll suggest more stretch breaks.")
            self.save_memory()

    def set_pref_reminder(self, mins):
        self.memory["preferred_reminder_lead_min"] = mins
        self.save_memory()
        print(f"⏰ Default reminders now {mins} minutes before events.")

def main():
    print("🤖 Personal Productivity Assistant (type 'help' for commands)")
    a = Assistant()
    while True:
        cmd = input("\nYou: ").lower()
        if cmd.startswith("add "):
            a.add_task(cmd)
        elif cmd.startswith("show"):
            a.show_tasks()
        elif cmd.startswith("wellness"):
            a.wellness()
        elif cmd.startswith("remind"):
            try:
                mins = int(cmd.split()[1])
                a.set_pref_reminder(mins)
            except:
                print("Usage: remind <minutes>")
        elif cmd in ["exit", "quit"]:
            print("👋 Goodbye!")
            break
        elif cmd == "help":
            print("Commands:\n add <task> at <time>\n show\n wellness\n remind <mins>\n exit")
        else:
            print("I didn’t understand that. Try 'help'.")

if __name__ == "__main__":
    main()
```

---

### 🧪 Example Run:

```
🤖 Personal Productivity Assistant
You: add study quantum computing at 7pm
✅ Task added: 'Study quantum computing' at 2025-10-31 19:00
You: show
📋 Your Tasks:
 1. Study quantum computing — 2025-10-31 19:00 — todo
You: wellness
💡 Wellness Tip: Take 3 deep breaths and stretch.
You: remind 10
⏰ Default reminders now 10 minutes before events.
You: exit
👋 Goodbye!
```

---

## 🌐 **STEP 2 — OpenAPI / Backend Integration Specification**

This step defines how you can connect the assistant to a backend or app.

### **Overview**

| Feature            | Endpoint       | Method | Input                                      | Output                                              |
| ------------------ | -------------- | ------ | ------------------------------------------ | --------------------------------------------------- |
| Add Task           | `/tasks`       | POST   | `{ "title": "call mom", "time": "18:00" }` | `{ "status": "added", "id": 1 }`                    |
| List Tasks         | `/tasks`       | GET    | —                                          | `[ { "id":1, "title":"call mom", "time":"18:00"} ]` |
| Delete Task        | `/tasks/{id}`  | DELETE | —                                          | `{ "status": "deleted" }`                           |
| Get Wellness Tip   | `/wellness`    | GET    | —                                          | `{ "tip":"Take a 5-minute walk." }`                 |
| Get Preferences    | `/preferences` | GET    | —                                          | `{ "reminderLead":15 }`                             |
| Update Preferences | `/preferences` | PUT    | `{ "reminderLead":10 }`                    | `{ "status":"updated" }`                            |

---

### **API Example (JSON)**

```json
POST /tasks
{
  "title": "Finish Quantum Homework",
  "time": "2025-11-01T17:00:00"
}

Response:
{
  "status": "added",
  "task_id": 42
}
```

---

### **Architecture (Simplified)**

```
Frontend (Web / Mobile)
        ↓
  REST API Gateway
        ↓
  Task Manager Module  →  stores tasks in DB
  Wellness Engine       →  AI wellness tip generator
  Preference Memory     →  user.json
```

---

## 💻 **STEP 3 — React Web UI Prototype**

Here’s a minimal **React + Tailwind + shadcn/ui** prototype that could represent your assistant visually.

---

### ⚛️ Code (React)

```jsx
import React, { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";

export default function ProductivityAssistant() {
  const [tasks, setTasks] = useState([]);
  const [input, setInput] = useState("");
  const [tip, setTip] = useState("");

  const addTask = () => {
    if (!input.trim()) return;
    const newTask = { title: input, time: new Date().toLocaleTimeString() };
    setTasks([...tasks, newTask]);
    setInput("");
  };

  const getTip = () => {
    const tips = [
      "Take a 5-minute walk.",
      "Hydrate yourself.",
      "Do 3 deep breaths.",
      "Stretch your neck and shoulders."
    ];
    setTip(tips[Math.floor(Math.random() * tips.length)]);
  };

  return (
    <div className="min-h-screen bg-gray-100 flex flex-col items-center p-6 space-y-6">
      <Card className="w-full max-w-md shadow-lg">
        <CardContent className="space-y-4">
          <h1 className="text-xl font-semibold">🧠 Personal Productivity Assistant</h1>

          <div className="flex space-x-2">
            <Input
              placeholder="Add a new task..."
              value={input}
              onChange={(e) => setInput(e.target.value)}
            />
            <Button onClick={addTask}>Add</Button>
          </div>

          <div>
            <h2 className="font-medium mt-4">📋 Tasks:</h2>
            {tasks.map((t, i) => (
              <p key={i} className="text-sm mt-1">• {t.title} <span className="text-gray-500">({t.time})</span></p>
            ))}
          </div>

          <div className="mt-6">
            <Button variant="secondary" onClick={getTip}>Get Wellness Tip</Button>
            {tip && <p className="mt-3 text-blue-600 font-medium">💡 {tip}</p>}
          </div>
        </CardContent>
      </Card>
    </div>
  );
}
```

---

### 🪄 Features of Web UI

* Add tasks interactively.
* Display current task list dynamically.
* Get random wellness tips.
* Can be extended to connect to the API above.

---

✅ **Summary of Deliverables**

| Step | Deliverable         | Description                               |
| ---- | ------------------- | ----------------------------------------- |
| 1    | `assistant_repl.py` | Interactive terminal-based assistant      |
| 2    | OpenAPI Spec        | Backend endpoints for integration         |
| 3    | React Prototype     | Simple web interface for user interaction |

---


# Result: 
The lab exercise resulted in the creation of a prototype concept for a personal assistant powered by large language models. Students were able to:
 Understand how to tailor LLM prompts to real-life applications.
 Foster creativity by designing features suited to their personal or academic lives.
 Learn prompt engineering techniques for optimal interaction with AI tools.
 Experience the versatility and utility of generative AI in solving everyday problems.

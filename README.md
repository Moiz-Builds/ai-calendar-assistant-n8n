 <div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:0b3a5c,100:0ea5e9&height=220&section=header&text=AI%20Calendar%20Assistant&fontSize=46&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Manage%20Google%20Calendar%20by%20just%20chatting%20on%20Telegram&descAlignY=60&descSize=16&descColor=9fd8ff" alt="AI Calendar Assistant banner" />

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=38BDF8&center=true&vCenter=true&width=820&lines=Book+meetings+by+sending+one+message;Search+your+schedule+in+seconds;Reschedule+without+opening+a+calendar;Cancel+events+with+a+single+text;Powered+by+n8n+and+Google+Gemini" alt="Animated project summary" />

<br/>

<a href="#about"><img src="https://img.shields.io/badge/About-0d1117?style=for-the-badge&labelColor=0d1117&color=0ea5e9" alt="About" /></a>
<a href="#features"><img src="https://img.shields.io/badge/Features-0d1117?style=for-the-badge&labelColor=0d1117&color=0ea5e9" alt="Features" /></a>
<a href="#how"><img src="https://img.shields.io/badge/Architecture-0d1117?style=for-the-badge&labelColor=0d1117&color=0ea5e9" alt="Architecture" /></a>
<a href="#nodes"><img src="https://img.shields.io/badge/Nodes-0d1117?style=for-the-badge&labelColor=0d1117&color=0ea5e9" alt="Nodes" /></a>
<a href="#stack"><img src="https://img.shields.io/badge/Tech_Stack-0d1117?style=for-the-badge&labelColor=0d1117&color=0ea5e9" alt="Tech Stack" /></a>
<a href="#setup"><img src="https://img.shields.io/badge/Setup-0d1117?style=for-the-badge&labelColor=0d1117&color=0ea5e9" alt="Setup" /></a>

<br/><br/>

<img src="https://img.shields.io/badge/n8n-Workflow_Automation-EA4B71?style=flat-square&labelColor=0d1117" alt="n8n" />
<img src="https://img.shields.io/badge/Telegram-Bot_Interface-26A5E4?style=flat-square&logo=telegram&logoColor=white&labelColor=0d1117" alt="Telegram" />
<img src="https://img.shields.io/badge/Google_Gemini-AI_Model-8E75B2?style=flat-square&logo=googlegemini&logoColor=white&labelColor=0d1117" alt="Google Gemini" />
<img src="https://img.shields.io/badge/Google_Calendar-Events-4285F4?style=flat-square&logo=googlecalendar&logoColor=white&labelColor=0d1117" alt="Google Calendar" />
<img src="https://img.shields.io/badge/PostgreSQL-Chat_Memory-4169E1?style=flat-square&logo=postgresql&logoColor=white&labelColor=0d1117" alt="PostgreSQL" />

<br/><br/>

<img src="assets/telegram-conversation.gif" alt="Live demo of the assistant on Telegram" width="320" />

</div>

<a id="about"></a>

## About

Managing a calendar through a mobile app means tapping through multiple screens just to create one event. Most users already live inside a chat app, so this project brings calendar management directly into Telegram.

Send a plain-language message to the Telegram bot. The AI assistant interprets the request, selects the appropriate calendar operation, executes it through Google Calendar, and returns the result in the same chat.

**No forms. No menus. No manual calendar navigation. Just chat.**

**Sample requests the assistant can handle:**

<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=18&pause=1200&color=22C55E&center=true&vCenter=true&width=800&lines=What+are+my+events+next+week%3F;Schedule+a+society+meeting+next+Wednesday+at+9+AM;Show+me+the+details+of+next+week%27s+events;Add+Islamabad+as+the+location+of+the+society+meeting;Delete+the+webinar+scheduled+for+Monday" alt="Example messages sent to the assistant" />

<br/>

<img src="assets/calendar-result.png" alt="Event created on Google Calendar from a Telegram message" width="720" />

</div>

### At a Glance

| Metric | Value |
|---|---|
| Workflow nodes | 10 |
| Calendar operations | 4 — Create, Search, Update, Delete |
| AI model | Google Gemini |
| Memory store | PostgreSQL |
| Default event duration | 30 minutes |
| Default timezone | Asia/Karachi |

---

<a id="features"></a>

## Features

- **Natural Language Input:** Manage calendar events using everyday messages instead of commands.
- **Full Calendar CRUD:** Create, search, update, and delete Google Calendar events.
- **AI-Powered Tool Selection:** Google Gemini helps the agent determine which calendar operation matches the user's request.
- **Conversation Memory:** PostgreSQL-backed chat history supports follow-up requests such as "Move it to 3 PM."
- **Live Feedback:** A typing indicator lets users know that their request is being processed.
- **Error Handling:** Retry-on-fail settings on configured nodes and a separate Error Trigger workflow help manage failures.
- **Sensible Defaults:** The assistant uses the `Asia/Karachi` timezone and a default event duration of 30 minutes when configured to do so.
- **Conversational Interaction:** Users can manage their calendar without learning special bot commands.

---

<a id="how"></a>

## Architecture

The workflow connects Telegram, an AI agent, a language model, conversation memory, and Google Calendar operations.

### Workflow Diagram

```mermaid
flowchart TD
    A["Telegram Trigger"] --> B["Show Typing Indicator"]
    A --> C["AI Agent"]

    G["Google Gemini Chat Model"] -.-> C
    M[("Postgres Chat Memory")] -.-> C

    C --> T1["Create Google Calendar Event"]
    C --> T2["Search Google Calendar Events"]
    C --> T3["Update Google Calendar Event"]
    C --> T4["Delete Google Calendar Event"]

    T1 --> R["Send Reply"]
    T2 --> R
    T3 --> R
    T4 --> R
    C --> R

    classDef telegram fill:#123047,stroke:#26A5E4,color:#ffffff
    classDef agent fill:#163b35,stroke:#22c55e,color:#ffffff
    classDef model fill:#30234a,stroke:#a78bfa,color:#ffffff
    classDef calendar fill:#3b2d16,stroke:#f59e0b,color:#ffffff
    classDef reply fill:#123047,stroke:#38bdf8,color:#ffffff

    class A,B telegram
    class C agent
    class G,M model
    class T1,T2,T3,T4 calendar
    class R reply
```

### Request Lifecycle

1. **Receive:** The Telegram Trigger receives a message and starts the workflow.
2. **Acknowledge:** The Show Typing Indicator node provides feedback while the request is processed.
3. **Understand:** The AI Agent uses Google Gemini to interpret the request. Postgres Chat Memory provides relevant conversation history.
4. **Select and Execute:** The agent chooses the appropriate Google Calendar tool to create, search, update, or delete an event.
5. **Respond:** The Send Reply node sends the result to the user's Telegram chat.

<div align="center">

<img src="assets/workflow.png" alt="n8n workflow canvas showing the AI Calendar Assistant nodes" width="900" />

</div>

<a id="nodes"></a>

## Nodes

The workflow contains 10 nodes, each responsible for a specific part of the automation.

| Node | Responsibility |
|---|---|
| **Telegram Trigger** | Receives incoming Telegram messages and starts the workflow. |
| **Show Typing Indicator** | Displays a typing indicator while the assistant processes a request. |
| **AI Agent** | Interprets the request and decides which calendar tool to use. |
| **Google Gemini Chat Model** | Provides language understanding and generates AI responses. |
| **Postgres Chat Memory** | Stores and retrieves conversation history for follow-up messages. |
| **Create Google Calendar** | Creates new calendar events. |
| **Search Google Calendar** | Retrieves existing calendar events. |
| **Update Google Calendar** | Modifies existing event details. |
| **Delete Google Calendar** | Deletes selected calendar events. |
| **Send Reply** | Sends the assistant's response to Telegram. |

---

<a id="stack"></a>

## Tech Stack

| Component | Role |
|---|---|
| **n8n Cloud** | Workflow automation and orchestration |
| **Telegram Bot API** | Chat interface for user interaction |
| **Google Gemini** | Language understanding and decision-making |
| **Google Calendar API** | Calendar event management |
| **PostgreSQL** | Conversation memory persistence |
| **JSON** | Workflow export and import format |
| **Mermaid** | Architecture diagram rendering |

---

<a id="structure"></a>

## Repository Structure

```text
.
├── README.md
├── <workflow>.json
└── assets/
    ├── telegram-conversation.gif
    ├── calendar-result.png
    └── workflow.png
```

**File descriptions:**

- `README.md` — Project documentation and setup instructions.
- `<workflow>.json` — Exported n8n workflow; replace this placeholder with your actual filename.
- `telegram-conversation.gif` — Animated Telegram conversation demo.
- `calendar-result.png` — Screenshot of a calendar event created by the assistant.
- `workflow.png` — Screenshot of the n8n workflow canvas.

---

<a id="setup"></a>

## Setup

### Prerequisites

Before importing the workflow, prepare the following:

- An **n8n Cloud** account or a compatible self-hosted n8n instance.
- A **Telegram bot token** from [@BotFather](https://t.me/BotFather).
- A **Google Gemini API key**.
- A **Google account** with Google Calendar access.
- A **PostgreSQL database** for chat memory.

### Installation

1. **Download the workflow:** Obtain the exported n8n JSON file from this repository.
2. **Import into n8n:** Open n8n and select the workflow import option to import the JSON file.
3. **Configure Telegram:** Create Telegram credentials and attach them to the Telegram Trigger, Show Typing Indicator, and Send Reply nodes.
4. **Configure Gemini:** Add your Gemini API credentials to the Google Gemini Chat Model node.
5. **Configure Google Calendar:** Connect your Google account using OAuth and assign the appropriate credentials to all four calendar tools.
6. **Configure PostgreSQL:** Enter the database connection details in the Postgres Chat Memory node and verify that the database is reachable.
7. **Review node settings:** Check the AI Agent instructions, calendar tool parameters, memory session identifier, and timezone settings.
8. **Activate and test:** Activate the workflow and send a test message to your Telegram bot.

> **Important:** The exported workflow does not provide your credentials. You must connect your own Telegram bot, Google account, Gemini API key, and PostgreSQL database. Calendar operations will affect the Google Calendar associated with your configured credentials.

## Usage Examples

Send any of the following messages to your Telegram bot:

| Request | Expected Action |
|---|---|
| `Schedule my class meeting tomorrow at 10 AM` | Create an event |
| `What events do I have this week?` | Search events |
| `Move my meeting to 3 PM` | Update an event, if it can identify the correct meeting |
| `Delete the class meeting` | Delete the matching event |
| `Add Islamabad as the location of my meeting` | Update the event location |
| `Show me the details of my next meeting` | Search and return event details |

## Future Improvements

- Add confirmation before deleting or significantly changing events.
- Detect scheduling conflicts before creating events.
- Support recurring events and reminders.
- Improve handling of ambiguous event names and relative dates.
- Add automated tests for common calendar operations.
- Expand error notifications and execution monitoring.

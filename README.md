# Calendar Chatbot

A conversational chatbot tailored to run in **Google Colab**, integrating OpenAI’s GPT-3.5-turbo with Google Calendar via AgentPro. This Colab-based chatbot can:

- Summarise your daily and weekly events in natural language  
- Reschedule existing events via plain-English commands  
- Return the current date and time (intended to show Pakistan Standard Time)  
- (Optionally) Consult the internet for public‐holiday checks via AresInternetTool  

The interface is exposed through Gradio in Colab, making it simple to share and interact with your calendar entirely within a notebook environment.

---

## 🔍 Project Overview

The Calendar Chatbot leverages a ReAct-style agent (AgentPro’s `ReactAgent`) to combine reasoning with external “tools.” The agent is powered by OpenAI’s GPT-3.5-turbo, while Google Calendar API provides real-time event data. AresInternetTool can be added for holiday lookups or other web searches.

**Key Components:**
1. **AgentPro + ReactAgent**  
   - Manages step-by-step reasoning:  
     1. **Thought** (LLM decides next step)  
     2. **Action** (calls a custom tool)  
     3. **Observation** (receives results from tool)  
     4. **Final Answer**  
2. **Custom Tools**  
   - **DailyEventSummaryTool**: Fetches and narrates events for any requested day (e.g., “today,” “tomorrow,” “June 10, 2025,” or “last day of this year”).  
   - **ModifyEventTool**: Parses commands like “Shift my first meeting on Friday to Monday, 3 PM” and updates the Google Calendar event accordingly.  
   - **CurrentDateTimeTool**: Returns current weekday, time, date, year, and month (intended as Pakistan Standard Time).  
   - **AresInternetTool**: (Optional) Searches the internet to check for holiday conflicts or fetch additional information.    
3. **Gradio Interface (Colab)**  
   - A simple two-line textbox → text output chat interface.  
   - Calls `agent.run(user_input)` and displays `response.final_answer`.  
   - Launched with `iface.launch(share=True)` for easy sharing directly from the Colab notebook.  

---

## 🚀 Features

- **Natural-Language Summaries**  
  - “What’s on my calendar today?” →  
    “On June 06, 2025, first meeting is ‘Team Sync’ at 10:00 AM–11:00 AM (1 hour), and second meeting is ‘Project Update’ at 2:00 PM–2:30 PM (30 minutes).”

- **Event Rescheduling**  
  - “Shift my first meeting on Friday to Monday at 3 PM for 90 minutes.” →  
    “Updated ‘Team Sync’: it now starts on Monday, June 09, 2025 03:00 PM and ends at 04:30 PM.”

- **Current Date & Time**  
  - “What time is it?” →  
    “Day of week: Friday  
     Current time: 07:45 PM  
     Date: 06 June 2025  
     Year: 2025  
     Month: June”

- **Public-Holiday Checks (Optional)**  
  - By integrating AresInternetTool, you can query:  
    “Is June 09, 2025 a public holiday in Pakistan?” before rescheduling an event.  

- **Easy Sharing via Gradio**  
  - Runs entirely in Google Colab.  
  - `iface.launch(share=True)` generates a publicly accessible URL.  
  - No extra front-end setup required; just run the notebook and interact.

---

## 🔧 Installation & Setup (Google Colab)

1. **Open a new Colab notebook** (or copy the provided notebook).  
2. **Upload `credentials.json`** (Google OAuth2 client secrets) via:
   ```python
   from google.colab import files
   files.upload()  # Upload your credentials.json

## Guideline 

**Google OAuth Flow**  
1. When prompted, you will see a Google authorization URL printed in the Colab output.  
2. Click the URL and grant access to your Google Calendar.  
3. After granting permission, Google will redirect you to an (invalid) page and show an error.  
   - **Don’t panic**—this is expected.  
4. Copy the entire URL from your browser’s address bar (look for the `code=…` parameter).  
5. Paste that full URL back into the Colab input prompt.  
6. The code value after `code=` is what the authentication function needs.  
7. Once entered, `token.pkl` is generated, and authentication completes.  


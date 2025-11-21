# CoHub — Collaboration Intelligence

> **Work smarter, not alone.**  
> CoHub automatically connects employees to colleagues who have already solved similar problems — directly inside their workflow.

---

##  What is CoHub?

CoHub is an internal **knowledge & people discovery assistant** for HPS.

When someone is working on a task CoHub:

1. Finds **similar past projects** (documents, case studies, internal assets).
2. Surfaces the **best people to talk to** based on skills, languages, and availability.
3. Lets you **contact them in one click** (email, Teams) or explore a detailed **technical sheet** for each person.

CoHub lives in two places:

- a **web app** for rich exploration, and  
- a **Chrome extension** that nudges you *in context* when you’ve been stuck on a task for a while.

---

##  Why CoHub?

Large organizations like **HPS** already have the answer to many problems — it’s just locked inside:

- past **projects** and proposals,  
- the heads of **subject-matter experts**, and  
- scattered tools (SharePoint, OneDrive, Outlook, Teams, …).

Result: people **reinvent the wheel**, repeat analyses, or don’t know *who* to ask.

CoHub bridges this gap:

- **Project-level intelligence** – “Who has done something like this before?”
- **People-level intelligence** – “Who can help me right now, in my language and time zone?”
- **Workflow-native** – suggestions appear *while* you are working, not in a separate portal.

---

##  Key Features

### 1. Semantic Project Search

- Natural-language search bar:  
  `“AI compliance in French banking on Azure (FR/EN)”`,  
  `“Kafka migration for MENA telco, low downtime, Arabic”`, etc.
- Returns **similar internal projects** with:
  - Title, domain, country, year
  - Short summary
  - Tech stack **(Kafka, Azure, ETL, AI, …)**
  - Similarity score
  - Link to **“View doc”** (SharePoint / OneDrive / internal docs)

### 2. Recommended People

For each query, CoHub suggests **people who are a strong fit**:

- Role, office (e.g. *Data Scientist — Casablanca*)
- Languages (FR/EN/AR badges)
- Key skills (**Kafka, Azure, AML, Compliance…**)
- Availability (e.g. *60% free*)
- Smart **reason badges**:  
  `skill match: Kafka, ETL` · `languages: fr/en` · `availability: 60%` · `overall 82%`

Actions per profile:

- **View technical sheet** (projects they worked on, skills, availability, time zone)
- **Email** (opens a pre-filled intro email)
- **Chat in Teams** (deep link to a 1:1 conversation)
- Optional: **“Build best-fit 3-person team”** helper.

### 3. Technical Sheet (Profile Modal)

A richer view for each person:

- Title, office, languages, skills
- Availability %
- Time zone, working hours, and **“Likely online now / Outside hours”** indicator
- Local time label (e.g. `Local time: 15:42`)
- Linked & affiliated projects:
  - Role, domain, year
  - Match scores (for skill overlap)
  - Links to documentation

### 4. One-Click Outreach

- **Email composer modal** in the app:
  - Pre-fills **To**, **Subject**, and **Body** with a professional intro.
  - Uses `mailto:` to open the user’s email client.
  - Shows a small toast: **“Email sent successfully!”**
- **Teams chat** deep links:
  - `Chat in Teams` opens a chat with the selected person.

### 5. Chrome Extension — CoHub Helper

- **Manifest V3** extension with a background service worker.
- Tracks focus on the current tab & domain.
- After focused work for a while (configurable), shows a **nudge notification**:

  > “You’ve been focused on your current task for a while. Want help finding similar projects & the right people?”

- Notification buttons:
  - **Open CoHub** – opens the web app at `http://localhost:3000/` or the deployed URL.
  - **Dismiss** – closes the suggestion.
- Popup UI:
  - “Open CoHub” button
  - “Reset timer” button
  - Test notification trigger for demos.

---

##  Architecture & Tech Stack

### Frontend (Web App)

- **Next.js** (React, App Router)
- **TypeScript / JavaScript**
- **Tailwind CSS** for styling
- Email & Teams links integrated into each card
- Responsive 2-column layout:
  - Left: **Similar Projects**
  - Right: **Recommended People**

### Backend & Data

- **Supabase (PostgreSQL + pgvector)**  
  - Tables for `projects`, `people`, and project-people links.
  - SQL functions:
    - `match_projects` – vector similarity on project embeddings.
    - `match_people` – vector similarity on people embeddings.
    - `project_contributors` – fetch contributors per project.
- **OpenAI Embeddings API**
  - Converts user queries and documents into vectors.
  - Used in Supabase RPC functions to do semantic search.

Custom scoring:

- combines **project similarity**, **skill overlap**, **language match**, and **availability** into a unified `_score`
- adds a **Top fit** badge when scores cross a threshold.

### Chrome Extension

- **Manifest v3** with:
  - `background.js` as a **service worker**
  - `popup.html` + `popup.js` for the browser action
  - `icons/` for 16/48/128px extension icons
- Uses:
  - `chrome.alarms` – periodic nudges
  - `chrome.notifications` – rich notifications with buttons
  - `chrome.tabs` – to open the CoHub web app
  - `chrome.runtime.onMessage` – communication from popup to background

### Future Integration (HPS Context)

Designed to align with HPS’s existing infrastructure:

- **Microsoft SharePoint / OneDrive** – project documents & knowledge assets
- **Outlook email metadata** – to understand collaboration patterns
- **Microsoft Teams / Office 365** – for direct collaboration
- Existing **APIs & security layers**
- Deployed in **cloud environments** (Azure, etc.)





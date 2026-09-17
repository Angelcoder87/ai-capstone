# CueFlow AI

> **Turn conversations into commitments. Keep people accountable.**

CueFlow AI is an AI-powered accountability and workflow platform that identifies important commitments, action items, decisions, deadlines, and unresolved questions from conversations.

Instead of allowing important tasks to get buried in meetings, voice notes, or transcripts, CueFlow AI turns them into structured, trackable follow-ups that users can review, confirm, and manage.

The capstone MVP focuses on converting uploaded audio and transcripts into verified action items and follow-up tasks.

---

## The Problem

Important work is often agreed upon in conversations but never properly recorded.

During meetings and discussions, people regularly say things like:

- "I'll send that tomorrow."
- "Brian will contact the supplier."
- "Let's confirm this before Friday."
- "We still need to decide who is responsible for this."
- "I'll follow up with the client next week."

These commitments can easily get lost in meeting notes, chat messages, voice recordings, or people's memory.

Traditional task-management tools require users to manually create tasks after the conversation.

**CueFlow AI aims to capture those commitments automatically.**

---



## The Solution

CueFlow AI analyzes a conversation and identifies information that requires action.

### Core workflow

```text
Conversation
     ↓
Audio / Transcript
     ↓
AI Analysis
     ↓
Action & Commitment Extraction
     ↓
Human Verification
     ↓
Follow-up Task
     ↓
Tracking
     ↓
Completion

```

The AI does not automatically assume that every extracted item is correct.

Users can review, edit, confirm, or dismiss AI-generated action items before they become active tasks.

This human-in-the-loop approach helps reduce errors and gives users control over what enters their workflow.

---



# Capstone MVP

The first version of CueFlow AI will focus on a small, achievable core experience.

## 1. User Authentication

Users can:

- Create an account
- Log in
- Log out
- Manage their basic profile

Authentication will use JWT and bcrypt.

---



## 2. Conversation Input

Users can provide a conversation through:

- Audio upload
- Transcript/text input

The initial version will not require direct integrations with external communication platforms.

Future versions can support services such as email, messaging, and collaboration platforms.

---



## 3. AI Conversation Analysis

The AI analyzes the conversation and identifies:

### Action Items

Tasks that someone needs to complete.

Example:

> Send the revised quotation to Sarah.



### Assignees

Who is responsible for the action.

Example:

> Brian



### Deadlines

When the action should be completed.

Example:

> Friday, September 18



### Priority

The system can identify the apparent priority of an action.

Example:

> High



### Decisions

Important decisions made during the conversation.

Example:

> The team agreed to use Supplier B.



### Unresolved Questions

Important issues that remain unanswered.

Example:

> Can the supplier deliver 500 units before September 30?

---



# 4. AI Verification

AI-generated information should not automatically become a task.

Users will be shown the extracted information and can:

- Confirm it
- Edit it
- Assign a person
- Change the deadline
- Change the priority
- Dismiss it

Example:

```text
AI DETECTED AN ACTION

Send revised quotation

Assigned to:
Brian

Deadline:
Friday

Priority:
High

Confidence:
High

[ Confirm ]   [ Edit ]   [ Dismiss ]

```

This creates a human verification layer between AI interpretation and task creation.

---



# 5. Follow-up Tasks

Once an action item is confirmed, it becomes a trackable task.

Each task can contain:

- Title
- Description
- Assignee
- Due date
- Priority
- Source conversation
- Status
- Creation date



### Task statuses

```text
Pending
   ↓
In Progress
   ↓
Completed

```

Tasks can also become **Overdue** when their deadline passes without completion.

---



# 6. Dashboard

The dashboard gives users a simple overview of their commitments.

### Example

```text
Good morning, Angela

Your Follow-ups

12   Open
4    Due Today
3    Overdue
18   Completed

```

Users can quickly see:

- Open tasks
- Tasks due today
- Upcoming deadlines
- Overdue tasks
- Recently completed tasks

---

# 7. Task Management

Users can:

- View tasks
- Edit tasks
- Change status
- Change priority
- Change assignee
- Update deadlines
- Mark tasks as completed
- Delete or dismiss tasks

Tasks should remain connected to their original conversation whenever possible.

This allows users to understand **where a commitment came from**.

---

# 8. Conversation History

Users can view previously processed conversations.

Each conversation can display:

- Original transcript
- Summary
- Extracted action items
- Decisions
- Unresolved questions
- Confirmed tasks

This creates a searchable history of commitments and decisions.

---

# Core Data Model

The initial application will use the following core entities:

```text
User
 ├── Conversations
 │      ├── Transcript
 │      ├── AI Analysis
 │      ├── Action Items
 │      ├── Decisions
 │      └── Questions
 │
 └── Tasks
        ├── Assignee
        ├── Deadline
        ├── Priority
        └── Status

```

The database will use PostgreSQL.

---

# AI Architecture

The AI layer is responsible for converting unstructured conversation data into structured information.

### Input

```text
Audio / Transcript

```



### Processing

```text
Transcription
      ↓
Conversation Analysis
      ↓
Structured Extraction
      ↓
Confidence / Validation

```



### Output

```json
{
  "action_items": [
    {
      "title": "Send revised quotation",
      "assignee": "Brian",
      "deadline": "2026-09-18",
      "priority": "high"
    }
  ],
  "decisions": [],
  "unresolved_questions": []
}

```

The exact AI response format should be validated by the backend before information is stored in the database.

---

# Tech Stack

## Frontend

- React
- TypeScript
- Vite
- Tailwind CSS

## Backend

- Node.js
- Express.js

## Database

- PostgreSQL

## Authentication

- JWT
- bcrypt

## AI

- OpenAI API or compatible LLM

## Development

- Git
- GitHub
- Conventional Commits

---

# Project Architecture

The application will use a component-based frontend and modular backend architecture.

A possible structure:

```text
followup-ai/
│
├── client/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── features/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── types/
│   │   └── utils/
│   │
│   └── ...
│
├── server/
│   ├── src/
│   │   ├── controllers/
│   │   ├── routes/
│   │   ├── services/
│   │   ├── middleware/
│   │   ├── models/
│   │   ├── utils/
│   │   └── config/
│   │
│   └── ...
│
├── README.md
└── ...

```

The exact structure can evolve as the project grows.

---

# UI/UX Principles

FollowUp AI should feel simple and focused rather than like a complicated project-management system.

### Design principles

- Mobile-first
- Clean and modern
- Simple navigation
- Clear task hierarchy
- Minimal visual clutter
- Responsive layouts
- Accessible components
- Clear loading and error states
- Obvious confirmation and editing actions

### Primary navigation

The initial application can use:

```text
Dashboard
Conversations
Tasks
Profile

```

The interface should prioritize the user's next action rather than displaying unnecessary information.

---

# Code Standards

- Write clean, readable, and maintainable code.
- Use TypeScript and modern JavaScript features.
- Prefer functional React components with hooks.
- Follow component-based architecture.
- Keep components small and reusable.
- Use descriptive variable, function, and component names.
- Avoid duplicate code.
- Follow the DRY principle.
- Use async/await where appropriate.
- Handle errors gracefully.
- Provide meaningful error messages.
- Validate data on both client and server where appropriate.
- Keep files organized by feature.
- Avoid unnecessary dependencies.
- Prioritize security and performance.

# AI Development Guidelines

AI-generated information should always be treated as an interpretation rather than unquestionable truth.

The application should:

- Validate AI responses.
- Use structured output where possible.
- Handle missing information gracefully.
- Avoid inventing assignees or deadlines.
- Distinguish between explicit commitments and uncertain suggestions.
- Allow users to correct AI-generated information.
- Preserve the original conversation as the source.
- Provide confidence information where useful.
- Never silently overwrite user-confirmed information.



### Important principle

> **AI proposes. The user verifies. The system tracks.**

---

# Git Workflow

Use Conventional Commits for every commit.

Examples:

```text
feat: add conversation upload
feat: implement AI action extraction
feat: add task verification flow
feat: add task dashboard
fix: resolve task deadline validation
fix: handle failed transcription
refactor: simplify AI extraction service
test: add authentication tests
docs: update project documentation
style: improve task card layout
chore: configure environment variables

```

Branches should be created for significant features or changes rather than developing everything directly on the main branch.

# Security

The application should follow basic security practices, including:

- Password hashing with bcrypt
- JWT-based authentication
- Protected API routes
- Environment variables for secrets
- Input validation
- Proper authorization checks
- Secure handling of uploaded files
- API error handling that does not expose sensitive information
- Protection against unauthorized access to conversations and tasks

API keys and secrets must never be committed to GitHub.

---

# Capstone Scope

The goal of the capstone is **not** to build the complete future platform.

The capstone should prove that the central concept works:

> **A conversation can be converted into verified, actionable follow-ups.**



### Must-have

- Authentication
- Audio/text input
- Transcription for supported audio
- AI action extraction
- Decision extraction
- Unresolved-question extraction
- AI verification interface
- Task creation
- Task status management
- Dashboard
- Conversation history
- PostgreSQL persistence
- Responsive UI

### Nice-to-have

- Search
- Filters
- Task reminders
- Confidence indicators
- Basic team functionality

---

# Long-Term Vision

FollowUp AI aims to become an **AI accountability and workflow layer for conversations**.

Instead of replacing project-management tools, the platform can sit between communication and task management.

```text
                 CONVERSATIONS
                       ↓
        ┌──────────────┴──────────────┐
        ↓                             ↓
      Meetings                    Messages
        ↓                             ↓
      Voice Notes                   Email
        ↓                             ↓
        └───────────┬─────────────────┘
                    ↓
               FOLLOWUP AI
                    ↓
          Understand & Extract
                    ↓
              Verify
                    ↓
              Commitments
                    ↓
             Tasks / Workflows
                    ↓
              Accountability

```

The long-term goal is to help individuals and teams answer three simple questions:

> **What did we agree to?**

> **Who is responsible?**

> **Did it get done?**

---

# Project Goal

Build a reliable, user-friendly AI system that transforms unstructured conversations into verified commitments and actionable follow-ups.

The capstone will demonstrate the foundation of a product that can later expand into a broader AI-powered accountability and workflow platform.

---


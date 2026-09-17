# CueFlow — Cursor Project Rules

## 1. Project Identity

**Project:** CueFlow

**Tagline:** From conversation cues to completed actions.

**Product:** AI-powered accountability and workflow platform.

### Core idea

CueFlow transforms conversations into verified commitments and actionable follow-ups.

The core workflow is:

```text
Conversation
    ↓
Audio / Transcript
    ↓
AI Analysis
    ↓
Extract Conversation Cues
    ↓
Human Verification
    ↓
Actionable Task
    ↓
Follow-up
    ↓
Completion
```

CueFlow is not primarily a meeting-notes application.

The primary purpose is to identify **what needs to happen next** and help users track whether it actually happens.

---

# 2. Capstone Objective

The goal is to build a functional, polished MVP that demonstrates the core CueFlow concept.

The capstone must prove that the application can:

1. Accept a conversation through supported input.
2. Analyze the conversation using AI.
3. Identify meaningful conversation cues.
4. Extract action items, decisions, deadlines, assignees, priorities, and unresolved questions.
5. Present AI-generated information for human verification.
6. Convert confirmed action items into tasks.
7. Allow users to manage and track those tasks.
8. Maintain a connection between a task and its source conversation.

### Important

Do not expand the MVP unnecessarily.

The capstone should prioritize:

**Reliability → Usability → Clear architecture → Core functionality → Visual polish**

Do not sacrifice the core workflow to add unnecessary features.

---

# 3. Product Philosophy

CueFlow follows this principle:

> **AI proposes. Humans verify. CueFlow tracks.**

AI output must not automatically be treated as fact.

The application should make it easy for users to:

* Confirm AI suggestions
* Edit extracted information
* Reject incorrect information
* Add missing information
* Correct assignees
* Correct deadlines
* Change priority
* Review the original source

The user should always remain in control of important actions.

---

# 4. What Is a Conversation Cue?

A conversation cue is information from a conversation that may require attention or action.

CueFlow should initially recognize:

### Action Items

Something someone needs to do.

Example:

> "Brian will send the revised quotation tomorrow."

Extract:

```text
Action:
Send revised quotation

Assignee:
Brian

Deadline:
Tomorrow
```

### Decisions

Something the participants agreed upon.

Example:

> "Let's use Supplier B for the next shipment."

Extract:

```text
Decision:
Use Supplier B for the next shipment.
```

### Deadlines

Dates or timeframes associated with commitments.

Examples:

* Tomorrow
* Friday
* Next week
* Before September 30
* By the end of the month

### Assignees

The person responsible for an action.

If responsibility is unclear, do not invent an assignee.

Use:

```text
Unassigned
```

### Priority

Priority should only be inferred when the conversation provides enough context.

Supported initial values:

```text
Low
Medium
High
```

If priority cannot reasonably be determined, default to:

```text
Medium
```

### Unresolved Questions

Important questions or issues that remain unanswered.

Example:

> "Can the supplier deliver all 500 units before the 30th?"

CueFlow should identify this as an unresolved question rather than turning it into a false fact.

---

# 5. Technology Stack

## Frontend

* React
* TypeScript
* Vite
* Tailwind CSS

## Backend

* Node.js
* Express.js
* TypeScript where practical

## Database

* PostgreSQL

## Authentication

* JWT
* bcrypt

## AI

* OpenAI API or compatible LLM API

## Version Control

* Git
* GitHub

---

# 6. Architecture Principles

Use a modular architecture.

Separate:

* UI
* API communication
* business logic
* AI services
* database operations
* authentication
* validation

Do not place large amounts of business logic directly inside React components.

Prefer:

```text
Component
    ↓
Hook / Service
    ↓
API
    ↓
Controller
    ↓
Service
    ↓
Database
```

For AI processing:

```text
User Input
    ↓
Backend
    ↓
AI Service
    ↓
Validation
    ↓
Structured Result
    ↓
Database
```

---

# 7. Frontend Rules

Use functional React components and hooks.

### Components should:

* Have one clear responsibility.
* Be reusable when appropriate.
* Avoid unnecessary complexity.
* Avoid excessive prop drilling.
* Handle loading, empty, success, and error states.

### Prefer

```tsx
<TaskCard task={task} />
```

over large components containing unrelated functionality.

### Avoid

* Giant components
* Repeated UI code
* Hardcoded data scattered throughout the application
* Business logic inside presentation components
* Unnecessary global state

---

# 8. TypeScript Rules

Use TypeScript properly.

Avoid:

```ts
any
```

unless there is a clear technical reason.

Create types/interfaces for important entities.

Example:

```ts
interface Task {
  id: string;
  title: string;
  description?: string;
  assignee?: string;
  dueDate?: string;
  priority: "low" | "medium" | "high";
  status: "pending" | "in_progress" | "completed" | "overdue";
}
```

Keep frontend and backend data structures consistent.

---

# 9. Backend Rules

Use Express with clear separation of concerns.

Recommended structure:

```text
server/
└── src/
    ├── controllers/
    ├── routes/
    ├── services/
    ├── middleware/
    ├── models/
    ├── validators/
    ├── utils/
    ├── config/
    └── app.ts
```

### Controllers

Controllers should handle HTTP concerns.

### Services

Services should contain business logic.

### Routes

Routes should remain concise and map requests to controllers.

### Database

Do not put raw database logic throughout controllers.

Centralize database access where practical.

---

# 10. Authentication

Implement secure authentication.

Requirements:

* Passwords must be hashed using bcrypt.
* Passwords must never be stored in plain text.
* Use JWT for authenticated sessions.
* Protect private API routes.
* Validate authenticated users before accessing their data.
* Users must only be able to access their own conversations and tasks.

Never expose:

* Password hashes
* JWT secrets
* API keys
* Database credentials

in the frontend.

---

# 11. AI Rules

AI is an important part of CueFlow, but it should not control the application blindly.

### AI should:

* Extract structured information.
* Return predictable structured output.
* Handle uncertainty.
* Avoid inventing information.
* Preserve important context.
* Distinguish explicit commitments from assumptions.

### AI should NOT:

* Invent names.
* Invent deadlines.
* Invent decisions.
* Claim certainty when information is unclear.
* Automatically modify user-confirmed tasks.
* Delete user information.
* Make important decisions on behalf of the user.

---

# 12. Structured AI Output

Whenever possible, AI responses should follow a predictable structured schema.

Example:

```json
{
  "actionItems": [
    {
      "title": "Send revised quotation",
      "assignee": "Brian",
      "deadline": "2026-09-18",
      "priority": "high",
      "confidence": 0.92
    }
  ],
  "decisions": [
    {
      "text": "Use Supplier B for the next shipment.",
      "confidence": 0.96
    }
  ],
  "unresolvedQuestions": [
    {
      "text": "Can the supplier deliver 500 units before September 30?",
      "confidence": 0.89
    }
  ]
}
```

The backend must validate AI-generated data before storing it.

Never assume an AI response is valid simply because it is returned successfully.

---

# 13. AI Confidence

Confidence can be used to communicate uncertainty.

Use confidence carefully.

Example:

```text
High confidence
Medium confidence
Low confidence
```

Do not present confidence as a scientifically guaranteed probability unless the system actually supports that interpretation.

Confidence should communicate:

> "How strongly does the AI's interpretation appear to be supported by the source conversation?"

---

# 14. Human Verification

AI-generated action items must enter a verification state before becoming confirmed tasks.

Example:

```text
AI DETECTED

Send revised quotation

Assigned to:
Brian

Deadline:
Friday

Priority:
High

[ Confirm ]
[ Edit ]
[ Dismiss ]
```

Possible states:

```text
AI_DETECTED
NEEDS_REVIEW
CONFIRMED
DISMISSED
```

Once confirmed, the action item can become an active task.

---

# 15. Task Rules

Tasks should support:

* Title
* Description
* Assignee
* Due date
* Priority
* Status
* Source conversation
* Created date
* Updated date

Initial statuses:

```text
Pending
In Progress
Completed
Overdue
```

A task should not automatically be marked completed based on AI inference.

Completion should be controlled by the user or an explicitly implemented workflow.

---

# 16. Source Traceability

Every AI-generated action item should maintain a relationship with its source conversation.

Users should be able to answer:

> "Where did this task come from?"

For example:

```text
Task:
Send revised quotation

Source:
Client Meeting — September 17

[View conversation]
```

This is a core trust feature.

---

# 17. Audio Processing

The MVP can support uploaded audio.

Basic workflow:

```text
Upload Audio
     ↓
Validate File
     ↓
Transcription
     ↓
Transcript
     ↓
AI Analysis
     ↓
Extracted Cues
```

Handle:

* Unsupported formats
* File size limits
* Failed uploads
* Transcription failures
* Empty recordings
* API failures

Never leave the user stuck on an infinite loading state.

---

# 18. Text/Transcript Input

Users should also be able to paste or upload text.

This is important because it allows the AI workflow to be tested without requiring audio processing every time.

Example:

```text
Paste conversation
       ↓
Analyze
       ↓
Review cues
```

Text input should use the same downstream AI pipeline as transcribed audio.

---

# 19. Dashboard

The dashboard should focus on actionable information.

Example:

```text
Good morning, Angela

Open
12

Due Today
4

Overdue
3

Completed
18
```

Include:

* Recent conversations
* Pending verification
* Upcoming tasks
* Overdue tasks
* Recently completed tasks

Avoid turning the dashboard into a collection of unnecessary charts.

---

# 20. UI/UX Principles

CueFlow should feel:

* Clean
* Modern
* Professional
* Simple
* Trustworthy
* Responsive

Design mobile-first, then expand to desktop.

Prioritize the needs of a user who wants to quickly answer:

1. What did we discuss?
2. What needs to happen?
3. Who is responsible?
4. When is it due?
5. What is still unresolved?
6. What has already been completed?

---

# 21. Navigation

Initial navigation:

```text
Dashboard
Conversations
Tasks
Profile
```

Do not add navigation items unless they support a real MVP feature.

---

# 22. Error Handling

Every async operation should have appropriate:

* Loading state
* Success state
* Empty state
* Error state

Errors should be understandable to normal users.

Avoid exposing technical errors such as:

```text
ECONNREFUSED
```

Instead:

```text
We couldn't process this conversation right now.
Please try again.
```

Log useful technical information on the server for debugging.

---

# 23. Validation

Validate data on both frontend and backend where appropriate.

Examples:

* Required fields
* Valid email addresses
* Password requirements
* Task titles
* Due dates
* Audio file types
* File sizes
* AI response structures

Never rely only on frontend validation for security.

---

# 24. Security

Follow secure development practices.

Never commit:

```text
.env
API keys
JWT secrets
Database credentials
```

Use environment variables.

Example:

```text
OPENAI_API_KEY=
DATABASE_URL=
JWT_SECRET=
```

Add `.env` to `.gitignore`.

---

# 25. Performance

Prioritize reasonable performance.

Avoid:

* Unnecessary API calls
* Repeated AI requests
* Excessive re-rendering
* Loading entire datasets unnecessarily
* Large components with unrelated responsibilities

AI calls can be expensive.

Do not call the AI API repeatedly when existing results can be reused.

---

# 26. Dependencies

Do not install a package simply because it makes a small task easier.

Before adding a dependency:

1. Check whether the functionality can be implemented simply with existing tools.
2. Consider bundle size and maintenance.
3. Use established libraries when they provide meaningful value.
4. Avoid unnecessary dependencies.

---

# 27. Database Principles

Use PostgreSQL.

Core entities will likely include:

```text
users
conversations
action_items
decisions
questions
tasks
```

Relationships should preserve ownership and source traceability.

Example:

```text
User
 ↓
Conversation
 ↓
Action Item
 ↓
Task
```

Do not duplicate data unnecessarily.

Use proper foreign keys and constraints.

---

# 28. API Design

Use clear RESTful endpoints.

Example:

```text
POST   /api/auth/register
POST   /api/auth/login

GET    /api/conversations
POST   /api/conversations
GET    /api/conversations/:id
DELETE /api/conversations/:id

POST   /api/conversations/:id/analyze

GET    /api/action-items
PATCH  /api/action-items/:id
POST   /api/action-items/:id/confirm
POST   /api/action-items/:id/dismiss

GET    /api/tasks
POST   /api/tasks
GET    /api/tasks/:id
PATCH  /api/tasks/:id
DELETE /api/tasks/:id
```

Do not create endpoints for features that do not exist.

---

# 29. Git Workflow

Use Conventional Commits.

Examples:

```text
feat: add user authentication
feat: add conversation upload
feat: implement transcript analysis
feat: extract action items with AI
feat: add action verification
feat: create task management
feat: add dashboard statistics

fix: handle failed transcription
fix: prevent unauthorized task access

refactor: separate AI processing service

test: add authentication tests

docs: update project documentation

style: improve task card layout

chore: configure environment variables
```

Use feature branches for meaningful features.

Do not make unrelated changes in the same commit.

---

# 30. Testing

Write tests for important functionality.

Prioritize:

### Authentication

* Registration
* Login
* Invalid credentials
* Protected routes

### Task management

* Create task
* Update task
* Complete task
* Authorization

### AI processing

* Valid AI response
* Invalid AI response
* Missing fields
* Failed AI request

### Verification

* Confirm action
* Edit action
* Dismiss action

Do not aim for artificial 100% coverage.

Focus on critical behavior.

---

# 31. Development Workflow

When implementing a feature:

### Step 1 — Understand

Read the existing relevant files before making changes.

### Step 2 — Plan

Identify:

* Components
* API endpoints
* Database changes
* Services
* Types
* Validation
* Tests

### Step 3 — Implement

Make the smallest clean change that solves the problem.

### Step 4 — Verify

Check:

* TypeScript errors
* Linting
* Tests
* API behavior
* UI behavior
* Responsive behavior

### Step 5 — Review

Look for:

* Duplicate code
* Security issues
* Unnecessary dependencies
* Broken existing functionality
* Poor error handling

Do not claim a feature is complete until it has been verified.

---

# 32. Cursor Behavior Rules

When asked to implement a feature:

1. Inspect the existing project structure.
2. Read relevant files before modifying them.
3. Reuse existing components and utilities when appropriate.
4. Do not rewrite unrelated code.
5. Do not introduce unnecessary dependencies.
6. Follow the existing architecture.
7. Explain important architectural decisions briefly.
8. Implement incrementally.
9. Verify the implementation.
10. Report what changed and what was tested.

If requirements are ambiguous, choose the simplest implementation consistent with the project goals.

Do not silently expand the scope.

---

# 33. Do Not Overbuild

This is a capstone project.

Do not automatically introduce:

* Microservices
* Kubernetes
* Redis
* Message queues
* Event-driven architecture
* Complex AI agents
* WebSockets
* Enterprise RBAC
* Multi-region infrastructure
* Complex analytics
* Payment systems

unless the project genuinely requires them.

Prefer a well-structured monolithic application for the MVP.

**Simple and working is better than complex and unfinished.**

---

# 34. MVP Scope

## Must Have

* User registration/login
* Authentication
* Conversation input
* Audio upload
* Transcription
* AI conversation analysis
* Action item extraction
* Decision extraction
* Unresolved question extraction
* AI verification
* Task creation
* Task management
* Task status
* Dashboard
* Conversation history
* PostgreSQL persistence
* Responsive interface

## Nice to Have

* Search
* Filtering
* Confidence indicators
* Basic reminders
* Basic team functionality

## Out of Scope

Do not implement these during the initial capstone unless explicitly requested:

* WhatsApp integration
* Gmail integration
* Slack integration
* Microsoft Teams integration
* Real-time meeting transcription
* Mobile application
* Payments
* Advanced team permissions
* Enterprise administration
* Automated external messaging
* Complex AI agents
* Advanced analytics

---

# 35. Future Vision

CueFlow can eventually expand into:

```text
Meetings
Voice Notes
Email
WhatsApp
Slack
Teams
       ↓
    CueFlow
       ↓
AI Understanding
       ↓
Commitments
Decisions
Questions
       ↓
Verification
       ↓
Tasks
       ↓
Reminders
       ↓
Workflows
       ↓
Accountability
```

Potential future capabilities include:

* Calendar integration
* Email integration
* Team collaboration
* Automated reminders
* Workflow automation
* CRM integrations
* Communication integrations
* Organization-level accountability
* Analytics
* AI-generated follow-up suggestions

These are future possibilities, not MVP requirements.

---

# 36. Product Positioning

Do not describe CueFlow simply as:

> "An AI meeting summarizer."

Instead:

> **CueFlow is an AI accountability and workflow platform that turns conversation cues into verified actions and helps people follow through.**

The core value proposition is:

> **What did we agree to? Who owns it? When is it due? Did it get done?**

---

# 37. Definition of Done

A feature is considered complete only when:

* The implementation works.
* The UI is usable.
* Errors are handled.
* Authentication/authorization is respected.
* Data is persisted correctly.
* Relevant types are defined.
* Important edge cases are considered.
* Existing functionality still works.
* Tests are added where appropriate.
* The code is clean and maintainable.
* The implementation has been manually verified where appropriate.

Do not mark features as complete based solely on code generation.

---

# 38. Final Development Principle

Always prioritize the core product loop:

```text
CAPTURE
   ↓
UNDERSTAND
   ↓
VERIFY
   ↓
ACT
   ↓
FOLLOW UP
   ↓
COMPLETE
```

Every feature should strengthen this loop or directly support the user's ability to manage it.

If a feature does not meaningfully contribute to the core experience, question whether it belongs in the MVP.

---

## CueFlow

**From conversation cues to completed actions.**

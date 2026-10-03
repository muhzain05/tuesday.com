# Tuesday.com

A collaborative task-management application built during **HackED 2025**. The checked-in implementation is a server-rendered Node.js application using Express, EJS, MongoDB/Mongoose, Passport authentication, and Together AI for one focused automation feature: decomposing an existing task into generated subtasks.

[Devpost](https://devpost.com/software/tuesday-com) · [Hackathon team repository](https://github.com/MisbahAN/tuesday.com)

## Implemented features

### Accounts and lists

- local username/password registration with Passport Local
- session-based authentication
- creation of task lists
- assignment of users to shared lists
- authenticated list selection

### Tasks

- create a task with description and due date
- view assigned and unassigned tasks
- claim/assign tasks
- mark assigned tasks complete
- preserve task/list/user relationships through Mongoose models

### AI-assisted subtask decomposition

For an existing task, the backend can send the task name, description, and due date to **Together AI**. The returned subtask text is parsed into new task records. If the original task had an assignment, that assignment is copied to the generated subtasks before the original task and assignment are removed.

This is the AI feature implemented in the current source. The repository does **not** currently implement predictive scheduling or a general autonomous project-management agent.

## Architecture

~~~text
Browser
  │
  ▼
Express + EJS
  ├── Passport Local sessions
  ├── task/list routes
  └── Together AI request for subtask generation
  │
  ▼
MongoDB / Mongoose
  ├── Users
  ├── Lists
  ├── Tasks
  ├── Assignments
  └── ListAssignments
~~~

The main runnable application is under <code>backend/</code>. A separate root React/Create React App package is also present in the repository, but the task-management flow audited here is the Express/EJS application.

## Stack

- Node.js
- Express 4
- EJS
- MongoDB / Mongoose
- Passport + <code>passport-local-mongoose</code>
- <code>express-session</code>
- Axios
- Together AI API
- HTML/CSS

## Repository layout

~~~text
tuesday.com/
├── backend/
│   ├── app.js
│   ├── models/
│   │   ├── Users.js
│   │   ├── Lists.js
│   │   ├── Tasks.js
│   │   ├── Assignments.js
│   │   └── ListAssignments.js
│   ├── views/
│   │   ├── login.ejs
│   │   ├── register.ejs
│   │   ├── main.ejs
│   │   └── tasks.ejs
│   └── package.json
├── package.json
└── README.md
~~~

## Run locally

~~~bash
git clone https://github.com/muhzain05/tuesday.com.git
cd tuesday.com/backend
npm install
~~~

The backend reads these environment variables:

~~~text
PASSWORD_MONGO
TOGETHER_AI_API_KEY
~~~

Then start the server:

~~~bash
node app.js
~~~

The current source listens on:

~~~text
http://localhost:3000
~~~

## Current limitations

- the current session secret is hard-coded in the application source and should be moved to environment configuration before deployment
- the MongoDB connection string has a fixed Atlas username/host with only the password supplied through the environment
- the AI feature depends on Together AI; it is not an OpenAI integration
- "real-time collaboration", predictive scheduling, and several ideas from the original hackathon README are not implemented in the audited source
- the root React package and the server-rendered EJS app are separate pieces; the EJS application is the runnable task-management implementation documented here

## HackED 2025

This repository is a hackathon project. The README is intentionally scoped to the features present in the checked-in implementation rather than the broader product ideas explored during the event.

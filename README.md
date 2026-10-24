# Thinkwell

**Your AI thinking partner.** Thinkwell is a full-stack AI chat app built with Next.js 15. Sign in, start conversations, and get markdown answers with highlighted code. Every chat is saved to your account. Replies come from the DeepSeek API.

> 🚧 **v2.0 redesign in progress.** This repo currently holds the rebranded 1.x app. A 2026 redesign is planned next: new design system, light/dark theme, demo data and a configurable AI provider.

## Features

- **Auth with Clerk.** Sign in with email or social login; every API route is scoped to the signed-in user's own chats.
- **Saved conversations.** Chats are stored in MongoDB, listed in the sidebar, and can be renamed or deleted.
- **AI replies** from DeepSeek (`deepseek-chat`) through the OpenAI-compatible SDK.
- **Rich messages.** Markdown rendering, Prism.js code highlighting and one-click copy.
- **User sync.** A Clerk webhook (verified with Svix) creates, updates and deletes users in MongoDB.
- **Toasts** for errors and actions (react-hot-toast).

## Tech stack

| Layer | Tech |
|---|---|
| Framework | Next.js 15 (App Router, Route Handlers), React 19 |
| Styling | Tailwind CSS v4 |
| Auth | Clerk (`@clerk/nextjs`) + Svix webhooks |
| Database | MongoDB + Mongoose |
| AI | DeepSeek API via the `openai` SDK |
| Rendering | react-markdown, Prism.js |

## Project structure

```
app/
  api/chat/{create,get,rename,delete,ai}/route.js   chat CRUD + AI completion
  api/clerk/route.js                                 Clerk → MongoDB user webhook
  layout.js, page.jsx, globals.css                   shell and chat screen
components/   Sidebar, ChatLabel, Message, PromptBox
context/      AppContext (user, chats, selected chat)
models/       Chat, User (Mongoose)
config/db.js  cached MongoDB connection
middleware.ts Clerk middleware
```

## Getting started

**Prerequisites:** Node.js 20+ and MongoDB (local or Atlas).

```bash
git clone https://github.com/Yusuf-Hussain25/thinkwell.git
cd thinkwell
npm install
cp .env.example .env   # then fill in the values
npm run dev            # http://localhost:3000
```

### Environment variables

Every key is documented in [`.env.example`](.env.example):

| Key | Where to get it |
|---|---|
| `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY`, `CLERK_SECRET_KEY` | Clerk dashboard → API keys. **Leave both empty in local dev** and Clerk runs in keyless mode with temporary keys. |
| `SIGNING_SECRET` | Clerk dashboard → Webhooks → endpoint `https://<domain>/api/clerk` |
| `MONGODB_URI` | `mongodb://127.0.0.1:27017/thinkwell` locally, or a MongoDB Atlas connection string |
| `DEEPSEEK_API_KEY` | https://platform.deepseek.com/api_keys |

Without `DEEPSEEK_API_KEY` the app runs and saves chats, but AI replies fail.

## Deployment

Deploy on Vercel: import the repo, add the environment variables above, and point the Clerk webhook at `https://<your-domain>/api/clerk`.

## Author

**Yusuf Hussain** · https://github.com/Yusuf-Hussain25

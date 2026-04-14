# Workspace

## Overview

pnpm workspace monorepo using TypeScript. WorkTruth — a guided, conversational job search and resume-building web app.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **AI**: Replit AI Integrations → OpenAI (gpt-5.2)
- **Frontend**: React + Vite (worktruth artifact)

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/         # Express API server (all backend routes)
│   └── worktruth/          # React + Vite frontend (main app at /)
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   ├── db/                 # Drizzle ORM schema + DB connection
│   ├── integrations-openai-ai-server/  # OpenAI server-side client
│   └── integrations-openai-ai-react/   # OpenAI React hooks
├── scripts/                # Utility scripts
├── pnpm-workspace.yaml     
├── tsconfig.base.json      
├── tsconfig.json           
└── package.json            
```

## WorkTruth App Flow

The app guides users through 14 modules:

1. **Entry** — Choose "Start from scratch" or "Paste existing resume"
2. **Guided Experience Capture** — AI conversation captures work history (paid, volunteer, caregiving, informal)
3. **Skill Handling** — Per-skill evaluation: guided, quick confirm, or user-asserted; tagged as evaluated/light-confirmed/user-asserted
4. **Proof Flow** — AI reflects back in plain language for confirmation
5. **Scope** — Asks about scale: people, frequency, complexity, independence, reach
6. **Relevance** — Classifies skills as proven, transferable, or gap; adds modern translation
7. **Opportunity Layer** — AI-generated job matches in 3 tiers: Ready Now, Quick Step-Up, Next-Level
8. **Job Action Tracker** — Track saved/viewed/applied/interview/archived jobs
9. **Strategy Choice** — "Get working sooner" vs "Build a stronger next step"
10. **Resume Builder** — AI-generated resume from validated profile only; editable
11. **Contingency Steps** — What's missing + exact steps to qualify
12. **Cover Letter Generator** — Job-matched letter with direct/transferable/gap analysis + tone editor
13. **Company Insights** (optional) — Sentiment, interview questions
14. **Export** — Download resume, cover letter, capability summary, job paths, upskilling plan

## Key Principles

- No inflated claims — only validated content in documents
- No generic resume phrases
- One step at a time, low text density
- Conversational tone, no jargon
- Skills tagged: evaluated / light-confirmed / user-asserted
- Skills classified: proven / transferable / gap

## Backend Routes

- `GET/POST /api/openai/conversations` — AI conversation management
- `POST /api/openai/conversations/{id}/messages` — SSE streaming chat
- `POST/GET /api/profiles` — Profile creation (anonymous sessions via nanoid)
- `GET/PATCH /api/profiles/{id}` — Profile management
- `GET/POST /api/profiles/{id}/experiences` — Work experience CRUD
- `GET/POST /api/profiles/{id}/skills` — Skills CRUD
- `POST /api/profiles/{id}/analyze-resume` — AI resume text analysis
- `POST /api/profiles/{id}/generate-resume` — AI resume generation
- `POST /api/profiles/{id}/generate-cover-letter` — AI cover letter generation
- `POST /api/profiles/{id}/job-matches` — AI job opportunity matching
- `GET/POST /api/profiles/{id}/saved-jobs` — Job tracker
- `PATCH /api/profiles/{id}/saved-jobs/{jobId}` — Update job status
- `GET/POST /api/profiles/{id}/documents` — Document storage

## Database Tables

- `conversations` — AI chat sessions
- `messages` — Chat messages per conversation
- `profiles` — User sessions (anonymous, nanoid ID)
- `experiences` — Work experiences with scope data
- `skills` — Skills with evaluation tags and classifications
- `saved_jobs` — Job tracker with status workflow
- `documents` — Saved resumes, cover letters, etc.

## Development Commands

- `pnpm --filter @workspace/api-server run dev` — Run backend
- `pnpm --filter @workspace/worktruth run dev` — Run frontend
- `pnpm --filter @workspace/api-spec run codegen` — Regenerate API client after spec changes
- `pnpm --filter @workspace/db run push` — Push schema changes to DB

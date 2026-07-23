# TaskFlow — Full-Stack Task Manager

A full-stack task management web app built with **Next.js**, **Tailwind CSS**, and **Supabase** (PostgreSQL + JWT auth).

## Features

### Authentication
- Register with name, email, and password
- Login with email/password
- JWT-based session management (handled by Supabase Auth)
- Logout
- Protected routes — unauthenticated users are redirected to login

### Task Management
- Create tasks with title, description, priority, and due date
- Edit existing tasks
- Delete tasks (with confirmation)
- Mark tasks as completed / pending (one-click toggle)
- Due date picker
- Priority levels: **Low**, **Medium**, **High**

### Dashboard
- Total tasks count
- Completed tasks count
- Pending tasks count
- Overdue tasks count (pending tasks past their due date)
- Completion progress bar
- Recent tasks list

### Search & Filter
- Search tasks by title
- Filter by status (All / Pending / Completed)
- Filter by priority (All / Low / Medium / High)

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js (App Router), React, Tailwind CSS, shadcn/ui |
| Backend | Next.js API routes + Supabase (PostgreSQL) |
| Auth | Supabase Auth (JWT, email/password) |
| Database | Supabase PostgreSQL |

## Database Schema

### `tasks` table
| Column | Type | Description |
|--------|------|-------------|
| `id` | uuid (PK) | Auto-generated unique ID |
| `title` | text | Task title (required) |
| `description` | text | Optional longer description |
| `priority` | text | `low`, `medium`, or `high` (default: `medium`) |
| `due_date` | date | Optional due date |
| `status` | text | `pending` or `completed` (default: `pending`) |
| `user_id` | uuid (FK → `auth.users`) | Owner of the task |
| `created_at` | timestamptz | Creation timestamp |

User accounts are managed by Supabase's built-in `auth.users` table — no custom users table needed.

### Row Level Security (RLS)
Every table has RLS enabled. Each user can only read, create, update, and delete their own tasks via four owner-scoped policies (`auth.uid() = user_id`).

## Project Structure

```
app/
  layout.tsx              # Root layout with AuthProvider + ThemeProvider
  page.tsx                # Redirects to /dashboard or /login
  globals.css             # Tailwind + theme variables (light/dark)
  login/page.tsx          # Login page
  register/page.tsx       # Registration page
  dashboard/
    layout.tsx            # Protected layout with sidebar
    page.tsx              # Dashboard with stats
    tasks/page.tsx        # Task list with search/filter/CRUD
components/
  sidebar.tsx             # Navigation sidebar
  task-form-dialog.tsx    # Create/edit task modal
  ui/                     # shadcn/ui components
lib/
  supabase/client.ts      # Supabase client + types
  auth-context.tsx        # Auth provider + useAuth hook
  theme-context.tsx       # Dark/light theme provider
```

## Installation & Running Locally

```bash
# Install dependencies
npm install

# Run the dev server
npm run dev

# Build for production
npm run build && npm start
```

The app runs at `http://localhost:3000`.

Supabase credentials are pre-configured in `.env`:
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

## Sample Data

Once you register an account, create sample tasks by clicking **New Task** on the Tasks page:

| Title | Priority | Due Date | Status |
|-------|----------|----------|--------|
| Finish quarterly report | High | 2026-07-30 | Pending |
| Review pull requests | Medium | 2026-07-25 | Pending |
| Update documentation | Low | — | Completed |
| Deploy v2.0 to production | High | 2026-07-28 | Pending |
| Team standup notes | Low | 2026-07-24 | Completed |

## Notes

- Email confirmation is disabled — you can sign in immediately after registering.
- Dark mode is enabled by default; toggle via the sidebar.
- The UI uses a glassmorphism aesthetic with backdrop blur and translucent cards.

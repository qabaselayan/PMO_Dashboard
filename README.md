# PMO Command Center

Enterprise-ready project management system for an internal PMO or delivery organization, with a `Next.js` frontend and `Django` backend. The platform covers projects, WBS/tasks, Gantt visibility, documents, communications, resource planning, costs, alerts, reporting, role-based access, and seeded demo data.

## Stack

- Frontend: `Next.js` App Router + TypeScript
- Backend: `Django 5`
- Database: PostgreSQL-ready configuration with SQLite fallback for local setup
- ORM: Django ORM
- Authentication: Django session auth with role-based access control
- Reporting: `reportlab` for PDF export and `openpyxl` for Excel export

## Solution Structure

- `frontend/` Next.js dashboard application
- `config/` Django project settings, middleware, and URL config
- `accounts/` custom user model and login flow
- `pmo/` PMO domain models, HTML views, JSON API endpoints, services, and seed command
- `templates/` existing Django-rendered screens retained for backend fallback/admin-style usage
- `static/` shared Django styles

## Core Capabilities

- Secure login with RBAC for `Admin`, `Project Manager`, `Team Member`, and `Viewer / Management`
- Executive dashboard with status summary, workload, deadlines, milestones, activity feed, and cost overview
- Projects module with owners, managers, team members, stakeholders, milestones, costs, documents, attachments, and audit trail
- Task / WBS module with hierarchy, dependencies, assignees, overdue highlighting, progress tracking, and recurring follow-up flags
- Documents module with category, version, status, and project/task association
- Communication timeline with meeting notes, reminders, follow-ups, mentions, and history
- Resource management with allocation tracking, over-assignment detection, and leave-day impact
- Report summaries plus backend PDF / Excel exports
- Seeded demo accounts and realistic project portfolio data

## Backend Setup

1. Create and activate a Python virtual environment.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Copy environment variables:

```bash
cp .env.example .env
```

4. For PostgreSQL, set:

```env
USE_SQLITE=False
DATABASE_NAME=pm_dashboard
DATABASE_USER=postgres
DATABASE_PASSWORD=postgres
DATABASE_HOST=127.0.0.1
DATABASE_PORT=5432
```

5. Run migrations:

```bash
python manage.py makemigrations accounts pmo
python manage.py migrate
```

6. Seed demo data:

```bash
python manage.py seed_demo
```

7. Start Django:

```bash
python manage.py runserver
```

## Frontend Setup

1. Move into the frontend app:

```bash
cd frontend
```

2. Install dependencies:

```bash
npm install
```

3. Create `frontend/.env.local`:

```env
NEXT_PUBLIC_API_BASE_URL=http://127.0.0.1:8000/api
```

4. Start Next.js:

```bash
npm run dev
```

The frontend runs on `http://127.0.0.1:3000` and talks to Django on `http://127.0.0.1:8000`.

## API Surface

The Django backend now exposes JSON endpoints under `/api/` for:

- `auth/csrf`, `auth/login`, `auth/logout`, `auth/me`
- `dashboard`
- `meta`
- `projects`, `projects/<slug>`, `projects/<slug>/archive`
- `projects/<slug>/tasks`
- `projects/<slug>/documents`
- `projects/<slug>/communications`
- `projects/<slug>/costs`
- `tasks`
- `gantt`
- `documents`
- `resources`
- `reports/summary`
- `notifications`
- `search`

## Demo Accounts

After seeding, all demo accounts use password `Password123!`.

- `admin`
- `manager1`
- `manager2`
- `member1`
- `member2`
- `member3`
- `viewer`

## Screens Included

- Login
- Dashboard
- Projects list
- Project detail
- Tasks / WBS
- Calendar
- Gantt chart
- Documents
- Resource management
- Reports

## Current Interactive Frontend Flows

- Create projects from the Next.js projects screen
- Edit project details and archive projects from the project detail screen
- Add project tasks / WBS items from the project detail screen
- Upload project and task-linked documents from the project detail screen
- Add communication log entries from the project detail screen
- Add project cost entries from the project detail screen
- Search projects, tasks, and documents globally from the workspace header
- Filter projects by status, priority, manager, and delay flag
- Filter tasks by project, assignee, status, priority, and overdue flag
- Filter documents by project, category, status, and task-link presence
- View a calendar-style schedule of deadlines and milestones
- Navigate the calendar month-by-month with server-side date-window queries
- Review task dependency chains directly from the Gantt screen
- Switch the Gantt timeline between projects from the frontend
- Create, edit, and delete project milestones from the project detail screen
- Trigger backend Excel/PDF report exports from the Next.js reports screen
- Create and edit resource allocations with leave-day adjustments from the resource management screen
- Edit and delete documents with version-history snapshots from the documents workflows
- Replace uploaded document files during edit flows while preserving version-history snapshots
- Delete resource allocations and navigate a month-style calendar view from the Next.js frontend
- Edit and delete tasks from both the task board and project detail workflow
- Edit task dependencies from both the task board and project detail workflow
- Read dashboard, gantt, documents, resources, and report summaries from Django JSON endpoints

## Notes

- File uploads are stored in `media/documents/`
- Document version history adds a new `DocumentVersion` model and requires a migration before use
- Audit trail entries are generated automatically for project, task, document, and communication updates
- Existing Django-rendered pages are still present; the new `frontend/` app is the primary UI
- Validation and verification still require installing Python and Node dependencies in the local environment

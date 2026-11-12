
# Project proposal

- **Name:** Life
- **Project title:** Life – Your Personal Life Dashboard
- **Pitch (one sentence):** A personalized productivity platform that brings together calendars, tasks, personal goals, habits and projects in one place, helping users organize their everyday lives.
- **Target users:** Myself and anyone who wants to better organize their personal and professional life.

## Pages

| **URL** | **Content** | **Dynamic segment?** | **Rendering strategy and why** |
|---|---|---|---|
| `/` | Landing page presenting Life, its features and a call to action. | No | **SSG:** The content is mostly fixed and can be generated at build time. |
| `/dashboard` | Personalized overview: today's events, pending tasks, habits and progress on goals. | No | **SSR:** The dashboard depends on the authenticated user and their latest data. |
| `/calendar` | Monthly, weekly and daily calendar with events and deadlines. | No | **CSR:** Users frequently interact with the calendar, navigate dates and update events. |
| `/tasks` | Task list with priorities, categories, deadlines and completion status. | No | **CSR:** Users frequently add, edit, complete and delete tasks. |
| `/goals` | Personal goals with progress indicators and milestones. | No | **SSR:** Goals and progress are specific to each authenticated user. |
| `/habits` | Habit tracker with daily check-ins and weekly statistics. | No | **CSR:** Users need to interact with and update their habits daily. |
| `/projects` | Personal projects with descriptions, tasks and progress. | No | **SSR:** The displayed projects depend on the authenticated user. |
| `/projects/[id]` | Detailed view of one project, including its tasks, deadlines and progress. | Yes (`id`) | **SSR:** The page needs to retrieve the specific project using its ID. |
| `/settings` | User profile, preferences and calendar integration settings. | No | **SSR:** Settings are private and specific to the authenticated user. |

## Data

Entities (2 to 4), their main fields, and how they are related:

### 1. User
- `id`
- `name`
- `email`
- `passwordHash`
- `createdAt`

### 2. Task
- `id`
- `title`
- `description`
- `dueDate`
- `priority` (low, medium, high)
- `status` (pending, completed)
- `userId`
- `projectId` (optional)

### 3. Event
- `id`
- `title`
- `description`
- `startDate`
- `endDate`
- `location`
- `userId`

### 4. Project
- `id`
- `name`
- `description`
- `startDate`
- `endDate`
- `status`
- `userId`

**Relationships:**
- A User can have multiple Tasks, Events and Projects.
- A Project can contain multiple Tasks.
- Each Task belongs to one User and can optionally belong to one Project.
- Each Event belongs to one User.

## Write actions

What users can create, update or delete:

- **Tasks:** Create tasks, edit descriptions, change priorities, set deadlines, mark as completed or delete them.
- **Events:** Create appointments, edit dates and details, or delete events.
- **Projects:** Create projects, update their information and progress, or delete them.
- **User settings:** Update profile information and personal preferences.

## Repository

GitHub URL: To be created.

---

**Technical stack:** Next.js (React), TypeScript, Tailwind CSS, Prisma ORM and PostgreSQL.

**Main differentiator:** Life doesn't aim to replace Outlook or Google Calendar. It centralizes personal organization around the calendar by combining appointments with tasks, projects and everyday priorities.

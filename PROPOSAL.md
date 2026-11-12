
# Project proposal

- **Name:** Life
- **Project title:** Life – Your Personal Life Dashboard
- **Pitch (one sentence):** A personalized productivity platform that brings together calendars, tasks, personal goals, habits and projects in one place, helping users organize their everyday lives.
- **Target users:** ME and anyone who wants to better organize their personal and professional life.

## Pages

| **URL** | **Content** | **Dynamic segment?** | **Rendering strategy and why** |
|---|---|---|---|
| `/` | Landing page presenting Dayly, its features and a call to action. | No | Static generation (SSG): content is mostly fixed and doesn't need frequent updates. |
| `/dashboard` | Personalized overview: today's events, pending tasks, habits and progress on goals. | No | Dynamic rendering: information depends on the authenticated user and changes frequently. |
| `/calendar` | Monthly, weekly and daily calendar with events and deadlines. | No | Dynamic rendering: events are user-specific and frequently updated. |
| `/tasks` | Task list with priorities, categories, deadlines and completion status. | No | Dynamic rendering: tasks are user-specific and frequently modified. |
| `/goals` | Personal goals with progress indicators and milestones. | No | Dynamic rendering: displays each user's current progress. |
| `/habits` | Habit tracker with daily check-ins and weekly statistics. | No | Dynamic rendering: tracking data changes every day. |
| `/projects` | Personal projects with descriptions, tasks and progress. | No | Dynamic rendering: projects and their associated tasks are user-specific. |
| `/projects/[id]` | Detailed view of one project, including its tasks, deadlines and progress. | Yes (`id`) | Dynamic rendering: content depends on the selected project and its data. |
| `/settings` | User profile, preferences and calendar integration settings. | No | Dynamic rendering: settings are specific to the authenticated user. |

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

**Main differentiator:** Dayly doesn't aim to replace Outlook or Google Calendar. It centralizes personal organization around the calendar by combining appointments with tasks, projects and everyday priorities.

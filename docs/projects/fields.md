# Zoho Projects -- Field Reference

Reverse-engineered by navigating a live logged-in session (Owner-authenticated, Zoho Projects Plus, portal "rajatagarwal") via Claude in Chrome, 2026-07-20. Read-only: forms were opened to enumerate fields then dismissed without saving; one accidental inline-rename-edit trigger on the project list row was caught and cancelled with Escape before any change was committed -- no records were created, edited, or deleted during this pass.

## Access path
Landing on `projectsplus.zoho.in/organization` shows a feature-tour splash (tabs: Classic Project / Agile Project / Hybrid Project / File Management / Advanced Analytics, each with marketing copy + screenshot) with a "Skip" button. Skipping loads the real app at `.../home/app/projects`.

**Note on this app's navigation behavior:** clicking a project's *name* directly in the list view triggers an inline rename-edit box (not a navigation into the project) -- a real risk during reverse-engineering since typing or clicking away could commit an unintended rename. Use the ID link or sidebar module links instead. Also, this app uses hash-based client-side routing (`#zp/tasks/...`) -- navigating to a new hash does NOT force a real page reload if a slide-out panel is open; a full path navigation (or page refresh) is needed to reliably dismiss a stuck panel.

## Top-level navigation
Left rail top icons (portal-wide, not project-scoped): Home, Reports, Projects, Users, Collaboration, My Approvals. Below that, an **Overview** group (visible while inside "Projects"): Tasks, Issues, Phases, Time Logs, Timesheets -- these are portal-wide roll-up views across all projects, not per-project pages.

Zoho One app switcher (same as CRM) confirms Projects sits alongside Sprints (`sprints.zoho.in`), Analytics, WorkDrive, Notebook, and Learn as bundled apps under the same login.

## Module: Projects (list)
Tabs above the list: Active Projects, Project Templates, Project Groups, Public Projects, plus a "..." overflow.
List columns observed: ID, Project Name, % (completion), Owner, Status, Tasks (count badge), and at least one more column truncated off-screen (prefixed with a folder-like icon, likely "Phases" or "Files" count).
Sample data: 1 seeded project, `RA-1 / "Explore Zoho Projects!" / 0% / Owner: Mr Rajat Agarwal / Status: Active / No Tasks`.

New Project creation was not completed (avoided triggering it after the accidental inline-edit risk above; the "New Project" button did not visibly open a modal in this pass -- possibly a slide-in panel similarly hash-routed, not confirmed).

## Module: Tasks (portal-wide "Overview" view)
List columns: ID, Task Name, Project, Owner, Status, plus more (table scrolls horizontally, not fully captured).
Row-level inline actions available directly in the list: "Add Task", "Add Task List", "Suggestions" (the last implying AI/ML-assisted task suggestions).

**Add Task** (quick-add slide-in panel, opened via the top "Add Task" button):
- **Project*** (required dropdown, "Select Project")
- **Task Name*** (required, single-line)
- Add Description (rich text editor: Bold/Italic/Underline/Strikethrough, font family "Puvi" + size 13, text color, alignment, bullet/numbered list, indent, an "auto-format"-looking icon, dark/light toggle, fullscreen expand)
- Buttons: **Add**, **Add More** (implies rapid sequential task entry), **Cancel**

This quick-add panel is intentionally minimal -- no assignee/due-date/priority fields are exposed here; those are presumed to exist on the full task detail view (not confirmed in this pass, see gaps below).

## Module: Issues (portal-wide "Overview" view)
Bug/issue-tracker-style module, Jira-like. List columns: ID, Issues Name, Project, Reporter, Create[d Time] (sortable), Assignee, plus more off-screen. Grouping control ("Group By None") and a saved-view selector ("All Issues") sit above the table, same pattern as Tasks. Primary action button: **Submit Issues** (not "Create" -- suggests a bug-report submission framing rather than generic record creation).

## Module: Phases (portal-wide "Overview" view)
Milestone/phase-tracking module. List columns: Phase Name, Project, % (completion), Status, Owner, Start Date, plus more off-screen. Primary actions: **Add Phase**, **Automation** (workflow automation entry point, same as seen on the Projects list).

## Not yet explored (real, disclosed gap -- ran out of scope for this pass)
- Time Logs, Timesheets (left-nav items, not opened)
- Users, Collaboration, My Approvals, Reports (top-nav items, not opened)
- Full Task detail view / edit form (only the quick-add panel was seen; assignee, due date, priority, tags, dependencies, subtasks -- all standard Zoho Projects task fields -- were not directly confirmed on-screen in this pass)
- New Project creation form (avoided after the inline-edit close call; unconfirmed whether it's a modal, slide-in panel, or full-page navigation)
- The existing sample project's own internal view (task lists, Gantt/timeline, files) -- only the portal-wide Overview roll-ups were explored, not the project itself
- Kanban/board views for Tasks or Issues (both showed "List" view selectors implying alternate views exist, per the CRM Deals precedent of a Kanban "Stage View")

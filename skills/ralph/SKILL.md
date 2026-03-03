---
name: ralph
description: "Convert PRDs to prd.json format for the Ralph autonomous agent system. Use when you have an existing PRD and need to convert it to Ralph's JSON format. Triggers on: convert this prd, turn this into ralph format, create prd.json from this, ralph json."
user-invocable: true
---

# Ralph PRD Converter

Converts existing PRDs to the prd.json format that Ralph uses for autonomous execution.

---

## The Job

Take a PRD (markdown file or text) and convert it to `prd.json` in your ralph directory.

---

## Output Format

```json
{
  "project": "[Project Name]",
  "branchName": "ralph/[feature-name-kebab-case]",
  "description": "[Feature description from PRD title/intro]",
  "userStories": [
    {
      "id": "US-001",
      "title": "[Story title]",
      "description": "As a [user], I want [feature] so that [benefit]",
      "acceptanceCriteria": [
        "Criterion 1",
        "Criterion 2",
        "Typecheck passes"
      ],
      "priority": 1,
      "dependsOn": [],
      "passes": false,
      "notes": ""
    }
  ]
}
```

---

## Story Size: The Number One Rule

**Each story must be completable in ONE Ralph iteration (one context window).**

Ralph spawns a fresh Amp instance per iteration with no memory of previous work. If a story is too big, the LLM runs out of context before finishing and produces broken code.

### Right-sized stories:
- Add a database column and migration
- Add a UI component to an existing page
- Update a server action with new logic
- Add a filter dropdown to a list

### Too big (split these):
- "Build the entire dashboard" - Split into: schema, queries, UI components, filters
- "Add authentication" - Split into: schema, middleware, login UI, session handling
- "Refactor the API" - Split into one story per endpoint or pattern

**Rule of thumb:** If you cannot describe the change in 2-3 sentences, it is too big.

---

## Story Ordering: Dependencies First

Stories execute in priority order. Earlier stories must not depend on later ones.

**Correct order:**
1. Schema/database changes (migrations)
2. Server actions / backend logic
3. UI components that use the backend
4. Dashboard/summary views that aggregate data

**Wrong order:**
1. UI component (depends on schema that does not exist yet)
2. Schema change

---

## Acceptance Criteria: Must Be Verifiable

Each criterion must be something Ralph can CHECK, not something vague.

### Good criteria (verifiable):
- "Add `status` column to tasks table with default 'pending'"
- "Filter dropdown has options: All, Active, Completed"
- "Clicking delete shows confirmation dialog"
- "Typecheck passes"
- "Tests pass"

### Bad criteria (vague):
- "Works correctly"
- "User can do X easily"
- "Good UX"
- "Handles edge cases"

### Always include as final criterion:
```
"Typecheck passes"
```

For stories with testable logic, also include:
```
"Tests pass"
```

### For stories that change UI, also include:
```
"Use 21st.dev Magic MCP to build compelling, polished UI components"
"Verify in browser using dev-browser skill"
```

Frontend stories must use the **21st.dev Magic MCP** tools to create relevant, intuitive, and compelling UI components. Ralph agents should:
- Use `21st_magic_component_builder` to generate new UI components (buttons, inputs, dialogs, tables, forms, cards, banners, etc.)
- Use `21st_magic_component_inspiration` to browse and get inspiration from existing high-quality components
- Use `21st_magic_component_refiner` to improve/redesign existing UI components
- Use `logo_search` when company/brand logos are needed

Frontend stories are NOT complete until visually verified. Ralph will use the dev-browser skill to navigate to the page, interact with the UI, and confirm changes work.

---

## Conversion Rules

1. **Each user story becomes one JSON entry**
2. **IDs**: Sequential (US-001, US-002, etc.)
3. **Priority**: Based on dependency order, then document order
4. **All stories**: `passes: false` and empty `notes`
5. **branchName**: Derive from feature name, kebab-case, prefixed with `ralph/`
6. **Always add**: "Typecheck passes" to every story's acceptance criteria
7. **dependsOn**: Array of story IDs that must complete before this story can start (see dependency rules below)

---

## Dependency Rules (`dependsOn`)

The `dependsOn` field enables parallel execution in team mode. Stories with no mutual dependencies can run simultaneously.

### How to assign `dependsOn`:

- **Schema/DB stories**: `dependsOn: []` — these have no dependencies and run first
- **Backend logic stories**: depend on the schema story they use (e.g., `dependsOn: ["US-001"]`)
- **UI stories**: depend on the backend story they consume (e.g., `dependsOn: ["US-002"]`)
- **Unrelated stories**: no mutual dependencies — they can run in parallel even if they share a priority level

### Rules:

1. A story can only depend on stories with a lower or equal priority number
2. Never create circular dependencies (A depends on B, B depends on A)
3. Stories that touch completely different parts of the codebase should NOT depend on each other
4. **Only add a dependency when story B reads, modifies, or imports something that story A creates.** If two stories just happen to appear on the same page or feature area but don't share code artifacts, they are independent. Fewer dependencies = more parallelism in team mode.
5. Use an empty array `[]` for stories with no dependencies, not an absent field

### Common false dependencies (do NOT add these):
- UI component → sibling UI component (a chart and a summary card are independent if they both just read from the same engine)
- Config/settings panel → display component (an assumptions panel and a results chart both depend on the calculation engine, not on each other)
- localStorage/persistence → unrelated UI features (persistence depends on the form, not on every feature that uses the form)

### Example:

```
US-001 (DB schema)        → dependsOn: []
US-002 (Badge UI)         → dependsOn: ["US-001"]
US-003 (Priority selector)→ dependsOn: ["US-001"]
US-004 (Filter UI)        → dependsOn: ["US-001"]
```

Here US-002, US-003, US-004 are independent of each other (they all only depend on US-001), so they can execute in parallel once US-001 is done.

---

## Splitting Large PRDs

If a PRD has big features, split them:

**Original:**
> "Add user notification system"

**Split into:**
1. US-001: Add notifications table to database
2. US-002: Create notification service for sending notifications
3. US-003: Add notification bell icon to header
4. US-004: Create notification dropdown panel
5. US-005: Add mark-as-read functionality
6. US-006: Add notification preferences page

Each is one focused change that can be completed and verified independently.

---

## Example

**Input PRD:**
```markdown
# Task Status Feature

Add ability to mark tasks with different statuses.

## Requirements
- Toggle between pending/in-progress/done on task list
- Filter list by status
- Show status badge on each task
- Persist status in database
```

**Output prd.json:**
```json
{
  "project": "TaskApp",
  "branchName": "ralph/task-status",
  "description": "Task Status Feature - Track task progress with status indicators",
  "userStories": [
    {
      "id": "US-001",
      "title": "Add status field to tasks table",
      "description": "As a developer, I need to store task status in the database.",
      "acceptanceCriteria": [
        "Add status column: 'pending' | 'in_progress' | 'done' (default 'pending')",
        "Generate and run migration successfully",
        "Typecheck passes"
      ],
      "priority": 1,
      "dependsOn": [],
      "passes": false,
      "notes": ""
    },
    {
      "id": "US-002",
      "title": "Display status badge on task cards",
      "description": "As a user, I want to see task status at a glance.",
      "acceptanceCriteria": [
        "Each task card shows colored status badge",
        "Badge colors: gray=pending, blue=in_progress, green=done",
        "Use 21st.dev Magic MCP to build compelling, polished UI components",
        "Typecheck passes",
        "Verify in browser using dev-browser skill"
      ],
      "priority": 2,
      "dependsOn": ["US-001"],
      "passes": false,
      "notes": ""
    },
    {
      "id": "US-003",
      "title": "Add status toggle to task list rows",
      "description": "As a user, I want to change task status directly from the list.",
      "acceptanceCriteria": [
        "Each row has status dropdown or toggle",
        "Changing status saves immediately",
        "UI updates without page refresh",
        "Use 21st.dev Magic MCP to build compelling, polished UI components",
        "Typecheck passes",
        "Verify in browser using dev-browser skill"
      ],
      "priority": 2,
      "dependsOn": ["US-001"],
      "passes": false,
      "notes": ""
    },
    {
      "id": "US-004",
      "title": "Filter tasks by status",
      "description": "As a user, I want to filter the list to see only certain statuses.",
      "acceptanceCriteria": [
        "Filter dropdown: All | Pending | In Progress | Done",
        "Filter persists in URL params",
        "Use 21st.dev Magic MCP to build compelling, polished UI components",
        "Typecheck passes",
        "Verify in browser using dev-browser skill"
      ],
      "priority": 2,
      "dependsOn": ["US-001"],
      "passes": false,
      "notes": ""
    }
  ]
}
```

---

## Archiving Previous Runs

**Before writing a new prd.json, check if there is an existing one from a different feature:**

1. Read the current `prd.json` if it exists
2. Check if `branchName` differs from the new feature's branch name
3. If different AND `progress.txt` has content beyond the header:
   - Create archive folder: `archive/YYYY-MM-DD-feature-name/`
   - Copy current `prd.json` and `progress.txt` to archive
   - Reset `progress.txt` with fresh header

**The ralph.sh script handles this automatically** when you run it, but if you are manually updating prd.json between runs, archive first.

---

## Dependency Validation

After assigning all `dependsOn` fields, draw the dependency graph and parallelization waves:

```
Example output:

US-001 ─┬── US-002 ─── US-005
        ├── US-003 ─┘
        └── US-004

Waves:
- Wave 1: US-002 + US-003 + US-004 (parallel)
- Wave 2: US-005
```

Then verify:
- **Are any sibling stories unnecessarily chained?** Two stories that both depend on the same parent but NOT on each other should be in the same wave.
- **Is the longest chain (critical path) as short as possible?** If a dependency can be removed without creating merge conflicts, remove it.
- **Could a story be split to increase parallelism?** If a story blocks 3+ others, consider splitting it into independent pieces.

---

## Checklist Before Saving

Before writing prd.json, verify:

- [ ] **Previous run archived** (if prd.json exists with different branchName, archive it first)
- [ ] Each story is completable in one iteration (small enough)
- [ ] Stories are ordered by dependency (schema to backend to UI)
- [ ] Every story has "Typecheck passes" as criterion
- [ ] UI stories have "Use 21st.dev Magic MCP to build compelling, polished UI components" as criterion
- [ ] UI stories have "Verify in browser using dev-browser skill" as criterion
- [ ] Acceptance criteria are verifiable (not vague)
- [ ] No story depends on a later story
- [ ] `dependsOn` is set for each story (empty array `[]` if no dependencies)
- [ ] No circular dependencies exist
- [ ] Independent stories (different parts of codebase) don't have unnecessary dependencies
- [ ] Dependency graph and parallelization waves are shown (see Dependency Validation)
- [ ] No false dependencies between sibling stories (e.g., two UI components that don't share code)

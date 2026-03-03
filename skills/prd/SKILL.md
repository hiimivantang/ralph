---
name: prd
description: "Generate a comprehensive Product Requirements Document (PRD) for a new feature or product. Use when planning a feature, starting a new project, or when asked to create a PRD. Triggers on: create a prd, write prd for, plan this feature, requirements for, spec out, product requirements."
---

# PRD Generator

Create detailed, customer-centric Product Requirements Documents that bridge business strategy and technical implementation. PRDs should be living documents — clear, actionable, and suitable for diverse stakeholders from junior developers to executives.

---

## The Job

1. Receive a feature or product description from the user
2. Ask 3-7 essential clarifying questions (with lettered options), organized by category
3. Generate a structured PRD based on answers
4. Save to `tasks/prd-[feature-name].md`

**Important:** Do NOT start implementing. Just create the PRD.

---

## Step 1: Clarifying Questions

Ask only critical questions where the initial prompt is ambiguous. Organize questions into three categories:

### Strategic Context
- **Problem/Goal:** What problem does this solve? Why does it matter to the business?
- **Market Context:** Who are the competitors? What's the industry landscape?
- **Lifecycle Stage:** Is this a new product, enhancement, or pivot?

### Customer Understanding
- **Target Users:** Who are the primary personas?
- **Jobs-to-be-Done:** What are users trying to accomplish? What outcomes do they want?
- **Current Workflow:** How do users solve this problem today?

### Scope & Constraints
- **Core Functionality:** What are the key actions?
- **Scope/Boundaries:** What should it NOT do?
- **Success Criteria:** How do we know it's done?
- **Constraints:** Timeline, budget, regulatory, or technical constraints?

### Format Questions Like This:

```
**Strategic Context**

1. What is the primary goal of this feature?
   A. Improve user onboarding experience
   B. Increase user retention
   C. Reduce support burden
   D. Other: [please specify]

2. Where does this sit in the product lifecycle?
   A. Brand new product/feature
   B. Enhancement of existing feature
   C. Migration/replacement of legacy system
   D. Experimental/prototype

**Customer Understanding**

3. Who is the target user?
   A. New users only
   B. Existing users only
   C. All users
   D. Admin/internal users only

4. What is the user's primary "job to be done"?
   A. [Contextual option based on prompt]
   B. [Contextual option based on prompt]
   C. [Contextual option based on prompt]
   D. Other: [please specify]

**Scope & Constraints**

5. What is the scope?
   A. Minimal viable version (MVP)
   B. Full-featured implementation
   C. Just the backend/API
   D. Just the UI

6. Are there known constraints?
   A. Tight timeline (< 2 weeks)
   B. Must integrate with existing system X
   C. Regulatory/compliance requirements
   D. No major constraints
```

This lets users respond with "1A, 2B, 3C, 4D, 5A, 6B" for quick iteration.

---

## Step 2: PRD Structure

Generate the PRD with these sections. All sections are required unless marked (Optional).

### 1. Introduction / Overview
Brief description of the feature/product and the problem it solves. This should answer "why are we building this?" in 2-3 sentences. Include the product's lifecycle stage (new, enhancement, etc.).

### 2. Strategic Context
- **Vision & Strategic Intent:** How this feature aligns with the broader product/business strategy.
- **Market Environment:** Competitive landscape, industry trends, or market segments this addresses.
- **Rationale:** Why now? What business or market signal triggered this initiative?

### 3. Goals
Specific, measurable objectives (bullet list). Each goal should be tied back to a business or customer outcome.

### 4. Personas & Customer Context
For each target persona, include:
- **Name/Role:** A descriptive label (e.g., "Growth-stage Startup CTO")
- **Characteristics:** Key traits, technical proficiency, decision-making context
- **Needs & Goals:** What they're trying to achieve
- **Pain Points:** Current frustrations or unmet needs
- **Current Workflow:** How they solve this problem today (the "before" state)

Use the Jobs-to-be-Done lens: focus on the outcome users want, not just features.

### 5. User Stories
Each story needs:
- **Title:** Short descriptive name
- **Description:** "As a [persona], I want [feature] so that [outcome/benefit]"
- **Acceptance Criteria:** Verifiable checklist of what "done" means
- **Priority:** P0 (must-have) / P1 (should-have) / P2 (nice-to-have)

Each story should be small enough to implement in one focused session.

**Format:**
```markdown
### US-001: [Title]  [P0]
**Description:** As a [persona], I want [feature] so that [benefit].

**Acceptance Criteria:**
- [ ] Specific verifiable criterion
- [ ] Another criterion
- [ ] Typecheck/lint passes
- [ ] **[UI stories only]** Use 21st.dev Magic MCP to build compelling, polished UI components
- [ ] **[UI stories only]** Verify in browser using dev-browser skill
```

**Important:**
- Acceptance criteria must be verifiable, not vague. "Works correctly" is bad. "Button shows confirmation dialog before deleting" is good.
- **For any story with UI changes:** Always include "Verify in browser using dev-browser skill" as acceptance criteria.
- Prioritize stories using P0/P1/P2 based on value and impact.

### 6. Functional Requirements
Numbered list of specific functionalities organized by category:
- "FR-1: The system must allow users to..."
- "FR-2: When a user clicks X, the system must..."

Be explicit and unambiguous. Group related requirements and note dependencies between them.

**For each requirement, include:**
- Unique hierarchical number (FR-1, FR-1.1, FR-1.2, etc.)
- Clear rationale linking it to a goal or user story
- Dependencies on other requirements (if any)

### 7. Non-Functional Requirements
Address these dimensions as applicable:
- **Usability:** Speed, transaction times, feedback, ease of use, accessibility
- **Performance:** Response times, concurrent users, capacity, reliability
- **Security:** Authentication, authorization, data protection
- **Scalability:** Expected growth, load handling
- **Operational Environment:** Deployment, compatibility, integration points
- **Data:** Fields, ranges, values, data structures, retention policies
- **Legal/Compliance:** Regulatory requirements, data privacy (GDPR, etc.)
- **Internationalization:** Localization, multi-language, regional considerations

### 8. Non-Goals (Out of Scope)
What this feature will NOT include. Critical for managing scope. Be explicit — ambiguity here causes scope creep.

### 9. Design Considerations (Optional)
- UI/UX requirements or wireframe descriptions
- Link to mockups if available
- Relevant existing components to reuse
- Visual aids or diagrams to clarify intent
- **21st.dev Magic MCP:** For UI stories, note that implementation should use the 21st.dev Magic MCP tools to search for and generate high-quality, intuitive UI components (e.g., buttons, dialogs, tables, cards, forms). Reference specific component types needed so Ralph agents know to use Magic MCP during implementation.

### 10. Technical Considerations (Optional)
- Known constraints or dependencies
- Integration points with existing systems
- Performance requirements
- Data model changes
- API contracts

### 11. Risks & Assumptions
- **Assumptions:** What are we assuming to be true? (e.g., "Users have modern browsers")
- **Risks:** What could go wrong? Include likelihood and impact.
- **Mitigations:** How will we address each risk?

### 12. Success Metrics
How will success be measured? Use specific, quantifiable metrics:
- "Reduce time to complete X by 50%"
- "Increase conversion rate by 10%"
- "Achieve 90% user satisfaction score in post-launch survey"

Include both leading indicators (adoption, engagement) and lagging indicators (revenue, retention).

### 13. Open Questions
Remaining questions or areas needing clarification. Flag who needs to answer each question.

### 14. Appendix (Optional)
- Glossary of terms / naming conventions
- Links to related documents, research, or prior art
- Revision history

---

## Writing Guidelines

### Write for Your Audience
The PRD reader may be a junior developer, AI agent, executive, or cross-functional stakeholder. Therefore:
- Be explicit and unambiguous
- Avoid jargon or explain it in a glossary
- Provide enough detail to understand purpose and core logic
- Number requirements for easy reference
- Use concrete examples where helpful
- Assume minimal outside context about the target user or task

### Emphasize Outcomes Over Features
Focus on what customers want to achieve, not just what the system does. A good user story beats a long spec because it keeps the "why" visible.

### Make It Testable
Every requirement should be specific enough that a team can determine when it's been successfully implemented. If you can't test it, rewrite it.

### Keep It Living
A PRD is not a one-time artifact. Flag sections that are likely to evolve and include a revision history in the appendix. Encourage validation with customers before and after implementation.

---

## Output

- **Format:** Markdown (`.md`)
- **Location:** `tasks/`
- **Filename:** `prd-[feature-name].md` (kebab-case)

---

## Example PRD

```markdown
# PRD: Task Priority System

## Introduction

Add priority levels to tasks so users can focus on what matters most. Tasks can be marked as high, medium, or low priority, with visual indicators and filtering. This is an enhancement to the existing task management feature, driven by user feedback requesting better workload management.

## Strategic Context

- **Vision:** Evolve the task system from a simple list into an intelligent workload manager.
- **Market Context:** Competing tools (Asana, Linear) all offer priority as a core primitive. Lack of priority is a top-3 churn reason in our exit surveys.
- **Rationale:** Q3 user research showed 68% of power users maintain a separate system to track task importance. This feature closes that gap.

## Goals

- Allow assigning priority (high/medium/low) to any task
- Provide clear visual differentiation between priority levels
- Enable filtering and sorting by priority
- Default new tasks to medium priority
- Reduce reliance on external prioritization tools by 50%

## Personas & Customer Context

### Persona: "Busy PM" — Project Manager at a mid-size SaaS company
- **Characteristics:** Manages 3-5 projects simultaneously, non-technical, uses the tool daily
- **Needs:** Quickly identify what needs attention now vs. later
- **Pain Points:** Currently uses color-coded sticky notes alongside the app to track priorities
- **Current Workflow:** Opens task list → scans titles → mentally ranks → switches to sticky notes

### Persona: "Solo Dev" — Independent developer managing client projects
- **Characteristics:** Technical, manages own backlog, values speed over process
- **Needs:** Fast keyboard-driven priority assignment
- **Pain Points:** Wastes time scrolling through flat task lists to find urgent items

## User Stories

### US-001: Add priority field to database  [P0]
**Description:** As a developer, I need to store task priority so it persists across sessions.

**Acceptance Criteria:**
- [ ] Add priority column to tasks table: 'high' | 'medium' | 'low' (default 'medium')
- [ ] Generate and run migration successfully
- [ ] Existing tasks default to 'medium'
- [ ] Typecheck passes

### US-002: Display priority indicator on task cards  [P0]
**Description:** As a Busy PM, I want to see task priority at a glance so I know what needs attention first.

**Acceptance Criteria:**
- [ ] Each task card shows colored priority badge (red=high, yellow=medium, gray=low)
- [ ] Priority visible without hovering or clicking
- [ ] Badge includes both color and text label for accessibility
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

### US-003: Add priority selector to task edit  [P0]
**Description:** As a Busy PM, I want to change a task's priority when editing it.

**Acceptance Criteria:**
- [ ] Priority dropdown in task edit modal
- [ ] Shows current priority as selected
- [ ] Saves immediately on selection change
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

### US-004: Filter tasks by priority  [P1]
**Description:** As a Busy PM, I want to filter the task list to see only high-priority items when I'm focused.

**Acceptance Criteria:**
- [ ] Filter dropdown with options: All | High | Medium | Low
- [ ] Filter persists in URL params
- [ ] Empty state message when no tasks match filter
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

### US-005: Keyboard shortcut for priority  [P2]
**Description:** As a Solo Dev, I want to set priority via keyboard so I can triage tasks quickly.

**Acceptance Criteria:**
- [ ] Press 1/2/3 when task is selected to set high/medium/low
- [ ] Visual feedback on priority change
- [ ] Shortcut documented in help/keyboard shortcut panel
- [ ] Typecheck passes

## Functional Requirements

**Priority Data (FR-1.x)**
- FR-1.1: Add `priority` field to tasks table ('high' | 'medium' | 'low', default 'medium')
  - *Rationale: Goal #1. Dependency: None.*
- FR-1.2: Expose priority in task API (read and write)
  - *Rationale: Enables FR-2.x and FR-3.x.*

**Priority Display (FR-2.x)**
- FR-2.1: Display colored priority badge on each task card
  - *Rationale: Goal #2. Dependency: FR-1.1.*
- FR-2.2: Badge must include text label alongside color for accessibility (WCAG AA)
  - *Rationale: NFR — accessibility.*

**Priority Editing (FR-3.x)**
- FR-3.1: Include priority selector in task edit modal
  - *Rationale: US-003. Dependency: FR-1.2.*
- FR-3.2: Add priority filter dropdown to task list header
  - *Rationale: US-004. Dependency: FR-1.2.*
- FR-3.3: Sort by priority within each status column (high → medium → low)
  - *Rationale: Goal #3.*

## Non-Functional Requirements

- **Usability:** Priority change must complete in < 2 clicks (or 1 keypress via shortcut)
- **Performance:** Filter/sort must render in < 200ms for up to 1,000 tasks
- **Accessibility:** Priority indicators must meet WCAG AA (color + text, screen reader support)
- **Data:** Priority values stored as enum; migration must be backward-compatible

## Non-Goals

- No priority-based notifications or reminders
- No automatic priority assignment based on due date
- No priority inheritance for subtasks
- No custom priority levels beyond high/medium/low (v1)

## Design Considerations

- Reuse existing badge component with color variants
- Priority badge placement: left side of task card title
- Mobile: priority selector uses native dropdown

## Technical Considerations

- Filter state managed via URL search params for shareability
- Priority stored in database, not computed
- Database migration must handle existing rows (default to 'medium')

## Risks & Assumptions

| Risk/Assumption | Type | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| Users may want custom priority levels | Assumption | Medium | Low | Use enum now; design schema to support extension later |
| Priority filter + status filter interaction may confuse users | Risk | Medium | Medium | Test with 3 users before launch; add "clear all filters" button |
| Migration on large tables may cause downtime | Risk | Low | High | Run migration during off-peak; test on staging with prod-size data |

## Success Metrics

- Users can change priority in under 2 clicks (leading)
- 60% of active users set at least one non-default priority within 2 weeks (adoption)
- High-priority tasks immediately visible at top of lists (functional)
- 20% reduction in churn citing "can't organize tasks" (lagging, 90-day measure)

## Open Questions

- Should priority affect task ordering within a column? → **Ask: Design team**
- Should we add keyboard shortcuts for priority changes in v1? → **Ask: PM (scope decision)**
- Do we need an audit log for priority changes? → **Ask: Compliance team**

## Appendix

**Glossary:**
- **Priority:** A user-assigned importance level (high/medium/low) for a task.
- **Badge:** A small colored label displayed on a task card.

**Revision History:**
| Date | Author | Change |
|---|---|---|
| 2025-01-15 | PM Name | Initial draft |
```

---

## Checklist

Before saving the PRD:

- [ ] Asked clarifying questions with lettered options, organized by category
- [ ] Incorporated user's answers into all relevant sections
- [ ] Strategic context explains the "why" behind the feature
- [ ] Personas are defined with needs, pain points, and current workflows
- [ ] User stories are small, specific, and prioritized (P0/P1/P2)
- [ ] User stories reference specific personas (not generic "user")
- [ ] Functional requirements are numbered hierarchically with rationale and dependencies
- [ ] Non-functional requirements address usability, performance, security, and accessibility
- [ ] Non-goals section defines clear boundaries
- [ ] Risks and assumptions are identified with mitigations
- [ ] Success metrics include both leading and lagging indicators
- [ ] Open questions flag who needs to answer them
- [ ] Saved to `tasks/prd-[feature-name].md`

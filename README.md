# su26-ai301-contribution

# Contribution #1: Print-Friendly CSS for Outage Reports and SLA Summaries

**Contribution Number:** 1

**Student:** Melosa Rao

**Issue:** [FE-W6-013] Add print-friendly CSS for outage reports and SLA summaries https://github.com/OpSoll/noc-iq-fe/issues/348

**Status:** Phrase 4 - Complete, PR submitted

---

## Why I Chose This Issue

This issue was well-scoped for a first contribution: a clear, self-contained frontend task (CSS + one small reusable component) with explicit acceptance criteria and implementation guidance already laid out in the issue body. It was labeled `good first issue` and `ready-to-pick-up`, meaning it could be self-assigned without waiting on maintainer triage.

---

## Understanding the Issue and Problem Description

### Expected Behavior

Ops managers should be able to print (or save as PDF) an outage detail page or the payments/SLA summary page and get a clean, readable report: page header with org name and date, full outage/SLA data in a readable format, and none of the surrounding app chrome. A `Print Report` button on both pages should trigger `window.print()`. Multi-page print output should handle table row page breaks cleanly (`page-break-inside: avoid`), and styles should render correctly in both Chrome and Firefox print preview.

### Current Behavior

The app had no print styles at all. Printing any page included the top navigation bar, all action buttons (Edit/Resolve/Delete/Export/etc.), filter controls, pagination controls, and toast notifications — wasting paper and burying the actual data ops managers need for post-mortems and SLA review sessions.

### Affected Components

1. `src/app/globals.css` — no `@media print` rules existed
2. `src/app/outages/[id]/page.tsx` — outage detail page, needed a Print button and print-safe markup
3. `src/components/payments/payments-view.tsx` — SLA/payments summary page, same needs
4. New component: `src/components/shared/PrintButton.tsx`

---

## Reproduction Process

### Environment Setup

- Forked and cloned `noc-iq-fe` on a Windows machine; installed Node.js 18+, ran `npm install`, copied `.env.example` to `.env.local`.
- Since several pages require authentication, also cloned the sibling backend repo `noc-iq-be` (not forked — no changes needed there) and set up a Python virtual environment, installed `requirements.txt`, and configured `.env`.

### Steps to Reproduce

1. Ran `npm run dev` and opened any page in the app (e.g., an outage detail page or `/payments`).
2. Opened the browser print dialog (`Ctrl+P`) or used Chrome DevTools' "Emulate CSS media: print" mode.
3. Observed that navigation, buttons, filters, and pagination all appeared in the print preview alongside the report content, with no visual distinction from the on-screen UI

---

## Solution Approach

### Analysis

The root cause was that print styling had never been implemented. There was no `@media print` block in `globals.css`, no convention for marking an element as "don't print this," and no reusable print-trigger component. 

### Proposed Solution

1. Add an `@media print` block to `globals.css` that hides `nav`, `footer`, and anything marked with a `.no-print` utility class or `[data-no-print]` attribute; strips animations/transitions; forces black-on-white text for readability; and adds `.print-avoid-break` / `.print-page-break` utility classes for controlling table/section page breaks.
2. Create a small, reusable `PrintButton` component 
3. Add `PrintButton` to the outage detail page and the payments (SLA summary) page

### Implementation Plan

1. Append an `@media print` section to `globals.css` (hide nav/footer/`.no-print`, strip animation, force print-safe colors, add break-control utility classes).
2. Add `src/components/shared/PrintButton.tsx` as a small client component with no required props.
3. Wire `PrintButton` into `src/app/outages/[id]/page.tsx` and `src/components/payments/payments-view.tsx`.
4. Mark the action-button rows, filter bar, and pagination controls on both pages with `no-print` so they disappear from the printed/PDF output.
5. Manually verify with Chrome DevTools print emulation.

---
### Manual Testing

Verified using Chrome DevTools on both the login page (to confirm the base `@media print` rules apply globally) and the `/payments` page. 

Full end-to-end testing against live outage/payment data was blocked by an unrelated, pre-existing backend bug (`sqlalchemy.exc.AmbiguousForeignKeysError` between `sla_results` and `sla_disputes` in `noc-iq-be`) that prevents account registration and data creation; this was confirmed as out of scope for this issue and not something introduced by this change.

---

## Implementation Notes

- Set up local dev environment on Windows for both `noc-iq-fe` and `noc-iq-be` (Node.js install, Python venv, `.env` configuration).
- Hit and resolved several environment issues along the way: a pre-existing React context crash (`useAccessibility` used outside its provider in `src/app/layout.tsx`), a missing `redis` dependency, a missing PostgreSQL driver (switched `DATABASE_URL` to SQLite for local testing), and a Pydantic settings validation error caused by placeholder Stellar/blockchain config values.
- Implemented the `@media print` CSS block, the `PrintButton` component, and wired it into both target pages.
- Added `no-print` classes to the action buttons, filter bar, pagination controls, and the inline edit form after noticing they were still appearing in the print preview during manual testing.
- Hit a runtime `Module not found: Can't resolve 'lucide-react'` error; resolved with `npm install lucide-react`.
- Encountered a backend crash (`sqlalchemy.exc.AmbiguousForeignKeysError`) that blocks registration/login and outage creation; confirmed this is a pre-existing, unrelated bug and worked around it for testing purposes by temporarily bypassing the auth redirect in `RouteGuard.tsx` (reverted before committing).


**Maintainer Feedback:**  awaiting review

**Status:** Awaiting review


































# su26-ai301-contribution
# Contribution [#]: Agno autolog spans

**Contribution Number:** 1  
**Student:** Melosa Rao
**Issue:**  [BUG] Agno autolog spans do not nest under manual span creation 
https://github.com/mlflow/mlflow/issues/24241
**Status:** Phase I [In Progress / Complete] 

---

## Why I Chose This Issue

This issue is a good fit because my recent research work involved building agentic AI workflows and ML evaluation pipelines, so I'm familiar with how tracing and span context works in practice. 
The bug is well-scoped with a clear diagnosis already provided.

---

## Understanding the Issue and Problem Description
### Expected Behavior
When mlflow.agno.autolog() is enabled and an Agno agent call is wrapped inside a manual span using mlflow.start_span(...), the Agno-generated spans (agent, model, tool) should nest as children under the manual span, producing a single unified end-to-end trace.

### Current Behavior
The Agno spans do not nest under the manual span. Instead, two completely disconnected traces are created: one containing just the empty manual span, and a separate one containing the Agno agent/model/tool spans as their own root. This makes it impossible to get a full end-to-end trace combining application-level and auto-instrumented spans. The bug only affects the Agno integration; other integrations like CrewAI nest correctly.


### Affected Components
1. area/tracing: MLflow Tracing features and LLM tracing functionality
2. Agno autolog integration (mlflow.agno.autolog())
3. OpenTelemetry tracer provider resolution
---

## Reproduction Process

### Environment Setup

Created a virtual environment and installed requirements.txt

### Steps to Reproduce

1. Forked and cloned gitbub repo
2. Currently working through code base to figure out tracing issue

### Reproduction Evidence



---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]


# su26-ai301-contribution
# Contribution [#]: ML Training Example

**Contribution Number:** 1  
**Student:** Melosa Rao
**Issue:**   https://github.com/apache/burr/issues/138
**Status:** Phase I [In Progress / Complete]

---

## Why I Chose This Issue

Apache Burr is an agentic AI framework that makes use of state machine. I am excited to explore it. I  choose this as I am familiar with ML training pipelines and LLM integration.

---

## Understanding the Issue and Problem Description
They want a complete ML training example on their framework. A template notebook with a training skeleton is included. I need to fill in the notebook with a training example including downloading a real dataset like MNIST and then adding a Epoch-based training loop and Human-in-the-loop pause/resume training option.


### Expected Behavior

A training loop should run on sample dataset with adjustable parameters and human in loop options.

### Current Behavior
Only template code is included so it does not run.

### Affected Components
https://github.com/MelosaRao/burr/tree/main/examples
The main issue is just reproducing and filling in example usage template.


---

## Reproduction Process

### Environment Setup

Created a virtual environment and installed requirements.txt

### Steps to Reproduce

1. Forked and cloned gitbub repo
2. Currently working through code base to figure out training modules and methods

### Notes
This repo had no feeback from the moderators and I was advised to choose another repo by Codepath tfs.

- **Commit showing reproduction:** :  https://github.com/MelosaRao/burr


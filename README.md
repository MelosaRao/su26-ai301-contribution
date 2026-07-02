# Contribution [#]: 

**Contribution Number:** 1  
**Student:** Melosa Rao
**Issue:**  [BUG] Agno autolog spans do not nest under manual span creation
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
area/tracing: MLflow Tracing features and LLM tracing functionality
Agno autolog integration (mlflow.agno.autolog())
OpenTelemetry tracer provider resolution
---

## Reproduction Process

### Environment Setup

Created a virtual environment and installed requirements.txt

### Steps to Reproduce

1. Forked and cloned gitbub repo
2. Currently working through code base to figure out training modules and methods

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


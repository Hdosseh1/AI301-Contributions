# Contribution [#]: [Issue Title]

**Contribution Number:** 1 
**Student:** Hayden Dosseh  
**Issue:** https://github.com/BOINC/boinc/issues/7162 
**Status:** Phase I  Complete

---

## Why I Chose This Issue

I'm interested in this issue because it sits right at the intersection of embedded/hardware systems and applied ML, which is where most of my project work has been — I've built neural network accelerators on FPGA (using VHDL on a PYNQ-Z1), done on-device ML inference with MediaPipe and PyTorch, and worked with cross-architecture constraints on STM32 and Raspberry Pi platforms as part of my hardware security research. Cross-compiling CUDA samples for arm64 combines exactly that background: understanding toolchains, linker/library paths, and target-architecture flags for GPU-accelerated code running on ARM boards like Jetson — hardware increasingly used for edge ML deployment, which is a space I want to keep working in. I'm hoping this issue teaches me the practical mechanics of cross-compilation tooling (host vs. target compilers, nvcc's -ccbin flag, and library path conventions) in a real open-source build system, while also giving me my first experience navigating a large collaborative C++ codebase's contribution process from issue to reviewed PR.
---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

[Which parts of the codebase are involved?]

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

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
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]

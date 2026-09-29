# Contribution 1: Cross-compile samples/nvcuda for Linux arm64

**Contribution Number:** 1 
**Student:** Hayden Dosseh  
**Issue:** https://github.com/BOINC/boinc/issues/7162 
**Status:** Phase II  Complete

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

Ubuntu 24.04 (x86_64). Installed the build dependencies (`libssl-dev`, `libcurl4-openssl-dev`, `m4`, `pkg-config`) and the arm64 cross-compiler (`g++-aarch64-linux-gnu`, `gcc-aarch64-linux-gnu`). Cloned BOINC and built only the core libraries with `./_autosetup` and `./configure --disable-server --disable-manager --disable-client`, then `make`. This produces `api/libboinc_api.a` and `lib/libboinc.a`, which the nvcuda sample links against.

Challenge: my machine has no NVIDIA GPU or CUDA toolkit, so I couldn't finish a full CUDA build. I reproduced the build-system side of the issue instead: the hardcoded paths, compilers and architecture assumptions in the Makefile.

### Steps to Reproduce

1. Build the BOINC core libraries natively on x64 (see above), then run `make` in `samples/nvcuda`.
2. Result: the build fails at the first compile step with `cuda_runtime.h: No such file or directory`. The Makefile hardcodes `CUDA_INSTALL_PATH ?= /usr/local/cuda` and `-L$(CUDA_INSTALL_PATH)/lib64`, both x64 host paths.
3. Compile a small BOINC API program with `aarch64-linux-gnu-g++` and link it against the x64-built `libboinc_api.a` and `libboinc.a`. This is what the Makefile would do if you only swapped `CXX`.
4. Observed result: the linker fails with `skipping incompatible ../boinc/api/libboinc_api.a` and `cannot find -lboinc_api` (the same happens for `-lboinc`).

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** 
- **My findings:** The Makefile has no concept of a target architecture. It assumes host = target = x64 in three places: the CUDA library path (`lib64`), the host C++ compiler (`CXX` defaults to native `g++`), and the `nvcc` call (no `-ccbin`, so it uses the native host compiler). I also found that BOINC's own `configure` already supports `--host=aarch64-linux-gnu`, and the core libraries cross-built for arm64 without errors. So the gap is only in the sample's Makefile.

---

## Solution Approach

### Analysis

The root cause is that `samples/nvcuda/Makefile` is written only for a native x64 build. To cross-compile for arm64 you need three things to point at arm64 versions: the host compiler, the CUDA target libraries and headers, and the BOINC libraries. Today the Makefile hardcodes the x64 version of each, with no variable to switch them.

### Proposed Solution

Add an optional `TARGET_ARCH` variable to `samples/nvcuda/Makefile` so the native x64 build keeps working unchanged. When it is set to `aarch64`, the Makefile would:

1. Set `CXX` to `aarch64-linux-gnu-g++`.
2. Point the CUDA include and library paths at NVIDIA's arm64 cross-toolkit (e.g. `$(CUDA_INSTALL_PATH)/targets/sbsa-linux/`) instead of `lib64`.
3. Pass `-ccbin aarch64-linux-gnu-g++` to `nvcc` so the host-side code in `cuda_kernel.cu` is compiled for arm64.
4. Link against BOINC libraries built with `./configure --host=aarch64-linux-gnu`.

I plan to also document the steps: install NVIDIA's cross-aarch64 CUDA packages, cross-build the BOINC libraries, then run `make TARGET_ARCH=aarch64`. If the maintainers want it, I would add a CI job to check the cross-build.
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

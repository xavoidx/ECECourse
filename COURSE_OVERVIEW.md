# DSP / ECE Self-Study Course — Overview

This document describes the structure and philosophy of an ongoing, indefinite-length self-study program in DSP, circuits, embedded systems, and supporting math. It is paired with `SESSION_LOG.md`, which tracks chronological progress.

**If you are a new Claude instance reading this:** read this entire file first, then `SESSION_LOG.md`. Together they fully reconstruct the course context. The student's code lives in a separate Git repo (`AudioApp`); ask the student to share it if you don't have it.

---

## Current state at a glance (as of Session 12, 2026-05-11)

For fast orientation if returning to the course after a gap. Read this first.

**Track A (DSP):** FIR thread, Day 3 of ~6 complete. Scalar FIR working with double-buffer optimization (no modulo, reversed coefficients, NEON-ready). Days 4-6 remaining (windowed-sinc derivation, NEON, IIR-vs-FIR close). IIR mastery thread queued after.

**Track B (Circuits):** Pedal input stage Day 1 complete. Schematic correct. **Blocked on MCP6022 op-amp arriving in mail.** When it arrives: build, 5-step test plan, capture frequency sweep plot. Can do Track B sessions in parallel without the chip: noise analysis primer (interview-rich, ADI-relevant) is recommended next non-blocking Track B work.

**Track C (Embedded):** Phase 1 Day 1 complete. Nucleo-F446RE blinking at 180 MHz, toolchain working end-to-end. Day 2 will configure ADC + DMA + TIM6 trigger at 48 kHz. Recommended approach: DAC→ADC loopback test (no analog hardware needed) — generates a known sine via DAC, verifies it appears at ADC.

**Track D (Math):** Still completely cold. EM sub-thread queued (6 sessions). Probability for stochastic signals is the strongest first opener (enables Track B noise analysis). Should be opened within ~3 sessions.

**Track E (FPGA):** Queued, not opened. Trigger conditions not yet met.

**Tooling:** Git + GitHub working (https://github.com/xavoidx/AudioApp). Python intro session still pending — needs venv with numpy/scipy/matplotlib before first integrated use. Claude Projects setup recommended but not done.

**Next-session options for student to choose:**
1. Track A Day 4 — windowed-sinc derivation by hand (the satisfying payoff).
2. Track C Day 2 — ADC+DMA+DAC loopback test.
3. Open Track D — probability or first EM session.
4. Continue Track B (noise analysis, since MCP6022 hasn't arrived).

---

## Student background

- **Stage:** Finishing 2nd year ECE undergraduate (as of May 2026).
- **Identity:** Owen Roush (`owenroush06@gmail.com`). GitHub: `xavoidx`. Mac development (`Owens-MacBook-Pro-3`, M-series, zsh).
- **DSP reading:** Lyons' *Understanding DSP* read through Chapter on Quadrature signals (stopped right before).
- **DSP implementation:** Has implemented IIR filters (Biquads, EQ Cookbook formulas) in a personal C++ audio app, including NEON SIMD optimization. Has implemented scalar FIR (Sessions 2-9) with double-buffer optimization.
- **Circuits:** Up through RLC + Laplace transforms. Beginning a semiconductors textbook. As of Session 5, has designed (with debugging) a complete guitar pedal input stage with proper anti-alias filtering and single-supply biasing. Finished an undergraduate EM course but professor never covered Maxwell's equations — stopped at inductance. EM gap addressed via Track D sub-thread.
- **Embedded:** Building an STM32 guitar pedal. Comfortable but new to bare-metal at this level. As of Session 7: Nucleo-F446RE running blink at 180 MHz, full toolchain working.
- **Languages:** Intermediate C++. Basically no Python or MATLAB experience. New to Git (started Session 10).
- **Math:** Standard 2nd-year math through diff eq, linear algebra, complex numbers (basic).

## The AudioApp project

Student's personal audio application written in C++. Architecturally:

- **Node-graph audio engine** with topological sort (Kahn's algorithm) for processing order.
- **Planar-internal, interleaved-at-boundaries** buffer convention (correct, professional choice).
- **NEON SIMD optimized Biquad** processing two channels in parallel using `vmla_f32` / `vmls_f32`.
- Uses PortAudio for I/O and ImGui + ImPlot for visualization.
- Direct Form I biquad implementation (not yet TDF2).
- As of Session 9: scalar FIR filter implemented with double-buffer pattern (no modulo in inner loop), reversed-coefficient storage for forward-direction dot product (NEON-ready), impulse response verified correct.

**GitHub repo:** https://github.com/xavoidx/AudioApp (public, as of Session 10).

**Build environment:** macOS, M-series Mac, NEON intrinsics (ARM64). Vendored ImPlot in `ImPlot/` directory (34 MB, no nested .git).

Known issues to be addressed in future sessions:

1. Direct Form I → Transposed Direct Form II rewrite (Track A IIR thread).
2. Coefficient updates not real-time safe (sin/cos/pow on calling thread, non-atomic writes).
3. `Nodal_Process` allocates and prints in audio callback — should precompute topo order once.
4. No denormal handling.
5. `Biquad.hh` missing `#pragma once`.
6. `processor.hh` defines an unused `Processor` class.

## The STM32 pedal project (hardware)

**Goal:** functional digital guitar pedal as both a learning vehicle and portfolio piece. Anchor for Tracks B and C.

**Hardware:**
- **MCU board:** Nucleo-F446RE (STM32F446RE, 180 MHz Cortex-M4F, 12-bit ADC, 2× 12-bit DACs). Has ST-Link/V2-1 with 8 MHz HSE source from the ST-Link MCU (not a crystal). User LED LD2 on PA5.
- **Analog input stage (Session 5 design):** 1MΩ shunt to GND at input tip → C_in DC block → 1MΩ bias to V_mid → unity-gain buffer → Sallen-Key LPF (R₁=R₂=2.2k, C₁=C₂=6.8nF, G=1.59, Q=0.707, fc≈10.6 kHz). V_mid from 10k+10k divider with 100nF + 10µF decoupling.
- **Op-amp:** MCP6022 ordered (Path B chosen — 3.3V single supply native). 5532 is too high min-supply (10V differential); OPA1652 also too high (4.5V min).
- **Audio output (planned):** internal DAC1 on PA4.
- **Power:** 9V battery → LDO → 3.3V.
- **Audio I/O pins on STM32:** ADC1_IN1 = PA1 (audio in); DAC1_OUT = PA4 (audio out).

**Software / firmware state (as of Session 7):**
- Toolchain: STM32CubeIDE 1.x on macOS, CubeMX for peripheral config.
- Clock tree configured correctly: HSE 8 MHz → PLL (M=4, N=180, P=2) → SYSCLK = HCLK = 180 MHz. APB1 = 45 MHz (PCLK1, /4), APB2 = 90 MHz (PCLK2, /2). Stopwatch-verified.
- LED blink works on PA5 with `HAL_Delay(500)`. Toolchain end-to-end verified.
- ADC/DAC/DMA peripherals were configured in CubeMX initially but disabled for clean Day 1; will be properly configured in Track C Day 2 with TIM6 trigger at 48 kHz.

**macOS toolchain quirks discovered (Session 7):**
- Default launch configs may be wrong type — must use "STM32 Cortex-M C/C++ Application," not generic "C/C++ Application."
- Bundled `arm-none-eabi-gdb` is not on PATH; must provide absolute path in launch config:
  `/Applications/STM32CubeIDE.app/Contents/Eclipse/plugins/com.st.stm32cube.ide.mcu.externaltools.gnu-tools-for-stm32.14.3.rel1.macosaarch64_1.0.0.202602081740/tools/bin/arm-none-eabi-gdb`
- CubeMX inherits wrong HSE frequency defaults (assumed 25 MHz; Nucleo-F446RE is 8 MHz). Must verify and stopwatch-test.

## Career goals

- **Target companies:** ADI + NVIDIA (primary, hardware DSP / mixed-signal). Shure / DiGiCo / Bose tier (secondary, audio DSP) — happy to land at any. Adjacent: Universal Audio, Cirrus Logic, Texas Instruments, Qualcomm, Microchip, Sonos.
- **Application timeline:** ~6 months out (early 3rd year) for **summer internships**. Not full-time new-grad.
- **Calibration implications:**
  - Track B (analog) gets significant weight — ADI is dominantly analog.
  - Track A IIR thread, fixed-point thread, and DSP architecture thread get extra weight (relevant to both ADI and NVIDIA).
  - Track E (FPGA) firmly committed once trigger conditions met.
  - CUDA mini-thread queued (~month 4-5) for NVIDIA-specific differentiation.
  - MATLAB queued, depth TBD — useful for ADI but not blocking.
- **Honest realistic assessment:** With this curriculum + good resume + referrals, ~40-70% chance of landing some solid internship; ~10-20% chance at one of the four named companies specifically. Apply broadly. Curriculum is not the bottleneck — resume strength, GitHub presence, and referrals are.
- **Highest-leverage non-curriculum actions:**
  - Get AudioApp on GitHub with polished README + measurements/demos.
  - Get STM32 pedal to a demo state (working buffer + one effect minimum).
  - Find research with a professor for this summer.
  - Apply to internships *now* for summer — most have rolling deadlines.

## Tools and language strategy

Building on the deep C++/DSP foundation. Each addition has a specific role:

- **C++:** primary implementation language. Already strong.
- **Python (NumPy/SciPy/Matplotlib):** Tier 1 add. *No dedicated thread* — integrated continuously, intro session pending. Used for filter design verification, frequency response plots, signal generation/analysis. Goal: functional fluency by month 3 through accumulated use. **Pre-Session-intro homework:** student needs Python venv set up with numpy/scipy/matplotlib installed.
- **Git + GitHub:** Tier 1 add, set up Session 10. Repo at https://github.com/xavoidx/AudioApp. Auth via macOS Keychain (PAT cached). Student is new to Git; got a crash course Session 11. Workflow: small focused commits with conventional-commit-style messages (`feat:`, `fix:`, `perf:`, `refactor:`, `docs:`, `test:`). End-of-session commits part of persistence ritual.
- **Linux command line:** Tier 1 add. macOS zsh terminal is the daily driver.
- **MATLAB / Simulink:** Tier 2 add, queued. Probably 3-5 dedicated sessions when natural moments arise (Filter Designer, Simulink for control, MATLAB HDL Coder for FPGA prep). Decision deferred.
- **Verilog / SystemVerilog:** Track E — queued, opens at trigger conditions.
- **CUDA:** ~5-session mini-thread, ~month 4-5.

What we are *not* learning right now: Rust, web/frontend, generic ML frameworks, Linux kernel internals.

## Course philosophy

### Tracks, no fixed weeks

Tracks are advanced independently. Each session, student picks (or is suggested) a track. The course has no end date; it runs as long as the student finds it useful.

- **Track A — DSP Theory & Implementation.** Lyons-and-beyond. Anchored in AudioApp + pedal.
- **Track B — Circuits & Analog.** Continues from RLC + Laplace. Most relevant to ADI.
- **Track C — Embedded & Systems.** Restructured around the STM32 pedal build.
- **Track D — Math & Foundations.** Slow-burn, 1-2 sessions/week.
- **Track E — FPGA / HDL.** *Queued, not yet open.* Opens at trigger conditions.

### Session format

20/20/20: concept → worked example → implementation/measurement task. ~1 hour. Student shows real artifacts (code, plots, measurements) before moving on.

### Session start signals

Student says one of:
- "Continue [track]" — Claude picks up next topic.
- "[Track], topic X" — student directs to specific topic.
- "What should I do today?" — Claude suggests based on recent sessions and cold tracks.
- "Project day" — no new theory, work on multi-session project with review.

### Dynamic adjustment

Claude reads signals and adjusts:
- **Compression** (faster): correct answers without hints, code working first try, unprompted forward-looking questions.
- **Expansion** (slower): wrong reasoning even when correct answer arrives, repeated similar questions, conceptual gaps in code.
- **Lateral** (branch): excitement on a topic → deeper; boredom → change track.

### Honest feedback contract

Student commits to telling Claude:
- When something clicked or didn't.
- When bored or frustrated.
- When life intrudes.
- When Claude is at the wrong level.

### Persistence ritual (end of every session)

1. Claude provides `SESSION_LOG.md` entry.
2. Claude provides any updates to `COURSE_OVERVIEW.md` (rare).
3. Student commits both to course repo.
4. Student commits AudioApp code with sensible message tied to session's work.

**Recovery protocol if chat is lost:** paste both `.md` files into a fresh Claude conversation. Tell the new Claude "I'm continuing a structured DSP/ECE course; read these two files first."

## Capability tier targets

### Tier 1 — Solid junior intern
- Build any standard audio effect from spec.
- Design windowed-sinc or Parks-McClellan FIR to a given mask.
- Implement Biquad in fixed-point on STM32 unaided.
- Read op-amp datasheet, design inverting amp with bandwidth + noise budget.
- Comfortable with Python for DSP analysis and visualization.

### Tier 2 — Strong new-grad / Shure-DiGiCo level
- Design multi-stage signal chain end-to-end with latency + noise budgets.
- Implement adaptive filters (LMS/RLS) for noise cancellation.
- Design discrete preamp from input jack to ADC.
- Profile and optimize DSP on real hardware.
- Comfortable with sample rate conversion, polyphase, filter banks.
- Functional Verilog skills with one substantial FPGA DSP project.

### Tier 3 — ADI/NVIDIA-competitive
- Architect sigma-delta ADC modulator (math, not silicon).
- Understand mixed-signal design tradeoffs.
- Implement non-trivial DSP at assembly/intrinsic level with full pipeline/cache/SIMD understanding.
- Design active filters with real component tolerances and stability margins.
- Comfortable reading IC datasheets and app notes.
- HDL DSP fluency. CUDA-implemented DSP algorithm (for NVIDIA-direction).

## Track A — DSP roadmap

Threads execute roughly in order. Each is multiple sessions. **Bold** = queued/in-progress.

1. **FIR design & implementation** (in progress)
   - Days 1-3 done: foundations, scalar implementation, optimization (double-buffer, no modulo, reversed coefficients for forward-direction dot product).
   - Days 4-6 remaining: windowed-sinc derivation by hand + design + measurement (Day 4), NEON vectorization (Day 5), IIR vs FIR comparison and thread close (Day 6).
   - Sampling sub-thread embedded as warmups (Q1 done, Q2 done; Q3-Q5 pending in Days 4-6).

2. **IIR mastery** (queued next, ~6-8 sessions)
   - Analog prototype filters (Butterworth, Chebyshev, elliptic).
   - Bilinear transform derivation from trapezoidal integration.
   - Apply BLT by hand to derive cookbook Biquad coefficients.
   - Frequency warping and pre-warping.
   - Pole-zero design.
   - Direct Form I → TDF2 (apply to AudioApp's Biquad).
   - IIR fixed-point: coefficient quantization, cascaded biquads, lattice form.
   - Higher-order IIR: cascade vs parallel forms.
   - Block IIR / parallel IIR for SIMD.
   - **Flagged as FPGA prep.**

3. Quadrature & complex signals.

4. **Fixed-point math & quantization** (general). **Flagged as FPGA prep.**

5. Spectral analysis (STFT, windows).

6. Multirate (decimation/interpolation/polyphase).

7. Audio effects (reverb FDN, chorus, pitch shifting).

8. Adaptive filters (LMS/RLS).

9. Advanced filter design (Parks-McClellan, elliptic, optimization).

10. **DSP architecture** (MAC pipelines, DMA patterns). **Flagged as FPGA prep**: extend with explicit MCU-vs-FPGA comparison.

11. PDM / sigma-delta ADCs.

12. System design (latency budgets, clock domains, SRC).

13. Wavelets.

14. Beamforming.

15. Spatial audio / MIMO.

16. Machine-learning DSP.

### Sampling sub-thread

5-question warmup sequence inserted at start of FIR sessions 2-6.

- **Q1 (Day 2):** Aliasing of 1/7/19kHz at fs=16kHz. **DONE — correct.**
- **Q2 (Day 3):** Why "greater than" not "greater than or equal" in Nyquist. Counterexample at exactly fs/2. **DONE — correct.** Student identified phase-dependent vanishing; sharpened to "rotation per sample = π means only two points on the unit circle, insufficient to recover both amplitude and phase."
- **Q3 (Day 4):** Exact reconstruction formula (sinc interpolation), why never used in practice.
- **Q4 (Day 5):** Anti-alias filter design for STM32 pedal — minimum spec, RC sufficiency, practical solutions.
- **Q5 (Day 6):** Sample-and-hold frequency response, droop at fs/2, correction methods.

## Track B — Circuits & Analog roadmap

Student's current state: completed pedal input stage design (Session 5).

1. ~~Diodes & transistors~~ — defer; pedal-driven path skipped this. Will return as completeness sweep.
2. **Op-amps ideal & real** — ✓ partially covered Session 5 (supply minimums, common-mode range, SR/BW/noise concepts, datasheet verification).
3. **Active filters** — ✓ Sallen-Key covered Session 5. MFB and state-variable still to do.
4. **Audio-specific circuits** — ✓ instrument input stage done. Tone stacks, gain stages, effects loops still to do.
5. Noise analysis (Johnson, shot, 1/f, NF calculation). *Strong ADI relevance — prioritize.*
6. ADC/DAC architectures (SAR, sigma-delta, R-2R). *Pedal-relevant.*
7. Analog signal integrity at PCB level.
8. Power supply design for low-noise audio.
9. Discrete preamp design (substantial standalone project).
10. Mixed-signal layout principles.
11. RF basics.

**Cross-track plumbing:** the pedal's output stage (DAC reconstruction filter + line driver) is both Track B and Track C work — handle when we get there.

## Track C — Embedded & Systems roadmap (pedal-aligned)

Restructured to track the pedal build. Phases align with concrete pedal milestones.

**Hardware confirmed (Session 7):**
- Nucleo-F446RE (STM32F446RE, 180 MHz Cortex-M4F, has FPU, has internal DAC)
- Audio output method: internal DAC1 on PA4
- Audio input: ADC1_IN1 on PA1 (shared pin across ADC1/ADC2/ADC3 — only one used)

### Phase 1: Get audio in
*(Pedal milestone: scope shows guitar audio inside STM32 memory.)*
1. ARM Cortex-M architecture overview (registers, modes, FPU). **Partial — covered in Day 1.**
2. STM32 clock tree. **DONE Session 7** — student went deep on this including the lengthy explanation in Session 8 (HSI/HSE/PLL/SYSCLK/HCLK/APB1/APB2, timer prescaler doubling rule, SysTick→HAL_Delay chain).
3. ADC peripheral deep-dive (SAR architecture, sample timing, jitter). **Pending Day 2.**
4. DMA for ADC (double-buffered). **Pending Day 2.**
5. Interrupt handling — DMA half-complete and complete interrupts. **Pending Day 2.**

### Phase 2: Get audio out
*(Pedal milestone: dry signal in → dry signal out.)*
6. STM32 DAC peripheral (internal DAC1 on PA4). For Day 2-3, plan is **DAC→ADC loopback test** (jumper PA4 to PA1) so student can test ADC + DAC + DMA pipeline without waiting on the MCP6022 analog input stage to arrive. Generates a known sine via DAC, verifies it appears at ADC. Decouples firmware development from analog hardware.
7. Output filtering (reconstruction filter — overlaps Track B).
8. Pairing input DMA with output DMA, sample-rate-matched.

### Phase 3: Real-time processing
*(Pedal milestone: dry signal in → biquad-filtered signal out.)*
9. Real-time scheduling without RTOS (ping-pong buffer pattern).
10. Latency budgets — exact time per buffer.
11. Port AudioApp Biquad to STM32. Profile.
12. Fixed-point Biquad. Bridges into Track A IIR thread.

### Phase 4: Make it a pedal
*(Pedal milestone: musical effect.)*
13. Implement an effect — distortion, delay, tremolo, chorus.
14. UI: pots/buttons via GPIO and ADC inputs.
15. Bypass switch handling.
16. Power-on click suppression, mute on switching.

### Phase 5: Productionize
*(Pedal milestone: would let someone play through it.)*
17. CMSIS-DSP library (and when not to).
18. Optimization: assembly inspection, MAC instructions, FPU usage.
19. Bare-metal optimization (cache, branch prediction).
20. PCB design (overlaps Track B item 7, EM sub-thread items 1, 4, 5).
21. Bootloader / DFU.

### Beyond the pedal
22. RTOS basics (FreeRTOS).
23. USB audio class.
24. SoC-class processors (Cortex-A, embedded Linux, NEON proper).

## Track D — Math & Foundations roadmap

Main threads:

1. Complex analysis beyond Lyons (residues, contour integrals, conformal maps).
2. Linear algebra for signal processing (least-squares, projections, SVD, eigendecomposition).
3. Probability for stochastic signals (random processes, PSD, ergodicity). *Important for noise analysis (Track B).*
4. Optimization (convex, gradient methods, applied to filter design).
5. Control theory (Bode, Nyquist, root locus).
6. Information theory basics.

### EM Essentials sub-thread (queued)

Targeted EM literacy for ADI/NVIDIA/Shure/DiGiCo interview readiness. *Not* comprehensive EM — covers must-know concepts in ~5-6 sessions over ~3 months, paced one session every 2-3 weeks alongside other Track D topics.

Student finished undergrad EM course but professor stopped at inductance — never covered Maxwell's equations. This sub-thread closes that gap with focus on what interviewers actually ask.

- **EM-1: Lumped vs. distributed boundary.** Electrical length, wavelength, the 1/10 rule. Edges drive the math, not the nominal frequency. *Interview prompt: "1 ns edge on 10cm trace — impedance control needed?"*
- **EM-2: Maxwell's equations.** Four equations in integral and differential form, English interpretation of each. Wave equation derivation (two lines). Speed of light from µ₀ and ε₀. Conceptual only, no PDE solving. *Interview prompt: "How did Maxwell predict EM waves?"*
- **EM-3: Skin effect and HF conductor non-idealities.** √f current crowding. Why RF inductors use Litz wire. Why audio cables don't suffer from skin effect (despite audiophile claims — useful talking point). *Interview prompt: "Why Litz wire in RF inductors?"*
- **EM-4: Transmission lines.** Characteristic impedance from L and C per unit length. Why 50Ω in RF, 75Ω in video, 100Ω differential. Reflection coefficient Γ. Open/short reflection intuition. *Interview prompt: "Reflection coefficient for open circuit?"*
- **EM-5: High-speed digital signal integrity.** Termination strategies (series, parallel, AC, Thevenin). DDR termination. Crosstalk. Ground bounce. *Interview prompt: "Why series termination on DDR4?"* **Strong NVIDIA hardware role relevance.**
- **EM-6: EMI/EMC basics.** Why a circuit radiates. Common-mode vs differential. Ground planes as shields. Connects to Track B decoupling work. FCC Class A/B. *Interview prompt: "Why does a ground plane under signal traces reduce EMI?"*

Recommended sources: Howard Johnson "High-Speed Digital Design" (sessions 1, 4, 5, 6); MIT OCW 6.013 lectures 1-5 (session 2); EEVblog YouTube for supplementary intuition. No textbook purchase required.

Cross-track tie-ins:
- EM-1, EM-4, EM-5 → relevant when Track C Phase 5 hits PCB design.
- EM-3 → revisits Track B noise analysis with HF conductor effects.
- EM-5 → relevant when Track E opens (FPGA high-speed I/O).
- EM-6 → extends Track B decoupling discussion with EM framing.

**Track D status: completely cold. Should be opened within next ~3 sessions per persistence ritual.**

## Track E — FPGA / HDL roadmap *(queued, not open)*

### Trigger conditions (any sufficient to open)
- Track A IIR mastery thread + fixed-point thread completed.
- Track C Phase 3 in progress.
- Project pull (e.g., multi-channel beamformer).

### Phases when opened

**Phase 1: Verilog literacy** (~6 sessions)
- Verilog vs SystemVerilog vs VHDL.
- Combinational vs sequential logic; `always @(*)` vs `always @(posedge clk)`.
- Simulation (Verilator, Icarus). Testbenches.
- Synthesis vs simulation. What synthesizes.

**Phase 2: DSP primitives in HDL** (~6-8 sessions)
- Registered MAC unit, pipelined.
- Biquad in HDL. Compare resources to STM32.
- FIR via systolic array.
- Fixed-point HDL: Q-format, overflow, scaling.

**Phase 3: Real FPGA project** (variable)
- Pick something portfolio-worthy and finishable in 4-8 weeks part-time.
- Synthesize, place-and-route, run on hardware.

**Phase 4: Industry alignment** (ongoing)
- Vivado / Quartus on Xilinx or Intel board.
- AXI4 bus protocol.
- Zynq / MPSoC familiarity.

### Pre-opening passive prep in other tracks
- Track A IIR thread → fixed-point IIR.
- Track A general fixed-point thread.
- Track A DSP architecture thread → MCU vs FPGA comparisons.
- Track C Biquad-on-STM32 → comparison point for FPGA implementation.

### Hardware to obtain (low priority)
- iCE40 board — open toolchain, $30-100. Good first board.
- Xilinx Artix-7 — industry-standard, $150-250. Step up.

## CUDA mini-thread *(queued, ~month 4-5)*

~5 sessions targeting NVIDIA differentiation. Implement at least one DSP algorithm in CUDA. Specific topics TBD when thread opens.

---

## Calibration notes

Observations about how this student learns. Update as more data accumulates.

**Strengths:**
- **Strong mathematical intuition.** Reaches correct conclusions quickly.
- **Recovers well from corrections.** When pushed, comes back with corrected reasoning rather than capitulating. Healthy pattern.
- **Pushes back appropriately.** Has corrected Claude when initially graded too harshly. Good — interview-positive trait.
- **C++ habits good for student level.** Architectural sense above average. Has internalized: narrowing-as-error, include-what-you-use (still occasionally forgets sign-compare).
- **Discovers principles organically.** In Session 5 self-discovered "everything references V_mid" rule. In Session 9 independently arrived at the double-buffer algorithm without peeking at the stub. Healthy integration.
- **Verifies datasheets independently.** Caught 5532 spec issue when assigned. Strong engineering hygiene.
- **Asks the right kind of "why" questions.** "Why two caps if it's basically one?" (exposed parasitic L / SRF). "Sallen-Key vs non-inverting amp Z difference?" "Why does 180 MHz matter for HAL_Delay?" These deepen rather than just confirm. Treat these as opportunities for full explanations even when it extends the session.
- **Self-aware about course continuity.** Suggested the recovery `.md` approach. Suggested aligning Track C to the pedal. Suggested Claude Projects. Asks forward-looking questions (FPGA timing, what skills to add, EM scope). Treat as a partner in course design.
- **Patient with long debugging.** Session 7 was ~hours of macOS toolchain debugging without giving up. Important indicator of how he'll handle real engineering frustration.

**Patterns to watch:**
- **Tendency to pattern-match to remembered structures rather than re-deriving.** Counter: push first-principles derivations. Has improved but still occasionally returns.
- **Communication compressed.** Skips explicit steps assuming they're implied (e.g. unit-circle reasoning on Sampling Q1 without spelling out the reflection check). Costs partial credit in interviews. Prompt to be more explicit when grading is on the line.
- **Style-of-code is solid but watch for clever-but-opaque expressions.** Example Session 9: `*inPtr` then `*inPtr++` on consecutive lines — worked by accident, intent obscure. Push toward named-variable clarity over evaluation-order tricks.

**Track-specific observations:**
- **Track A:** Math intuition strong; willing to do full derivations when prompted. Day 4 windowed-sinc derivation expected to go well.
- **Track B:** Asks good skeptical questions about every component. Will benefit from noise analysis (interview-rich content).
- **Track C:** Patient through unfamiliar tooling. Will probably enjoy the moment audio first streams into RAM (Day 2 deliverable).
- **Track D:** Cold. EM gap from undergrad course is real (professor didn't reach Maxwell's equations). Sub-thread queued.

### Claude self-corrections logged

These are mistakes Claude made that the student caught or that became evident in retrospect. Both for self-awareness and as evidence that the student does verify recommendations:

- Session 5: Initially miscalculated impedance divider as 53dB drop instead of 6dB (forgot parallel 1MΩ bias). Caught when reconciling theory with student's actual measurement of "no signal at all."
- Session 5: Recommended OPA1652 without verifying 4.5V min supply > student's 3.3V rail. Caught when student checked the datasheet as part of assigned habit.
- Session 7: Initial grading of Sampling Q1 was too harsh — student had used unit-circle reasoning correctly but compressed it. Recovered.

**General lesson:** verify own recommendations against datasheets before stating them as fact. Student is reliable about catching this — habit-builder for both.

---

## Persistence ritual

At the end of each session, Claude provides:
1. A `SESSION_LOG.md` entry to append.
2. Any updates to this `COURSE_OVERVIEW.md` (rare — once every 5-10 sessions).

Student commits both to course materials repo + AudioApp code with session-tied commit message.

**Recovery protocol** if chat is lost: paste both `.md` files into a fresh Claude conversation along with relevant code from AudioApp. Tell new Claude "I'm continuing a structured DSP/ECE course; read these two files first."

**Claude Projects setup (recommended, not yet done as of Session 12):** create a Claude Project with both `.md` files and key AudioApp source attached as project knowledge. Custom instructions point Claude at the files. Eliminates re-paste step each conversation. When done, replace files in the project at end of each session.

**Course materials repo:** recommended separate from AudioApp repo (keeps the portfolio repo clean for reviewers). Status as of Session 12: not yet created — `.md` files live locally only.
### Phase 4.5: RTOS for audio
*(Pedal milestone: audio runs in one task, UI/config in another,
without breaking real-time guarantees.)*
- RTOS concepts: tasks, priorities, preemption, scheduler.
- Synchronization primitives: mutex, semaphore, queue, event flag.
  Priority inversion (Pathfinder).
- Audio-specific patterns: highest-priority audio, lock-free
  ring buffers for ISR ↔ task communication, no malloc/printf
  on audio path.
- FreeRTOS on STM32 via CubeMX integration. Two-task demo.
- Diagnostics: stack-overflow detection, runtime stats,
  SystemView/Tracealyzer.
- Hard-real-time limits — when to use an interrupt instead.

Strong ADI/Shure interview relevance.
## Portfolio goals for AudioApp

Defined Session 17. Currently AudioApp is a learning vehicle with
strong architectural bones but not yet structured as a portfolio
deliverable. Goals in priority order:

1. **README that does the work.** Elevator pitch, screenshot,
   30s screencap GIF, architecture bullets, build instructions.
   Status: not started.
2. **`measurements/` directory.** Filter response plots,
   NEON-vs-scalar benchmarks, latency measurements. Use the
   Python pipeline from Session 16. Status: not started.
3. **One polished effect or feature.** Recommended:
   real-time spectrum analyzer (reuses FFT + ImPlot already
   in repo, visible product-like screenshot). Status: not started.
4. **Targeted code-quality items from TODO.** Biquad.hh
   `#pragma once`; precompute topo order outside audio
   callback; FTZ/DAZ for denormals. DF1→TDF2 and lock-free
   coefficient updates deferred to their respective threads.

Time estimate: ~3 focused weekend sessions to inflection point
where reviewer-time-on-page goes from 30 seconds to 5 minutes.

What NOT to do: pursue "completeness." Pick a few things and
polish them; let the rest be visible-but-rough.

## Session 18 — 2026-05-19 — Meta: Summer Plans, Career Direction Conversation

**Track:** Meta

**What happened:** Several student-driven discussions to close the
session, no track work.

**Major update — summer plans:** Student starts a Software Engineering
internship at Veterans United (Columbia MO) this week (May 2026).
Writing C#. Not directly ECE-relevant but real software team
experience. Student has explicitly chosen to keep the course at its
normal 2-3 sessions/week pace through the summer — internship is not
to be treated as a reason to slow down. Honest-feedback contract
still applies: revisit only if real signs of overload appear.
Implications that stand regardless:
- Track C (pedal) needs hardware access — to confirm whether student
  has his STM32 setup available during the internship. If not, bias
  toward non-hardware tracks (A, D, AudioApp portfolio).
- AudioApp portfolio sprint is elevated priority: ECE summer 2027
  internship applications open Sept-Oct 2026, so AudioApp polish
  should land before that window.
- VU experience strengthens 2027 applications (software-team
  competence, language adaptability, real-job record). Put on
  LinkedIn/resume now; ask for a recommendation at internship end.

**Discussions covered:**

(1) **RTOS in Track C, revisited.** Student asked whether his pedal
will actually ever need an RTOS. Honest answer: no — the pedal is one
real-time activity (audio DMA) plus low-rate UI polling, bare-metal
serves it well. Bumping the RTOS thread forward is still right for
interview value and for the next project after the pedal, but the
pedal itself doesn't justify it.

(2) **Cool STM32 RTOS-justified project idea.** Sketched a polyphonic
MIDI synth with USB-MIDI, OLED + encoder UI, 16 flash presets,
8-voice polyphony, stereo I²S DAC out. 5 concurrent activities =
genuine RTOS justification. ~3-4 months weekend work given existing
DSP code ports over. Strong portfolio piece. NOT logged as a queued
project yet — student wants to think on it.

(3) **Granular effects on STM32.** Microcosm-style granular pedal
feasibility. DSP ~5/10 difficulty; the constraint is memory. F446RE's
128 KB SRAM gives ~1.3 s stereo float32 — workable mono, tight
stereo. Options: mono, int16 stereo, external PSRAM, or STM32H7.
~4-6 weekends to credible mono granular pedal. Recommended: finish
pedal v1 simple, treat granular as v2 with possible HW upgrade.

(4) **Boutique vs mid-size vs big audio companies.** Walrus / Hologram
/ Strymon-tier don't really do internships and pay below industry.
UA / Waves mid-size with real programs. Sonos / Dolby / Bose / Apple
audio / Meta Reality Labs / Cirrus Logic = real audio engineering at
competitive comp. Recommended adding Sonos, Dolby, Meyer Sound to the
application list. Keep ADI/NVIDIA primary. L-Acoustics / d&b: real
engineering but European-HQ, awkward fit for a US undergrad summer
cycle, comp below ADI/NVIDIA.

(5) **GPA worry.** Student asked if dipping below 3.9 was concerning.
Calibrated: no. Real soft floors ~3.5 FAANG/big-chip, ~3.3 mid-size
audio, ~3.0 small audio. 3.9 is well above any threshold that
matters; 3.7-3.95 movement is noise. Projects + referrals dominate
above the floor.

**Where to pick up next:** Student to confirm hardware access for the
summer; that determines whether Track C continues uninterrupted or
pauses. Tracks A, D, and AudioApp portfolio work proceed regardless.
AudioApp portfolio sprint is elevated priority given the Sept-Oct
application timeline.

**No deliverable beyond this entry and a corresponding
COURSE_OVERVIEW update.**

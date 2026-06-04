
All projects
ECE Course
This is a structured, indefinite-length DSP/ECE self-study course. The student is a 2nd-year ECE undergraduate targeting summer internships at ADI, NVIDIA, Shure, DiGiCo, and adjacent companies. Before responding to any session-related message: 1. Read COURSE_OVERVIEW.md for course structure, tracks, calibration notes, and roadmap state. 2. Read SESSION_LOG.md to understand where each track currently stands. Maintain the persistence ritual: at the end of each session, provide a SESSION_LOG.md entry to append and any needed updates to COURSE_OVERVIEW.md. The student commits both files to a course- materials Git repo. Calibration notes in COURSE_OVERVIEW describe how this specific student learns; honor them.
Show more



How can I help you today?


Setting up SSH for GitHub updates
Last message 7 minutes ago
Track B next steps
Last message 2 hours ago
Switching from transistor principles to art of electronics
Last message 6 days ago
Essential BJT and MOSFET circuits for interviews
Last message May 27
Common emitter amplifier gain derivation
Last message May 25
Track B next steps
Last message May 24
Track C next steps
Last message May 24
Block convolution applications in audio processing
Last message May 24
Digital parametric EQ high frequency cramping
Last message May 24
💬 how does up/downsampling betwe…
Last message May 23
Diode bridge current and signal interaction
Last message May 23
Track A workout
Last message May 21
Track B schedule inquiry
Last message May 20
Creating and optimizing an online course
Last message May 20
Multiple ADC callbacks with single function
Last message May 17
PCB design skills for ADI resume
Last message May 17
Multiplexing potentiometers on STM32 ADC
Last message May 16
Track A workout
Last message May 13
ECE self-study course for DSP internship preparation
Last message May 11
Memory
Only you
Purpose & context Owen is a second-year ECE undergraduate running a structured, self-directed multi-track course covering DSP theory, analog electronics, embedded firmware, and stochastic signals. The practical endpoint is a fully functional guitar effects pedal built on an STM32 Nucleo-F446RE. The course doubles as interview preparation, with Analog Devices (ADI) as the primary target employer and NVIDIA as a secondary target (for a CUDA thread planned around months 4–5). A time-critical AudioApp portfolio sprint is flagged as elevated priority ahead of a Sept–Oct application window. Course tracks: Track A – DSP theory/algorithms (FIR derivation, oversampling, IIR/BLT, polyphase resampling, convolution) Track B – Analog/hardware (pedal input stage, noise analysis, anti-alias filter) Track C – STM32 firmware (ADC/DMA/DAC pipeline, real-time audio passthrough, biquad insertion) Track D – Probability and stochastic signals (foundations, power/dB fluency, autocorrelation, PSD/Wiener–Khinchin) Track E (planned) – PCB layout (KiCad, deferred to months 4–5) CUDA mini-thread (planned) – ~months 4–5; shortlisted projects: GPU batched mel-spectrogram pipeline or GPU batched polyphase resampler Session history is logged in SESSIONLOG.md; course structure is maintained in COURSEOVERVIEW.md. Both live in a Git repository Owen manages locally. --- Current state Track B (analog/hardware): Session 19 covered noise analysis foundations (Johnson/shot/flicker, PSD, quadrature addition, angle-bracket notation) Outstanding: confirm 100nF decoupling cap (pin 8) is physically installed; characterize input stage frequency response (read exact −3 dB point from REW plot, export PNG) Next: noise figure calculation and total anti-alias response characterization Track C (STM32 firmware): Real-time audio passthrough confirmed working on hardware (Sessions 20–21) Four bugs resolved: hadc pointer comparison, missing ADC guard on half-callback, single-buffer DMA tearing (fixed to double-buffer), DMA init order before TIM2 start ST-Link debug failure resolved (Mac USB stack reset fixed it) Pending: insert biquad into processblock; add GPIO toggle for CPU headroom measurement; uint16↔signed float↔uint16 data-format conversion with clamping; MCP6022 input stage swap Track A (DSP theory): L=4 oversampling interpolation filter designed and verified in Python (79-tap Hamming windowed-sinc, 20 kHz passband, 28 kHz stopband, fc=24 kHz, 192 kHz rate) L-gain correction (scale hcoeffs by L=4) flagged but not conclusively closed AudioApp C++ project has fft.cpp and a 73-tap NEON-vectorized FIR filter BLT cramping behavior confirmed; measurement via Python freqz overlay recommended Track D (probability/stochastic signals): Sessions completed: Day 1 (probability foundations), Day 2 (power/dB fluency), Day 3 (autocorrelation and white noise) Autocorrelation of first-order recursive smoother derived: $RY[k] = RY[0]\,\alpha^{|k|}$; cross-term cancellation technique explicitly demonstrated Paper task (5-part, 2-tap moving average on white noise + Wiener–Khinchin preview) issued and pending response Next: Day 4 (Wiener–Khinchin / PSD link), Day 5 (noise sources, partially overlapping Session 19 Track B content), Day 6 (pedal noise budget closing back to Track B) --- On the horizon Track B: noise figure calculation; full anti-alias response characterization Track C: biquad insertion into processblock; analog output stage design (deferred until DAC1 is running) Track D: Wiener–Khinchin and PSD link (Day 4); noise budget closure (Day 6) Track A: Day 4 part 2 (Dirichlet kernel, window menagerie, real filter implementation) — banked PCB layout (KiCad): deferred to months 4–5, after firmware pipeline is working and Track D EM sessions (EM-5, EM-6) are complete CUDA thread: deferred; shortlisted projects logged (mel-spectrogram pipeline, polyphase resampler) AudioApp portfolio sprint: time-critical, elevated priority for Sept–Oct application window --- Key learnings & principles Design before hardware is testable is wasteful: analog output stage deferred until DAC1 is running; hardware-dependent design decisions held until measurements are in hand Noise analysis requires no hardware: pure theory sessions (Track B noise, Track D) can advance independently of hardware state Track D / Track B noise content overlaps: Sessions 19 (Track B noise) and Track D Days 5–6 share territory; coordinate to avoid redundancy BLT cramping: intrinsic to the bilinear transform's nonlinear frequency mapping; single-point prewarping corrects only center frequency, not full shape; mitigations are oversampling (2×–4×), Orfanidis-style design, or acceptance when well below Nyquist Oversampling cost: normalized transition band narrows with L, driving filter tap count up proportionally — polyphase doesn't make L computationally free Fixed-point vs. float on F446: Cortex-M4F has 1-cycle float MACs; NEON-style 16-bit SIMD is 2-wide only via DSP extension — measure before optimizing, float32 is already accelerated DMA init order: arm DMA listeners before starting trigger source (TIM2) to guarantee phase-locked startup — general embedded principle CUDA project framing: a GPU batched partitioned-convolution engine is legitimate if framed around the algorithm, not just cuFFT glue; Parks-McClellan dropped as a candidate (fast on CPU, sequential inner loop) Interview-explicitness: Owen's reasoning is sound but occasionally compressed — stating both voltage and power ratios, and explicitly showing why cross terms vanish, earns interview credit at ADI; this nudge recurs across sessions --- Approach & patterns Session format: 20/20/20 structure (concept block, worked example, paper task) for theory tracks; staged build sequences with stop-and-verify gates for hardware tracks Calibration contract: Claude provides honest redirection when Owen is thinking ahead into future threads at the expense of time-critical work; Owen accepts this correction explicitly Derivation-first: full algebraic detail shown at each step; Owen stops and asks when uncertain rather than guessing through Self-monitoring: Owen flags when sessions run long, when questions land in the wrong thread, and when his own knowledge gaps need targeted remediation (e.g., power fluency was explicitly identified and addressed as a Track D Day 2 pivot) Record correction protocol: session logs are corrected when Owen clarifies what actually happened vs. what was recorded (e.g., the REW sweep clarification in Session 18) Tool search pattern: when reconstructing a track's current state, run both project knowledge search and conversation search in parallel — session numbering and topic reassignments are distributed across both stores File workflow: SESSIONLOG.md and COURSEOVERVIEW.md are read-only in the project directory; edits are made by copying to a writable workspace, applying targeted strreplace operations, verifying with diff, then staging for download; new session entries go above the sentinel comment line at the end of SESSION_LOG.md --- Tools & resources Hardware: STM32 Nucleo-F446RE, MCP6022 dual op-amp, breadboard, DMM, oscilloscope, Mac audio interface Power supply: OneSpot (center-negative); battery connection being rewired to match standard center-negative convention with reverse polarity protection Software/firmware: STM32CubeIDE, STM32 HAL, Python (NumPy, matplotlib, scipy.signal), REW (Room EQ Wizard) as tone generator and measurement tool Audio software: AudioApp (C++ project with fft.cpp, NEON-vectorized FIR) Version control: Git repository for course materials and firmware; .gitattributes with * text=auto eol=lf recommended for CRLF normalization Planned tools: KiCad (PCB layout, months 4–5), CUDA/cuFFT (CUDA mini-thread, months 4–5) Reference: Audio EQ Cookbook (biquad formulas); JLCPCB/OSHPark (fabrication options for future PCB) Employer targets: ADI (primary, internship), NVIDIA (secondary, SWE intern funnel)

Last updated 13 hours ago

Instructions
Add instructions to tailor Claude’s responses

Files
3% of project capacity used
Search mode

SESSION_LOG.md
1,266 lines

md



main.c
237 lines

c



COURSE_OVERVIEW (3).md
566 lines

md



wav.hh
60 lines

hh



wav.d
2 lines

text



synth.hh
22 lines

hh



wav.cpp
111 lines

cpp



TODO.txt
9 lines

txt



OutputNode.hh
43 lines

hh



osc.hh
65 lines

hh



synth.cpp
30 lines

cpp



osc.d
4 lines

text



processor.hh
21 lines

hh



osc.cpp
81 lines

cpp



Node.hh
75 lines

hh



Node.d
2 lines

text



fft.cpp
59 lines

cpp



Node.cpp
26 lines

cpp



main.d
11 lines

text



Makefile
54 lines

text



main.cpp
248 lines

cpp



imgui.ini
21 lines

ini



fft.d
2 lines

text



FIRFilter.cpp
59 lines

cpp



FIRFilter.d
2 lines

text



FIRFilter.hh
35 lines

hh



fft.hh
11 lines

hh



Biquad.hh
128 lines

hh



envelope.d
2 lines

text



envelope.hh
99 lines

hh



envelope.cpp
33 lines

cpp



AudioUtils.hh
21 lines

hh



Biquad.cpp
176 lines

cpp



AudioTypes.hh
18 lines

hh



AudioEngine.cpp
98 lines

cpp



AudioEngine.hh
70 lines

hh



AudioEngine.d
4 lines

text



AudioApp.hh
16 lines

hh



Biquad.d
2 lines

text



AudioUtils.d
2 lines

text



AudioUtils.cpp
30 lines

cpp


SESSION_LOG.md


raw
# DSP Course — Session Log
 
Chronological log of sessions. Append a new entry after each session. Read alongside `COURSE_OVERVIEW.md` for full context.
 
**Format:** Newest sessions go at the bottom. Each entry is self-contained enough that reading it gives a Claude instance the context to continue.
 
---
 
## Session 1 — 2026-05-04 — Course Setup
 
**Track:** Meta (no track yet)
 
**What happened:**
- Initial conversation about goals and background.
- Initial 12-week curriculum proposed, then restructured into open-ended four-track system. Capability tier targets (Tier 1/2/3) established.
- Discussed application timeline (~6 months → summer internships, not new-grad). Honest assessment given: curriculum is not the bottleneck for getting interviews; resume + GitHub + referrals are.
- Discussed dynamic adjustment: Claude reads compression/expansion/lateral signals to adjust pace.
**Code review of AudioApp (initial):** Architecture good, NEON Biquad correct, several issues identified for future sessions (DF1→TDF2, real-time-safety, denormals, etc.).
 
**Where to pick up next:** Track A Day 1 — FIR foundations.
 
---
 
## Session 2 — 2026-05-04 — Track A Day 1: FIR Foundations
 
**Track:** A — FIR thread, Day 1
 
**Concepts covered:** FIR definition, impulse response, four key properties (stability, linear phase, length cost, parallelism), DTFT.
 
**Theory questions completed:**
- **Q1 — Linear phase from symmetry.** First attempt confused DFT/DTFT with "equidistant from π" reasoning (category error). Pushed back with sum-to-product derivation. Second attempt correct: pair `h[k]` with `h[N-1-k]`, factor out `e^(-jω(N-1)/2)`, use even/odd properties of cos/sin. Group delay = (N-1)/2.
- **Q2 — Hamming `k` constant.** k≈4 (main-lobe convention) or 3.3 (empirical). Walked through tradeoff: smaller k → sharper transition + more ripple. Discussed C/k naming differences across textbooks.
- **Q3 — FIR vs IIR parallelism.** Correctly identified IIR's `y[n-1]` dependency. Underestimated tap-axis SIMD; explained `vaddvq_f32` horizontal sum is cheap. Two SIMD axes (tap-axis vs output-axis).
**Code submitted:** FIRFilter class stub. Type narrowing fix and `<cstring>` include.
 
**Curriculum updates:** IIR mastery thread queued after FIR. Sampling sub-thread queued (5 warmups for Sessions 3-7).
 
**Where to pick up next:** Day 2 — Sampling Q1, then scalar FIR convolution.
 
---
 
## Session 3 — 2026-05-05 — Track A Day 2: Scalar FIR Implementation
 
**Track:** A — FIR thread, Day 2
 
**Sampling Q1 (warmup):** 1kHz/7kHz/19kHz at fs=16kHz. Student answered 1k→1k, 7k→7k, 19k→3k via unit-circle reasoning. Full credit after clarification. Bonus: discussed colloquial vs strict meaning of "aliased" — multiple input frequencies map to same output (13kHz and 19kHz both → 3kHz).
 
**Code submitted:** `FIRFilter::Process()` with circular buffer.
 
**Code review:**
- Per-channel reference caching ✓
- Write-then-convolve ordering ✓
- `+ h_.size()` before mod for size_t underflow safety ✓
- Minor: int vs size_t mixed types, single-precision accumulator (awareness only)
**Discussion of "think about" prompts:**
- **Q1 (eliminate modulo):** Branch-and-subtract `if(idx<0) idx += N`, power-of-2 bitmask `idx & (N-1)`. Claude added double-buffer trick (allocate 2N, write at both `idx` and `idx+N`).
- **Q2 (array direction):** SIMD `vld1q_f32` requires forward; same-direction enables `dot_product` (BLAS sdot, CMSIS arm_dot_prod_f32). Fixes: store coefficients reversed, or walk delay line forward.
**Note:** This conversation was originally lost from a previous chat session. Student re-uploaded FIRFilter.cpp from local file. Reconstruction successful.
 
**Pending:** Impulse response test with `h = {0.1, 0.2, 0.3, 0.4, 0.5}` (expected output = `h` then zeros).
 
**Where to pick up next:** Day 3 — Sampling Q2, then optimize scalar (eliminate modulo, fix sign types) or skip to NEON.
 
---
 
## Session 4 — 2026-05-07 — Recovery Documents
 
**Track:** Meta
 
**What happened:**
- Student noted per-session log alone insufficient for full recovery.
- Created `COURSE_OVERVIEW.md` (durable course structure) and `SESSION_LOG.md` (chronological).
- Established persistence ritual.
**Where to pick up next:** Same as Session 3.
 
---
 
## Session 5 — 2026-05-07 — Track B Day 1: Pedal Input Stage
 
**Track:** B — pedal-driven, Day 1 (substantial multi-hour session)
 
**Topic:** Complete redesign and debug of student's STM32 guitar pedal input stage.
 
**Concepts covered (in depth):**
- **Input impedance:** source/load voltage divider analysis. Why series 1MΩ before a 2.2kΩ Sallen-Key is wrong (~6dB drop, *not* the 53dB I initially miscalculated). Canonical guitar topology: 1MΩ shunt to ground at tip, then DC block, then bias to V_mid through second 1MΩ.
- **Sallen-Key topology:** Q = 1/(3-K). K→3 = instability. Student had K=2.69 (gain resistors swapped) → Q=3.23, near-oscillatory. Frequency-dependent input impedance (high-Z at low f, ~R1 at high f).
- **Buffer stages:** unity-gain follower as the way to break source-Z/load-Z dependency.
- **Op-amp non-idealities at low supply:** 5532 needs ≥10V differential (V+ ≥ +5V, V− ≤ -5V). Common-mode range. Datasheet verification habit.
- **Single-supply biasing:** "V_mid replaces ground" principle. Student discovered organically that gain-resistor R_g going to 0V instead of V_mid was THE smoking gun (op-amp saturated at rail). General rule articulated and locked in.
- **V_mid as AC ground:** voltage divider impedance, decoupling cap requirements.
- **Capacitor parasitic model:** ESL, self-resonant frequency. Why 100nF + 10µF in parallel covers different frequency ranges. Antiresonance mentioned.
- **Debugging methodology:** measure DC bias first, then AC, then sweep. Don't commit to one root cause prematurely.
**Schematic delivered:** Student drew corrected schematic with all fixes:
- 1MΩ shunt-to-ground at tip
- C_in DC block, 1MΩ bias to V_mid
- Unity-gain buffer
- Sallen-Key with R_f=1.3k, R_g=2.2k (G=1.59, Q=0.707), R_g referenced to V_mid
- V_mid voltage divider with 10k+10k and 100nF+10µF decoupling
- Anti-alias cutoff fc ≈ 10.6kHz
**Outstanding items:**
- Add 100nF supply decoupling at op-amp V+ pin (close to chip).
- Op-amp ordered: **MCP6022** (Path B chosen; OPA1652 ruled out — 4.5V min supply > 3.3V rail).
- Datasheet verification of MCP6022 supply min, CMR, output swing, GBW pending.
**Test plan when MCP6022 arrives (5-step):**
1. DC bias check at all op-amp pins.
2. Buffer-only AC test: 100mV @ 1kHz in, see ~100mV at buffer out.
3. Sallen-Key AC test: see ~159mV at filter out.
4. Frequency sweep 100Hz–30kHz; produce magnitude plot; expect −3dB at 10.6kHz, ~−14dB at 23.5kHz.
5. Plug in guitar, verify ADC sees clean audio in 0–3.3V range.
**Calibration insights:**
- Student catches recommendations against datasheets (caught 5532 spec; will do same for MCP6022).
- Self-discovered "everything references V_mid" rule mid-discussion.
- Asked excellent skeptical questions: "why two caps?", "Sallen-Key vs non-inverting amp Z difference?".
- Claude's mistakes: 53dB miscalculation (forgot 1MΩ bias in parallel), recommended OPA1652 without verifying 3.3V supply compatibility.
**Track B roadmap items implicitly covered:**
- Op-amp non-idealities (item 2 — partial)
- Active filters / Sallen-Key (item 3 — partial)
- Audio input stages (item 4 — partial)
- Mixed-signal layout principles (item 10 — partial)
**Where to pick up next:**
- Wait for MCP6022 to arrive.
- Datasheet verification (assigned).
- Build, perform 5-step test plan, capture frequency sweep plot.
- Track B Day 2 will be either (a) gain stage / pre-distortion, (b) noise analysis (instructive sweep), or (c) project day reviewing measured response.
---
 
## Session 6 — 2026-05-07 — Course Updates: FPGA, Languages, Direction
 
**Track:** Meta
 
**What happened:**
- Discussed FPGA track timing. Decided to **queue Track E with trigger conditions** (Track A IIR + fixed-point complete, OR Track C Phase 3 in progress, OR project pull). Realistic open: month 3-4.
- Pre-opening FPGA prep flagged in Track A IIR thread, fixed-point thread, and DSP architecture thread.
- Discussed adding **Python, MATLAB, Git, Linux**. Decided:
  - Python: 1-session intro (Session 7), then continuous integration. No dedicated thread.
  - MATLAB: queued, depth TBD.
  - Git/Linux/GitHub: integrated into persistence ritual.
- **CUDA mini-thread queued ~month 4-5** for NVIDIA targeting.
- **Company direction confirmed:** ADI + NVIDIA primary, Shure/DiGiCo/Bose secondary. Heavily weights Track B (analog) and FPGA prep.
- **Track C restructured around the pedal** (5 phases, each with concrete pedal milestones).
- 3 outstanding student questions to lock in Track C Phase 1: STM32 chip, output method, current embedded state.
**COURSE_OVERVIEW.md updated** with all changes above. Significant rewrite.
 
**Pre-Session-7 homework for student:**
1. Install Python + numpy/scipy/matplotlib in a venv.
2. Get AudioApp on GitHub (public repo).
3. Decide on course materials repo (separate vs. AudioApp subdirectory — recommended: separate).
**Where to pick up next:** Session 7 — Python intro session (~1 hour). DSP-flavored, ending with first portfolio-quality plot. Or, alternative: open Track C Phase 1 once student answers the 3 outstanding questions about their STM32 setup.
 
---
 
<!-- Append new session entries below this line -->
 
## Session 7 — 2026-05-08 — Track C Day 1: Chip Orientation (substantial multi-hour session)
 
**Track:** C — pedal-driven, Day 1
**Hardware confirmed:** STM32F446RE on Nucleo-F446RE, CubeIDE 1.x on macOS (zsh terminal). Internal DAC1 (PA4) for output. ADC1_IN1 on PA1 for input.
 
**Concepts covered:**
- Reference materials (RM0390 reference manual, F446 datasheet, ARM Cortex-M4 user guide).
- Clock tree in full: HSI vs HSE vs LSI vs LSE, what each is for. PLL math (`SYSCLK = HSE × (PLLN/PLLM) / PLLP`), VCO constraints. SYSCLK/HCLK/APB1/APB2 hierarchy, max rates (180/180/45/90 MHz). Timer-clock-doubling rule for non-/1 prescalers.
- Peripheral inventory; "ADC123_IN1" naming convention (one pin shared across three ADC peripherals).
- CubeMX USER CODE zone system. Code outside `BEGIN`/`END` pairs gets deleted on regenerate.
**Major debugging episodes (5 separate issues, ~hours):**
 
1. **Wrong launch type.** Project had "C/C++ Application" launch (desktop debug), not "STM32 Cortex-M C/C++ Application." Symptom: empty console, no LEDs, silent failure. Fix: delete the bad config, create new one under STM32 Cortex-M C/C++ Application.
2. **GDB path missing.** macOS doesn't have `gdb`; bundled `arm-none-eabi-gdb` not on PATH. Symptom: `Error with command: gdb --version` / `Cannot run program "gdb": Unknown reason`. Fix: provide absolute path to the bundled GDB in the launch config's "GDB debugger" field:
   ```
   /Applications/STM32CubeIDE.app/Contents/Eclipse/plugins/com.st.stm32cube.ide.mcu.externaltools.gnu-tools-for-stm32.14.3.rel1.macosaarch64_1.0.0.202602081740/tools/bin/arm-none-eabi-gdb
   ```
 
3. **GDB command file misconfigured.** Path to executable got pasted into the *command file* field (init script). GDB tried to run itself as a script. Symptom: `Error in sourced command file: Undefined command: ""`. Fix: clear the command file field or restore to default `.gdbinit`.
4. **Clock misconfiguration.** CubeMX defaulted to HSE=25 MHz but Nucleo-F446RE has 8 MHz HSE. Initial PLL values (M=15, N=216, P=2) produced 57.6 MHz instead of 180 MHz. `SystemCoreClock` variable lied (cached the configured value, not measured). Found via stopwatch test: 10 toggles in 15s instead of 5s. Fix: set HSE = 8 MHz in CubeMX RCC settings; PLL re-solves to M=4, N=180, P=2.
5. **User code preservation.** Code accidentally placed between `/* USER CODE END WHILE */` and `/* USER CODE BEGIN 3 */` — the unprotected zone. Discussed CubeMX USER CODE zone system; recommended moving blink to `BEGIN 3` block.
**Deliverable verified:** LD2 blinks at 1 Hz on PA5, stopwatch-confirmed 5s for 10 toggles. Clock tree at SYSCLK=180MHz, HCLK=180MHz, APB1=45MHz, APB2=90MHz from 8MHz HSE.
 
**Calibration insights:**
- Lesson absorbed: trust measurement over configuration. SystemCoreClock and HAL_RCC_GetHCLKFreq() both agreed at 180MHz, but reality was 57.6MHz.
- Always verify input clock frequency from dev board user manual.
- CubeMX is a starting point, not ground truth — always inspect generated SystemClock_Config() in main.c.
- Student asked excellent meta-question about USER CODE zones — sign of building proper embedded discipline.
**Where to pick up:** Track C Day 2 — ADC + DMA + TIM6 trigger at 48 kHz. Recommended DAC→ADC loopback test approach (no need to wait on MCP6022 hardware).
 
---
 
## Session 8 — 2026-05-09 — Clock Tree Deep Explanation (post-Day 1)
 
**Track:** C — explanatory follow-up
 
**What happened:** Student asked, after closing Day 1, to actually understand what HSI/HSE/PLL are and why 180 MHz matters for HAL_Delay to be accurate. Day 1 had been "set it to these values" without conceptual depth.
 
**Concepts covered (lecture-style, no exercises):**
- What a clock physically is (square wave driving state machine transitions).
- Four oscillators on F446: HSI (internal RC, 16 MHz, imprecise but instant), HSE (external, 8 MHz on Nucleo, precise), LSI (32 kHz, watchdog), LSE (32.768 kHz, RTC). When to use which.
- Why audio needs HSE: HSI's 1-2% drift causes pitch artifacts at digital audio boundaries.
- PLL as feedback frequency multiplier: VCO + divider + phase comparator. The F446's M/N/P structure and constraints (VCO input 1-2 MHz, VCO output 100-432 MHz).
- Distribution: SYSCLK → AHB prescaler → HCLK; HCLK → APB1 prescaler → PCLK1 (max 45); HCLK → APB2 prescaler → PCLK2 (max 90). Timer-clock-doubling rule.
- The full chain explaining the bug from Session 7: HAL_Delay → SysTick → reload = HCLK/1000. If SystemCoreClock lies, every time-based HAL function lies in proportion.
**No deliverable.** Pure explanation. Student processed and moved on.
 
---
 
## Session 9 — 2026-05-10 — Track A Day 3: FIR Optimization
 
**Track:** A — FIR thread, Day 3
 
**Sampling Q2 (warmup):** Why Nyquist is strict inequality. Student correctly identified phase-dependent vanishing at fs/2: a sine at fs/2 with φ=π/2 produces all-zero samples. Bonus framing added: deeper insight is that rotation-per-sample = π means only two points on the unit circle, insufficient information to recover both amplitude and phase. Practical fmax at fs=48kHz: ~20kHz due to finite-width anti-alias filter transition band.
 
**Main work:** Optimize the scalar FIR.
 
Chose **double-buffer** technique (Option C) for modulo elimination. Rationale: sets up SIMD perfectly (forward-direction reads, contiguous), 2x memory trivial for audio, used in every production-grade FIR (FFTW, JUCE, CMSIS-DSP).
 
**Code submitted:** New `FIRFilter.cpp`. Student independently arrived at essentially the same algorithm as the spec, without peeking. Used `std::reverse` on coefficient copy (cleaner than spec's explicit loop). Helpful ASCII diagram in comments documenting buffer layout.
 
**Code review:**
- ✓ Algorithm correct. Impulse response `{1,2,3,4,5}` produces `{1,2,3,4,5,0,0,0,0,0}` — definitional test passes.
- ✓ Double-buffer pattern: write at both `currIdx` and `currIdx+N`, read forward from `currIdx+1`, advance index with branch (no modulo anywhere in hot path).
- ✓ Coefficient reversal in constructor enables forward-direction dot product.
- ⚠️ Bug: `*inPtr` then `*inPtr++` on consecutive lines — works by accident via evaluation order, intent obscure. Fix: read once into named variable, then write to both positions explicitly.
- ⚠️ Sign-compare: `const int N = h_.size()` narrowed to int; `int i, j, k` compared to size_t. Should be `const std::size_t N = ...` and size_t loop counters throughout.
**Calibration insight:** Student independently deriving the same algorithm is a strong "concepts integrated" signal. The post-increment-in-write style is a discipline issue to flag.
 
**Pending:** Apply the two cleanup fixes, commit to AudioApp.
 
**Where to pick up:** Day 4 — windowed-sinc derivation by hand, design and measure first real FIR filter.
 
---
 
## Session 10 — 2026-05-10 — Git + GitHub Setup
 
**Track:** Meta / Tooling
 
**What happened:** Student confessed essentially zero Git experience. Walked through setup from scratch.
 
**Steps completed:**
1. Global Git identity set: Owen Roush, owenroush06@gmail.com.
2. Global config: `init.defaultBranch=main`, `push.autoSetupRemote=true`, `credential.helper=osxkeychain`.
3. `git init` in AudioApp folder.
4. Created `.gitignore` covering: object files, common executable names, build dirs, macOS junk (.DS_Store), editor/IDE files, ImGui state, optional WAV exclusion, misc.
5. Discussed ImPlot dependency (34 MB, no nested .git) — chose vendoring (Option A) for portfolio-friendliness.
6. First commit with full message; amended for typo ("dependancy" → "dependency"). Discussed when amending is safe (before push, yes; after push, complicated).
7. Created GitHub repo `xavoidx/AudioApp`, public, no initialize boxes checked.
8. Connected remote, pushed via PAT auth, macOS Keychain cached credential.
**Verification:** Repo live at https://github.com/xavoidx/AudioApp. Files visible.
 
**Calibration insight:** Student values being walked through tooling carefully rather than dumped a tutorial link. Maintain this for future tooling intros (Python venv, SSH keys eventually, etc.).
 
---
 
## Session 11 — 2026-05-10 — Git Crash Course
 
**Track:** Meta / Tooling
 
**What happened:** Student asked for crash course on commit/merge/pull/push and Git in general.
 
**Content delivered (lecture-style):**
- Mental model: three trees (working directory → staging → repo) and two locations (local → remote).
- Commands grouped by purpose: inspection, recording, syncing, branching, undoing.
- Recommended workflow on solo personal project: small focused commits, conventional commit message style (`feat:`, `fix:`, `perf:`, `refactor:`, `docs:`, `test:`), `git add -p` for review discipline.
- Merge conflicts walkthrough.
- "I broke something" cheat sheet.
- Things to skip for now: rebase, stash, cherry-pick, submodules, tags.
**No deliverable.** Reference material to use going forward.
 
---
 
## Session 12 — 2026-05-11 — EM Sub-thread Decision + Course Updates + State Snapshot
 
**Track:** Meta / Curriculum design
 
**What happened:**
 
1. **EM coverage question.** Student finished an undergrad EM course but professor never reached Maxwell's equations (stopped at inductance). Asked whether to read a full EM textbook before applications. Discussed honestly:
   - For target roles (ADI/NVIDIA/Shure/DiGiCo internships), comprehensive EM not needed.
   - Targeted essentials are: lumped-vs-distributed boundary, Maxwell's equations conceptually, wave equation derivation, skin effect, transmission line basics, signal integrity for digital, EMI/EMC basics.
   - Time-budget math: full EM textbook ≈ 80-120 hours; targeted ≈ 10-15 hours.
   - Decision: add as Track D sub-thread, 6 sessions over ~3 months.
2. **Track D EM sub-thread added to COURSE_OVERVIEW.md.** Six sessions (EM-1 through EM-6), each anchored to an interview question and cross-track tie-in:
   - EM-1: Lumped vs distributed boundary
   - EM-2: Maxwell's equations
   - EM-3: Skin effect and HF conductor non-idealities
   - EM-4: Transmission lines
   - EM-5: High-speed digital signal integrity (NVIDIA-relevant)
   - EM-6: EMI/EMC basics
3. **Claude Projects discussion.** Student asked whether Projects would work better than the paste-files-each-time approach. Explained: yes, Projects auto-attach files and custom instructions to every conversation in the project, eliminating re-paste friction. Recommended setup: project with both .md files + key AudioApp source + custom instructions pointing Claude at the .md files first. Not done yet as of this session.
4. **Comprehensive overview update.** Added current-state snapshot near top of COURSE_OVERVIEW for fast recovery. Filled in gaps: student identity (GitHub handle, email, dev environment), AudioApp repo location, pedal hardware spec, macOS toolchain quirks (GDB path, launch config type, HSE quirk), Git workflow conventions, Claude Projects status, course materials repo status. Expanded calibration notes with observations from Sessions 5-11.
**No deliverable beyond updated COURSE_OVERVIEW and this SESSION_LOG entry.**
 
**Where to pick up next:** Student's choice between:
1. Track A Day 4 — windowed-sinc derivation by hand (satisfying payoff).
2. Track C Day 2 — ADC+DMA loopback test.
3. Open Track D — probability for stochastic signals (best Track B prep) or first EM session.
4. Continue Track B non-blocking work (noise analysis).
Also pending: Python intro session (homework: venv with numpy/scipy/matplotlib installed), Claude Projects setup, course materials repo creation.
 
---
 
<!-- Append new session entries below this line -->
 
## Session 13 — 2026-05-12 — Track A Day 4 (part 1): Whittaker–Shannon + Ideal LPF Derivation
 
**Track:** A — FIR thread, Day 4 (split across two sessions)
 
**Sampling Q3 (warmup):** Exact reconstruction formula. Student's first
instinct was correct in shape (inverse-continuous-FT of the DTFT) but
not yet derived. Walked through full derivation: DTFT → bandlimited
identity $X_c(f) = T \cdot X_d(f)$ for $|f| < f_s/2$ → swap sum and
integral → evaluate $\int_{-f_s/2}^{f_s/2} e^{j2\pi f \tau}\,df = f_s
\cdot \text{sinc}(f_s \tau)$ → Whittaker–Shannon.
 
Why never used in practice: student initially said "computationally
heavy," pushed for deeper reason. Got it on second pass — formula
requires samples extending arbitrarily into the future of $t_0$,
so structurally non-causal. Combined with sinc's $1/t$ decay,
truncation incurs meaningful error even far from the support
center. Bridged this to "what people actually do" (ZOH, polynomial
interpolation, windowed-sinc) — windowed-sinc being exactly what
we're about to build.
 
**Sub-derivation (student-initiated):** student asked whether he
should be able to derive Whittaker–Shannon from scratch. Answered
yes (interview-relevant, primes the discrete-time derivation, locks
in time-frequency duality). Provided structured 7-step skeleton +
the one non-obvious fact (Poisson summation / bandlimited identity).
Student executed it cleanly. Also gave him the symmetric-interval-
exponential-to-sinc identity since he asked — $\int_{-a}^{a} e^{j2\pi
f\tau}\,df = 2a \cdot \text{sinc}(2a\tau)$, derived in 4 lines.
 
**Main work — ideal lowpass $h[n]$ derivation:**
 
Student set up the inverse DTFT correctly:
$h[n] = \frac{1}{2\pi}\int_{-\omega_c}^{\omega_c} e^{j\omega n}\,d\omega$
and integrated to get $\frac{\sin(\omega_c n)}{\pi n}$.
 
First sinc-form attempt: $h[n] = \frac{\omega_c}{2\pi}\text{sinc}(\omega_c n)$
— off by a factor of 2 in coefficient and missed that the $\pi$
needs to come out of the sin argument to match normalized sinc
convention. Showed the correct massage: pick $B = \omega_c/\pi$
to match sin arguments; result is
 
$h[n] = \frac{\omega_c}{\pi} \text{sinc}\!\left(\frac{\omega_c}{\pi} n\right)$
$\quad=\quad \frac{2f_c}{f_s} \text{sinc}\!\left(\frac{2f_c}{f_s} n\right)$
 
Sanity-checked $n=0$ both via $\text{sinc}(0)=1$ and direct integral
evaluation. Both agree at $\frac{2f_c}{f_s}$.
 
**Three problems with truncated $h[n]$ — student identified all three
unprompted:**
1. Non-causal (fix: shift right by $M$; cost is constant group delay).
2. Slow $1/n$ decay → truncation lossy.
3. Ripple in frequency response.
Framed (3) properly: truncation = multiplication by rectangular
window in time = convolution with that window's spectrum in
frequency. The rectangular window's spectrum is the Dirichlet
kernel with -13 dB first side lobe; that side lobe stays at -13 dB
no matter how long the filter — this is Gibbs. Reframed window
design as "pick a window with lower-side-lobe spectrum, pay for
it in main-lobe width."
 
**Stopping point:** 1.5 hours in, past session target. Banked the
rest of Day 4 for next session.
 
**Where to pick up next:** Day 4 part 2 —
- Rectangular window spectrum (Dirichlet kernel) — student to pick
  derive-from-scratch vs intuition-only.
- Window menagerie: Hann, Hamming, Blackman. Close loop on
  Sampling Q2 / Day 1 Q2 (the $k \approx 4$ Hamming constant
  finally gets its proper explanation from main-lobe width).
- Design a real filter to spec; compute coefficients; drop into
  `FIRFilter`; measure impulse + magnitude response.
**Calibration note:** student is fluent with the inverse-DTFT
machinery and complex exponential integration. Caught the
factor-of-2 / normalized-sinc-convention slip cleanly once shown
where to look. The student-initiated Whittaker–Shannon detour
was the right call but doubled session length — pattern to
watch: when student asks "should I be able to derive X from
scratch," expect roughly a doubling of session length, and flag
the bank-rest-for-next-session option explicitly up front.
## Session 14 — 2026-05-13 — Track D Day 1: Probability for Stochastic Signals (open)
 
**Track:** D — Probability sub-thread, Day 1 (opens Track D)
 
**Concepts covered:**
- Engineer's framing of random variable and random process.
- Mean is linear, variance is quadratic. Independent noise sources sum
  in *power* (variance), not amplitude. Verified with worked example:
  σ₁=5μV, σ₂=12μV → σ_tot = √(25+144) = 13μV, not 17μV. Rule of thumb:
  source 3× smaller than another contributes <5% — usually ignore.
- Gaussian distribution; two key facts: linear combinations of Gaussians
  are Gaussian; LTI systems preserve Gaussianity of random processes.
- CLT as the *reason* electronic noise is Gaussian (sum of ~10^20
  independent contributions in Johnson noise, etc.).
- Random process = sequence of RVs indexed by time. Stationarity and
  ergodicity assumed for noise (and validated for the noise sources
  we'll meet).
- Sigma rules: 68/95/99.73% for ±1σ/±2σ/±3σ. Crest factor of Gaussian
  ≈ 6 (vs √2 for sine, 1 for square). Use 6σ for peak-headroom design.
**Task results (4 questions):**
 
(a) RMS of unit-variance Gaussian. Student: σ. Correct. Sharpened the
robustness: RMS is a time-average operation on a waveform; σ is an
ensemble-average property of an RV; ergodicity is what makes them equal
for long records of stationary noise. Datasheets quote RMS because
that's what an instrument measures; meaning is σ.
 
(b) Fraction outside ±3σ. Student: 0.27%. Correct.
 
(c) Gaussian-through-LTI preservation. Student said yes to all three
sub-questions but worried about "periodicity" of filtered output. Fixed:
filtered noise isn't periodic — it acquires *correlation* between
adjacent samples (the filter smears each input across many outputs),
which makes it look "smoother" but not periodic. Distinction between
correlation structure (changes) and Gaussianity (preserved) and
periodicity (never appears from random input).
 
(d) Variance of lowpass-filtered noise. Student: lower variance,
because high-freq info is removed. Correct intuition. Made it
quantitative using $S_Y(f) = |H(f)|^2 S_X(f)$: ideal LPF with $f_c=1$kHz
at $f_s=48$kHz scales variance by $f_c/(f_s/2) = 1/24$. ~96% of noise
power killed. Connected to pedal: Sallen-Key at 10.6 kHz cuts bandwidth
to needed band, doing both anti-aliasing and noise reduction.
 
**Where to pick up next:** Day 2 — autocorrelation R_X[k]. What "white"
means precisely (autocorrelation = delta). Sets up Wiener–Khinchin
(Day 3) and filter-output PSD (Day 4) and Johnson/shot/1/f noise
sources (Day 5) and pedal noise budget (Day 6, closes back to Track B).
 
**Calibration insight:** Student arrived at correct intuitions on all 4
sub-questions. The one confusion (periodicity vs correlation) is a
common "filter smooths things" intuition that needs sharpening to the
PSD framing — not a deficit, just a vocabulary upgrade. Math machinery
fluent enough that we'll be able to move at a brisk pace through this
thread.
 
## Session 15 — 2026-05-17 — Track A Day 4 (part 2): Windowed-sinc design
 
**Track:** A — FIR thread, Day 4 (closes Day 4)
 
**Concepts covered:**
 
- Rectangular window DTFT derivation from scratch. Student
  applied finite geometric series, then the "factor out half-angle
  exponent to expose sine" trick on numerator and denominator.
  Got Dirichlet kernel cleanly. Banked the trick.
- Magnitude of $e^{j\omega}$ multiplier is always 1 for real $\omega$
  (it's a pure phase shift / unit-circle rotation). Caveat for
  complex exponents and z-transform with $|z| \ne 1$.
- Found -13 dB first side lobe rigorously. Main lobe peak = N
  (small-angle limit), first sidelobe peak at $\omega \approx 3\pi/N$
  evaluates to $2N/(3\pi)$, ratio is $2/(3\pi) \approx 0.212 = -13.4$ dB.
  N cancels — the punchline. Refined: exact value is -13.26 dB;
  student's approximation is fine.
- Window menagerie: Hann, Hamming, Blackman. Tradeoff table
  (peak side lobe, rolloff, main lobe width, transition width
  constant k). Hamming coefficients (0.54/0.46) chosen specifically
  to cancel the first rectangular side lobe.
- Closed the loop on the $k \approx 3.3$ constant from Day 1 Q2 — it's
  the Hamming main-lobe width measured between first significant
  zeros divided by $2\pi$. The $k=4$ alternative is a more
  conservative convention for the same shape.
- **Duality discussion (student-initiated):** rectangle-in-time-→-Dirichlet
  and rectangle-in-frequency-→-sinc are the same operation viewed
  from opposite sides of the DTFT pair. Both produce sinc-shaped
  envelopes; Dirichlet is periodic because its conjugate domain
  is discrete, sinc is aperiodic because its conjugate domain is
  continuous. Same rectangle-to-ripple-kernel principle.
**Filter designed by hand:**
 
Spec: $f_s = 44.1$ kHz, passband 0–4 kHz, stopband 6 kHz+ at
$\geq 40$ dB. Linear phase.
 
- Window: Hamming (gives -53 dB stopband; comfortable margin
  over 40 dB spec).
- Length: $N = 3.3 \cdot 44100 / 2000 = 72.77$, rounded to 73 for
  Type I linear phase (integer group delay).
- Group delay: 36 samples = 0.82 ms.
- Cutoff: center of transition band, $f_c = 5$ kHz; normalized
  $2f_c/f_s = 0.2268$.
- Ideal sinc: $h_{\text{ideal}}[m] = 0.2268 \cdot \text{sinc}(0.2268m)$, $m \in [-36, 36]$.
- Window: $w[m] = 0.54 + 0.46\cos(2\pi m / 72)$.
- Final: $h[m] = h_{\text{ideal}}[m] \cdot w[m]$, then shift to causal indexing.
**Spot-check tap calculations:**
 
Student computed by hand:
- $h_{\text{ideal}}[2] = 0.1575$ ✓
- $h_{\text{ideal}}[3] = 0.0896$ ✓
- $h[\pm 36] = 0.000349$ (student got 0.00434 — forgot to multiply
  by $w[36] = 0.08$)
- $h[18] = 0.00244$ (student got 0.0045 — forgot to multiply by
  $w[18] = 0.54$)
Consistent slip: computed $h_{\text{ideal}}$ correctly and missed the
final pointwise multiply by $w$ on tail taps. Not present in
center taps because $w \approx 1$ there. Caught and flagged.
 
Also: Claude's earlier seat-of-pants estimates for $h_{\text{ideal}}[2]$ and
$h_{\text{ideal}}[3]$ (0.169 and 0.115) were both wrong; student's
calculated values were right. Logged as Claude self-correction —
sinc decays faster at small $m$ than my mental model assumed.
 
**Day 4 status:** Closed. Theory complete; filter designed on paper.
Measurement deferred to Python session.
 
**Where to pick up next:** Dedicated Python intro session. Goal:
generate the 73-tap vector programmatically, run a sweep through
`FIRFilter` via existing WAV infrastructure, plot $|H(f)|$ in Python
and verify spec (-3 dB at passband edge, $\leq$ -40 dB at 6 kHz).
First portfolio plot.
 
Pre-session homework: Python venv with numpy/scipy/matplotlib.
 
After Python session: Day 5 (NEON vectorization of FIRFilter) and
Day 6 (IIR vs FIR thread close), then open IIR mastery thread.
 
**Calibration insight:** Student's hand-arithmetic of sinc and
Hamming values was *more* accurate than Claude's mental
estimates. Pattern: when delegating partial computations as
sanity checks, Claude should not anchor with imprecise estimates
("should be around 0.169") because the student will correctly
override them but it muddies the signal of whether *they* got it
right. Better pattern: "compute X, report what you get" without
target value, then verify.
 
## Session 16 — 2026-05-18 — Python for DSP (Track tooling, broad fluency session)
 
**Track:** Tooling — first Python session. Closes the Python intro that's been pending since Session 6.
 
**Environment setup (rocky):**
- Initial `python -c "import numpy, scipy, matplotlib"` failed (Claude typo'd
  `scipi`; also numpy wasn't installed despite the venv).
- `pip list` showed Jupyter installed but no numpy/scipy/matplotlib. Fixed
  with `pip install numpy scipy matplotlib`.
- Then Jupyter launched the *conda base* Python instead of the venv's,
  surfacing a broken conda numpy. Fixed by installing ipykernel into the
  venv, then `python -m ipykernel install --user --name=dsp --display-name
  "Python (dsp)"`, then selecting the dsp kernel inside the notebook.
- Verified with `import sys; print(sys.executable)` returning the venv path.
- Lesson: `(dsp)` prefix in the shell prompt doesn't guarantee the kernel
  Jupyter spawns; must register the venv as a kernel explicitly.
**Concepts covered:**
 
*Block 1 — Python language essentials from a C++ background:*
- Names point to objects; no declared types; indentation is syntax.
- Core containers: list (mutable, heterogeneous), tuple (immutable), dict.
- `for` loops iterate over iterables, not indices. `range`, `enumerate`.
- Slicing: `a[start:stop:step]`, negative indices, `a[::-1]` reverse.
- List comprehensions: `[expr for var in iter if cond]`.
- f-strings for formatting.
- Tuple unpacking for multi-return.
- Import aliasing (`import numpy as np`).
*Block 2 — NumPy/SciPy/Matplotlib for DSP:*
- NumPy arrays as contiguous typed memory under a Python interface.
- Element-wise default for `+ - * /`. `@` for matrix mul / dot product.
  This is the major mental flip from C++ / linear algebra notation.
- `np.linspace(a, b, N)` for N points (endpoint=True), `np.arange(a, b, step)`
  for stepped. `endpoint=False` for tileable DSP signals.
- Slicing + boolean masks (`x[x > 10]`) for vectorized region selection.
- `np.fft.fft`, `np.fft.fftfreq` for spectra.
- `scipy.signal.freqz(h, worN, fs)` for filter magnitude/phase response —
  cleaner than FFT-of-impulse-response.
- `scipy.signal.windows.hamming`, `.hann`, `.blackman`.
- Matplotlib: `plt.plot` connects samples with lines (smooth-looking);
  `plt.stem` shows discrete-time honestly. Multi-subplot via
  `fig, (ax1, ax2) = plt.subplots(2, 1)`. Object-oriented `ax.set_xlabel`
  vs procedural `plt.xlabel`.
- DSP plotting idiom: `20 * np.log10(np.abs(H) + 1e-12)` to avoid log(0).
- Numerical noise floor of FFT'd pure sine is ~-310 dB (machine epsilon
  for float64). Real-signal noise floors are 60-120 dB depending on source.
*Block 3 — Designed and measured yesterday's 73-tap filter:*
 
Built the filter in three vectorized lines (after a false start where
student wrote a 73-iteration list comprehension that accidentally multiplied
by the entire window vector each iteration, producing a 73×73 matrix —
fixed by indexing `hamming[x]`, then by switching to fully vectorized form):
 
```python
n = np.arange(N) - (N - 1) // 2
h_ideal = (2 * fc / fs) * np.sinc((2 * fc / fs) * n)
h_coeff = h_ideal * signal.windows.hamming(N)
```
 
One bug caught: student wrote `np.sin` instead of `np.sinc` on first
pass. Plot looked plausible due to small-argument coincidence; corrected
to `np.sinc`.
 
Measured with `signal.freqz(h_coeff, worN=8192, fs=fs)`. Spec verification:
 
| Metric | Spec | Measured | Status |
|---|---|---|---|
| -3 dB point | ~5 kHz (center of transition) | 5 kHz | ✓ |
| Attenuation at 6 kHz | ≥ 40 dB | 40 dB exactly | ✓ |
| Peak stopband ripple | < -40 dB | -51.07 dB | ✓ (Hamming theory: -53 dB) |
 
Plotted phase response separately (unwrapped) and discussed that the
stopband phase becomes meaningless at zero-crossings of magnitude.
Linear-phase signature visible as straight ramp in passband.
 
**Calibration insights:**
- Student fluent enough by end of session to write his own subplot code.
  Pace can be brisk on Python from here.
- Python-as-language transition smooth; main pitfalls were idiom-level:
  the C++ instinct to write `for x in range(N): array[x] = ...` instead of
  vectorizing, and the `np.sin` vs `np.sinc` slip. Both classic. Worth
  flagging future sessions: "if you're writing a for loop over array
  indices in NumPy, you're probably doing it wrong."
- The 73x73 matrix mistake is a useful one — student saw the broken
  plot, recognized something was off, and debugged with help. Good
  signal that the diagnostic loop is working.
**Where to pick up next:** Student's choice:
1. Track A Day 5 — NEON vectorization of FIRFilter (with the working
   measurement pipeline now in place, can verify NEON output bit-exactly
   against scalar).
2. Track A Day 6 — IIR vs FIR thread close. Then open IIR mastery thread.
3. Track C Day 2 — ADC + DMA + DAC loopback test.
4. Track D continuation — Day 2 of probability thread (autocorrelation,
   white noise definition).
5. Project day — port the 73-tap filter to AudioApp, run on real audio
   through the existing PortAudio pipeline, write impulse-response test
   that compares C++ output to Python-designed taps.
Suggested: option 5 ties the new Python tooling back into AudioApp
immediately and produces another portfolio artifact. Or option 1 keeps
DSP momentum and is the natural Day 5.
 
Pending tooling: Claude Projects setup, course materials repo creation.
 
## Session 17 — 2026-05-19 — Track A Day 5: NEON Vectorization of FIRFilter
 
**Track:** A — FIR thread, Day 5
 
**Pre-session work:** Reorganized AudioApp into include/ and src/.
Used `git mv` for rename-preserving history. Updated Makefile with
-Iinclude flag and src/*.cpp wildcard. Fixed main.cpp's ImPlot include
to short-form to match existing imgui includes. Built clean, committed.
 
**Concepts covered:**
- SIMD dot-product pattern: parallel-lane accumulation, horizontal sum
  at end. Four taps per `vmlaq_f32` instruction (vs 2-channel parallel
  in Biquad's vmla_f32).
- Distinction between 64-bit (`float32x2_t`, vmla_f32) and 128-bit
  (`float32x4_t`, vmlaq_f32) NEON registers. The `q` suffix.
- `vld1q_f32` for 4-float loads (unaligned-OK on AArch64).
- `vaddvq_f32` for horizontal sum across 4 lanes.
- Tail handling: scalar cleanup loop for N % 4 remainder taps.
- The `K_vec = (N/4)*4` idiom vs `N-3`: both work for N ≥ 4, but the
  former avoids unsigned underflow as a defensive habit.
**Implementation:** Student wrote NEON path in FIRFilter::Process with
`#ifdef __ARM_NEON` guard matching Biquad's pattern. Cached delay-line
and reversed-coefficient pointers outside the per-frame loop. Used
`vmlaq_f32`, `vaddvq_f32`, scalar tail cleanup. Working impulse
response on first compile.
 
**Verification:** Three-way correctness check via Python pipeline from
Session 16:
- NEON output (C++, float32)
- Scalar output (C++, float32)
- Python `signal.lfilter` ground truth (float64)
Sample comparison (NEON vs Python truth, first 8 samples after impulse):
C++:    0.00042  1e-05  -0.00104  -0.00196  -0.00064  0.00095  0.00184  0.00233
Python: 0.0004   0      -0.001    -0.002    -0.0006   0.0009   0.0018   0.0023
 
Differences in 4th-5th decimal place: float32 quantization + SIMD
parallel-lane summation order + library implementation differences.
Well below the standard 1e-4 threshold for float32 FIR correctness.
NEON FIR verified correct.
 
**Loose ends (deferred):**
- Type-tightening: int → size_t in loop counters throughout
  FIRFilter::Process. Same sign-compare flag as Session 9.
- NEON vs scalar benchmark (harness exists in main.cpp from Biquad
  benchmarking, just needs swap). Expected 2.5-3.5x speedup on inner
  loop. Will run when next in the file.
**Discussion items:**
 
(1) Student asked about RTOS coverage in Track C. Current roadmap
has FreeRTOS as item 22 "beyond the pedal" — too late for the goals.
Will bump to a properly-positioned thread between Phase 4 (effects)
and Phase 5 (productionize) since the natural moment is "audio
works, want a UI without breaking the audio thread." Sketched 6-session
arc: concepts → sync primitives → audio-specific patterns → FreeRTOS
on STM32 → diagnostics → when not to use RTOS. Strong ADI/Shure
interview relevance.
 
(2) Student asked about AudioApp as a deliverable. Honest assessment:
currently a learning vehicle, not a polished portfolio piece. Identified
4 concrete goals: (a) README with screenshots/GIF and architecture
section, (b) measurements/ directory with filter plots and NEON
benchmarks, (c) at least one polished effect — recommended real-time
spectrum analyzer since it reuses FFT + ImPlot already in repo,
(d) targeted code-quality items from existing TODO. ~3 weekend
sessions to "portfolio-ready" inflection point.
 
**Where to pick up next:** Student's choice between:
1. Track A Day 6 — IIR vs FIR thread close, opens IIR mastery thread.
2. NEON vs scalar benchmark for FIR (15 min, banks portfolio data).
3. Track C Day 2 — ADC + DMA + DAC loopback.
4. Open Track D Day 2 — autocorrelation of stochastic signals.
5. Portfolio sprint — README and measurements/ folder for AudioApp.
**Calibration insight:** Student's NEON implementation worked first
compile and passed Python ground truth on the first run. Indicates
the conceptual scaffolding (parallel-lane MAC, horizontal sum, tail
loop) integrated cleanly. Pace can stay brisk on SIMD work.
 
The student-driven discussion items (RTOS scope, portfolio framing)
are characteristic of his pattern of using sessions partly as
curriculum design conversations. Treat as feature not bug; he is a
partner in shaping the course.
 
## Session 18 — 2026-05-20 — Track B Day 2: MCP6022 Pedal Input Stage Build + Measurement
 
**Track:** B — pedal-driven, Day 2. Closes the build that's been blocked
since Session 5 waiting on the MCP6022 op-amp.
 
**What happened:** MCP6022 arrived. Built, bias-verified, and
frequency-swept the complete guitar pedal input stage designed in
Session 5.
 
**Datasheet verification (homework, confirmed done):**
- Supply 2.5–5.5V — 3.3V rail fine.
- Rail-to-rail in and out — the reason it works single-supply where
  5532 / OPA1652 don't.
- GBW 10 MHz → closed-loop BW ≈ 6.3 MHz at G=1.59. No concern.
- Slew rate 7 V/µs → ~30× margin for 1.65V peak at 20 kHz.
- Input noise ≈ 8.7 nV/√Hz (middling — revisit in noise analysis).
**Build (staged, power/bias-first methodology):**
1. Power rails + V_mid divider only, no chip — verified 3.3V / 0V / 1.65V.
2. Op-amp inserted, inputs tied to V_mid, 100nF decoupling pin 8→4
   close to chip (the item missing from the Session 5 schematic).
3. Buffer (U1A) feedback closed + input network (1MΩ shunt, C_in,
   1MΩ bias to V_mid).
4. Sallen-Key (U1B): R₁=R₂=2.2k, C₁=C₂=6.8nF, R_f=1.3k, R_g=2.2k,
   C₂ and R_g both referenced to V_mid.
**DC bias check (test plan step 1): PASS.**
All midpoints at 1.65V, power rail 3.3V. No oscillation. Both
op-amp halves alive, feedback loops closed correctly.
 
**Gain discussion:** Confirmed G=1.59 (+4 dB) is not a loudness
choice — for the equal-R / equal-C Sallen-Key, Q = 1/(3−K), so
K = 3−√2 ≈ 1.586 is what sets Butterworth Q=0.707. Gain and Q are
locked together in this topology.
 
**Second-stage question:** Student asked whether to add a second
Sallen-Key. Answer: no for anti-aliasing (guitar pickup self-rolls
off above ~5 kHz; 48 kHz fs gives wide Nyquist gap; 4th-order is
luxury not necessity). If signal range at the ADC turns out too
small, add a *dedicated non-inverting gain stage* after the filter
— not a second filter — to decouple gain from Q. Deferred: measure
with a real guitar first before deciding.
 
**Frequency sweep (test plan step 4):** Done in REW (Python
measurement script deferred to a dedicated intro session).
Measured **−11.3 dB at 24 kHz (Nyquist for 48 kHz fs)** vs ~−14 dB
ideal. ~2.7 dB shallower — consistent with 5% component tolerance
pushing fc up ~10% (toward ~11.7 kHz) and/or slight Q error. Within
expected tolerance band; not pathological.
 
**Test plan status:** Steps 1–4 effectively complete. Step 5 (plug in
real guitar, verify ADC sees clean audio in 0–3.3V range) pending —
naturally pairs with Track C Day 2 once audio can get into the STM32.
 
**Calibration insight:** Student executed the staged build cleanly
and got correct DC bias first try — the power/bias-first methodology
from Session 5 was internalized. Reported measurement honestly
("things look good i think") rather than overclaiming; good
engineering humility. Did the datasheet homework as assigned.
 
**Where to pick up next:** Student's choice between:
1. Track B Day 3 — noise analysis (Johnson/shot/1/f, NF calculation;
   interview-rich, ADI-relevant; pairs with Track D probability).
2. Track C Day 2 — ADC + DMA + DAC loopback test (gets audio into
   RAM; also enables test-plan step 5 with the real input stage).
3. Track A Day 6 — IIR vs FIR thread close, then open IIR mastery.
4. Track D Day 2 — autocorrelation / white noise definition.
Also still pending: REW plot readout (above), Claude Projects setup,
course materials repo creation.
 
## Session 19 — 2026-05-20 — Track B Day 3: Noise Analysis Foundations
 
**Track:** B — pedal-driven, Day 3. First noise-analysis session.
Non-hardware (student had no scope access this session).
 
**Record correction (carryover from Session 18):** Student clarified
he never produced a REW magnitude plot of the input stage. What
actually happened: used REW as a tone generator, measured output
amplitude on a scope at discrete frequencies, hand-computed dB from
voltage ratios. So the −11.3 dB @ 24 kHz figure is one point in a
sparse manual point-by-point response, **not** a swept REW curve.
Session 18's "frequency sweep done in REW" overstates it; "export
REW plot as PNG" is not actionable — no plot file exists.
- Revised Track B loose ends: (1) −3 dB point and passband flatness
  still unpinned — getting them now means taking *more scope points
  near fc* (~8–14 kHz), current data likely too sparse around the
  corner to see a bump/sag; (2) "export REW plot" → replace with
  "plot the point-by-point data in Python" (Session 16 pipeline,
  doable now from existing data) or do a proper REW loopback
  measurement later with an audio interface.
- Output-stage question (student asked if the analog output stage is
  a good next Track B step): deferred. It's cross-track plumbing —
  design + measure together in Track C Phase 2 when DAC1 is actually
  clocking samples. Designing it now = another untestable circuit.
**Concepts covered:**
- Noise as a quantity: PSD (V²/Hz, flat = white), voltage noise
  density (V/√Hz — a density, not a voltage; MCP6022's 8.7 nV/√Hz is
  this), total RMS noise = $e_n\sqrt{B}$. The $\sqrt B$ (not $B$)
  follows from Session 14: power = variance adds linearly across
  frequency slices, voltage is its square root.
- Three physical sources:
  - **Johnson** — thermal agitation in any resistance. White.
    $e_n=\sqrt{4kTR}$. Depends only on R and T; scales as $\sqrt R$.
    Reference point: 1 kΩ @ room temp ≈ 4 nV/√Hz.
  - **Shot** — discrete carriers crossing a *potential barrier* as
    independent events. White. $i_n=\sqrt{2qI}$. Needs a junction —
    does NOT occur in a plain resistor (no barrier; diffusive
    conduction).
  - **Flicker (1/f)** — carrier trapping at defects. NOT white, PSD
    rises as 1/f. Dominates below the 1/f corner frequency.
- Quadrature addition derived three ways: (1) it's the Session 14
  variance-adds rule with a square root — RMS = σ for zero-mean
  noise; (2) cross-term derivation — $\langle v^2\rangle = \langle
  v_1^2\rangle+\langle v_2^2\rangle+2\langle v_1v_2\rangle$, the
  cross-term is the covariance, zero for uncorrelated sources;
  correlated sources retain it and add between linear and quadratic;
  (3) geometric — uncorrelated = orthogonal vectors, Pythagoras; "in
  quadrature" = 90° apart; law of cosines gives the correlated case.
**Notation taught (student-initiated):** angle brackets
$\langle\cdot\rangle$ = averaging operator, interchangeable with
$E[\cdot]$ and overbar. Ensemble vs time average; equal for
stationary+ergodic processes (Session 14 ergodicity). Flagged that
$R_x[k]=\langle x[n]x[n+k]\rangle$ autocorrelation uses the same
notation — direct bridge to Track D Day 2. Warned against
C++-template visual false-friend; it's an operator that collapses a
process to a scalar.
 
**Task results (4 questions, all correct):**
- (a) 1k vs 4k Johnson density ratio = 2 (√ of resistance ratio).
  Correct. Sharpened: 2× in voltage, full 4× in power — name the
  quantity.
- (b) 20k→80k bandwidth: RMS voltage ×2, power ×4. Correct, both
  quantities stated.
- (c) "Resistor carries current → shot noise" — correctly identified
  as wrong; "no junction." Sharpened to potential-barrier /
  discrete-crossing vs diffusive-collisional conduction.
- (d) 8 µV ⊕ 3 µV = √73 ≈ 8.54 µV; removing the 3 µV source → 8.0
  µV, ~6% drop. Correct design lesson drawn (chase dominant
  contributors; 3 µV is already in Session-14 ignore-territory at
  2.5× smaller).
**Calibration insight:** All four task answers correct with sound
reasoning; student's only request was the *why* behind quadrature
addition, then the angle-bracket notation — both genuine gap-fills,
not error recovery. Reasoning is clean but still slightly compressed
(e.g. (a) didn't spontaneously state the power ratio) — the
interview-explicitness nudge from the COURSE_OVERVIEW calibration
notes still applies, prompted it twice this session. Pace can stay
brisk through the rest of the noise thread.
 
**Where to pick up next — Track B Day 4:** noise figure / noise
factor (SNR-degradation metric), referring noise to the input,
combining op-amp input-referred noise with source-resistance Johnson
noise. Then the full pedal input-stage noise budget (closes back to
Sessions 5/18). Natural worked-example opener already teed up: refer
the MCP6022's noise to its input and combine with the source-R
Johnson noise.
 
**Pending (unchanged):** Track B −3 dB point readout (needs scope);
plot the manual point-by-point data in Python for the
`measurements/` folder; Claude Projects setup; course materials repo
creation.
## Session 13 — 2026-05-11 — Track C Day 2: DAC→ADC Loopback (audio in RAM milestone)
 
**Track:** C — pedal-driven, Day 2
 
**Goal achieved:** sample-accurate 48 kHz audio I/O pipeline working end-to-end via DAC→jumper→ADC loopback. Samples visible in RAM. Half/complete buffer interrupts firing on schedule. This is the "audio in RAM" pedal milestone — when MCP6022 arrives, the analog input stage drops in to replace the jumper, no firmware changes needed.
 
**Architecture built (memorize this — it's the pedal's spine):**
**Concepts covered:**
 
- **Master trigger architecture:** one timer (TIM2) drives both DAC and ADC synchronously. Hardware trigger ≠ software loop — sub-ns jitter vs µs-scale jitter. Same architecture every pro audio system uses.
- **TIM2 vs TIM6 for ADC:** TIM6 TRGO is NOT routed to ADC trigger mux on F446 (silicon constraint, RM0390 §13.3.7). TIM6 can drive DAC but not ADC. Switched to TIM2 (32-bit, on APB1, drives both).
- **PSC/ARR design space:** for a 16-bit timer driven at 90 MHz needing 48 kHz output, dozens of valid (PSC, ARR) factorizations of 1875. Chose PSC=0, ARR=1874 to maximize ARR resolution for future runtime sample-rate changes. Interview-relevant framing: "I held PSC=0 to preserve ARR adjustment headroom."
- **APB1 timer kernel clock doubling:** APB1 = 45 MHz, but TIM2 sees 90 MHz because APB1 prescaler ≠ 1. (Session 8 rule applied.)
- **SAR ADC math:** 12-bit + 15-cycle sample = 30 cycles / 22.5 MHz = 1.33 µs per conversion. At 48 kHz triggers, ~6% of ADC capacity used. Headroom for longer sample times (high-Z sources) or scan-mode multi-channel later.
- **Source impedance & sample time:** `T_smp_min ≈ 10 × R_source × C_S/H` (S/H cap ~6 pF). The "always buffer before ADC" rule — same reason Sallen-Key has unity-gain follower upstream.
- **DMA fundamentals:** circular mode + half-transfer + transfer-complete interrupts = ping-pong. CPU has 2.67 ms budget per half-buffer at 48 kHz / 128 samples.
- **DMA controller mapping:** ADC1/2/3 hardwired to DMA2; DAC1/2 hardwired to DMA1. Can't choose. RM0390 Tables 27/28.
- **`volatile` for DMA buffers:** correct usage — tells compiler "this memory changes outside C visibility." Not a synchronization primitive, but sufficient for the CPU-reads-only case.
- **`__weak` callback override mechanism:** linker silently replaces weak HAL stub with user's non-weak symbol *if signatures match exactly*. Missing `void` return type = silent dead code, most-common HAL bug.
**Debugging episodes:**
 
1. **TIM6 not in ADC trigger dropdown.** Diagnosis: F446 silicon doesn't route TIM6 TRGO to ADC. Fix: switched both DAC and ADC to TIM2 TRGO. Lesson: always check external trigger table in reference manual.
2. **CubeMX regenerated clock config with wrong values after HSE switch.** Regressions: voltage scale 3 (max 120 MHz), missing overdrive, FLASH_LATENCY_2, and ADC prescaler stomped from /4 to /2 (overclocked ADC 25%). Lesson: **CubeMX doesn't preserve everything across regenerations** — always byte-inspect `SystemClock_Config()` and peripheral inits after touching the clock tree. Session 7 lesson resurfacing.
3. **`const sine_lut` + runtime fill contradiction.** Caught during code review. `const` puts array in `.rodata` → flash → silent failure or hardfault on write. Pivoted to mutable SRAM LUT, runtime fill (Path A).
4. **Order-of-operations bug:** original code started DAC DMA before filling LUT → would emit zeros at startup. Moved fill above Start calls.
5. **LED at 187 Hz seemed wrong but was correct.** Toggle rate = 375 Hz (1 HT + 1 TC per 5.33 ms buffer). Square wave frequency = toggle rate / 2 = 187.5 Hz. Claude got the math wrong twice in conversation before deriving it correctly. Rule: **`f_square = toggle_rate / 2`**.
**Verified on scope:**
- PA4: 187.5 Hz sine, ~0.2V to 3.1V (rail-buffered DAC, expected slight clipping at extremes).
- DAC staircase visible at 20 µs/div timebase — direct visual confirmation of 48 kHz sample clock.
- PA5 (LD2): clean 187.5 Hz square wave from HT+TC callbacks.
**Verified in Expressions view:**
- `adc_buf[0..255]`: clean sine pattern centered ~2048, range ~200–3800. Matches DAC output. Audio is in RAM.
**Files modified:** `main.c`, `adc.c`, `dac.c`, `dma.c`, `tim.c` (CubeMX-managed). Project regenerated with separate-files-per-peripheral mode this time.
 
**Calibration insights:**
- Student requested walkthrough rather than code dump — strong signal for "explain, let me write." Honor this for future Track C sessions.
- Student caught the LED frequency anomaly clearly and reported it. Good signal-to-noise in bug reports.
- Two CubeMX-regression bugs caught by Claude in code review — student verified both. Healthy review loop.
**Pending for next Track C session:**
- Replace constant LUT with a real-time passthrough: copy `adc_buf` halves into `sine_lut` halves inside the callbacks. First true real-time DSP loop.
- Then: insert a biquad in the path (bridges to Track A).
- MCP6022 arrival → swap jumper for analog input stage.
**Where to pick up:**
- Track C Day 3: real-time passthrough (sub-ms task, then biquad).
- Or: pivot back to Track A Day 4 (windowed-sinc) since FIR thread is still mid-thread.
- Or: open Track D as per persistence ritual (cold for 13 sessions running).
## Session 20 — 2026-05-23 — Track C Day 3: Real-Time Passthrough (dry in → dry out)
 
**Track:** C — pedal-driven, Day 3
 
**Goal achieved:** First genuine real-time DSP loop. Audio enters ADC,
ping-pongs through `process_block`, exits DAC, sample-clocked end to end.
The "dry signal in → dry signal out" pedal milestone. Confirmed on hardware
via DAC→jumper→ADC loopback.
 
**Bugs fixed in main.c (carried in from the Session 13 loopback version):**
1. `HAL_ADC_ConvCpltCallback`: `if(hadc = hadc1)` — assignment, not
   comparison; and comparing pointer to struct. Fixed to `hadc == &hadc1`
   / `&hadc2` (address-of the global handle).
2. `HAL_ADC_ConvHalfCpltCallback` had no ADC guard — added
   `if(hadc == &hadc1)` so other ADCs can't trigger audio processing.
3. `out_buffer` was single-buffered (`BUFFER_SIZE`) while `adc_buffer`
   was double (`BUFFER_SIZE*2`). Both callbacks wrote `&out_buffer[0]` —
   DAC DMA reads and callback writes overlapped in the same 128 cells →
   tearing. Fixed: `out_buffer` is now `BUFFER_SIZE*2`; half-callback
   writes `out_buffer[0]`, complete-callback writes `out_buffer[BUFFER_SIZE]`.
   Producer always writes the half the consumer isn't reading.
4. Init order: DMAs were armed *after* TIM2 was already running → startup
   race, DAC and ADC could land one sample out of phase. Fixed: arm all
   DMAs first, start TIM2 last. First TRGO edge hits both DMAs
   simultaneously → phase-locked from sample zero.
**Concepts covered:**
- Why single output buffer tears: the callback has nonzero duration, the
  DMA never stops, so reader and writer cross. Double buffer converts
  "callback must be instantaneous" (impossible) into "callback must finish
  within 2.67 ms" (achievable, measurable). That 2.67 ms (128 samples /
  48 kHz) is the real-time deadline.
- General embedded principle: arm the listeners before starting the thing
  that talks. Anything armed after the source is running races the source.
**Debugging episode (non-code):** Lengthy debug-connection failure —
`localhost:61234` timeout, then `darwin_claim_interface` libusb error,
then empty `system_profiler` (macOS not enumerating ST-Link at all).
Worked through cable / CN2 jumpers / USB port. Actual fix: **Mac reboot** —
USB stack was wedged. Lesson banked: when `system_profiler` is empty but
the board is clearly powered, try a reboot early; macOS USB enumeration
can wedge after long uptime.
 
**Calibration insight:** Student derived the two core fixes himself —
correctly reasoned that arming DMAs before the timer eliminates the
startup race, and (with prompting) that the output buffer needs to be
double because `process_block` takes time. Strong "concepts integrate"
signal. Asked the right "wait, does this matter?" question about init
ordering unprompted.
 
**Pending for next Track C session:**
- Insert a biquad into `process_block` ("dry in → biquad-filtered out",
  Phase 3). Bridges to Track A.
- Data-format front/back end: ADC gives uint16 0–4095 biased ~2048;
  filter math wants signed, zero-centered float. `process_block` needs
  bias-subtract + scale on input, scale + bias-add + **clamp to 0–4095**
  on output (unclamped uint16 wrap = audible click).
- Add GPIO toggle around `process_block` to scope CPU headroom vs the
  2.67 ms budget before the biquad goes in — establish a baseline.
- MCP6022 input stage swap for the jumper (test-plan step 5).
**Where to pick up:** Track C Day 4 — biquad in the path, or the
oversampling firmware integration (see Session 21 entry).
 
---
 
## Session 21 — 2026-05-23 — Oversampling: L=4 Interpolation Filter Design + Verification
 
**Track:** A / Tooling — oversampling design session, Python-verified.
Same paper-design → Python-measure pipeline as Sessions 15→16.
 
**Goal achieved:** Designed an L=4 (48→192 kHz) interpolation filter from
spec, implemented in NumPy, and produced before/after spectrum plots
demonstrating imaging and its suppression. Coefficients ready to port to
the STM32 later.
 
**Concepts covered:**
- Why oversampling exists: DAC images sit at fs±audio. At 48 kHz the first
  image edge is at 28 kHz → brutal 0.15-decade analog transition band.
  Run the DAC at 192 kHz → first image at ~172 kHz → trivial analog
  filter. Trade analog filtering difficulty for digital computation.
- You can't just clock the DAC faster — that pitch-shifts. You must
  *manufacture* the extra samples: **upsample = zero-stuff + lowpass**.
- Zero-stuffing by L: DTFT relation X_L(e^jΩ) = X(e^jLΩ). Pure axis
  compression by L. Shape of each spectral copy unchanged, but because
  the original DTFT was 2π-periodic, compression makes L copies appear
  in the working band — the L−1 extras are imaging artifacts. In Hz:
  nothing moves; declaring a higher fs just widens the window onto copies
  periodicity always implied.
- The interpolation lowpass kills the L−1 image copies. It's a windowed-
  sinc FIR — same design as Session 15. Discrete sibling of Whittaker–
  Shannon (Sampling Q3): the zeros are the in-between positions, the
  lowpass convolution computes the bandlimited interpolated values.
- Sharp filter relocates analog→digital, doesn't vanish. But the digital
  version is exact (no tolerance/drift) and can be perfectly linear-phase;
  the analog filter that remains gets to be the trivial one. Costs: CPU
  budget and FIR group-delay latency.
- Polyphase eliminates multiply-by-zero waste but does NOT make L free:
  the transition band, normalized to the filter's operating rate L·fs,
  gets narrower as L grows → N grows ~proportionally to L → compute scales
  with L. Multistage interpolation softens this (Track A item 6).
- 16-bit SIMD not worth it here: F446 is Cortex-M4F — has a single-
  precision FPU (float MACs are 1-cycle), has NO NEON, only 2-wide
  DSP-extension int16 MACs (SMLAD). Fixed-point would race an already-
  accelerated float path. Default to float32; measure before quantizing.
**Filter spec (designed to):**
- Operating rate 192 kHz. Passband 0–20 kHz. Stopband edge 28 kHz
  (first image edge for 20-kHz-bandlimited content). Transition 8 kHz.
  Sinc cutoff fc = 24 kHz (transition center).
- Window: Hamming (~−53 dB stopband; ≥40 dB spec met with margin).
- Length: N = k·fs/Δf = 3.3·192000/8000 ≈ 79. Type I (odd) for integer
  group delay = 39 samples @ 192 kHz ≈ 0.20 ms.
- Note: pad to 80 for clean ÷4 polyphase split later.
**Decisions reasoned through (student-driven):**
- Image edge tracks *true signal bandwidth*, not a fixed 20 kHz. If
  content exists to 24 kHz, image edge is at 24 kHz → zero transition
  band → unrealizable. Therefore usable bandwidth must be *chosen*
  strictly below Nyquist; the guard band is the cost of a buildable
  filter. Chose usable BW = 20 kHz for guitar (amp/speaker kill >5–6 kHz
  anyway).
- Nonlinear effects generate content above Nyquist that aliases into
  baseband — separate problem from DAC imaging, fixed by oversampling
  *around the nonlinearity*. Filed as a future session.
**Python work:** Built 79-tap Hamming windowed-sinc, verified with freqz
(−6 dB at 24 kHz, ≥40 dB down by 28 kHz, ~−53 dB stopband). Generated
48 kHz test signal (1/12/19 kHz tones), zero-stuffed ×4, FFT'd before and
after filtering. Before plot: baseband + 2 visible image copies. After
plot: images suppressed 40–80 dB, baseband intact.
 
**Bugs hit (all NumPy-idiom, same family as Session 16):**
- `test_signal` built with list comprehensions + `+=` → list concatenation
  (length 2N), not element-wise sum. Fixed by vectorizing:
  `np.sin(...)` arrays, `+=` element-wise.
- Zero-stuff array made with `np.array([0]*N)` → int64 dtype → float
  samples truncated to −2…2 on assignment. Fixed: `np.zeros(N)` (float64).
- Initially put a 25 kHz tone in a 48 kHz signal — above Nyquist, aliases
  to 23 kHz. Replaced with in-band tones.
- `fftfreq` passed original fs instead of fs_up → all peaks 4× too low.
- "Horizontal line artifact" across spectrum plots: `fftfreq` returns a
  non-monotonic axis (positive half then negative half); `plt.plot`
  connects array elements in index order, so it draws one segment across
  the +Nyquist→−Nyquist seam. NOT a plt bug, NOT |H| being multi-valued —
  it's connect-the-dots on an unsorted x-array. Fixed with `rfft`/
  `rfftfreq` (monotonic, positive-only; correct for real signals).
- Missing `20*` and misplaced `1e-12` in dB conversion. Correct idiom:
  `20*np.log10(np.abs(X) + 1e-12)`.
**Loose end:** L-gain — zero-stuffing drops energy by 1/L; baseband peaks
appear ~12 dB low. Fix is scaling h_coeffs by L=4. Flagged, not yet
conclusively closed — one-line check next session.
 
**Calibration insight:** Student's skeptical questions were the strongest
part of the session — "is polyphase free?", "doesn't 25 kHz alias?", "|H|
should pass the vertical line test" — each caught a real issue or exposed
a real concept. Interview-grade behavior. Recurring NumPy-idiom slips
(list-vs-vectorize, int dtype, fft-vs-rfft) are reps, not conceptual gaps —
same note as Session 16. Worth a standing reminder: in NumPy, reach for
vectorized ops and `np.zeros`/`rfft` by default.
 
**Where to pick up next:** Student's choice:
1. Port the L=4 interpolation filter to the STM32 — TIM2 to 192 kHz, DAC
   DMA resize, FIR in `process_block`, fit the 2.67 ms budget. The Track C
   firmware integration of this session's design.
2. Track C Day 4 — biquad in the passthrough path.
3. Track B — output stage (reconstruction filter + DC block + line driver)
   so the pedal can drive a guitar amp. Deferred from this session.
4. Track A Day 6 — IIR vs FIR close, open IIR mastery.
**Pending (long-standing):** Claude Projects setup and course-materials
repo still not created — flagged since Session 12, now 8+ sessions
overdue. Highest-leverage tooling fix on the board. L-gain check (above).
 
## Session 22 — 2026-05-29 — Track C Day 4: Biquad in Real-Time Path
 
**Track:** C — pedal-driven, Day 4
 
**Goals achieved:** Data format conversion written and verified, biquad inserted into `process_block`, CPU budget measured. Phase 3 ("real-time processing") milestone complete on the firmware side.
 
**Code written:**
 
Full `process_block` with three stages: ADC→float conversion (bias-subtract + scale), Direct Form I biquad, float→DAC conversion (scale + bias-add + clamp). Key decisions:
- `int32_t` intermediate before cast to `uint16_t` — prevents wrap-around clicks on out-of-range samples.
- Biquad struct declared as global so state variables persist between buffer callbacks.
- GPIO PA0 toggle wrapping `process_block` for CPU timing.
**Bugs caught in review:**
- `memcpy` byte count was `BUFFER_SIZE` instead of `BUFFER_SIZE * sizeof(float)` — would have copied only 32 bytes (8 floats) out of 128.
- `biquad.a2 * biquad.a2` typo in difference equation — caught before flash.
- Operator precedence bug in output conversion — cast applied before `+ 2048.0f` addition; fixed by wrapping full float expression before cast.
**Biquad coefficients:** 1 kHz Butterworth lowpass, Q=0.707, fs=48 kHz. Computed via Python (EQ Cookbook lowpass formulas). b0=b2=0.00391607, b1=0.00783215, a1=−1.81531792, a2=0.83098222.
 
**CPU budget measured:** PA0 pulse width = **216 µs** out of 2.67 ms budget (~8% consumed). ~92% headroom remaining. Sufficient for biquad, 79-tap FIR, and simple effects without pressure.
 
**Pending hardware verification (needs audio interface):**
1. PA4 clean sine confirmed after conversion round-trip
2. Biquad attenuation above 1 kHz confirmed on scope
**Side note — burning cycles in ISR context:** `HAL_Delay()` deadlocks inside DMA callbacks (SysTick priority too low to fire). Use a volatile counter loop instead: `for(volatile int i = 0; i < 10000; ++i);` — `volatile` prevents compiler from optimizing it away, calibrate count experimentally.
 
**Where to pick up next:** Track C Day 5 — L=4 interpolation filter firmware integration (TIM2 → 192 kHz, DAC DMA resize, 79-tap FIR in `process_block`). PA0 pulse measurement will be the key metric there. Or pivot to Track B Day 4 (noise figure) or Track A Day 6 (IIR vs FIR close).
 
## Session 23 — 2026-05-30 — Track C Day 5: Interpolation FIR Firmware Integration
 
**Track:** C — pedal-driven, Day 5
 
**Goals achieved:** Full interpolation pipeline written and integrated into `process_block`. TIM6 added for DAC at 192 kHz. Buffers moved to global scope to fix stack overflow risk.
 
**Architecture finalized:**
- TIM2 → ADC1 at exactly 48 kHz (ARR=1874, PSC=0)
- TIM6 → DAC at ~192 kHz (ARR=468, PSC=0) — 0.05% error, ~12 Hz cutoff shift, inaudible
- TIM3 → ADC2 (pots) — unchanged
- `out_buffer` resized to `UPSAMPLE_BUFFER_SIZE * 2` to accommodate 4x upsampled DAC stream
**`process_block` signal chain:**
1. ADC → float conversion (bias-subtract + scale, 128 samples)
2. Biquad lowpass (1 kHz Butterworth, Direct Form I)
3. Zero-insertion upsample 4x → 512-sample scratch buffer
4. 79-tap FIR interpolation filter (circular buffer, Direct Form, processes all 512 samples)
5. float → DAC conversion (scale + bias-add + clamp, 512 samples)
**FIR implementation details:**
- Circular buffer state in `FIR_Interpolation` struct with `currIndex`
- `currIndex` wraps at `FIR_NUM_TAPS - 1` back to 0
- Global scope: `processInputBuffer`, `processOutputBuffer`, `oversampledScratchBuffer`, `oversampledOutputBuffer` — moved from stack (~5 KB) to BSS segment to prevent hard fault
**Bugs caught:**
- Double `currIndex++` — bare increment left after wrap-aware increment was added
- FIR outer loop iterating `BUFFER_SIZE` instead of `UPSAMPLE_BUFFER_SIZE` — only 128 of 512 samples would have been filtered
- `out_buffer` sized at `BUFFER_SIZE * 2` — too small for upsampled output, would have caused DMA overrun
- Stack overflow risk from four large float arrays declared locally in `process_block`
**Timer architecture note:** ADC and DAC on separate timers introduces a fixed phase offset at startup but zero drift (both derive from same APB clock). Inaudible for guitar pedal use case.
 
**Pending before flash:**
1. FIR coefficients generated from Python (fs=192 kHz, passband=20 kHz, stopband=28 kHz, 79 taps) and pasted into `interpolationFilter.coeffs`
2. PA4 clean output verified on scope
3. PA0 pulse width measured with full FIR active — expect significantly more than 216 µs baseline
4. TIM6 DAC trigger confirmed on scope
**Where to pick up next:** Track C Day 6 — polyphase decomposition. Restructure 79-tap FIR into 4 sub-filters of ~20 taps each, eliminate zero-sample multiply-accumulate work. Target: 4x reduction in FIR CPU cost. Prerequisites: coefficients in place and hardware baseline measured first.
 
---
 
## Session 26 — 2026-05-31 — Planning: FPGA Project Scoping + Sigma-Delta Deep Dive + Summer Roadmap
 
**Track:** Meta / planning (no technical track advanced; pedal work paused mid-debug)
 
**Context:** Wide-ranging planning + concept session. No pedal code changed. Pedal's open issues (periodic ~4s glitch, breadboard noise) remain banked from Session 25 — not touched today.
 
**FPGA project — scoped and largely decided:**
- Confirmed FPGA + DSP as Project #2 for application season. Owen's key advantage: already understands the DSP deeply, so only fighting the HDL learning curve, not algorithm + HDL simultaneously. Smartest projects = porting things he already built.
- Project idea spread discussed (streaming FIR parallel-vs-folded, streaming FFT, I²S audio effects processor, pitch detector, DDC/comms blocks, CIC/sigma-delta).
- **Leading candidate: sigma-delta ADC project.** Most ADI-aligned thing on the list (ADI is a major ΣΔ converter company), connects directly to pedal's oversampling work, and the CIC decimator is the FPGA sibling of the pedal's polyphase FIR decimator.
- **Scope laddering decided (each stage independently complete — never bet whole project on hardest part):**
  1. Weeks ~1–4: Learn HDL on trivial→real projects, culminating in streaming FIR verified vs Python golden reference (doubles as reusable building block + "learn the tools" tax).
  2. Weeks ~4–8: CIC + FIR decimation chain (Option A — digital only, fed a Python-generated or real 1-bit modulator stream). Complete, demonstrable project even if stopped here.
  3. Weeks ~8–12 stretch: Add real first-order analog modulator (Option B — op-amp integrator + comparator + FPGA closing feedback loop via flip-flop + 1-bit feedback DAC). True mixed-signal ADC. Falls back gracefully to Option A if analog loop fights.
  - NOT realistic: high-order, high-SNR, polished converter (Option C). Don't let scope drift there.
**Sigma-delta ADC — conceptual walkthrough delivered:**
- Philosophy: trade precise analog components for speed + digital cleverness. Oversample hard, use 1-bit quantizer.
- Two halves: (1) analog modulator (Σ summing junction → integrator → 1-bit comparator → 1-bit feedback DAC, in a loop) producing pulse-density-modulated 1-bit stream; (2) digital decimator (CIC + FIR) recovering clean multi-bit samples.
- Noise shaping is the magic: feedback loop highpasses quantization noise (pushes it out of signal band, up high where it's filtered away) while passing signal flat. First-order ~9 dB/octave OSR (~1.5 bits), second-order ~15 dB/octave.
- **Comparator threshold is FIXED** (clarified Owen's question) — the integrator OUTPUT moves and hunts across the fixed threshold; 1s-density tracks input. Balancing loop, not adjustable-threshold loop. Thermostat analogy.
- Higher-order = more integrator stages = more shaping but stability problems → start first-order.
- Banked: draw first-order modulator block diagram + run noise-shaping loop transfer function by hand when building.
**Demo strategy for Option B (mixed-signal ΣΔ):**
- A crude first-order breadboarded modulator is NOT audio-quality (~8–10 bits, audibly noisy). Audio playback = fun closer, NOT the headline.
- **The demos that prove it works are MEASUREMENTS** (and signal analog-engineer rigor): (1) noise-shaping FFT — THE money shot, visually proves shaped noise floor; (2) DC sweep / transfer function → linearity, INL/DNL; (3) 1-bit stream on scope (density modulation visible); (4) SNR vs OSR matching first-order theory; (5) reconstruction overlay.
- Measurement infra compounds with pedal's Bode setup + Session 16 Python FFT pipeline.
**Hiring-manager assessment of ΣΔ project (Owen asked for honest rating):**
- Solidly tier-3 (genuine engineering depth), credible reach to tier-4 (indistinguishable from early professional work) IF characterized well.
- Pushes up: architecturally on-target for ADI specifically (memorable, signals interest in their actual products); mixed-signal (rare among undergrads, exactly what analog cos hunt); measurement rigor crosses "built a thing" → "characterized a thing vs theory."
- Caps: execution + documentation are everything (half-finished/uncharacterized → drops to tier-2); a first-order breadboard ΣΔ is NOT impressive AS a converter — impressiveness is understanding + mixed-signal integration + characterization, so FRAME it that way ("built to understand the architecture, here's the measured noise-shaping"), not as "I built a great ADC."
- Verdict: top-quartile among undergrad projects at an analog-converter company IF finished + characterized + defensible. Risk is execution/scope, not ceiling. Complete first-order beats unfinished higher-order.
**VHDL vs Verilog decision:** Soft lean **Verilog → SystemVerilog**. Closer to Owen's C background (eases the learning curve, his binding constraint); SystemVerilog dominant in US semiconductor/analog (ADI); larger hobbyist/budget-FPGA ecosystem; ΣΔ/CIC/FIR well-trodden in Verilog. CAVEAT flagged: if his university's digital-logic courses use VHDL, institutional support (TAs, coursework reuse) may override the lean — open question for Owen to check.
 
**Third (smaller) project — options laid out, not yet decided:**
- Category 1 (highest payoff/hour — productize existing work): package his filter-design Python tooling (FIR/biquad coefficient gen, Bode, freqz) into a clean tool/library/web app; OR a measurement/characterization harness (Python driving DS1054Z + function gen over VISA, auto-generating SNR/THD/freq-response/INL-DNL plots — force multiplier, generates impressive plots for BOTH pedal and ΣΔ).
- Category 2 (broaden toward pure analog — best ADI gap-fill): discrete analog project characterized on bench (discrete op-amp, instrumentation amp, low-noise preamp); OR a noise-measurement project straight out of Track B Day 4 (measure amp input-referred noise, compare measured vs theory).
- Category 3 (CUDA breadth hedge — least ADI-aligned, highest effort): batched convolution reverb (partitioned FFT, natively GPU-shaped); OR offline spectral/granular GPU renderer. Only if general-SWE/ML optionality matters.
- **Recommendation:** for a low-risk finishable slot, lean Category 1 (characterization harness especially — components exist, low completion risk, useful to other projects). If broadening toward pure analog matters more, the noise-measurement project is the smallest finishable analog option (plugs into Track B). CUDA only as deliberate breadth hedge.
**PCB learning estimate (pedal):** ~2–4 weeks part-time to a first finished board for someone at Owen's level. Gentler curve than FPGA. Big advantage: schematic already designed (transcribing known design, not designing in the tool). KiCad recommended (free, resume-legible). Key scope move: **first PCB = a carrier/shield board** holding analog circuitry (filters, op-amps, power, jacks, pots) with headers for the Nucleo to plug into — vastly simpler than laying out the STM32 chip directly (no crystal/decoupling/high-density routing), still gets ground plane + clean analog layout, fixes most breadboard noise. Full integrated board = great second board later. Parallelizes with everything via the 1–2 week fab wait.
 
**Refined summer arc (confirmed direction):** Pedal (now→mid-July, hold the deadline) → FPGA+ΣΔ (Project #2, the depth flagship) → third smaller project (TBD per above). Owen leaning pedal → FPGA → CUDA, but third slot still genuinely open; analog-measurement alternative noted as possibly better ADI fit.
 
**Non-technical:** Discussed sustainable daily structure for a 6–6:30 wake transition (gradual shift, morning light, protect total sleep, guard a mid-morning deep-work block for hardest learning, triage shallow tasks to low-energy windows, 60–90 min focused cycles) and diet/exercise basics (sleep is the hub; regular movement; stable blood sugar; hydration; morning-only caffeine). Framed with appropriate skepticism re: overclaiming. Not course content — captured only as context for summer execution.
 
**Where to pick up next:** Technical track resumes with **CMSIS-DSP swap** (Session 25's committed next step — replace hand-written polyphase FIR with `arm_fir_interpolate_f32`, re-measure PA0). Then input-oversampling architecture (decimation FIR @ 192 kHz ADC), then granular engine scoping. Pedal debugging (periodic ~4s glitch) slots in when hardware is in front of him. Still banked: Track B Day 4 (noise figure — also feeds the possible third project), Track A Day 6 (IIR vs FIR close), Track D Day 3 (autocorrelation). FPGA project is a parallel summer track once HDL ramp begins.
 

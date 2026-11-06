<p align="center">
  <img src="https://raw.githubusercontent.com/hiptop-ai/hiptop-ai/main/docs/brand/hiptop-lockup.svg" alt="hiptop — AI agents for Android, from app to AOSP" width="440">
</p>

<p align="center"><strong>Nothing is done until a test proves it.</strong></p>

<p align="center">
  <a href="https://hiptop-ai.github.io/get-started/">Get Started</a> ·
  <a href="https://hiptop-ai.github.io/how-it-works/">How it Works</a> ·
  <a href="https://hiptop-ai.github.io/agent/">Agent Commands</a> ·
  <a href="https://hiptop-ai.github.io/customize/">Customize</a> ·
  <a href="https://hiptop-ai.github.io/demos/">Demos</a> ·
  <a href="https://hiptop-ai.github.io/faq/">FAQ</a>
</p>

Hiptop is an open source toolkit that gives the AI coding agent your team already uses the knowledge, the commands and the checks to build and maintain Android products, from a new app screen to a new board. **Your agent generates; Hiptop's code verifies.** Its agents do the work, and its checks prove every result before it counts as done.

```sh
uv tool install hiptop
```

## See Hiptop in action

```text
you › Show the rear camera when the driver selects reverse, even before Android is up.

● /android-build
  REQ-1  Rear view on screen within 2.0 s of selecting reverse
         source  HAR camera view · Android 17 · verified
         test    rear_view_response_time
  REQ-2  At least 30 frames per second
         source  HAR camera view · Android 17 · verified
         test    rear_view_frame_rate
  Threat analysis  1 risk: a frozen frame shown as live · covered by REQ-2

● Writing the HAR app change in stage/ (Rust)
  ✓ every symbol exists in Android 17: ICameraManager, harry::ICameraDevice
  ✓ 2 files changed, the AOSP tree untouched

● /android-prove rear-view-01
  ✓ builds for sdv_har_cf
  ✓ rear view within 2.0 s, 20 of 20 runs, Android VM stopped
  ✓ 30 fps or more on Cuttlefish
  ✓ each test fails without the change
  ✓ security: no new permissions, no known CVEs in new dependencies
  ✓ evidence bound to commit 4e1a2c9
  Done: rear-view-01
```

- **Specified with sources.** Each requirement cites the official docs for your Android version and names the test that will prove it. Security risks are analysed before the code.
- **Every API checked.** Each symbol the agent writes is checked against your Android version, so code never calls an API that does not exist.
- **Proved on the target.** Done means the tests ran on the target, failed without the change, and are bound to the commit.

## Faster projects. Problems found early.

- **A project accelerator.** The agents do the repetitive work: searching the docs, porting patches, running and fixing tests. Each project reuses the last one's Product Map, tests and decisions, so the next one starts ahead.
- **A shift-left solution.** Requirements, security analysis and tests come before the code, and every step is checked as it happens. A problem shows up at the step that caused it, not weeks later in integration.
- **Proof, not promises.** Done means a test ran, failed without the change, and is bound to the commit. Every claim on your product is backed by evidence you can run again.

## The whole life of an Android product

Six areas, one agent for each. They run in the agent tool you already use and do the work end to end; Hiptop's code checks every step.

| Area | Agent | What you get | Ask it |
|---|---|---|---|
| **Understand** | `/android-understand` | Know what you ship, and what blocks the next Android: a Product Map of every module, HAL, interface and change from AOSP, plus a Readiness Report | *Map our product and tell me what blocks Android 17.* |
| **Bring up** | `/android-bring-up` | New hardware running sooner: the Bring-up Ladder (build, console, adb, SELinux, HALs), each step checked, flashing only with your approval | *Bring Android 17 up on our Raspberry Pi 5 board.* |
| **Build** | `/android-build` | New features with proof at every step: sourced requirements, threat analysis, tests before code, the change in `stage/`, proved on Cuttlefish or your lab | *Show the rear camera when the driver selects reverse.* |
| **Comply** | `/android-comply` | Stay compliant release after release: CTS, VTS and the other suites, failures grouped by cause, product bugs separated from flaky tests and waivers | *Fix the CTS failures from last night's run.* |
| **Protect** | `/android-protect` | Security fixes shipped faster: each monthly Android security bulletin applied, your product assessed against current vulnerabilities, security tests run | *Apply the November security bulletin.* |
| **Migrate** | `/android-migrate` | Move to a new Android version at a lower cost: every AOSP change triaged, HALs and apps ported, the update proved with the new version's tests and the OTA Check | *Move our product from Android 16 to Android 17.* |

One more command serves every area: **`/android-prove <ticket>`** checks that the evidence still holds for the current code (the tests ran and passed, each failed without the change, the result is bound to the commit) and writes the Proof Report.

## How it works

Every piece of work follows the same flow, with a check at every stage. Your agent never edits your AOSP tree directly: every change goes through `stage/`, a mirror of the AOSP paths, is listed by ticket and can be undone. Every decision and test result is kept under `work/`, next to your code.

**Trust Checks** make sure a made-up answer never becomes a deliverable. Each one is deterministic code that runs on your machine:

| Check | What it refuses |
|---|---|
| Symbol check | APIs, modules, HALs or SELinux types that do not exist in your tree |
| Verified citations | A cited source that does not exist or does not contain the quoted words |
| The test must fail without the change | A test that passes anyway, because it proves nothing |
| Your version only | Knowledge from any Android version other than yours |
| Provenance | Facts that do not say whether they were extracted, inferred or declared |
| Failure clustering | Hundreds of test failures left unsorted: they become a handful of causes, by local statistics |
| Learning from outcomes | Recipes that usually fail, flagged before they are used |
| Second opinion | Doubts from an independent review left untested: they become tests, never decisions |

## Works with the tools you already use

Claude Code, Codex, Antigravity CLI, OpenCode and local models. Connect a tool once with `hiptop use`, and switch at any time: the project lives in files, so work can start in one tool and finish in another.

Hiptop holds no API keys and never calls a model. It never uploads your code or data; it downloads only public documentation and security feeds. Your source tree and your knowledge stay on your machine.

## Get started

1. `uv tool install hiptop` (Linux, Python 3.11+, [uv](https://docs.astral.sh/uv/)).
2. `cd my-product && hiptop use claude` (or `codex`, `antigravity`, `opencode`).
3. In your agent, run `/android-understand`: it learns your Android version and platform, downloads the official docs for that version and maps your product.
4. Pick the work: ask in your own words, or call the agent of the area.
5. `/android-prove <ticket>` whenever you want to know whether something is really done.

Platform work needs an AOSP checkout and Cuttlefish, or your own board; app work runs on any Android project. Android 14, 15, 16 and 17 are covered. Hiptop has no SoC built in: you describe your board in a platform profile, or capture it from a device, with Cuttlefish, Raspberry Pi 5 and Raspberry Pi 4 as references.

## Make it yours

- **Adapt it** to your hardware (`hiptop platform`), your team's notes and runbooks (`hiptop kb add`), your agent tool (`hiptop use`), and your rules in `hiptop.toml`: how much the agents do on their own, whether Hiptop may download, and hardware actions that always wait for a person.
- **Create your own agent.** An agent is one short Markdown skill that runs `hiptop` commands; Hiptop's checks hold it to the same rule as the built-in ones.
- **Share what you learn.** `hiptop export --as pack` turns agents, knowledge and decisions into a pack, reviewed by a person before it leaves; your team installs it with `hiptop pack add`.

## Where to go

| You want to | Go to |
|---|---|
| Try it on your product | [Get Started](https://hiptop-ai.github.io/get-started/) |
| Understand how it works | [How it Works](https://hiptop-ai.github.io/how-it-works/) |
| Pick the right agent for the job | [Agent Commands](https://hiptop-ai.github.io/agent/) |
| Adapt it to your product or build your own agent | [Customize](https://hiptop-ai.github.io/customize/) |
| See real projects built with it | [Demos](https://hiptop-ai.github.io/demos/) |
| Get a straight answer | [FAQ](https://hiptop-ai.github.io/faq/) |
| Read or improve the code | [`hiptop-ai`](https://github.com/hiptop-ai/hiptop-ai) |
| Ask a question | [Open an issue](https://github.com/hiptop-ai/hiptop-ai/issues/new) |

## Why "Hiptop"?

Before Android, there was the Hiptop: the device Andy Rubin's team built at Danger. It opened by swivelling its screen on a pivot, and halfway through that turn the drawing is a check. Hiptop is our tribute to where Android began, and a toolkit for where it goes next.

---

Open source under Apache 2.0. Hiptop produces evidence; it does not certify products as secure or compliant. Not affiliated with or endorsed by Google. Android is a trademark of Google LLC.

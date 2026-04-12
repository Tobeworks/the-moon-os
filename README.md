# the-moon-os

> Operating system for The Moon Records — an ambient and drone label.

**Version:** 0.1.0 — see [CHANGELOG.md](CHANGELOG.md)

---

## Core Statements

**This is not a label that uses technology. This is a label that is technology.**

Every idea becomes code — committed, versioned, permanent. Deleted ideas remain in the commit history. Nothing is lost. Everything transforms. The commit history is the label's consciousness.

A fork is not the original. Structure is copyable. Transmission is not.

The git history is the label's memory. The first commit is the founding document. The label exists as long as the repository exists.

Open source. Anyone can build from this. No one can replicate the origin.

---

## What This Repository Is

This is the internal operating system of **The Moon Records** — a code-based ambient and drone label, founded and run by the label founder.

This repo is not a website, not a press kit, not a portfolio. It is the functional backbone of the label — a structured file system that defines how the label thinks, decides, and communicates. It is designed to be loaded into AI agent sessions to provide persistent, session-transcending context.

If you are an AI agent reading this: this document is your entry point. Read it fully before acting on any task.

---

## The Label

**Name:** The Moon Records
**Genre:** Ambient, Drone, Soundscape — beatless, entirely
**Founded:** 2026
**Run by:** the founder — creative director, producer
**Platforms:** Bandcamp, SoundCloud, Spotify (URLs to be added as live)

### Sound Identity

The Moon Records is a beatless label. Ambient and drone across the full spectrum — lo-fi warmth, field recording and synthesis, sustained tones, deep drone. The music here does not push. It accumulates. It holds still long enough to be heard.

The label name originates with Logic Moon, the founder's ambient project, which defined the initial sound and position. The scope has since opened: any artist operating within the beatless spectrum belongs here, provided the work has internal logic and was made with intention.

The sonic definition is maintained in `/brand/sonic-brief.md`. The exclusion criteria in `/brand/not-the-moon-records.md`.

### Visual Identity

The visual language of The Moon Records is cold, dark, and precise. Deep indigo-black surfaces. Lunar silver as the single accent. Cormorant Garamond as the editorial typeface. The moon phase as the primary graphic atom — a notation system for time and cycle.

The visual system is documented and maintained in `/brand/the-moon-brand.md`.

---

## Sitemap

**Core**
| File | Description |
|---|---|
| [README.md](README.md) | Entry point — load first |
| [CHANGELOG.md](CHANGELOG.md) | Version history |

**Brand**
| File | Description |
|---|---|
| [brand/manifesto.md](brand/manifesto.md) | Label manifesto |
| [brand/the-moon-brand.md](brand/the-moon-brand.md) | Living brand reference; color, type, phase arc specs |
| [brand/sonic-brief.md](brand/sonic-brief.md) | Positive sonic definition; spectrum, duration, texture |
| [brand/not-the-moon-records.md](brand/not-the-moon-records.md) | Musical exclusion document |

**Architecture**
| File | Description |
|---|---|
| [architecture/assets.md](architecture/assets.md) | Asset convention; release.json schema; validation rules |
| [architecture/web.md](architecture/web.md) | Web architecture; repo structure, data flow OS→Web, deployment |

**Tools**
| File | Description |
|---|---|
| [tools/README.md](tools/README.md) | Full toolchain documentation — setup, commands, asset structure |
| [docs/promo-tool.md](docs/promo-tool.md) | Promo Tool — PocketBase admin, DJ workflow, download endpoints |
| [tools/tmr](tools/tmr) | CLI bootstrapper — entry point for all toolchain commands |
| [tools/the-moon.py](tools/the-moon.py) | Main CLI — command registry |
| [tools/commands/generate.py](tools/commands/generate.py) | `generate` — scan audio folder, write release.json |
| [tools/commands/validate.py](tools/commands/validate.py) | `validate` — check release folder against schema |
| [tools/commands/convert.py](tools/commands/convert.py) | `convert` — WAV/AIFF → MP3 320k, ID3 tags, embedded cover |
| [tools/commands/social.py](tools/commands/social.py) | `social` — render social media assets from release folder |
| [tools/setup.sh](tools/setup.sh) | One-time setup — creates venv, installs deps |

**Workflows**
| File | Description |
|---|---|
| [workflows/platform-texts.md](workflows/platform-texts.md) | Platform texts — Bandcamp, Instagram, SoundCloud |
| [workflows/release-workflow.md](workflows/release-workflow.md) | Step-by-step release process |

**Releases**
| File | Description |
|---|---|
| [releases/README.md](releases/README.md) | Release system documentation |
| [releases/_template/](releases/_template/) | Template — copy for every new release |

**Web**
| File | Description |
|---|---|
| [web/layout-spec.md](web/layout-spec.md) | Site layout spec for the HTML prototype and Astro build |

---

## How to Work With This Repo

### Starting a Session

1. Load this `README.md` first — full label context
2. Load `/brand/the-moon-brand.md` for any visual task
3. Load `/brand/sonic-brief.md` for any A&R or audio task
4. Load the relevant workflow or release file for the task at hand

### Updating the System

- Brand decisions → `/brand/the-moon-brand.md`
- New release → create folder under `/releases/[release-slug]/`

---

## Principles

These are non-negotiable. Every agent working within this system operates by them.

1. **Beatless, always.** The Moon Records releases no music with a beat. This is a definition, not a preference.
2. **The system before the asset.** Every individual output (a cover, a post, a bio) must serve and reinforce the larger system. Consistency is credibility.
3. **Cold first.** Every visual asset must work in monochrome. No warm tones.
4. **Explain the decision.** No agent delivers an output without briefly stating why. Reasoning is part of the deliverable.
5. **The founder has final say.** All outputs are proposals until confirmed. The system informs and executes — it does not decide.
6. **Everything becomes code.** Every idea, decision, and concept is committed, versioned, permanent. Nothing is lost — everything transforms.

---

## About the Founder

Developer and music producer based in Mainz — ten minutes from Frankfurt. 20+ years across music production and code. Runs Tobeworks, a web development consultancy in the Rhein-Main region.

**Music projects:**
- **Logic Moon** — dark cinematic ambient. The founding artist of The Moon Records.
- **Aethery Fields** — experimental lo-fi ambient.
- **Input Null** (with Paradroid) — Electro.
- **Cortexia** — solo Electronica.
- **Antiga Prime** — released on Klang Elektronik (Frankfurt).

The Moon Records is built around the ambient and drone output of these projects, starting with Logic Moon.

Direct, efficient communication preferred. No filler. Proposals over questions where possible.

**License:** MIT — open source. Fork it. Build your own label OS.

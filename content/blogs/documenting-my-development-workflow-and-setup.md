---
title: "Documenting My Development Workflow and Setup"
slug: "documenting-my-development-workflow-and-setup"
description: "How I document my end-to-end dev workflow: tools I use, hardware on my desk, and how it all fits together."
longDescription: "A walkthrough of my personal development environment and documentation process, covering software choices, repeatable workflows, and the physical desk setup that keeps me productive."
tags: ["workflow", "productivity", "setup", "tooling", "macOS", "Aerospace", "WezTerm", "Raycast", "Perplexity", "JetBrains"]
readTime: 5
featured: false
# ISO 8601 date; schema transforms z.date() to Date
timestamp: 2025-09-11
---

Keeping my development environment documented has saved me countless hours. This post captures my current setup and, more importantly, how I keep it reproducible and well-documented.

## Why document your workflow?
- Onboarding new machines becomes fast and stress-free.
- Context survives across projects—future-me thanks present-me.
- Sharing with collaborators is easier, and feedback improves the setup.

## Software stack (current)
- OS: macOS
- Tiling window manager: Aerospace
- Terminal: WezTerm
- Launcher/Spotlight alternative: Raycast (also for quick AI assists)
- Search engine: Perplexity
- IDEs: JetBrains
- Repositories: GitHub and SourceHut
- Package manager: pnpm for JS/TS; language version managers (asdf, nvm, pyenv) as needed
- Shell: zsh with minimal plugins; starship prompt
- Git: conventional commits; pre-commit hooks where helpful; commit templates for clarity
- Notes/Docs: Markdown-first. Keep a /docs directory per project and a top-level SETUP.md for machine bootstrap.

## AI coding assistance (in flux)
I’m currently evaluating alternatives for AI coding because JetBrains changed their AI quota in a way that doesn’t work for me. This is an open item; I’m testing options and haven’t settled on a replacement yet.

## Repeatable bootstrap
I keep a private dotfiles repo with an install script that:
- Installs runtimes and package managers.
- Pulls editor settings and extensions.
- Applies shell configuration and git aliases.
- Sets sane defaults (locale, key repeat rates, etc.).

This script is idempotent: safe to re-run and designed for partial application on existing machines.

## Hardware
- Laptop/workstation matched to project needs (RAM first, then CPU/GPU).
- External monitor with adjustable arm (eye height, reduce neck strain).
- Mechanical keyboard with tactile switches and a low-profile wrist rest.
- Trackpad or ergonomic mouse depending on task.
- Webcam and cardioid mic for calls and recordings.

## Desk setup
- Sit/stand desk with cable management (velcro ties + under-desk tray).
- Good lighting: key light angled at ~45° to reduce glare.
- Dock with a single-cable connection to power, peripherals, and network.
- Dedicated notebook and pen—quick capture beats context switching.

## How I keep the documentation current
- Projects include a short /docs/environment.md explaining versions, commands, and quirks.
- I update a checklist whenever I add/remove a tool; the dotfiles installer references that list.
- Major changes get a brief changelog entry with the “why,” not just the “what.”

## Wrap-up
Documenting your workflow isn’t busywork—it’s leverage. If you’re inspired to make your own, start by writing down your current tools and one automation you can add this week. Iterate from there.

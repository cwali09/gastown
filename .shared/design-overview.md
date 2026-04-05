# Gas Town — Design Overview

## What It Is
Multi-agent orchestration system for AI coding agents (Claude Code, Copilot, Codex, Gemini, etc.) with persistent work tracking via git-backed hooks.

## Entry Points
- `cmd/gt/main.go` — CLI entry point (`gt` binary)
- `internal/` — All core logic packages

## Key Abstractions
- **Town** — Top-level workspace (~`/gt/`), contains all rigs
- **Rig** — Project container wrapping a git repo + agents
- **Polecat** — Ephemeral worker agent with persistent identity
- **Hooks** — Git worktree-based persistent storage
- **Beads** — Git-backed issue/work tracking (structured data)
- **Convoy** — Work batch grouping multiple beads for assignment
- **Mayor** — Primary AI coordinator with full workspace context
- **Witness/Deacon/Dogs** — Three-tier agent health monitoring
- **Refinery** — Per-rig merge queue (Bors-style bisecting)
- **Molecules** — Workflow templates (TOML formulas -> tracked steps)

## Language & Build
- Go (primary), with npm-package for JS consumers
- Build: `make build`, goreleaser for releases
- Nix flake for reproducible dev environment

## Data Flow
1. User talks to Mayor or runs `gt` CLI commands
2. Commands operate on Town -> Rigs -> Polecats
3. Work state persists in Beads (git-backed) and Hooks (worktrees)
4. Convoys batch beads for agent assignment
5. Refinery handles merge queue after `gt done`
6. Witness/Deacon monitor agent health continuously

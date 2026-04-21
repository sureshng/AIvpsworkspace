# AI VPS Workspace

A portable, Git-tracked command center for managing multiple VPS servers
with Claude Code as an operations assistant.

## What this is

- A **local VS Code workspace** (clone of a Git repo) that lives on
  whatever laptop I'm currently using.
- A structured set of **notes, runbooks, and session logs** for each
  VPS I manage.
- A set of **instructions for Claude Code** (`CLAUDE.md`) so it knows
  how to organize work, pick up where we left off, and behave safely
  around production servers.

## What this is NOT

- Not a secrets store. No private keys, passwords, or `.env` files
  belong in this repo.
- Not an automation platform. It's documentation + assistant context,
  not a deployment tool.

## Folder layout

```
AIvpsworkspace/
├── CLAUDE.md              Operating instructions for Claude
├── README.md              This file
├── SETUP.md               First-time setup on a new laptop
├── .gitignore             Keeps secrets out of the repo
│
├── servers/               One folder per VPS
│   ├── _template/         Copied when onboarding a new server
│   └── <vps-name>/
│       ├── server.md      Static facts about the server
│       ├── session-log.md Running log of work done
│       ├── runbooks/      Reusable procedures
│       └── snippets/      Saved command outputs, configs
│
└── shared/                Cross-server reference material
    ├── common-commands.md
    └── troubleshooting.md
```

## Typical session

1. Open this folder in VS Code.
2. Open a terminal and run `claude`.
3. Tell Claude which VPS you're working on ("I'm on caggurukulam-prod today").
4. Claude reads that server's `server.md` and recent `session-log.md`,
   catches you up on open items, and you get to work.
5. Claude logs actions, findings, and decisions into `session-log.md`
   as you go.
6. At the end, Claude offers to commit the changes.

## Adding a new VPS

Just tell Claude: "I want to add a new server, alias `<name>`."
It will copy the template, walk you through the onboarding questions,
and set up the folder structure.

## Using this on a different laptop

See `SETUP.md` for the one-time setup steps on a new machine (install
Claude Code, configure `~/.ssh/config`, copy your SSH keys). Once
set up, just `git pull` and keep working.

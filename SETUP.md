# Setup on a new machine

This workspace is portable. When you clone it onto a new laptop, do these
one-time steps.

## 1. Clone the repo

```bash
# macOS / Linux
cd ~/Documents/work/vpsworkfolder   # or wherever you keep it
git clone git@github.com:<owner>/AIvpsworkspace.git
cd AIvpsworkspace
```

On Windows, clone wherever you like (e.g. `C:\work\AIvpsworkspace\`).
Absolute location does not matter — nothing in the repo depends on it.

## 2. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
claude   # authenticate on first run
```

Requires Node.js 18 or newer.

## 3. Configure SSH access to your VPS servers

The workspace references SSH aliases (e.g. `ssh caggurukulam-prod`),
never raw IPs. You need to define these aliases on each new machine
in `~/.ssh/config`.

Check each `servers/<n>/server.md` for the alias it expects, then
add matching entries. Example:

```
Host caggurukulam-prod
    HostName <ip-or-hostname>
    User <username>
    IdentityFile ~/.ssh/<keyfile>
```

You also need to copy your SSH **private keys** to this machine. Move
them over securely (USB, password manager, or `scp` between trusted
machines) — **do not commit them to this repo**.

Typical key location: `~/.ssh/` with `chmod 600` on each key file.

## 4. Open in VS Code and start Claude

```bash
code .
# Then, inside VS Code's integrated terminal:
claude
```

When Claude starts, tell it which VPS you want to work on. It will
read the relevant `server.md` and `session-log.md` to catch you up.

## 5. First-session sanity check

Ask Claude:

> "Verify SSH access to all configured servers."

Claude will attempt each alias and report which ones work on this
machine. Anything failing means the alias isn't set up in
`~/.ssh/config` yet or the key isn't in place.

## 6. Pulling before you start

If you use this workspace on more than one laptop, always run
`git pull` before starting a session. Otherwise your session log
will diverge from the version on your other laptop.

Likewise, commit and push at the end of each session — Claude will
offer to do this for you.

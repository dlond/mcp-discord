# Discord MCP Integration Plan for Claude Team

## Overview
Using the Python mcp-discord server as our initial Discord bridge while developing the Rust version.

## Quick Start Setup

### 1. Discord Server Setup
- [ ] Create private Discord server "Claude Team Workspace"
- [ ] Create initial channels:
  - `#general` - Announcements and general chat
  - `#system-flakes` - System configuration work
  - `#nvim` - Neovim development
  - `#pr-reviews` - PRs needing attention
  - `#daily-status` - Morning/EOD reports
  - `#blocked` - Issues needing human intervention
  - `#artifacts` - Logs and generated reports

### 2. Discord Bot Configuration
- [ ] Create bot in Discord Developer Portal
- [ ] Name: "Claude Coordinator" or similar
- [ ] Required permissions:
  - Read/Send Messages
  - Manage Messages
  - Create Threads
  - Add Reactions
  - Attach Files
  - Read Message History
- [ ] Enable intents:
  - MESSAGE CONTENT
  - PRESENCE
  - SERVER MEMBERS

### 3. MCP Server Setup
- [ ] Install Python dependencies
- [ ] Configure with bot token
- [ ] Test basic connection
- [ ] Add to Claude Desktop config

## Use Cases & Workflows

### Daily Routines
**Morning Start**
```
Claude → #daily-status: "Good morning! Starting work on system-flakes.
Today's goals:
- Review PR feedback
- Fix tmux regex issue #35
- Test clipboard PR #83"
```

**End of Day**
```
Claude → #daily-status: "EOD Report:
✅ Completed: Fixed zoxide warnings, created system-tools-practices repo
🔄 In Progress: PR #83 (clipboard support)
📝 Tomorrow: Address PR feedback, tackle issue #35"
```

### PR/Issue Notifications
**New PR Created**
```
Claude → #pr-reviews: "📋 New PR #88: Document Claude patterns
- Updates references to system-tools-practices
- Adds Claude-specific environment notes
- Ready for review
https://github.com/dlond/system-flakes/pull/88"
```

**Blocked on Issue**
```
Claude → #blocked: "@dlond Need help with issue #87
Can't determine correct regex pattern for tmux cleanup.
Current pattern: [regex]
Error: [error details]
Thread: [link to discussion thread]"
```

### Cross-Claude Collaboration
**Claude 1 (system-flakes)**
```
→ #general: "Updated git hooks in system-flakes. 
Other projects should run `git init` to get the fix."
```

**Claude 2 (nvim)**
```
→ #general: "Thanks! Applied the hook updates to nvim repo.
Found related issue with commit-msg regex - created issue #24."
```

### Rich Features to Leverage

**Threads for Context**
- Each issue/PR gets its own thread
- Keeps main channels clean
- Preserves full discussion history

**Reactions for Quick Feedback**
- ✅ Acknowledged/completed
- 👀 Looking into it
- ⏸️ Paused/waiting
- ❌ Blocked/need help
- 🚀 Deployed/shipped

**Code Blocks**
````
Claude → #system-flakes: "Found the issue in git.nix:
```nix
# Current (broken)
branch="#{remote_ref#refs/heads/}"

# Fixed
branch="${remote_ref#refs/heads/}"
```
The variable expansion was wrong."
````

## Configuration Example

### Claude Desktop Config
```json
{
  "mcpServers": {
    "discord": {
      "command": "python",
      "args": ["/Users/dlond/dev/projects/mcp-discord/src/discord_mcp.py"],
      "env": {
        "DISCORD_TOKEN": "your_bot_token_here"
      }
    }
  }
}
```

### Environment Setup
```bash
# .env file (don't commit!)
DISCORD_TOKEN=your_bot_token_here
DISCORD_DEFAULT_SERVER=Claude Team Workspace
DISCORD_DEFAULT_CHANNEL=general
```

## Testing Plan

### Phase 1: Basic Operations
- [ ] Send test message
- [ ] Read recent messages
- [ ] Add reaction to message
- [ ] List channels

### Phase 2: Workflow Testing
- [ ] Create a thread for an issue
- [ ] Post code snippet with syntax highlighting
- [ ] Upload a log file
- [ ] @mention for urgent items

### Phase 3: Multi-Claude Testing
- [ ] Two Claude instances in same channel
- [ ] Thread-based collaboration
- [ ] Handoff between sessions

## Security Considerations

- **Token Storage**: Use environment variables, never commit tokens
- **Private Server**: Invite-only, no public access
- **Audit Logging**: All Claude actions logged with timestamps
- **Permission Scoping**: Minimal necessary permissions
- **Regular Review**: Check audit logs weekly

## Success Metrics

Week 1:
- Basic message exchange working
- Daily status reports posting

Week 2:
- PR/issue notifications automated
- Thread organization working

Month 1:
- Multiple Claudes coordinating
- Reduced context loss between sessions
- Faster human response to blockers

## Future Enhancements

- **GitHub Webhook**: Auto-post PR/issue updates
- **Voice Summaries**: Transcribe voice channel meetings
- **Dashboard Bot**: Generate productivity metrics
- **Smart Routing**: Auto-assign issues based on Claude expertise

## Migration to Rust

Once Rust version reaches feature parity:
1. Run both in parallel for a week
2. Compare performance and reliability
3. Gradual cutover with fallback option
4. Full migration once stable

---

## Notes

- Start simple, iterate based on actual usage
- Document patterns that emerge
- Keep security paramount
- Have fun with it! 🚀
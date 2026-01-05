# Claude Post-Meeting Processor

A Claude Code skill that automates post-meeting processing using Fireflies.ai transcripts.

## What It Does

1. **Fetches** meeting transcripts from Fireflies
2. **Extracts** action items with WHO/WHAT/WHEN accountability
3. **Creates** GitHub issues (with duplicate detection)
4. **Generates** EOS Level 10 Meeting summaries

## Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed
- [Fireflies.ai](https://fireflies.ai) account with MCP configured
- [GitHub CLI](https://cli.github.com/) authenticated (`gh auth login`)

### Setting Up Fireflies MCP

Add to your Claude Code MCP settings (`~/.claude/settings.json`):

```json
{
  "mcpServers": {
    "fireflies": {
      "command": "npx",
      "args": ["-y", "@anthropic/fireflies-mcp"],
      "env": {
        "FIREFLIES_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

Get your Fireflies API key from: https://app.fireflies.ai/integrations

## Installation

### 1. Clone this repo

```bash
git clone https://github.com/skinnyandbald/claude-post-meeting-processor.git
cd claude-post-meeting-processor
```

### 2. Copy skill to Claude Code

```bash
# Create skills directory if needed
mkdir -p ~/.claude/skills/process-meeting-notes

# Copy skill files
cp SKILL.md ~/.claude/skills/process-meeting-notes/
cp -r workflows ~/.claude/skills/process-meeting-notes/
cp -r templates ~/.claude/skills/process-meeting-notes/
cp -r references ~/.claude/skills/process-meeting-notes/
```

### 3. Copy command file

```bash
# Create commands directory if needed
mkdir -p ~/.claude/commands

# Copy command
cp process-meeting.md ~/.claude/commands/
```

## Usage

From any git repository, run:

```bash
claude
> /process-meeting
```

The skill will:
1. Present a menu of options (recent meeting, search, manual notes, L10 only)
2. Fetch and analyze the meeting transcript
3. Extract action items and categorize them
4. Check for duplicate GitHub issues
5. Create new issues with your confirmation
6. Generate an EOS L10 summary

### Command Options

```bash
/process-meeting              # Interactive menu
/process-meeting standup      # Search for meetings with "standup"
/process-meeting 2024-01-15   # Search by date
```

## EOS Level 10 Meeting Format

The generated summary follows the [EOS (Entrepreneurial Operating System)](https://www.eosworldwide.com/) Level 10 Meeting structure:

- **Scorecard**: Key metrics review
- **Rock Review**: Quarterly goals status
- **Headlines**: News and announcements
- **To-Dos**: Action items with accountability (WHO/WHAT/WHEN)
- **IDS**: Issues identified, discussed, solved

## File Structure

```
├── SKILL.md                 # Main skill definition
├── process-meeting.md       # Slash command
├── workflows/
│   ├── process-recent-meeting.md   # Full workflow
│   ├── search-meeting.md           # Find specific meeting
│   ├── create-issues-from-notes.md # From manual notes
│   └── generate-l10-summary.md     # Summary only
├── templates/
│   ├── l10-meeting-summary.md      # L10 format template
│   └── github-issue-checklist.md   # Issue template
└── references/
    ├── eos-level-10-format.md      # L10 structure guide
    └── github-project-config.md    # Repo detection patterns
```

## Pairing with Requirements Builder

For product-focused meetings, combine this tool with [claude-code-requirements-builder](https://github.com/rizethereum/claude-code-requirements-builder):

**Example: After a product discovery call**

```bash
# Stage 1: Operational processing
> /process-meeting discovery call

# Creates GitHub issues for action items:
# - #42: Schedule follow-up demo
# - #43: Send pricing proposal
# Generates L10 summary with WHO/WHAT/WHEN

# Stage 2: Requirements extraction
> /requirements-builder

# Paste the same Fireflies transcript when prompted
# Extracts product requirements discussed:
# - User needs bulk import feature
# - Must integrate with Salesforce
# - Target: 50 concurrent users
# Outputs structured PRD
```

**Stage 1** captures *what to do next* (tasks, follow-ups).
**Stage 2** captures *what to build* (requirements, specs).

## License

MIT

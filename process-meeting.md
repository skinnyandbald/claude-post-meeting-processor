---
description: Process a Fireflies meeting transcript to extract action items, create GitHub issues, and generate EOS L10 summaries
allowed-tools: Skill(process-meeting-notes), mcp__fireflies__*, Bash(gh issue create:*), Bash(gh issue list:*), Bash(gh label list:*), Bash(gh repo view:*), Bash(gh api:*), Bash(gh project:*), Read, Write, Glob, Grep, TodoWrite
---

# Process Meeting Notes

Load and execute the `process-meeting-notes` skill to process Fireflies meeting transcripts.

$ARGUMENTS - Optional: meeting keyword or date to search for (e.g., "standup", "2024-01-15")

<process>
1. Invoke the skill: `Skill(process-meeting-notes)`
2. Follow the skill's intake menu and routing
3. If $ARGUMENTS provided, use them as search criteria
</process>

1. CLAUDE.md instruction (loaded every session automatically):

### Auto-Update Memory (MANDATORY)

**Update memory files AS YOU GO, not at the end.** When you learn something new, update immediately.

| Trigger | Action |
|---------|--------|
| User shares a fact about themselves | → Update `memory-profile.md` |
| User states a preference | → Update `memory-preferences.md` |
| A decision is made | → Update `memory-decisions.md` with date |
| Completing substantive work | → Add to `memory-sessions.md` |

**Skip:** Quick factual questions, trivial tasks with no new info.

**DO NOT ASK. Just update the files when you learn something.**

2. Stop hook (settings.json → hooks.Stop):

#!/bin/bash
CONTEXT=$(cat)

STRONG_PATTERNS="fixed|workaround|gotcha|that's wrong|check again|we already|should have|discovered|realized|turns out"
WEAK_PATTERNS="error|bug|issue|problem|fail"

if echo "$CONTEXT" | grep -qiE "$STRONG_PATTERNS"; then
    cat << 'EOF'
{
  "decision": "approve",
  "systemMessage": "This session involved fixes or discoveries. Consider running /reflect to capture learnings in project docs."
}
EOF
elif echo "$CONTEXT" | grep -qiE "$WEAK_PATTERNS"; then
    echo '{"decision":"approve","systemMessage":"If you learned something non-obvious this session, run /reflect to update docs."}'
else
    echo '{"decision": "approve"}'
fi

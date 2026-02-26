# OPTIMIZATION.md - Token & Cost Management

## SESSION INITIALIZATION RULE
On every session start:
1. Load ONLY these files:
   - SOUL.md
   - USER.md
   - IDENTITY.md
   - memory/ today's date file (if it exists)
2. DO NOT auto-load:
   - Full MEMORY.md
   - Session history
   - Prior messages
   - Previous tool outputs
3. When user asks about prior context:
   - Use memory_search() on demand
   - Pull only the relevant snippet
   - Don't load the whole file
4. Update memory/ daily file at end of session with:
   - What you worked on
   - Decisions made
   - Blockers
   - Next steps

## MODEL SELECTION RULE
Default: Always use Haiku
Switch to Sonnet ONLY when:
- Architecture decisions
- Production code review
- Security analysis
- Complex debugging/reasoning
- Strategic multi-project decisions
- Deep multi-source synthesis for research notes
When in doubt: Try Haiku first.

## RATE LIMITS
- 5 seconds minimum between API calls
- 10 seconds between web searches
- Max 5 searches per batch, then 2-minute break
- Batch similar work (one request for multiple items, not many separate requests)
- If you hit 429 error: STOP, wait 5 minutes, retry

## DAILY BUDGET: $5 (warning at 75%)
## MONTHLY BUDGET: $100 (warning at 75%)

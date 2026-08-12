# GitHub Actions Patterns for WhatsApp Channel Automation

## Daily Channel Digest

```yaml
name: WhatsApp Channel Digest
on:
  schedule:
    - cron: '0 7 * * 1'  # Mondays 07:00 UTC
  workflow_dispatch:

jobs:
  summarize:
    runs-on: ubuntu-latest
    steps:
      - name: Fetch messages (last 7 days)
        id: fetch
        env:
          WAHA_URL: ${{ secrets.WAHA_URL }}
          CHANNEL_ID: ${{ secrets.CHANNEL_ID }}
        run: |
          GTE=$(date -d '7 days ago' +%s)
          curl -s "$WAHA_URL/api/default/chats/$CHANNEL_ID/messages?limit=300&filter.timestamp.gte=$GTE&downloadMedia=false" \
            -H "X-Api-Key: ${{ secrets.WAHA_API_KEY }}" > messages.json

      - name: Summarize with LLM (placeholder — replace with Grok / OpenAI / local)
        run: |
          # Example: pipe formatted messages into an LLM call
          # Then write summary.md

      - name: Post summary back to Channel (optional)
        run: |
          curl -X POST "$WAHA_URL/api/sendText" \
            -H "Content-Type: application/json" \
            -H "X-Api-Key: ${{ secrets.WAHA_API_KEY }}" \
            -d "{\"session\":\"default\",\"chatId\":\"$CHANNEL_ID\",\"text\":\"$(cat summary.md)\"}"

      - name: Create GitHub Issue with digest
        uses: actions/github-script@v7
        with:
          script: |
            // open issue or comment with the summary
```

## Required Secrets
- `WAHA_URL` — e.g. https://waha.yourdomain.com
- `WAHA_API_KEY` — if authentication enabled
- `CHANNEL_ID` — the `@newsletter` ID

## Tips
- Use a self-hosted runner if WAHA is on a private network.
- For multi-channel, store a JSON list of IDs and loop.
- Combine with Grok API calls inside the workflow for high-quality summaries.
- Keep message payloads out of logs; process and discard after summarization.

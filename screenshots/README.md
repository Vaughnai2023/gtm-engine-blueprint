# Screenshots Needed

Add screenshots to this folder and update the image references in the README and docs.

## Must Have (for README.md)

| # | What to Capture | Where | Filename |
|---|---|---|---|
| 1 | All 15 workflows visible in sidebar/list view | n8n instance | `workflow-list.png` |
| 2 | Reply Handler workflow — zoomed out showing the 6-way branch | n8n canvas | `reply-handler.png` |
| 3 | A signal intelligence workflow (NHS Jobs or Board Paper monitor) | n8n canvas | `signal-workflow.png` |
| 4 | Pilot Lifecycle kickoff workflow | n8n canvas | `pilot-lifecycle.png` |
| 5 | Claude Code terminal — scoring companies and showing tier table | Terminal | `claude-scoring.png` |
| 6 | Claude Code terminal — full pipeline completing (Score > Find > Enrich > Write > Deploy) | Terminal | `pipeline-deploy.png` |

## Nice to Have (for doc pages)

| # | What to Capture | Where | Filename |
|---|---|---|---|
| 7 | Slack notification showing a signal alert from n8n | Slack | `slack-alert.png` |
| 8 | Instantly campaign view — draft campaign with email sequences | Instantly | `campaign-draft.png` |
| 9 | Scored companies output in terminal (tier distribution) | Terminal | `tier-distribution.png` |
| 10 | Claude Code generating an email sequence | Terminal | `email-generation.png` |
| 11 | System architecture diagram (clean flywheel visual) | Excalidraw / Mermaid | `system-diagram.png` |

## Capture Tips

- **Dimensions**: 1200px wide minimum. Keep all screenshots the same width.
- **Theme**: Use dark mode in n8n and terminal for a polished look.
- **Crop**: Remove the browser address bar / n8n instance URL from all screenshots.
- **Blur**: Blur or crop any credential nodes, API keys, or instance URLs.
- **Redact**: Cover any API keys visible in terminal output.
- **Format**: PNG preferred. Keep file sizes reasonable (compress if over 1MB).

## Adding Screenshots to Docs

Once captured, uncomment the image references in README.md:

```markdown
<!-- ![System Overview](screenshots/workflow-list.png) -->
<!-- ![Reply Handler](screenshots/reply-handler.png) -->
```

Remove the `<!-- -->` comment markers to make them visible.

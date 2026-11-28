# Issue tracker: GitHub

Issues and specifications for this repository live in GitHub Issues for `aldip4886/whatsapp_gateway_self`. Use the `gh` CLI for issue operations.

## Conventions

- Create: `gh issue create --title "..." --body "..."`
- Read: `gh issue view <number> --json number,title,body,labels,comments`
- List: `gh issue list --state open`
- Comment: `gh issue comment <number> --body "..."`
- Apply/remove labels: `gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- Close: explain the resolution in a comment, then run `gh issue close <number>`.

Infer the repository from the current clone and verify the target with `gh repo view --json nameWithOwner` before creating issues. File issues only in this repository. Pull requests are not part of the triage request surface by default.

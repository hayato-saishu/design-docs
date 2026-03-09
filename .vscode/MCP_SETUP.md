# MCP setup for this workspace

This workspace has MCP server definitions in `.vscode/mcp.json` for:
- `context7`
- `github` (OAuth)
- `github-pat` (PAT)
- `desktop-commander`
- `playwright`
- `notionApi`
- `figma`
- `serena`

## 1. Install required runtimes on Windows

PowerShell (run as Administrator):

```powershell
winget install OpenJS.NodeJS.LTS
winget install astral-sh.uv
```

Then restart VS Code.

## 2. Optional: verify commands

```powershell
node --version
npm --version
npx --version
uv --version
```

## 3. Tokens you will be prompted for

When servers start, VS Code may ask for:
- `notion_token` for `notionApi`
- `figma_api_key` for `figma`
- `github_mcp_pat` for `github-pat`

You can skip `github_mcp_pat` if you use `github` OAuth server.

## 4. Where tokens come from

- Notion token: Notion integration token (`ntn_...`)
- Figma API key: Figma personal access token
- GitHub PAT: GitHub personal access token (for PAT mode)

## 5. Start using MCP tools

1. Open Copilot Chat in Agent mode.
2. Open MCP server picker/manage UI.
3. Enable needed servers from `.vscode/mcp.json`.
4. If prompted, enter token values.

## 6. Notes

- `context7` and `github` use remote HTTP MCP and do not require local runtime.
- `desktop-commander`, `playwright`, `notionApi`, and `figma` require `npx`.
- `serena` requires `uvx` (provided by `uv`).

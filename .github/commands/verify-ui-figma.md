---
allowed-tools: Bash(git diff*), Read, Glob, Grep, Agent, mcp__figma__get_design_context, mcp__figma__get_screenshot, mcp__figma__get_metadata, mcp__figma__get_variable_defs
description: Compare current branch diff against an origin branch and verify UI implementation matches Figma designs
---

## Your task

You are verifying that UI code changes on the current branch match Figma design specifications. The user will provide:
- An origin branch to diff against (e.g., `origin/develop`)
- One or more Figma design URLs

## Workflow

### Step 1: Fetch Latest & Get the Diff

First, fetch the latest remote state:

```bash
git fetch origin
```

Then list changed files:

```bash
git diff <ORIGIN_BRANCH>...HEAD --name-only
```

Then get the full diff:

```bash
git diff <ORIGIN_BRANCH>...HEAD
```

Read all changed files from the diff to understand the full scope of UI changes. For each changed file that contains UI code (Composables, layouts, themes, colors, dimensions), also read the full file in the codebase to understand context.

**Filter**: Focus only on files relevant to UI — Composable functions, theme definitions, color/typography tokens, drawable resources, dimension values. Skip non-UI files (repositories, use cases, data models, build files, etc.).

### Step 2: Fetch Figma Design Details

**IMPORTANT — Figma Desktop App Required:**
The Figma tools (`mcp__figma__*`) are provided by the **Figma MCP server**, which requires the **Figma Desktop App** to be running and logged in on the user's machine. The MCP server connects to the local Figma Desktop App instance to access design data.

Before calling any Figma tool:
1. Check that the Figma MCP tools are available in this session (they will appear in the available tools list as `mcp__figma__*`)
2. If the tools are **NOT available**, stop and inform the user:
   > "The Figma MCP server is not connected. To use Figma verification, please make sure the **Figma Desktop App** is running and logged in on your machine, and that the Figma MCP server is properly configured."
3. Do **NOT** attempt to fetch Figma URLs via `WebFetch` or any other tool as a fallback — the Figma API requires authenticated MCP access through the Figma Desktop App.

For each Figma URL provided by the user:

1. Extract `fileKey` and `nodeId` from the URL (convert `-` to `:` in nodeId)
2. Call `mcp__figma__get_design_context` with the fileKey and nodeId to get the design code, screenshot, and contextual hints
3. Call `mcp__figma__get_screenshot` for visual reference
4. Call `mcp__figma__get_metadata` for detailed component info
5. Call `mcp__figma__get_variable_defs` to get design tokens (colors, spacing, typography)

### Step 3: Compare Code Against Figma Designs

For each design, compare the changed code against the Figma specification:

- **Layout & Structure**: Does the Compose layout match the Figma hierarchy? Row/Column/Box nesting, alignment, arrangement
- **Colors**: Are colors matching design tokens? Check hex values, opacity, light/dark theming
- **Typography**: Font family, size, weight, line height, letter spacing
- **Spacing & Padding**: Margins, paddings, gaps between elements
- **Corner Radius**: Border radius values
- **Icons & Assets**: Correct icons used, correct sizes
- **States**: Normal, pressed, disabled, loading, error states implemented correctly
- **Animations & Transitions**: Auto-dismiss timing, transitions, motion specs
- **Responsive behavior**: How the UI adapts to different screen sizes or content scenarios
- **Component mapping**: If Figma uses a design system component, check that the code uses the corresponding project component (not a raw implementation)

### Step 4: Output Report

Structure your report as follows:

```
# UI Figma Verification: <current_branch> vs <origin_branch>

## Changed UI Files
<List of UI-related files that were changed>

## Figma Compliance

### <Design Name / Screen 1>
| Status | Element | Expected (Figma) | Actual (Code) | File:Line | Notes |
|--------|---------|-------------------|---------------|-----------|-------|
| ...    | ...     | ...               | ...           | ...       | ...   |

### <Design Name / Screen 2>
...

## Summary
- ✅ Matches: <count>
- ⚠️ Minor differences: <count>
- ❌ Mismatches: <count>

## Verdict
<PASS / NEEDS FIXES>
<List of items that must be fixed, if any>
```

Use status indicators:
- ✅ = Matches Figma design
- ⚠️ = Minor difference (acceptable but worth noting)
- ❌ = Mismatch that should be fixed

## Important Notes
- Be thorough but avoid false positives — only report real differences
- Focus on what is actually different, not minor pixel-level variations that are implementation artifacts
- If a design token or style is ambiguous in Figma, note it as ⚠️ rather than ❌
- Always provide actionable recommendations with specific values from Figma
- When Figma uses absolute positioning, translate to the closest Compose equivalent (Arrangement, Alignment, padding) — don't flag the difference in layout approach itself

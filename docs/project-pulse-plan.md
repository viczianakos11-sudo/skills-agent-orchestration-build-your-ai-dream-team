# Project Pulse implementation plan

## Summary

Build Mona’s Project Pulse as a small static dashboard for contributors, showing project names, owners, status, recent activity, priority, and a short summary. Follow the brief and Step 3 acceptance criteria: project cards, accessible responsive styling, JSON-backed data, and a VS Code launch configuration that serves `app/` and opens `index.html`. The app directory and `launch.json` do not currently contain implementation files.

## Responsibilities and file assignments

- **Designer — `app/styles.css`:** Define the visual hierarchy and responsive card layout; specify and style stable hooks such as `.dashboard` and `.project-card`; ensure status and priority are readable without relying on color alone; account for keyboard focus and contrast. Agree on class hooks with Coder before implementation.
- **Coder — `app/index.html`:** Build semantic page structure, link `styles.css`, load `project-data.json`, and render project cards. Keep the implementation within the requested files; use safe DOM text insertion for data-driven content.
- **Coder — `app/project-data.json`:** Supply a top-level `projects` array, with each project containing `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a concise `summary` field so the UI meets the brief’s contributor-friendly summary requirement without weakening the required schema.
- **Coder — `.vscode/launch.json`:** Add strict JSON with no comments and a **Run Project Pulse Dashboard** configuration. Serve from `${workspaceFolder}/app` using `python3 -m http.server 5500`; configure `serverReadyAction` to open `http://localhost:%s/index.html`.
- **Orchestrator:** Coordinate the design contract, keep file ownership separate, then review the integrated result and validation evidence. Do not take over Designer or Coder implementation files.

## Ordered implementation steps and dependencies

1. **Agree on the interface contract.** Designer and Coder settle the page’s semantic structure and shared CSS hooks before writing the app. The brief defines the required data fields; use `summary` as an additional field.
2. **Implement independent files in parallel once the contract is settled.** Designer writes `app/styles.css`; Coder writes `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. The HTML and CSS work can proceed in parallel because both use the agreed hooks. JSON and launch setup are independent of the visual styling.
3. **Integrate and review sequentially.** Orchestrator checks that the HTML selectors match the stylesheet, the rendered cards consume the JSON fields, and the launch configuration opens the dashboard. Resolve any mismatches before handoff.

**Parallel:** Step 2’s CSS, HTML, JSON, and launch work can run concurrently after Step 1; ownership does not overlap.  
**Sequential:** Agree on shared selectors first, then integrate and validate after implementation. Do not treat parallel file creation as proof that the app works together.

## Risks and edge cases

- `fetch()` of JSON may fail when opening the page with `file://`; validate through the configured HTTP server instead.
- Empty, malformed, or unavailable project data must not produce a misleading blank success state. Show a clear empty or loading/error state.
- Render data as text rather than interpolated HTML; status and priority values should remain legible for unexpected values.
- The brief asks for summaries but its minimum field list omits one; the plan resolves this by adding `summary`.
- The repository does not specify brand colors, sample project content, or a framework. Keep the implementation framework-free and choose an accessible, restrained visual style unless Mona supplies preferences.

## Validation expectations

- Parse `app/project-data.json` and `.vscode/launch.json` with `python3 -m json.tool`.
- Confirm the HTML references `styles.css` and `project-data.json`, and renders cards containing each required field and the summary.
- Confirm the stylesheet includes `.dashboard`, `.project-card`, rounded corners, shadows, and responsive layout.
- Start **Run Project Pulse Dashboard**; verify the browser displays the dashboard at `/index.html`, not a directory listing. Check that project data loads and that missing/empty data is handled clearly.
- Check keyboard accessibility, readable contrast, and narrow-screen layout; inspect the browser console for load or rendering errors.
- Existing `scripts/validate-exercise.sh` validates repository/template setup; it does not replace the app-specific checks above.

## Open questions

- Does Mona have preferred sample project names, status/priority labels, or a brand palette? If not, use clear representative sample data and a neutral accessible palette.

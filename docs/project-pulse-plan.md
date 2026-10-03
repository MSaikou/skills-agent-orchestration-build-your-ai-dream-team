# Project Pulse implementation plan

## Goal

Build Mona's static Project Pulse dashboard for contributors. The page must be titled exactly **Project Pulse** and present multiple projects as polished, accessible, responsive cards showing each project's name, owner, status, recent activity, and priority. The preview experience must open the dashboard rather than a directory listing.

The repository's Step 2 workflow checks that this plan names the Project Pulse context, Designer and Coder responsibilities, all four assigned paths, dependencies, parallel work, and validation. Step 3 checks the app files, markup and data fields, styling hooks, and valid JSON described below.

## Responsibilities and file ownership

- **Planner** (`.github/agents/planner.agent.md`) researches requirements, edge cases, dependencies, and open questions, then phases and assigns the work. The Planner does not code.
- **Designer** (`.github/agents/designer.agent.md`) defines the dashboard's UX, accessibility, information hierarchy, responsive behavior, and visual treatment. The Designer exclusively owns **`app/styles.css`**, including the `.dashboard` and `.project-card` hooks, visible status and priority treatments, rounded cards, and shadows.
- **Coder** (`.github/agents/coder.agent.md`) implements the assigned app, data, and preview files and validates them. The Coder exclusively owns **`app/project-data.json`**, **`app/index.html`**, and **`.vscode/launch.json`**. The Coder must keep the markup/data contract agreed with the Designer and use strict JSON for both JSON files.
- **Orchestrator** (`.github/agents/orchestrator.agent.md`) delegates explicit scopes, sequences dependent work, integrates and verifies the result, and does not implement app changes. When a defect is found, the Orchestrator assigns its fix to the owner of the affected file; ownership does not shift implicitly.

No other files are in implementation scope for this plan.

## Decisions to resolve before implementation

1. Agree on the exact data and markup contract. Start with a top-level `projects` array; each project has `name`, `owner`, `status`, `recentActivity`, and `priority`. Agree with the Designer on the corresponding HTML structure, class hooks, and how these properties are rendered before parallel work starts.
2. Mona's request calls for a short contributor-friendly summary, but the suggested schema has no summary field. Ask Mona whether to add a `summary` property (and agree its meaning and rendering) or to omit it. Do not invent a field or silently substitute another property.
3. No real project records or allowed `status` and `priority` vocabularies are supplied. Ask Mona for source records and allowed values, or obtain approval to use clearly identified fictional examples and agree on the allowed values. Do not present invented records as real.
4. Confirm the preview launch mechanism in the target Codespaces/VS Code environment. The suggested launch uses `python3 -m http.server 5500`; verify Python 3 is available and that the chosen VS Code launch type/request and `serverReadyAction` are supported. The repository does not document a launch mechanism or Python extension, so these remain environment assumptions until confirmed.

Record the agreed decisions in the implementation handoff and keep the schema, HTML rendering, sample data, and styling consistent with them.

## Ordered phases and dependencies

### 1. Agree on schema and CSS/markup contract

The Orchestrator coordinates Mona's answers to the open questions above, then fixes the project data schema and the markup/classes that connect `app/index.html` to `app/styles.css`. Include how loading, empty, and failed-data states will be represented, and how long activity text behaves.

**Dependency:** This phase must finish before parallel implementation begins. It prevents the Designer and Coder from making incompatible assumptions.

### 2. Parallel independent styling and data/launch work

Once the contract is fixed, the Orchestrator may assign these non-overlapping scopes in parallel:

- Designer creates **`app/styles.css`** using the agreed class contract and establishes the responsive, accessible card visual system.
- Coder creates **`app/project-data.json`** using the agreed schema and approved source or clearly labeled fictional records, and **`.vscode/launch.json`** using the confirmed preview mechanism.

These tasks can run in parallel because they write separate files and the schema/class contract is already agreed. Dependencies: both depend on Phase 1; data content also depends on Mona's source/examples decision, and launch configuration depends on environment confirmation.

### 3. Implement dashboard markup and data rendering

After the class and data contracts are fixed (and Phase 2 has established the concrete CSS and data files), the Coder implements **`app/index.html`**. It must use the agreed hooks, load `styles.css` and `project-data.json`, and visibly render multiple project cards with name, owner, status, recent activity, and priority. Set the page title exactly to `Project Pulse`. Include the approved summary only if Mona confirms it belongs in the schema.

This phase is sequential after the contract agreement and must integrate with the Designer's stylesheet and Coder's JSON. The Coder owns only `app/index.html` here.

### 4. Integrate and validate

The Orchestrator checks all four files together and runs the acceptance checks below. If a check fails, assign the correction to the existing owner of that file; the Orchestrator does not edit implementation files.

## Validation expectations

### Automated and file-level checks

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Confirm the document's `<title>` is exactly `Project Pulse`; the HTML references `styles.css` and `project-data.json`, uses the agreed `.dashboard`/`.project-card` contract, and renders the required name, owner, status, `recentActivity`, and priority values from project data.
- Confirm `app/project-data.json` parses as JSON and matches the agreed top-level `projects` array schema. Check that records contain the agreed required fields and that status/priority values use the agreed vocabularies.
- Confirm `app/styles.css` includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, with responsive layout and accessibility considerations reviewed.
- Confirm `.vscode/launch.json` is strict-valid JSON and includes the agreed launch configuration name **Run Project Pulse Dashboard**, working directory `${workspaceFolder}/app`, deterministic serving command/port, and a browser target ending in `index.html`. For the suggested setup, verify the command is `python3 -m http.server 5500` and `serverReadyAction` opens `http://localhost:%s/index.html`.

These file/content expectations align with the repository's `.github/workflows/3-step.yml`; the plan completeness expectations align with `.github/workflows/2-step.yml`.

### Manual behavior and accessibility review

- Run the configured preview in Codespaces and confirm the browser shows the dashboard at `index.html`, not a directory listing.
- Review that multiple cards display all required fields with readable hierarchy and distinguishable status/priority treatments; check keyboard access, semantic structure, contrast, and narrow viewport behavior.
- Exercise empty project data, malformed JSON, and failed JSON loading. Confirm the page provides useful empty or error feedback instead of a blank dashboard or an unhandled failure.
- Check long recent-activity text and narrow screen widths for overflow, clipping, and loss of important information.

Do not report launch validation as complete until the configuration has actually been exercised in the target Codespaces environment.

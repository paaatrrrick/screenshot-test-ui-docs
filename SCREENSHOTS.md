# Screenshot manifest

Every `<Frame>` in these docs that points at `/images/screenshots/<id>.png` is a placeholder. This file is the source of truth for what each one should contain, how to reproduce it, and which automation edge case it exercises.

Target app: https://screenshot-test-ui.vercel.app (source: https://github.com/paaatrrrick/screenshot-test-ui). All data is seeded and deterministic; timestamps are anchored to a fixed clock. Default capture: 1440×900 viewport, light theme, device scale factor 2, unless a row says otherwise.

| # | id | Page | Capture target | Steps before capture | What it tests |
| --- | --- | --- | --- | --- | --- |
| 1 | `01-overview-dashboard` | `/` | Full page (viewport) | Wait for chart animation (~1.5s) | Baseline full-screen capture |
| 2 | `02-sidebar-navigation` | `/` | Element: `[data-slot="sidebar-container"]` (the left sidebar only) | None | Cropping to a single component |
| 3 | `03-deployment-build-error` | `/deployments/dpl_7d3m0q` | Element: `[data-testid="build-logs"]` | Scroll element into view | Error state + scroll-then-crop of a dark terminal panel |
| 4 | `04-env-vars-revealed` | `/projects/atlas-api?tab=environment` | Element: `[data-testid="env-vars"]` | Click `[data-testid="reveal-STRIPE_SECRET_KEY"]` | Deep-linked tab + interaction that changes state before capture |
| 5 | `05-create-flag-targeting` | `/flags` | Element: `[data-testid="create-flag-dialog"]` | Click `[data-testid="create-flag"]` → type `New onboarding flow` in **Name** → click the **2. Targeting** tab → type `enterprise` in the rule value input | Modal with nested tabs; form must be partially filled to reach the view |
| 6 | `06-monitor-alert-conditions` | `/monitors/new` | Element: the wizard card (parent of `[data-testid="monitor-alert-conditions"]`) | Step 1: **Name** = `Checkout API — /health`, **URL** = `https://api.atlas.orbit.dev/health`, click **Continue** → Step 2: leave defaults, click **Continue** → now on step 3 | Multi-step wizard; **Continue** is disabled until earlier steps validate |
| 7 | `07-log-row-expanded` | `/logs` | Element: `[data-log-id="log_0002"]` | Click the row header inside `[data-log-id="log_0002"]` (the first ERROR line) | Expanding a row changes layout height; capture must include the expanded JSON panel |
| 8 | `08-api-key-secret` | `/api-keys?new=1` | Element: `[data-testid="api-key-reveal-dialog"]` | Type `Docs screenshot bot` in **Name** → click **Create key** | Transient one-time state reached through a chained action; secret value is random, so the capture is intentionally non-deterministic (mask or accept) |
| 9 | `09-delete-project-confirmation` | `/projects/atlas-web?tab=settings` | Element: `[data-testid="delete-project-dialog"]` | Scroll to **Danger zone** → click `[data-testid="delete-project"]` → type `atlas-web` in the confirm input (the red button becomes enabled) — **do not click it** | Destructive flow three levels deep; automation must stop before the irreversible step |
| 10 | `10-command-palette` | any page, e.g. `/deployments` | Element: `[role="dialog"]` containing the `cmdk` list | Press `Meta+K` (macOS) / `Control+K` → type `atlas` | Keyboard-shortcut-triggered overlay with a filtered list |
| 11 | `11-project-empty-deployments` | `/projects/sandbox-playground?tab=deployments` | Full page | None | Empty state (project with no deployments) |
| 12 | `12-usage-chart-dark` | `/usage` | Element: the card containing `[data-testid="usage-chart"]` | Click `[data-testid="theme-toggle"]` → wait ≥1.5s for the chart animation | Dark theme switch + waiting for an animated chart to settle |

## Optional extras

Not referenced by the docs yet, but the app supports them if you want more cases:

- **Mobile viewport** (390×844) on `/` — the sidebar collapses into a sheet behind the top-left trigger.
- **Hover tooltip** on `/projects/atlas-web?tab=domains` — hover the amber "Pending DNS" text.
- **Toast** — click **Deploy** on any project page; a toast appears bottom-right for ~4s.
- **Building state** — `/deployments/dpl_5c9x2p` shows a live spinner in the pipeline.
- **Paginated table page 2** — `/deployments`, click the next-page chevron.
- **Alert banner** — `/projects/ingest-pipeline` shows a destructive "Production is down" alert.

## File naming

Save captures as `images/screenshots/<id>.png`. Each MDX page already references that path.

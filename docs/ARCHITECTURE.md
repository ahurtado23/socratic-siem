# Architecture

`index.html` is the entire application — one file, no build step. This doc explains how it's put together so it's navigable without reading all ~2,500 lines top to bottom.

## Page system

Each top-level view is a `<div class="page" id="page-{name}">`. Only one page has the `active` class at a time; everything else is `display:none` via CSS. Navigation is handled by a single `go(pageName, navEl)` function that:

1. Removes `active` from every `.page` and every `.nav-item`
2. Adds `active` to the target page and the clicked nav item
3. Updates the breadcrumb text from a `titles` lookup map

There's no router, no history API — it's intentionally as simple as a tab switcher.

## View toggles

Two pages (Connectors, Access Control) support switching between "realistic SIEM feature" content and the original presentation content, using a shared `.conn-view-toggle` / `.conn-view-btn` pattern. Clicking a toggle button shows/hides two sibling `<div>`s within the same page.

## Modals

Connector details, Add Source, user profiles, and rule details all reuse one modal pattern:

```html
<div class="conn-modal-overlay" onclick="if(event.target===this) closeXModal()">
  <div class="conn-modal">...</div>
</div>
```

Clicking the dimmed backdrop closes the modal; clicking inside it doesn't (because the click target check only matches the overlay itself).

## Data-driven rendering

Content for the Connectors, Rule Library, and Access Control pages lives in plain JS arrays of objects near the bottom of the `<script>` block:

- `connectorData` — data sources shown on the Connectors page
- `ruleData` — detection rules shown on the Rule Library page (see [DETECTION-LOGIC.md](DETECTION-LOGIC.md))
- `rbacRoleInfo`, `rbacPerms`, `rbacUsers` — roles/permissions/users on the Access Control page

Each is rendered with `array.map(...).join('')` into a container's `innerHTML`. To add or edit an entry, edit the array — the render functions don't need to change.

## Charts

All charts are inline SVG (sparklines, donut/pie via `stroke-dasharray`/`stroke-dashoffset`, bar/area fills) — there's no charting library. This keeps the file dependency-free at the cost of writing SVG math by hand.

## Images

Any embedded image (logos, headshot, poster art) is inlined as a base64 `data:` URI directly in the HTML, so the whole site stays a single portable file with no external asset requests.

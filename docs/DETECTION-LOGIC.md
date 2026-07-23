# Detection Logic — Rule Library data model

The Rule Library page renders from a single JS array, `ruleData`, defined in `index.html`. Each entry is a self-contained detection rule with everything needed to render the list row, the severity/status badges, and all four tabs of the detail modal (Overview, Playbook, Rule Logic, Change History).

## Fields

| Field | Purpose |
|---|---|
| `id` | Unique slug, used as the DOM/lookup key |
| `name` | Rule title shown in the list and modal header |
| `sub` | Category · subtype label (e.g. `Identity · Adaptive`) |
| `sev` | Severity — drives the color of the severity badge (`Critical`/`High`/`Medium`/`Low`) |
| `status` | `enabled` / `disabled` — drives the status dot |
| `created` / `modified` | Display dates |
| `type` | Rule type key, mapped to a display label via `ruleTypeLabel` |
| `color` / `ic` | Accent color and 2-letter monogram for the rule's icon |
| `desc` | Overview tab body copy |
| `source` | Originating log source, e.g. `{name:'Okta System Log'}` |
| `mitre` | Array of MITRE ATT&CK tactic labels the rule maps to |
| `playbook` | HTML string — the analyst response steps, shown in the Playbook tab |
| `logic` | Plain-text pseudocode of the actual detection condition, shown in the Rule Logic tab |
| `history` | Array of strings, most recent first — shown in the Change History tab |
| `cases` | Linked case count (currently illustrative) |

## Example entry

```js
{
  id:'okta-impossible-travel',
  name:'Impossible Travel — Okta Sign in',
  sub:'Identity · Adaptive',
  sev:'High',
  status:'enabled',
  created:'Jan 14, 2026',
  modified:'Jun 20, 2026',
  type:'adaptive',
  source:{name:'Okta System Log'},
  mitre:[{label:'Initial Access'},{label:'Defense Evasion'}],
  playbook:'1) Confirm the two sign in locations against the user\'s travel calendar...',
  logic:'WHEN okta.signin.success\n  AND distance(loc_a, loc_b) / time_delta > 800 km/h\n  AND asn_reputation(loc_b) != "trusted"\nTHEN emit_finding(severity="high")',
  history:['Jun 20, 2026 — Tuned distance threshold from 900 km/h to 800 km/h (fewer false positives on VPN exits)', '...']
}
```

This shape mirrors how a real detection engineering team tends to document a rule: what fires it (`logic`), what a human does about it (`playbook`), what it maps to in ATT&CK (`mitre`), and its tuning history (`history`) — so a rule library reads less like a list of alert names and more like an operational runbook.

## Adding a rule

Add a new object to the `ruleData` array in `index.html`. `renderRuleRows()` and `openRuleModal()` both read from this array directly — no other code needs to change.

## Note on realism

These are illustrative sample rules for a presentation, not production detection content — the thresholds, sources, and playbooks are representative examples of what a mature rule library looks like, not tuned detections from a live environment.

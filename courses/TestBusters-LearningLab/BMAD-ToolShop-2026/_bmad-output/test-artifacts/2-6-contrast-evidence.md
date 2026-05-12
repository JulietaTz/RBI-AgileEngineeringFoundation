# Story 2.6 Contrast Evidence

Date: 2026-04-20

## Token Pairs and Ratios

| Surface | Foreground | Background | Contrast Ratio | WCAG AA Text (4.5:1) | Result |
|---|---|---|---:|---|---|
| Guided primary CTA | `#ffffff` | `#0b5ed7` | 5.30:1 | Pass | Pass |
| Guided AI accent CTA | `#ffffff` | `#5b21b6` | 8.64:1 | Pass | Pass |
| Guided secondary CTA | `#0f172a` | `#ffffff` | 17.86:1 | Pass | Pass |
| Secondary border vs background (non-text affordance) | `#64748b` | `#ffffff` | 4.76:1 | Meets 3:1 UI component contrast | Pass |

## Method

- Ratios calculated using WCAG relative luminance formula for sRGB.
- Checked token mappings introduced for guided surfaces:
  - `--guided-primary-*`
  - `--guided-secondary-*`
  - `--guided-ai-accent-*`

## Outcome

- All text-bearing CTA token pairs meet or exceed WCAG 2.1 AA contrast requirement.
- AI accent remains scoped to contextual action (`More like this`) and is not applied to all CTAs, preserving hierarchy.

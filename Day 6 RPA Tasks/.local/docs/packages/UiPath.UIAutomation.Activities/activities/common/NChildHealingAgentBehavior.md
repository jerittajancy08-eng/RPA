# NChildHealingAgentBehavior

`UiPath.UIAutomationNext.Enums.NChildHealingAgentBehavior`

Configures the Healing Agent actions on activities inside a **Use Application/Browser** scope. Adds the `SameAsCard` option (compared to [`NHealingAgentBehavior`](NHealingAgentBehavior.md)) so child activities can inherit the parent card's setting.

## Values

| Value | Description |
|-------|-------------|
| `NChildHealingAgentBehavior.SameAsCard` | Use the setting from the parent Application/Browser activity. |
| `NChildHealingAgentBehavior.Job` | Use the Governance or Orchestrator process/job/trigger level settings. |
| `NChildHealingAgentBehavior.Disabled` | Disable Healing Agent for this activity. |
| `NChildHealingAgentBehavior.RecommendationOnly` | Allow Healing Agent to provide fix recommendations only if allowed by Governance or Orchestrator process/job/trigger level settings. |

## Usage

Reference values as `NChildHealingAgentBehavior.<Value>`, e.g. `NChildHealingAgentBehavior.SameAsCard`.

# NHealingAgentBehavior

`UiPath.UIAutomationNext.Enums.NHealingAgentBehavior`

Configures the Healing Agent actions on **Use Application/Browser**. For child activities, see [`NChildHealingAgentBehavior`](NChildHealingAgentBehavior.md), which adds a `SameAsCard` value.

## Values

| Value | Description |
|-------|-------------|
| `NHealingAgentBehavior.Job` | Use the Governance or Orchestrator process/job/trigger level settings. |
| `NHealingAgentBehavior.Disabled` | Disable Healing Agent for this activity. |
| `NHealingAgentBehavior.RecommendationOnly` | Allow Healing Agent to provide fix recommendations only if allowed by Governance or Orchestrator process/job/trigger level settings. |

## Usage

Reference values as `NHealingAgentBehavior.<Value>`, e.g. `NHealingAgentBehavior.Job`.

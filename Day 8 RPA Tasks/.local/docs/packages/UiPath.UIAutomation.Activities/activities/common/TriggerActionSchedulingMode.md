# TriggerActionSchedulingMode

`UiPath.Platform.Triggers.TriggerActionSchedulingMode`

How a trigger handler schedules events.

## Values

| Value | Description |
|-------|-------------|
| `TriggerActionSchedulingMode.Sequential` | Processes events in order, non-concurrently. |
| `TriggerActionSchedulingMode.Concurrent` | Processes events concurrently. |
| `TriggerActionSchedulingMode.OneTime` | Processes an event only once. |
| `TriggerActionSchedulingMode.SequentialCollapse` | Processes events in order, but collapses all events in the queue except the latest one. |
| `TriggerActionSchedulingMode.SequentialDrop` | Same as `SequentialCollapse`, but also cancels any currently running event. |

## Usage

Reference values as `TriggerActionSchedulingMode.<Value>`, e.g. `TriggerActionSchedulingMode.Sequential`.

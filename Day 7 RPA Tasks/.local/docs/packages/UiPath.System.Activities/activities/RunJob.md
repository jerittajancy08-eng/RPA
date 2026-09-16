# Run Job

`UiPath.Activities.System.Jobs.RunJob`

Initiates any type of job (Agent, Agentic process, RPA workflows, API workflow, Testing process, etc.) via Orchestrator to be picked up by any available robots. It allows for flexible configuration of the execution mode (Do not wait, Wait for job completion or Suspend execution until job completion). It returns output arguments unless started in the Do not wait mode.

**Package:** `UiPath.System.Activities`
**Category:** Run

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `ProcessName` | Process name | InArgument | `string` | Yes | — | The name of the Orchestrator process to start. Supports auto-complete from available processes in the configured folder. |
| `Machine` | Machine | InArgument | `string` | No | — | The machine on which the job will execute. If not specified, any available machine is used. |
| `Account` | Account | InArgument | `string` | No | — | The account used to execute the job. If not specified, the default account associated with the machine is used. |
| `EntryPointPath` | Entry point path | InArgument | `string` | No | — | Override the default entry-point workflow of the target process (e.g. `Main.xaml` or a coded entry point). Leave empty to use the process's configured entry point. |
| `Priority` | Priority | InArgument | `JobPriorityV2` | No | `Inherited` | The priority the job is started with. `Inherited` (default) uses the priority configured on the target process. Granular values map to Orchestrator's `SpecificPriorityValue`; on Orchestrators without granular priority support the value is down-converted to Low/Normal/High. In raw XAML use the enum name, e.g. `Priority="High"`. |
| `TimeoutMS` | Timeout MS | InArgument | `int` | No | `600000` (10 min) | Maximum time in milliseconds to wait for the job to complete. Only applicable when `ExecutionMode` is `Busy` (Wait for job completion). Read-only for other modes. |
| `ContinueOnError` | Continue On Error | InArgument | `bool` | No | `false` | When `True`, execution continues even if this activity encounters an error. Read-only when `ExecutionMode` is `None`. |
| `Input` | Input | InArgument (untyped) | dynamic | No | null | Single-object input mode. The variable bound here is serialized to JSON and passed as the process's input. Type is determined by Studio when the target process's schema is imported; see **Authoring `Input` / `Output` in raw XAML** below. |
| `Arguments` | Arguments | `Dictionary<string, Argument>` | dynamic | No | empty | Dictionary mode — per-argument bindings keyed by the target process's argument names. Mixed `InArgument` / `OutArgument` entries are allowed; `OutArgument` entries capture top-level output properties. This is the **only Input/Output binding that is straightforward to author in raw XAML.** |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ExecutionMode` | Execution mode | `JobExecutionMode` | `JobExecutionMode.Busy` | Determines how the initiated job is processed. See Valid Configurations below. |
| `FailWhenFaulted` | Continue when job fails | `bool` | `true` (i.e., fail when faulted) | When `True`, a faulted job causes this activity to throw an error. When `False`, a faulted job is ignored and execution continues. Designer label shows the inverse semantics ("Continue when job fails" — checked = `FailWhenFaulted=False`). Read-only when `ExecutionMode` is `None`. |
| `FolderPath` | Folder Path | `string` | — | The Orchestrator folder containing the process. Leave empty to use the folder of the current process. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Job` | Job data | OutArgument | `OrchestratorJob` (`UiPath.Core.Activities`) | An object containing information about the initiated job (e.g., Job ID, status, output arguments). Not populated when `ExecutionMode` is `None`. |
| `Output` | Output | OutArgument (untyped) | dynamic | Single-object output mode. The variable bound here is populated by deserializing the process's `OutputArguments` JSON. Type is determined by Studio when the target process's schema is imported; see **Authoring `Input` / `Output` in raw XAML** below. Not populated when `ExecutionMode` is `None`. |

## Valid Configurations

The behavior and available properties depend on the `ExecutionMode` value:

| `ExecutionMode` | Behavior | `TimeoutMS` | `ContinueOnError` | `FailWhenFaulted` | Output |
|---|---|---|---|---|---|
| `None` (Do not wait) | Starts the job and returns immediately without waiting. | Read-only | Read-only | Read-only | `Job` is populated with basic job info; no output arguments. |
| `Busy` (Wait for job completion) | Starts the job and blocks the current robot until the job completes or times out. | Editable | Editable | Editable | `Job` is fully populated including output arguments. |
| `Suspend` (Suspend until job completion) | Starts the job and suspends the current workflow (persistence required). Resumes when the job completes. | Read-only | Editable | Editable | `Job` is fully populated including output arguments on resume. |

### Enum Reference

**`JobExecutionMode`**

| Value | Description |
|-------|-------------|
| `None` | Do not wait — fire and forget. |
| `Busy` | Wait for job completion by blocking the robot thread. |
| `Suspend` | Suspend the workflow until the job completes (requires persistence infrastructure). |

**`JobPriorityV2`** (namespace `UiPath.Activities.System.Jobs`)

| Value | Numeric value |
|-------|---------------|
| `Inherited` | 0 (use the target process's configured priority) |
| `Lowest` | 5 |
| `VeryLow` | 15 |
| `Low` | 25 |
| `MediumLow` | 35 |
| `Medium` | 45 |
| `MediumHigh` | 55 |
| `High` | 65 |
| `VeryHigh` | 75 |
| `Highest` | 85 |
| `Critical` | 95 |

## XAML Example

```xml
<!-- Wait for job completion -->
<ui:RunJob
    xmlns:ui="clr-namespace:UiPath.Activities.System.Jobs;assembly=UiPath.System.Activities"
    DisplayName="Run Job"
    ProcessName="InvoiceProcessor"
    ExecutionMode="Busy"
    TimeoutMS="[600000]"
    FailWhenFaulted="True"
    Job="[jobData]" />
```

```xml
<!-- Fire and forget -->
<ui:RunJob
    xmlns:ui="clr-namespace:UiPath.Activities.System.Jobs;assembly=UiPath.System.Activities"
    DisplayName="Run Job"
    ProcessName="BackgroundReporter"
    ExecutionMode="None" />
```

Dictionary-mode arguments — pass typed inputs and capture typed outputs by argument name:

```xml
<ui:RunJob
    xmlns:ui="clr-namespace:UiPath.Activities.System.Jobs;assembly=UiPath.System.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
    DisplayName="Run Job"
    ProcessName="InvoiceProcessor"
    ExecutionMode="Busy"
    Job="[jobData]">
  <ui:RunJob.Arguments>
    <scg:Dictionary x:TypeArguments="x:String, Argument">
      <InArgument x:TypeArguments="x:String" x:Key="in_InvoiceId">[invoiceId]</InArgument>
      <InArgument x:TypeArguments="x:Double" x:Key="in_Amount">[amount]</InArgument>
      <OutArgument x:TypeArguments="x:String" x:Key="out_Status">[processStatus]</OutArgument>
      <OutArgument x:TypeArguments="x:Boolean" x:Key="out_Approved">[wasApproved]</OutArgument>
    </scg:Dictionary>
  </ui:RunJob.Arguments>
</ui:RunJob>
```

## Coded Workflow Example

In coded workflows, `RunJob` is exposed as `system.RunJob(...)` / `system.RunJobAsync(...)`. The priority is set through `RunJobOptionalParameters.Priority`:

```csharp
using UiPath.Activities.System.Jobs;   // JobPriorityV2 — NOT auto-imported (only the .Jobs.Coded namespace is)

[Workflow]
public void Execute()
{
    var opts = new RunJobOptionalParameters
    {
        Priority = JobPriorityV2.High,
        FailWhenJobFails = true
    };

    var (jobData, outputJson) = system.RunJob(
        processName: "InvoiceProcessor",
        orchestratorFolderPath: "Finance/Invoices",
        inputArguments: new { InvoiceId = invoiceId },
        runJobOptionalParameters: opts
    );

    Log($"Job {jobData.Id} completed with priority {opts.Priority}. Output: {outputJson}");
}
```

Leaving `Priority` unset (or setting it to `JobPriorityV2.Inherited`, the default) starts the job with the priority configured on the target process. See the package's coded API documentation (`docs/coded/coded-api.md`) for the full `RunJob` signature and the other `RunJobOptionalParameters` members.

## Authoring `Input` / `Output` in raw XAML

`RunJob` exposes three mutually-exclusive ways to pass arguments to and from the triggered process. They look the same in Studio (selectable via the designer's mode menu) but serialize very differently:

| Mode | Property/properties used | Authorable in raw XAML? | Notes |
|------|--------------------------|-------------------------|-------|
| **Dictionary** | `Arguments` (`Dictionary<string, Argument>`) | **Yes** — see the example above. | Per-argument `InArgument`/`OutArgument` entries keyed by the target process's argument names. Each entry carries its own `x:TypeArguments`. |
| **Data Mapper** | `Input` + `Output` (both untyped `Argument`) | Only with effort — requires knowing the target process's argument schema and binding a matching typed variable. Studio normally fills this in when you point at a process. | The `Input` and `Output` properties are marked `[Browsable(false)]` on the activity. They can still be serialized; their effective type is taken from the bound variable. Without Studio's schema import, you must know the target process's argument shape and declare a typed variable of the same shape. |
| **Object** | `Input` *or* `Output` (single object, untyped) | Same as Data Mapper, with the constraint that the variable type matches the schema-implied object class for the process. | Same caveat — no auto-discovery without Studio. |

**Recommendation for coding agents authoring without Studio:** use the **Dictionary** mode (`Arguments`). It's the only mode whose XAML form is fully self-describing — type info travels inline with each argument, and no external schema is required.

## Notes

- Requires an active Orchestrator connection. The process must be published and available in the specified (or current) Orchestrator folder.
- **Suspend mode** requires a persistence-enabled environment. The calling workflow is suspended and resumed by the Orchestrator persistence infrastructure once the triggered job finishes. This allows the calling robot to be freed while waiting.
- Input and output arguments for the triggered process are configured via the dynamic **Arguments** / **Input** / **Output** properties in the designer. The designer auto-imports the argument schema from the selected process when `ProcessName` is set to a literal string. See **Authoring `Input` / `Output` in raw XAML** above for the three modes and their raw-XAML feasibility.
- The XAML-serialized property is `FailWhenFaulted` (default `True`). The Studio designer displays it under the inverted label **"Continue when job fails"** — checking that designer box sets `FailWhenFaulted = False`. Always use `FailWhenFaulted` as the attribute name in raw XAML.
- The `Job` output is of type `UiPath.Core.Activities.OrchestratorJob` — when binding from XAML, declare its namespace with `xmlns:uca="clr-namespace:UiPath.Core.Activities;assembly=UiPath.System.Activities"` and use `Variable x:TypeArguments="uca:OrchestratorJob"` for the receiving variable.
- `Machine` and `Account` support auto-complete from Orchestrator-connected data sources. Leave empty to allow Orchestrator to assign automatically.
- `FolderPath` supports Orchestrator modern folder paths (e.g., `Finance/Invoices`).

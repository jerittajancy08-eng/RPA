# Set Asset

`UiPath.Core.Activities.SetAsset`

Updates the value of an indicated asset, that is already available in Orchestrator, be it a global or a Per Robot asset.

**Package:** `UiPath.System.Activities`
**Category:** Assets

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `AssetName` | Asset Name | InArgument | `string` | Yes | — | The name of the Orchestrator asset to update. |
| `Value` | Value | InArgument | `object` | Exactly one of `Value`/`JsonValue` | — | The new value to assign to the asset. Must be compatible with the asset's type configured in Orchestrator (text, integer, boolean, or JSON). For a JSON asset, provide either a `String` holding JSON text or an already-typed variable (e.g. `JObject`/`JToken`). |
| `JsonValue` | JSON Content | InArgument | `string` | Exactly one of `Value`/`JsonValue` | — | The new value of the asset, in JSON format. Either type the JSON directly in the code editor, or provide a String variable that holds it. Only meaningful for JSON-typed assets. |

## XAML Example

```xml
<ui:SetAsset
    xmlns:ui="clr-namespace:UiPath.Core.Activities;assembly=UiPath.System.Activities"
    DisplayName="Set Asset"
    AssetName="LastRunTimestamp"
    Value="[DateTime.Now.ToString()]" />
```

Writing a JSON asset through the JSON editor argument (the leading `{}` escapes the `{` so XAML does not parse it as a markup extension):

```xml
<ui:SetAsset
    xmlns:ui="clr-namespace:UiPath.Core.Activities;assembly=UiPath.System.Activities"
    DisplayName="Set Asset"
    AssetName="RetryPolicy"
    JsonValue="{}{&quot;retries&quot;: 3, &quot;mode&quot;: &quot;linear&quot;}" />
```

## Notes

- Requires an active Orchestrator connection. The asset must already exist in Orchestrator; this activity updates an existing asset's value and cannot create new assets.
- The asset type in Orchestrator determines which value types are accepted. Passing a value of the wrong type will cause a runtime error.
- `Value` and `JsonValue` are separate overload groups: exactly one of them must be set. Setting both, or neither, fails workflow validation.
- For a JSON asset, the value must be well-formed JSON — malformed JSON fails locally, before anything is sent to Orchestrator. A well-formed value that violates the JSON schema configured on the asset is rejected by Orchestrator and surfaced as an error.
- A JSON asset stores plain text, so it cannot hold secrets: a `Credential` or `SecureString` is rejected locally, including when it is nested anywhere inside the value being serialized.
- For **Per Robot** assets, the asset must be assigned to the robot running the process.
- To update a credential asset (username and password), use **Set Credential** instead.
- To update a secret asset, use **Set Secret** instead.
- `FolderPath` is available via the Orchestrator connection context (inherited from the base activity class) and can be configured in the activity's Orchestrator scope.

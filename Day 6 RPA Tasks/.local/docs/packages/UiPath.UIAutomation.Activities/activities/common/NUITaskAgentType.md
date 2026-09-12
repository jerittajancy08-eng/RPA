# NUITaskAgentType

`UiPath.UIAutomationNext.Shared.Enums.NUITaskAgentType`

The agent / model used by the **ScreenPlay** activity.

## Values

| Value | Description |
|-------|-------------|
| `NUITaskAgentType.DOMBased` | Works best on browsers. Uses a proprietary implementation based on the page's DOM, using Gemini Flash for reasoning and image understanding. Moderately fast. An order of magnitude cheaper than the others. |
| `NUITaskAgentType.GeminiFlash25` | Basic model. Works best on browsers. Uses a proprietary implementation based on the page's DOM, using Gemini Flash for reasoning and image understanding. Moderately fast. |
| `NUITaskAgentType.DOMBasedGPT41` | Standard model — for complex tasks. Works best on browsers. Uses a proprietary implementation based on the page's DOM and image understanding, using GPT-4.1 for reasoning. Not very fast. |
| `NUITaskAgentType.DOMBasedGPT41Mini` | Basic model — faster, cheaper. Works best on browsers. Uses a proprietary implementation based on the page's DOM and image understanding, using GPT-4.1 mini for reasoning. Moderately fast. |
| `NUITaskAgentType.GPT5` | Standard model — for complex tasks. Works best on browsers. Uses a proprietary implementation based on the page's DOM and image understanding, using GPT-5 for reasoning. Slow. |
| `NUITaskAgentType.GPT5Mini` | Basic model — faster, cheaper. Works best on browsers. Uses a proprietary implementation based on the page's DOM and image understanding, using GPT-5 mini for reasoning. Moderately fast. |
| `NUITaskAgentType.OpenAIOperator` | Standard model — for complex tasks. Works on any type of application, including image-based interfaces. Uses OpenAI Operator, an image-based reasoning model. Slow. |
| `NUITaskAgentType.AnthropicClaudeCU` | Standard model — for complex tasks. Works on any type of application, including image-based interfaces. Uses Anthropic Computer Use, an image-based reasoning model. Slow. |
| `NUITaskAgentType.UiPathComputerUse` | Standard model — for complex tasks. Works on any type of application, including image-based interfaces. Uses UiPath Computer Use, an image-based reasoning model. Fast. |
| `NUITaskAgentType.GeminiFlash3Preview` | Basic model. Works best on browsers. Uses a proprietary implementation based on the page's DOM, using Gemini Flash for reasoning and image understanding. Moderately fast. |

## Usage

Reference values as `NUITaskAgentType.<Value>`, e.g. `NUITaskAgentType.DOMBased`.

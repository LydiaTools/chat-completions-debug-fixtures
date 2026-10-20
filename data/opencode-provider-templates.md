# OpenCode OpenAI-compatible provider templates

These are separate, copyable `opencode.json` examples. Pick the schema documented for the OpenCode version you use; do not combine fields between them. Replace the example URL, provider ID, environment variable and model IDs with values from your API provider. The examples contain no real credentials.

## Standard provider documentation

Official source: https://opencode.ai/docs/providers/
Source text: “You can customize the providers through the `provider` section in your OpenCode config.”

Copy this complete JSON object into your `opencode.json` and add the credential through `/connect`.

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "myprovider": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "My Provider",
      "options": {
        "baseURL": "https://api.example.com/v1"
      },
      "models": {
        "MODEL_ID_FROM_PROVIDER": {
          "name": "My model"
        }
      }
    }
  }
}
```

## Versioned v2 provider documentation

Official source: https://opencode.ai/v2/docs/providers/
Source text: “The map key is the model ID used by OpenCode; `modelID` changes the ID sent to the provider.”

Copy this complete JSON object into your `opencode.json`. The `my-model` key is the OpenCode selection ID; `modelID` is the provider's model ID.

```json
{
  "$schema": "https://opencode.ai/config.json",
  "providers": {
    "myprovider": {
      "name": "My Provider",
      "env": ["MY_PROVIDER_API_KEY"],
      "package": "@opencode/ai/providers/openai-compatible",
      "settings": {
        "baseURL": "https://api.example.com/v1"
      },
      "models": {
        "my-model": {
          "name": "My model",
          "modelID": "MODEL_ID_FROM_PROVIDER"
        }
      }
    }
  }
}
```

Never commit a real API key. Follow the credential instructions for the matching OpenCode documentation and check the installed version's current provider package before using a template.

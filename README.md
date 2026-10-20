# Chat Completions Debug Fixtures

A small, reproducible corpus for testing local parsers, API clients, and error-reporting workflows around OpenAI-compatible Chat Completions. It contains **24 synthetic test cases**: 11 JSON/SSE response captures and 13 request-body boundary cases. The cases were copied from an existing browser self-test and preserve the original inputs, expected outcomes, and SHA-256 digests.

No API key, user data, live provider response, or network request is included in the fixtures. The request-rules table describes one site's current gateway behavior; its limits are not OpenAI platform limits. Check the implementation source and official API documentation before applying the table to another service.

## Files

- `data/chat-completions-debug-fixtures.json` — all 24 inputs with category, expectations, and SHA-256.
- `data/api-error-symptom-index.csv` — 18 error symptoms, their exact source passages, source URLs, and diagnostic notes.
- `data/chat-completions-request-rules.csv` — request fields and current gateway validation boundaries.
- `data/opencode-provider-templates.md` — copyable OpenCode standard and v2 OpenAI-compatible provider templates, labeled by documentation generation.

## Verify the fixture bundle

The JSON is UTF-8. Each case's `input` is the exact text submitted to the browser-only test; preserve SSE blank lines and final line endings. To verify a case digest in Python:

```python
import hashlib, json
from pathlib import Path
manifest = json.loads(Path("data/chat-completions-debug-fixtures.json").read_text())
for case in manifest["cases"]:
    actual = hashlib.sha256(case["input"].encode("utf-8")).hexdigest()
    assert actual == case["sha256"], case["fixture_file"]
print(f"verified {len(manifest['cases'])} fixtures")
```

## Interpretation boundaries

- A parseable JSON body does not establish factual correctness or completeness. Read the finish reason and requested-field checks separately.
- A captured SSE stream without its terminal markers may be an incomplete capture; it does not by itself prove the provider stopped early.
- Synthetic examples demonstrate parser cases. They do not prove a provider currently emits every shown sequence. Provider-specific envelopes can differ.
- The CSV symptom quotes are linked to the named source page; check the live source before treating wording as current.
- The OpenCode examples are version-labeled templates, not a claim that all OpenCode installs share a single schema.

## License

No license file is provided. Public visibility does not grant permission to reuse this material; contact the repository owner for permission.

## References

- [OpenAI Chat Completions API reference](https://developers.openai.com/api/reference/resources/chat/subresources/completions/methods/create)
- [OpenAI streaming events reference](https://developers.openai.com/api/reference/resources/chat/subresources/completions/streaming-events#chat.completion.chunk)
- [MDN: Using server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events#event_stream_format)
- [OpenRouter: handling rate-limit errors in streaming](https://openrouter.ai/docs/api_reference/limits#handling-429-errors)
- [OpenCode standard provider documentation](https://opencode.ai/docs/providers/)
- [OpenCode v2 provider documentation](https://opencode.ai/v2/docs/providers/)

For the browser-only companion checker and the gateway's current connection instructions, see [ChinaGenAI's API connection guide](https://chinagenai.com/connect).

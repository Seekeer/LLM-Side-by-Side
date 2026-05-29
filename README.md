# LLM Model Response Diff Tool

A single-file browser tool for comparing responses from multiple Large Language Models side by side. It is useful when you want to test the same prompt against several OpenAI-compatible models, compare latency and token usage, and inspect response differences without adding a backend service.

<img width="1440" height="1200" alt="configuration" src="https://github.com/user-attachments/assets/29b6326b-f350-4253-a590-2b9ded60e4f0" />
<img width="1440" height="1200" alt="reference-comparison" src="https://github.com/user-attachments/assets/4b72adcf-1aab-4b4c-b56e-940b2003814f" />

## What Changed In This Fork

- Compare any number of configured models, not just two.
- Add model cards one by one with the `+` button.
- Bulk-add model names from a newline-separated or comma-separated list.
- Export and import the full working setup as JSON.
- Send an optional system prompt together with the user prompt.
- Compare model outputs against a manually entered reference answer.
- Mark latency and token metrics as better or worse than reference values.
- Keep per-model API failures visible instead of hiding all results when one model fails.
- Use OpenRouter examples in the docs and UI placeholders.

## Features

- **Multi-model comparison**: run the same prompt against one or more configured models.
- **Side-by-side response view**: inspect every model response in separate result cards.
- **Reference comparison**: paste a baseline answer and compare every model against it.
- **Metric tracking**: display response time, prompt tokens, completion tokens, total tokens, and optional cost values returned by the API.
- **Difference highlighting**: highlight word-level differences between model output and the reference answer.
- **Settings import/export**: save endpoint, key, model, prompt, reference, and highlighting settings to JSON.
- **OpenAI-compatible API support**: works with endpoints that accept the chat completions request shape and Bearer token authorization.
- **No backend required**: the app is a standalone HTML file that runs in the browser.

## Quick Start

Open `llm_diff_tool.html` in a browser.

If your browser blocks API calls or file features from a local file, serve the folder locally:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/llm_diff_tool.html
```

## Usage

1. Configure the first model card:
   - Endpoint, for example `https://openrouter.ai/api/v1/chat/completions`
   - API key
   - Model name, for example `openai/gpt-4o-mini`

2. Add more models:
   - Click `+` to duplicate the previous card, then edit the model name.
   - Or paste multiple model names into `Model Names` and click `Add listed models`.

3. Enter prompts:
   - `System Prompt` is optional.
   - `User Prompt` is required.

4. Click `Compare Responses`.

5. Optionally fill the `Reference` card in the results area:
   - Response time and token counts become comparison baselines.
   - Reference response is used for highlighted output differences.
   - Click `Compare` in the reference card to re-render the comparison.

## Settings JSON

`Export settings` saves a JSON file with:

- Model endpoint, API key, and model name for each configured model.
- Reference metrics and reference response.
- System prompt and user prompt.
- Difference-highlighting state.

Treat exported settings files as sensitive because they can contain API keys.

## Supported APIs

The request payload uses the OpenAI chat completions shape:

```json
{
  "model": "model-name",
  "messages": [
    { "role": "system", "content": "optional system prompt" },
    { "role": "user", "content": "user prompt" }
  ]
}
```

The tool sends `Authorization: Bearer <api-key>` and parses OpenAI-style `choices[0].message.content`. It also understands Anthropic-style text and token fields when an endpoint or proxy returns them in the response.

## Privacy Notes

- There is no server component in this repository.
- Requests are sent directly from your browser to the endpoints you configure.
- The app does not persist settings automatically.
- Exported settings files may include API keys, so store or share them carefully.

## Troubleshooting

**CORS errors**

Some providers block browser-origin requests. Use a provider endpoint or local proxy that allows browser requests.

**`Please fill in all fields`**

Every configured model must have endpoint, API key, and model name values, and the user prompt must not be empty.

**One model fails**

The failed model is shown as an error card while other model results can still render.

**Unexpected response format**

Check that the endpoint returns either OpenAI-compatible chat completions JSON or a compatible text response shape.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

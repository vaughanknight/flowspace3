# Azure resource plan — flowspace3 providers (DRAFT, awaiting Vaughan's go)
o-prime pij-straightforward-walrus · 2026-11-14 · nothing created yet (read-only checks only)

## Target
- Subscription `MCAPS-Hybrid-REQ-177459-2026-vknight` (d071d89d-8f6a-46bc-ab03-148da31a975c), tenant 16b3c013…, Enabled.
- `Microsoft.CognitiveServices` provider: **NotRegistered** → step 1 registers it (free).
- No resource groups exist yet.

## Resources (all in australiaeast; every model below verified available there)
| # | Resource | Name | SKU / settings |
|---|---|---|---|
| 1 | Resource group | `rg-flowspace3-vk` | australiaeast |
| 2 | Azure OpenAI account | `aoai-fs3-vk` (custom subdomain) | kind OpenAI, S0, `disableLocalAuth=true` (no keys) |
| 3 | Deployment `text-embedding-3-small` | model text-embedding-3-small v1 | Standard (region-local), ~120K TPM; fs3 config `dimensions = 1024` |
| 4 | Deployment `gpt-5-mini` | model gpt-5-mini 2025-08-07 | GlobalStandard, ~100K TPM |
| 5 | Role assignment | Cognitive Services OpenAI User → vknight@microsoft.com, scope = the account | Entra auth via `az login` |

Alternative for #4: `gpt-5.4-mini` (2026-03-17, the model upstream tested) at 3x the price.

## Cost (pay-per-token; **$0 when idle**, no hourly charge for Standard/GlobalStandard)
| Model | Price per 1M tokens |
|---|---|
| text-embedding-3-small | $0.02 |
| gpt-5-mini (Global) | $0.25 in / $2.00 out |
| gpt-5.4-mini (Global) | $0.75 in / $4.50 out |

Estimate for today's two roots (dodo-gogo 159 files + live-transcripts 335 files, ~5k elements):
- Embeddings, full index: ~2.5M tokens → **~$0.05**.
- Summaries, full index: ~7.5M in + ~1.5M out → **~$5 (gpt-5-mini)** or ~$12 (gpt-5.4-mini).
- After that, live edits re-embed/summarise only changed elements: cents per day.
- Note: once the JS grammar ships, dodo-gogo's 59 JS files yield many more elements (functions), so expect that number to grow somewhat.

## Separate go (not part of this one)
Switching the running daemon from fake providers to these (config + daemon restart + one full re-index of both roots).

## Commands (run only after go)
az provider register -n Microsoft.CognitiveServices; az group create; az cognitiveservices account create --custom-domain aoai-fs3-vk --kind OpenAI --sku S0 (then disableLocalAuth); two `az cognitiveservices account deployment create`; `az role assignment create --role "Cognitive Services OpenAI User"`.

## DEPLOYED 2026-11-14 ~15:00 AEDT (Vaughan go "Go, gpt-5-mini", spine 106983)
- Provider registered; `rg-flowspace3-vk` (australiaeast); `aoai-fs3-vk` S0, endpoint https://aoai-fs3-vk.openai.azure.com/, disableLocalAuth=true.
- Deployments: `text-embedding-3-small` (v1, Standard, 120K TPM), `gpt-5-mini` (2025-08-07, GlobalStandard, 100K TPM).
- Role: Cognitive Services OpenAI User → oid d5cf3396… (vknight), scope = account.
- Smoke (Entra token, az login): embeddings api 2024-02-01 dims=1024 → 1024-length vector; chat api 2024-12-01-preview max_completion_tokens → "ok".

## Config for the switch (NOT applied; needs Vaughan's separate go — restarts daemon + full reindex)
```toml
[providers.aoai-embed]
kind = "azure_openai"
endpoint = "https://aoai-fs3-vk.openai.azure.com"
deployment = "text-embedding-3-small"
api_version = "2024-02-01"
dimensions = 1024
[providers.aoai-chat]
kind = "azure_openai"
endpoint = "https://aoai-fs3-vk.openai.azure.com"
deployment = "gpt-5-mini"
api_version = "2024-12-01-preview"
```
(Exact `kind` spelling and how roles bind embedder/summarizer to these names must be checked against `fs3_core::config` and `flowspace3 config show` before applying.)

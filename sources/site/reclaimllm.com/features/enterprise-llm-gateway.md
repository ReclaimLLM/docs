# Source: https://reclaimllm.com/features/enterprise-llm-gateway

Enterprise feature

# Govern LLM API access 
at the proxy layer.

Enterprise admins can issue team-scoped gateway keys, restrict provider access, and log LLM API calls under the right organization and team.

[Talk to enterprise→](https://reclaimllm.com/pricing#enterprise) [Read the release note](https://reclaimllm.com/news/enterprise-llm-gateway)

Control point

## A hosted gateway for governed team traffic.

### Team-scoped gateway keys

Issue a key to a backend team, data team, or product service without exposing the upstream provider secret.

### Provider and model policy

Allow a team to use all configured providers, or narrow access to one provider and a small model set.

### Central provider credentials

Keep OpenAI, Azure OpenAI, Anthropic, and Gemini credentials controlled by the organization.

### Proxy session logging

Store request metadata, response status, model, duration, token counts, and gateway key metadata as proxy sessions.

How it is used

## From provider credentials to auditable team calls.

01

### Configure

An enterprise admin stores provider credentials once for the organization.

02

### Scope

The admin creates gateway keys for teams and chooses which providers or models each key can use.

03

### Route

Applications call the ReclaimLLM gateway URL with the team gateway key instead of a provider key.

04

### Review

Gateway calls are logged under the organization and team for enterprise admins and team leads.

Setup path

## Admins configure policy. Teams use one gateway key.

The team key is not an OpenAI, Anthropic, Gemini, or Azure OpenAI key. It is a ReclaimLLM gateway key that authorizes access according to the organization's configured provider policy.

That keeps provider credentials server-side while still giving developers and services a normal API-shaped integration path.

1\. Open the gateway pageEnterprise admin goes to the organization's LLM Gateway/API keys page.

2\. Add provider credentialsStore the provider credentials the organization wants teams to use.

3\. Create a team keyChoose a team, name the key, and select allowed providers or model patterns.

4\. Update the app base URLPoint the app or SDK at the enterprise gateway URL and use the team gateway key as bearer auth.

5\. Review trafficAdmins and team leads inspect logged proxy requests in enterprise reporting and session views.

Request examples

## Use it like an OpenAI-compatible proxy.

### OpenAI-compatible chat

Use the gateway like an OpenAI-compatible endpoint, with the model name carrying the LiteLLM provider prefix.

```
curl https://reclaimllm.com/acme/v1/chat/completions \
  -H "Authorization: Bearer rclm-gw-your-team-key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "openai/gpt-4o-mini",
    "messages": [
      { "role": "user", "content": "Summarize this incident report." }
    ]
  }'
```

### Embedding request

Embedding traffic can use the same team gateway key, so retrieval and search pipelines follow the same org policy.

```
curl https://reclaimllm.com/acme/v1/embeddings \
  -H "Authorization: Bearer rclm-gw-your-team-key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "azure/text-embedding-small",
    "input": ["Searchable context from an LLM session."]
  }'
```

Logging

## Every gateway call becomes enterprise context.

The gateway is not only a pass-through. It creates proxy session records that enterprise admins and team leads can use for review, investigation, and usage analysis.

IdentityOrganization, team, attributed team lead, and gateway key metadata

RequestEndpoint, method, model, streaming flag, and sanitized request payload metadata

ResponseStatus, duration, input tokens, output tokens, and provider response metadata when available

AccessEnterprise admins and team leads can review proxy requests for their organization context

Current limits

## Built for governed access first, with sharper controls coming next.

### Organization-level provider credentials

Team-owned provider credentials and separate provider billing by team are not part of the first version.

### Gateway reliability is in the request path

The gateway becomes part of customer LLM traffic, so uptime and retry behavior matter more than passive capture paths.

### Provider compatibility still needs real traffic

Provider APIs differ around streaming, token reporting, responses, and embeddings. Feedback from production edge cases will shape the next pass.

## Route team LLM traffic through a governed gateway.

Use team-scoped keys for provider access, then keep request history visible in the same enterprise workspace as the rest of your captured AI activity.

[Contact enterprise →](https://reclaimllm.com/pricing#enterprise) [Back to capture methods](https://reclaimllm.com/features/capture)
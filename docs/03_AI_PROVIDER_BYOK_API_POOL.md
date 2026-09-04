# 03 — AI PROVIDER, BYOK & API POOL SPECIFICATION

## 1. Purpose

Dokumen ini mendefinisikan arsitektur AI infrastructure untuk platform AI Content Intelligence.

Tujuan utama:
- mendukung Admin AI Pool;
- mendukung BYOK (Bring Your Own Key);
- mendukung banyak provider dan banyak credential;
- memungkinkan routing model berdasarkan task;
- menyediakan failover, health checking, rate-limit handling, dan circuit breaker;
- memisahkan credential AI dari credential payment;
- mengontrol quota, token, credit, dan biaya;
- memungkinkan custom provider dan custom Base URL secara aman;
- menjaga agar Analysis Engine tidak bergantung langsung pada satu provider.

AI infrastructure adalah fondasi operasional. Kualitas output tetap menjadi prioritas utama.

## 2. Core Architecture

```text
Analysis Engine
      ↓
AI Service
      ↓
Credential Resolver
      ↓
Provider Router
      ↓
Model Router
      ↓
Provider Adapter
      ↓
AI Provider
```

Business logic tidak boleh memanggil SDK provider secara langsung. Semua provider diakses melalui abstraction layer.

## 3. Credential Sources

### Admin AI Pool

Credential/API key disediakan administrator. Admin dapat memiliki beberapa credential untuk provider yang sama.

### BYOK

Member dapat memasukkan credential miliknya sendiri sesuai plan. Credential harus terenkripsi, dimasking, tidak masuk log, dapat diuji/revoke, dan memiliki health/usage tracking.

## 4. Membership Policy

### Starter — Rp10.000/bulan
- Admin AI Pool;
- 10 analysis/month;
- limited models.

### Pro — Rp75.000/bulan
- larger Admin AI Pool allocation;
- BYOK 1 API;
- more analysis;
- better models;
- priority processing.

### Power — Rp150.000/bulan
- larger Admin AI Pool allocation;
- BYOK pool;
- multiple providers;
- custom API;
- custom Base URL;
- advanced routing.

### Complimentary / Special Member

Tidak ada public Free Plan. Admin dapat membuat member khusus dengan quota, expiry, provider/model access, dan akses BYOK yang custom.

## 5. Provider Registry

Provider tidak boleh hard-coded sebagai business logic.

Minimal field:

```text
provider_id
name
slug
adapter_type
enabled
base_url
auth_type
supported_models
capabilities
priority
health_status
created_at
updated_at
```

Provider yang dapat dipersiapkan antara lain OpenAI, Anthropic, Google Gemini, OpenRouter, Mistral, Groq, DeepSeek, xAI, dan provider lain yang kompatibel. Daftar bersifat configurable.

## 6. Provider Adapter

Contoh adapter:
- OpenAIAdapter
- AnthropicAdapter
- GeminiAdapter
- OpenRouterAdapter
- GenericOpenAICompatibleAdapter

Adapter menangani authentication, request mapping, response normalization, streaming bila diperlukan, error normalization, usage extraction, headers, dan timeout.

## 7. Generic OpenAI-Compatible Provider

Config minimal:

```text
name
base_url
api_key
model
headers
endpoint_mapping
timeout
```

Custom Base URL wajib melewati validasi keamanan.

## 8. Custom Base URL & SSRF Protection

Server tidak boleh bebas melakukan request ke URL internal.

Minimal protection:
- HTTPS only untuk production;
- block localhost;
- block loopback;
- block private IP ranges;
- block link-local;
- block cloud metadata endpoints;
- DNS rebinding protection;
- resolve DNS lalu validasi IP;
- redirect validation;
- request timeout;
- response size limit;
- allowlist bila diperlukan;
- audit log.

Jangan hanya memvalidasi string URL; validasi target network setelah DNS resolution.

## 9. Model Registry

Model diregistrasikan secara terstruktur.

```text
model_id
provider_id
model_name
display_name
task_capabilities
context_window
input_cost
output_cost
enabled
priority
```

Capabilities dapat mencakup reasoning, research, vision, structured_output, long_context, fast_generation, creative_generation, dan QA.

## 10. Task-Based Model Routing

Model dipilih berdasarkan task dan capability, bukan hanya nama provider.

Contoh task:

```text
research
topic_intelligence
audience_analysis
viral_analysis
strategy
story_architecture
script
image_prompt
video_prompt
qa
revision
```

Routing mempertimbangkan task, capability, quality tier, latency, cost, member plan, provider health, credential health, quota, BYOK, dan admin policy.

## 11. Routing Strategy

Mendukung:
- Priority;
- Weight;
- Round Robin;
- Health-Based;
- Failover;
- Rate-Limit Aware.

Credential/provider yang bermasalah sementara dikeluarkan dari routing.

## 12. Circuit Breaker

State:

```text
CLOSED
OPEN
HALF_OPEN
```

Kegagalan berulang melewati threshold membuka circuit. Setelah cooldown dilakukan health probe. Jika pulih, routing dapat kembali aktif.

## 13. Quota Architecture

Quota dipisahkan pada beberapa level:

```text
Global Budget
    ↓
Member Budget
    ↓
Plan Budget
    ↓
Provider Budget
    ↓
Credential Budget
    ↓
Analysis Unit
```

Batas dapat mencakup analysis/month, daily analysis, token, credit, estimated cost, provider spend, credential spend, dan global spend.

## 14. Usage Tracking

Setiap execution sebaiknya mencatat:

```text
request_id
member_id
analysis_id
task
provider_id
credential_id
model_id
input_tokens
output_tokens
total_tokens
estimated_cost
latency_ms
status
error_code
created_at
```

Secret/API key tidak boleh disimpan di usage log.

## 15. Cost Control

Admin dapat mengatur global budget, provider budget, credential budget, member budget, plan quota, model availability, maximum tokens, timeout, dan retry limit.

Jika budget terlampaui, router menghentikan atau mengalihkan request sesuai policy.

## 16. Retry Policy

Retry hanya untuk transient/network/rate-limit conditions yang layak di-retry. Jangan retry membabi buta untuk invalid credential, invalid request, policy rejection, atau permanent 4xx.

Gunakan exponential backoff, jitter, dan retry limit.

## 17. BYOK vs Admin Pool

Routing policy harus configurable:

```text
BYOK preferred
    ↓
BYOK healthy?
 ├── YES → use BYOK
 └── NO  → Admin Pool fallback (if allowed)
```

Policy dapat berupa BYOK only, Admin Pool only, BYOK preferred, atau Admin preferred.

## 18. Secret Management

API key/secret harus:
- encrypted at rest;
- decrypted hanya di execution boundary;
- masked di UI;
- masked di logs;
- tidak dikirim kembali penuh ke frontend;
- tidak masuk prompt;
- tidak masuk analytics;
- access diaudit.

## 19. Audit Log

Perubahan provider, credential, BYOK, Base URL, routing, quota, budget, model, dan credential test harus dapat diaudit.

Minimal:

```text
actor_id
action
resource_type
resource_id
metadata
created_at
```

Metadata tidak boleh mengandung secret.

## 20. Suggested Database Entities

```text
ai_providers
ai_models
ai_credentials
ai_provider_routes
ai_usage_logs
ai_budgets
ai_health_checks
ai_circuit_breakers
ai_routing_policies
member_ai_settings
```

Payment credential harus terpisah dari AI credential.

## 21. Error Model

Normalisasi error internal:

```text
AUTH_ERROR
RATE_LIMITED
TIMEOUT
NETWORK_ERROR
INVALID_REQUEST
MODEL_UNAVAILABLE
PROVIDER_UNAVAILABLE
QUOTA_EXCEEDED
BUDGET_EXCEEDED
SAFETY_REJECTED
SCHEMA_ERROR
UNKNOWN_PROVIDER_ERROR
```

Frontend menerima error yang aman dan actionable, bukan raw secret/provider dump.

## 22. Observability

Admin membutuhkan request count, success/failure rate, latency, token usage, estimated cost, provider/credential health, quota/budget consumption, retry count, dan failover count.

## 23. AI Execution Contract

Contoh request internal:

```json
{
  "task": "strategy",
  "memberId": "member_x",
  "analysisId": "analysis_x",
  "requirements": {
    "qualityTier": "professional",
    "structuredOutput": true
  },
  "preferredSource": "byok",
  "fallbackAllowed": true
}
```

Router mengembalikan normalized result yang mencakup provider, model, status, content, usage, dan latency.

## 24. Security Requirements

Wajib:
- authorization per member/admin;
- credential encryption;
- secret masking;
- SSRF protection;
- rate limiting;
- abuse protection;
- request timeout;
- payload limits;
- audit logging;
- secure webhook handling bila diperlukan;
- no secret exposure in errors;
- no hard-coded API key.

## 25. Definition of Done

- [ ] Admin dapat membuat provider.
- [ ] Admin dapat menambahkan multiple credentials.
- [ ] Credential dapat di-enable/disable.
- [ ] Provider adapter abstraction berjalan.
- [ ] Generic OpenAI-compatible adapter tersedia.
- [ ] Model registry tersedia.
- [ ] Task-based routing tersedia.
- [ ] Priority/weight/round-robin tersedia sesuai kebutuhan.
- [ ] Health check tersedia.
- [ ] Failover tersedia.
- [ ] Rate-limit handling tersedia.
- [ ] Circuit breaker tersedia.
- [ ] Admin budget tersedia.
- [ ] Member quota tersedia.
- [ ] Usage/token/cost tracking tersedia.
- [ ] BYOK tersedia sesuai plan.
- [ ] BYOK secret terenkripsi.
- [ ] Custom Base URL tersedia untuk Power.
- [ ] SSRF protection diuji.
- [ ] Audit log tersedia.
- [ ] Raw provider error tidak bocor ke user.
- [ ] Tidak ada API key hard-coded.
- [ ] Test provider failure/failover berhasil.
- [ ] Test quota/budget berhasil.
- [ ] Test routing berhasil.
- [ ] Test credential isolation berhasil.

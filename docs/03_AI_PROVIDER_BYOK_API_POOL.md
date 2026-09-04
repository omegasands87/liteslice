# DOKUMEN PELENGKAP — AI PROVIDER, BYOK & API POOL

> **CURRENT MEMBERSHIP AUTHORITY:** Membership pricing, quota, feature access, and public-plan BYOK entitlement are defined by `docs/06_CURRENT_MEMBERSHIP_PLANS.md` and Decision 030 in `docs/05_PROJECT_DECISIONS.md`. This document defines the AI provider architecture and must follow that membership policy. Any older Free/Starter/Pro/Power entitlement examples are historical examples only and are not current product requirements.

## 1. Tujuan Dokumen

Dokumen ini melengkapi konsep project utama dengan sistem **AI Provider Management** yang fleksibel.

Project wajib mendukung tiga sumber AI:

1. **Admin Pool** — API milik platform yang disediakan untuk member.
2. **BYOK (Bring Your Own Key)** — member memasukkan API key miliknya sendiri sesuai entitlement membership.
3. **Custom Provider** — provider/API dengan Base URL yang dapat dikonfigurasi sesuai policy keamanan.

Sistem mendukung lebih dari satu API key/provider secara bersamaan agar dapat melakukan routing, load balancing, fallback, quota management, dan failover.

> Member tidak boleh dipaksa menggunakan satu API key global.

> Admin dapat menyediakan Admin AI Pool dengan quota tertentu.

> Provider baru dapat ditambahkan tanpa mengubah core Analysis Engine.

---

# 2. CURRENT MEMBERSHIP / AI SOURCE POLICY

Current public plans:

```text
Starter — Rp30.000/month
├── Admin AI Pool
├── 5 analysis/month
├── Limited analysis features
└── No BYOK entitlement

Growth — Rp90.000/month
├── Admin AI Pool
├── 20 analysis/month
├── Full analysis features
└── No BYOK entitlement defined by current membership decision

Creator — Rp270.000/month
├── Admin AI Pool
├── 75 analysis/month
├── Full analysis features
└── BYOK after Admin AI Pool quota is exhausted
```

Tidak ada public Free Plan.

Untuk membership publik, routing sumber AI adalah:

```text
Starter → Admin AI Pool
Growth  → Admin AI Pool
Creator → Admin AI Pool
           ↓ quota exhausted
           BYOK may be used
```

Complimentary member tetap dapat dibuat Admin secara manual dengan entitlement khusus.

---

# 3. MODEL PENGGUNAAN AI

Sistem memiliki tiga mode.

```text
MODE 1
ADMIN POOL
Platform menyediakan API
        ↓
Member menggunakan membership quota
```

```text
MODE 2
BYOK
Member memasukkan API key sendiri
        ↓
Analysis Engine menggunakan API member
        ↓
Tidak mengurangi quota Admin Pool
```

Untuk current public plans, BYOK hanya tersedia bagi Creator setelah quota Admin AI Pool habis. Policy Admin/complimentary dapat memberikan entitlement berbeda secara eksplisit.

```text
MODE 3
CUSTOM PROVIDER
Member/Admin menentukan:
Provider
Base URL
Authentication
Model
        ↓
Analysis Engine
```

Ketiga mode dapat hidup berdampingan.

---

# 4. PRIORITAS SUMBER API

Default untuk current public membership:

```text
Admin AI Pool
      ↓
Check member quota
      ↓
Use Admin Pool

Creator + Admin Pool quota exhausted
      ↓
Check BYOK entitlement
      ↓
Use Member BYOK if configured and active
```

Admin tetap dapat mengatur routing policy untuk entitlement yang memang diberikan.

Contoh policy:

```text
[ ] BYOK First
[X] Admin Pool First
[ ] BYOK Only
[ ] Admin Pool Only
```

Policy tidak boleh digunakan untuk memberikan BYOK kepada plan yang tidak memiliki entitlement tersebut.

---

# 5. ADMIN API POOL

Admin dapat memasang lebih dari satu API key.

Contoh:

```text
ADMIN AI POOL

OpenAI
├── Key #1
├── Key #2
└── Key #3

Anthropic
├── Key #1
└── Key #2

Google
├── Key #1
└── Key #2

OpenRouter
├── Key #1
└── Key #2
```

Setiap credential instance dapat memiliki konfigurasi sendiri:

```text
Provider
Credential Name
API Key / Secret
Status
Priority
Weight
Daily Limit
Monthly Limit
Models
Enabled
```

---

# 6. API POOL ROUTING

Pool harus mendukung:

## Round Robin

```text
Key 1 → Key 2 → Key 3 → Key 1
```

## Weighted Round Robin

Credential dengan weight lebih tinggi menerima lebih banyak traffic.

## Priority

Credential dengan priority lebih tinggi dipilih terlebih dahulu.

## Least Usage

Pilih credential dengan penggunaan paling rendah.

## Health Based

Pilih credential yang aktif, sehat, belum mencapai quota, tidak mengalami error berulang, dan memiliki model yang diminta.

---

# 7. FAILOVER

Jika API gagal:

```text
Request
  ↓
API #1
  ↓
FAILED
  ↓
API #2
  ↓
SUCCESS
```

Failure yang dapat memicu failover:

- timeout;
- rate limit;
- temporary server error;
- provider unavailable;
- credential invalid;
- quota exhausted;
- model unavailable.

Gunakan maximum attempts, retry delay, exponential backoff, dan circuit breaker. Jangan melakukan infinite retry.

---

# 8. CIRCUIT BREAKER

```text
ACTIVE
  ↓
ERROR THRESHOLD
  ↓
DEGRADED
  ↓
TEMPORARILY DISABLED
  ↓
HEALTH CHECK
  ↓
ACTIVE
```

---

# 9. QUOTA SYSTEM

Quota terpisah dari API credential.

```text
Member Quota
≠
Provider/Credential Quota
```

V1:

```text
1 Analysis = 1 quota unit
```

Current public membership quota:

```text
Starter = 5 analysis/month
Growth  = 20 analysis/month
Creator = 75 analysis/month
```

Quota provider/credential dan global pool budget tetap dihitung terpisah.

Sistem sebaiknya mendukung:

```text
Requests
Input Tokens
Output Tokens
Total Tokens
Credits
Analysis Units
```

---

# 10. QUOTA RESOLUTION

Sebelum request:

```text
Determine member
        ↓
Determine membership
        ↓
Determine task/capability
        ↓
Check membership entitlement
        ↓
Check Member Quota
        ↓
Check Provider Credential Quota
        ↓
Check Global Pool Budget
        ↓
Resolve allowed AI source
        ↓
Select credential
        ↓
Execute request
        ↓
Record usage
```

Jika quota Admin AI Pool membership habis:

```text
Starter / Growth
→ Do not call AI through Admin Pool
→ Wait for reset or upgrade

Creator
→ Check BYOK entitlement
→ If BYOK configured and active, use BYOK
→ Otherwise wait for reset or upgrade/other entitled access
```

---

# 11. ADMIN AI POOL BUDGET

Admin dapat menetapkan:

```text
Daily Pool Budget
Monthly Pool Budget
Per Member Budget
Per Provider Budget
Per Credential Budget
```

Cost tracking harus tetap dilakukan walaupun quota member masih tersedia.

---

# 12. BYOK — BRING YOUR OWN KEY

BYOK adalah credential milik member.

Untuk current public plans:

```text
Starter → No BYOK
Growth  → No BYOK
Creator → BYOK after Admin Pool quota exhausted
```

Creator menggunakan BYOK sebagai mekanisme lanjutan setelah quota Admin AI Pool habis.

Penggunaan API provider melalui BYOK menjadi tanggung jawab pemilik API credential.

Jumlah credential/provider BYOK untuk Creator **belum ditetapkan oleh keputusan membership terbaru**. Jangan mengasumsikan batas atau pool size baru tanpa keputusan product tambahan.

---

# 13. BYOK SECURITY

API key milik member merupakan credential sensitif.

Aturan:

- jangan dikirim kembali plaintext setelah disimpan;
- masking di UI;
- encrypted/secure secret storage;
- jangan log API key;
- jangan masukkan API key ke analytics;
- jangan expose API key ke browser jika request dapat dilakukan server-side;
- hapus credential ketika diminta member;
- audit perubahan credential.

---

# 14. BYOK OWNERSHIP

```text
ADMIN
    └── Admin Pool Credential

MEMBER A
    └── BYOK Credential(s)

MEMBER B
    └── BYOK Credential(s)
```

Credential milik satu member tidak boleh digunakan member lain.

---

# 15. PROVIDER REGISTRY

Arsitektur dipersiapkan untuk provider besar yang relevan, misalnya:

```text
OpenAI
Anthropic
Google Gemini
OpenRouter
Mistral
Groq
DeepSeek
xAI
```

Daftar tersebut adalah provider yang dipersiapkan arsitektur, bukan kewajiban mengaktifkan semuanya pada hari pertama.

Admin dapat enable, disable, configure, test, set priority.

Provider/model capability dapat mencakup:

```text
Text
Vision
Image Input
Structured Output
Streaming
Tool Calling
Image Generation
Video Generation
```

---

# 16. CUSTOM PROVIDER

Admin atau member hanya dapat menggunakan custom provider jika entitlement dan policy mengizinkannya.

Konfigurasi:

```text
Provider Name
Base URL
Authentication
API Key / Secret
Model
Headers
Endpoint Mapping
```

Jika OpenAI-compatible, gunakan Generic OpenAI-Compatible Adapter.

---

# 17. CUSTOM BASE URL

Base URL configurable tetapi wajib diproses melalui backend dengan SSRF protection.

Minimal:

```text
HTTPS required
Public hostname required
Private IP blocked
Loopback blocked
Localhost blocked
Metadata endpoint blocked
Redirect validation
DNS/IP validation
Timeout
Response size limit
```

---

# 18. CUSTOM AUTHENTICATION

Dukungan konseptual:

```text
Bearer Token
API Key Header
API Key Query Parameter
Basic Auth
Custom Header
No Auth
```

Sensitive values harus disimpan sebagai secret.

---

# 19. CUSTOM ENDPOINT MAPPING

Provider configuration dapat memiliki:

```text
Base URL
Chat Endpoint
Models Endpoint
Embeddings Endpoint
Image Endpoint
Video Endpoint
```

Untuk V1 core analysis minimal membutuhkan Chat/Generation Endpoint.

---

# 20. MODEL REGISTRY

Model dipisahkan dari provider.

```text
Provider
    ↓
Model
```

Admin dapat mengatur:

```text
Model Name
Provider
Capabilities
Cost Input
Cost Output
Context Limit
Enabled
Priority
```

Harga/token bersifat configurable.

---

# 21. MODEL & TASK ROUTING

Analysis Engine tidak boleh bergantung pada satu model.

Task dapat mencakup:

```text
TOPIC_ANALYSIS
VIRAL_ANALYSIS
IMAGE_ANALYSIS
VIDEO_ANALYSIS
SCRIPT_GENERATION
SCENE_GENERATION
QUALITY_CONTROL
```

Setiap task dapat memiliki model preference dan fallback berdasarkan capability.

---

# 22. AI REQUEST ROUTER

```text
Analysis Request
        ↓
Quota Service
        ↓
Entitlement Check
        ↓
Credential Resolver
        ↓
Provider Router
        ↓
Model Router
        ↓
Health Check
        ↓
Selected Credential
        ↓
AI Provider
        ↓
Response
        ↓
Usage Meter
        ↓
Analysis Result
```

---

# 23. CREDENTIAL RESOLUTION

Urutan konseptual:

```text
1. Determine member
2. Determine membership
3. Determine task
4. Determine requested model/capability
5. Check membership BYOK entitlement
6. Check member BYOK credentials if entitled
7. Check Admin Pool
8. Check provider health
9. Check quota/budget
10. Select credential
11. Execute request
12. Record usage
```

---

# 24. COST TRACKING

Setiap AI request sebaiknya mencatat:

```text
request_id
user_id
provider_id
credential_id
model
task
input_tokens
output_tokens
total_tokens
estimated_cost
actual_cost
latency
status
error_code
created_at
```

---

# 25. ADMIN AI DASHBOARD

Admin dashboard harus dapat memperlihatkan:

```text
AI PROVIDERS
API POOL
Credentials
Health
USAGE
Requests
Tokens
Estimated Cost
POOL BUDGET
ERRORS
```

---

# 26. MEMBER BYOK UI

BYOK UI hanya ditampilkan jika entitlement member mengizinkannya.

Contoh:

```text
MY AI API

[+ Add Provider]

Provider:
OpenAI

API Key:
**************

Model:
[Auto Detect / Select]

[Save & Test]
```

Untuk Creator, UI harus menjelaskan bahwa BYOK menjadi opsi setelah quota Admin AI Pool habis.

---

# 27. BYOK VS ADMIN POOL

```text
ADMIN POOL
Provided by the platform.
Uses membership quota.

BYOK
Uses member's own API account and billing.
Does not consume Admin Pool quota.
```

---

# 28. API TEST CONNECTION

Setiap credential harus memiliki test connection yang aman dan murah.

Hasil dapat mencakup:

```text
CONNECTED
Provider reachable
Credential valid
Model available
Latency
```

atau:

```text
FAILED
Invalid API key
```

Credential tidak boleh muncul dalam error log.

---

# 29. RATE LIMIT HANDLING

Bedakan:

```text
429 Rate Limit
401 Unauthorized
403 Forbidden
400 Bad Request
404 Model Not Found
408 Timeout
5xx Provider Error
Network Error
```

Contoh behavior:

```text
401 → disable/revalidate credential
429 → deprioritize and try another credential
5xx → retry with backoff, then failover
404 → try compatible model if available
```

---

# 30. IDEMPOTENCY & AUDIT

Gunakan request ID dan idempotency key jika provider mendukung.

Admin audit log minimal mencatat:

```text
credential added/changed/disabled
provider disabled
quota changed
routing policy changed
pool budget changed
```

---

# 31. DATABASE EXTENSION

Minimal:

```text
ai_providers
ai_provider_credentials
ai_models
ai_routing_policies
ai_usage_records
ai_quota_allocations
ai_pool_budgets
ai_health_checks
ai_request_logs
```

Untuk BYOK:

```text
user_ai_credentials
```

Credential dapat dimodelkan generik dengan ownership:

```text
owner_type
owner_id
provider_id
credential_type
```

---

# 32. PROVIDER ADAPTER INTERFACE

```text
AIProviderAdapter

validateCredential()
listModels()
generate()
estimateUsage()
normalizeResponse()
normalizeError()
healthCheck()
```

Provider-specific adapter dan Generic OpenAI-Compatible Adapter harus dapat digunakan tanpa mengikat Analysis Engine ke provider tertentu.

---

# 33. MULTIMODAL SUPPORT

Arsitektur harus siap untuk:

```text
Analyze Reference Image
Analyze Screenshot
Analyze Existing Video Metadata
Analyze Visual Concept
```

Namun fitur yang belum diperlukan tidak perlu diaktifkan.

---

# 34. FALLBACK MATRIX

Fallback dipilih berdasarkan task dan capability.

```text
IMAGE_ANALYSIS
Primary → Vision Model A
Fallback → Vision Model B

VIDEO_ANALYSIS
Primary → Multimodal Model A
Fallback → Multimodal Model B

SCRIPT_GENERATION
Primary → Reasoning Model A
Fallback → Reasoning Model B
```

---

# 35. PAYMENT SEPARATION

AI credentials dan payment credentials adalah dua domain berbeda.

```text
AI Credential Management
        ↓
AI Providers / Models / Pool

Payment Credential Management
        ↓
Xendit / NOWPayments / Manual Transfer
```

Secure secret infrastructure boleh digunakan bersama, tetapi service layer tetap terpisah.

---

# 36. ADMIN OVERRIDE

Admin dapat mengatur:

```text
Force Provider
Force Model
Disable Provider
Disable Credential
Change Priority
Change Weight
Set Quota
Set Budget
Set Fallback
```

Admin override tidak boleh melanggar security policy atau memberikan public-plan entitlement yang belum diputuskan.

---

# 37. MEMBER POLICY

Admin dapat menentukan entitlement khusus untuk complimentary member.

Untuk public plans:

```text
Starter → Admin Pool only
Growth  → Admin Pool only
Creator → Admin Pool, then BYOK after quota exhausted
```

Custom provider dan advanced credential capabilities hanya boleh tersedia jika entitlement/policy mengizinkannya.

---

# 38. OBSERVABILITY

Admin perlu mengetahui alasan routing:

```text
Task
Member
Selected Provider
Credential
Model
Reason
Fallback
Latency
Tokens
Cost
Status
```

Credential secret tidak boleh muncul dalam observability output.

---

# 39. USER EXPERIENCE

Member yang memakai Admin Pool tidak perlu mengetahui kompleksitas routing.

Contoh:

```text
AI Access

● Platform AI
   4 / 5 analyses remaining
```

Untuk Creator setelah quota habis:

```text
Platform quota exhausted.
Connect your own API to continue with BYOK.
```

---

# 40. CORE ARCHITECTURE

```text
                    ANALYSIS REQUEST
                           │
                           ▼
                    QUOTA SERVICE
                           │
                     ENTITLEMENT
                           │
                           ▼
                 CREDENTIAL RESOLVER
                           │
             ┌─────────────┴─────────────┐
             │                           │
          BYOK                       ADMIN POOL
             │                           │
             └─────────────┬─────────────┘
                           ▼
                    PROVIDER ROUTER
                           │
                    MODEL ROUTER
                           │
                    HEALTH CHECK
                           │
                           ▼
                    AI PROVIDER
                           │
                           ▼
                    AI RESPONSE
                           │
                  ┌────────┴────────┐
                  │                 │
             USAGE METER       ERROR HANDLER
                  │                 │
                  └────────┬────────┘
                           ▼
                    ANALYSIS ENGINE
                           │
                           ▼
                  PROFESSIONAL BLUEPRINT
```

---

# 41. V1 PRIORITY

Tier 1:

```text
Admin Pool
BYOK
Multiple Credentials
Quota
Failover
Generic OpenAI-Compatible Provider
```

Tier 2:

```text
Provider Registry
Model Registry
Cost Tracking
Weighted Routing
Health Monitoring
```

Tier 3:

```text
Advanced Cost Optimization
Automatic Model Selection
Advanced Budgeting
Dynamic Routing
```

Implementasi harus tetap tunduk pada membership entitlement terbaru.

---

# 42. DEFINITION OF DONE — AI PROVIDER SYSTEM

Sistem dianggap selesai jika:

1. Admin dapat menambahkan provider.
2. Admin dapat menambahkan lebih dari satu API key.
3. Admin dapat mengaktifkan/nonaktifkan credential.
4. Admin dapat menentukan priority/weight.
5. Sistem dapat melakukan routing.
6. Sistem dapat melakukan failover.
7. Sistem dapat menangani rate limit.
8. Sistem dapat menghitung quota member.
9. Sistem dapat menghitung penggunaan credential.
10. Admin dapat menyediakan Admin AI Pool.
11. Admin AI Pool memiliki quota per member.
12. Admin AI Pool memiliki budget.
13. Creator dapat menggunakan BYOK setelah quota Admin Pool habis.
14. BYOK tidak mengurangi Admin Pool quota.
15. Multiple providers didukung secara arsitektural.
16. Provider registry didukung.
17. Model registry didukung.
18. Custom Base URL didukung dengan SSRF protection.
19. Custom authentication didukung.
20. Generic OpenAI-compatible adapter tersedia.
21. Credential disimpan secara aman.
22. Credential tidak pernah masuk client-side response.
23. Credential tidak pernah masuk log.
24. AI usage dicatat.
25. Error provider dicatat.
26. Health status credential dapat diketahui.
27. Admin dapat melihat penggunaan dan estimasi biaya.
28. Core Analysis Engine tidak bergantung pada provider tertentu.
29. Entitlement membership diverifikasi sebelum BYOK digunakan.

---

# 43. KETENTUAN PENTING UNTUK AI BUILDER

Jangan mengimplementasikan AI integration sebagai:

```text
Analysis → OpenAI API langsung
```

Implementasikan:

```text
Analysis
   ↓
AI Service
   ↓
Provider Router
   ↓
Credential Resolver
   ↓
Model Router
   ↓
Provider Adapter
```

API Pool harus mempunyai:

- routing;
- quota;
- health;
- priority;
- weight;
- failover;
- usage tracking;
- budget;
- ownership.

BYOK harus menjadi credential yang dapat dipakai oleh Credential Resolver dan hanya tersedia sesuai membership entitlement.

Custom Provider harus melalui URL validation, SSRF protection, authentication configuration, provider adapter, capability detection, dan health check.

---

# 44. FINAL PRODUCT PRINCIPLE

> **The product owns the AI orchestration layer, not the AI provider.**

Provider dapat berubah. API key dapat berubah. Model dapat berubah. Harga provider dapat berubah.

Yang harus tetap stabil:

```text
Analysis Engine
Professional Content Blueprint
Quota System
Member Experience
```

Membership terbaru tidak mengubah prinsip provider abstraction. Ia hanya menentukan **siapa yang berhak menggunakan sumber AI tertentu dan kapan BYOK dapat digunakan**.

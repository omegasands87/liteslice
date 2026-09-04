# DOKUMEN PELENGKAP — AI PROVIDER, BYOK & API POOL

## 1. Tujuan Dokumen

Dokumen ini melengkapi konsep project utama dengan sistem **AI Provider Management** yang lebih fleksibel.

Project wajib mendukung tiga sumber AI:

1. **Admin Pool** — API milik platform yang disediakan untuk member.
2. **BYOK (Bring Your Own Key)** — member memasukkan API key miliknya sendiri.
3. **Custom Provider** — member atau admin dapat menggunakan provider/API dengan Base URL yang dapat dikonfigurasi.

Sistem juga harus mendukung **lebih dari satu API key/provider secara bersamaan**, sehingga aplikasi dapat melakukan routing, load balancing, fallback, quota management, dan failover.

Prinsip utama:

> Member tidak boleh dipaksa menggunakan satu API key global.

> Admin dapat menyediakan pool API gratis dengan quota tertentu.

> Member yang memiliki API sendiri dapat menggunakan BYOK.

> Provider baru dapat ditambahkan tanpa harus mengubah core Analysis Engine.

---

# 2. Model Penggunaan AI

Sistem memiliki tiga mode.

```text
MODE 1
ADMIN POOL
Platform menyediakan API
        ↓
Member menggunakan quota gratis
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

Ketiga mode harus dapat hidup berdampingan.

---

# 3. Prioritas Sumber API

Sistem harus mempunyai policy routing yang jelas.

Default:

```text
Member BYOK
      ↓
Jika tersedia dan aktif
      ↓
Gunakan BYOK

Jika BYOK tidak tersedia:
      ↓
Gunakan Admin Pool
      ↓
Pilih API yang tersedia
      ↓
Consume quota member
```

Namun Admin harus dapat mengubah policy.

Contoh:

```text
[ ] BYOK First
[X] Admin Pool First
[ ] BYOK Only
[ ] Admin Pool Only
```

Policy juga dapat ditentukan berdasarkan fitur.

Contoh:

```text
Analysis:
    BYOK preferred
    fallback → Admin Pool

Premium Analysis:
    BYOK required

Free Analysis:
    Admin Pool only
```

---

# 4. ADMIN API POOL

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

Setiap API key merupakan **credential instance** yang dapat memiliki konfigurasi sendiri.

Contoh:

```text
Provider:
OpenAI

Credential Name:
OpenAI Production #1

API Key:
**************

Status:
ACTIVE

Priority:
10

Weight:
5

Daily Limit:
1000 requests

Monthly Limit:
20000 requests

Models:
model-a
model-b

Enabled:
YES
```

---

# 5. API POOL ROUTING

Jangan selalu menggunakan API key pertama.

Pool harus mendukung beberapa strategi.

## Round Robin

```text
Key 1
↓
Key 2
↓
Key 3
↓
Key 1
```

## Weighted Round Robin

Contoh:

```text
Key 1 = weight 5
Key 2 = weight 3
Key 3 = weight 2
```

Key #1 mendapatkan lebih banyak traffic.

## Priority

```text
Priority 1 → Key A
Priority 2 → Key B
Priority 3 → Key C
```

## Least Usage

Pilih credential dengan penggunaan paling rendah.

## Health Based

Pilih credential yang:

- aktif;
- sehat;
- tidak mencapai quota;
- tidak mengalami error berulang;
- model yang diminta tersedia.

---

# 6. FAILOVER

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

Jangan melakukan infinite retry.

Gunakan:

```text
Maximum Attempts
Retry Delay
Exponential Backoff
Circuit Breaker
```

---

# 7. CIRCUIT BREAKER

Jika sebuah API credential berkali-kali gagal:

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

Tujuannya mencegah sistem terus-menerus mengirim request ke API yang sedang bermasalah.

---

# 8. QUOTA SYSTEM

Quota harus menjadi konsep terpisah dari API key.

Ada dua jenis quota utama:

## Member Quota

Quota yang diberikan kepada member.

Contoh:

```text
Free Member

Analysis:
10 analyses / month

Premium Member:

100 analyses / month
```

## Provider Quota

Batas penggunaan sebuah API credential.

Contoh:

```text
OpenAI Key #1
1000 requests/day

OpenAI Key #2
500 requests/day
```

Kedua quota tersebut harus dihitung terpisah.

---

# 9. ADMIN FREE AI POOL

Admin dapat menyediakan pool API untuk member gratis.

Contoh:

```text
FREE AI POOL

Provider        Credential    Daily Limit
------------------------------------------------
OpenAI          Key #1        500 requests
OpenAI          Key #2        500 requests
Google          Key #1        500 requests
Anthropic       Key #1        300 requests
```

Member tidak perlu memasukkan API key.

Sistem otomatis menggunakan pool.

Namun setiap member memiliki quota sendiri.

Contoh:

```text
Member A
10 analyses/month

Member B
10 analyses/month

Member C
10 analyses/month
```

Pool digunakan bersama, tetapi quota member tetap dihitung secara individual.

---

# 10. QUOTA SHOULD NOT BE ONLY REQUEST COUNT

Untuk AI, jumlah request saja tidak selalu mencerminkan biaya.

Sistem sebaiknya mendukung beberapa unit:

```text
Requests
Input Tokens
Output Tokens
Total Tokens
Credits
Analysis Units
```

Untuk V1 dapat menggunakan:

```text
1 Analysis = 1 quota unit
```

Namun database harus dirancang agar nantinya dapat mendukung token/credit metering.

---

# 11. CREDIT SYSTEM

Opsional tetapi disarankan.

Contoh:

```text
Free Member
100 credits

Premium Member
1,000 credits
```

Contoh penggunaan:

```text
Basic Analysis
10 credits

Professional Class
30 credits

Image Analysis
15 credits

Video Analysis
30 credits
```

Harga credit harus dapat dikonfigurasi Admin.

---

# 12. BYOK — BRING YOUR OWN KEY

Member dapat memasukkan API credential sendiri.

Contoh:

```text
MY AI PROVIDERS

OpenAI
API Key: **************
Status: Connected

Anthropic
API Key: **************
Status: Connected

Google Gemini
API Key: **************
Status: Connected
```

Member dapat memiliki beberapa provider sekaligus.

Contoh:

```text
Member BYOK Pool

OpenAI #1
Anthropic #1
Google #1
OpenRouter #1
```

Sistem dapat menggunakan routing yang sama seperti Admin Pool.

---

# 13. BYOK SECURITY

API key milik member merupakan credential sensitif.

Aturan:

- Jangan dikirim kembali dalam bentuk plaintext setelah disimpan.
- Jangan tampilkan API key penuh.
- Masking di UI.
- Simpan secara encrypted/secure secret storage.
- Jangan log API key.
- Jangan masukkan API key ke analytics.
- Jangan expose API key ke browser jika request dapat dilakukan server-side.
- Hapus credential ketika member meminta penghapusan.
- Audit perubahan credential.

Contoh tampilan:

```text
OpenAI

sk-••••••••••••••••7X9A

Connected
[Remove]
[Test Connection]
```

---

# 14. BYOK OWNERSHIP

Credential harus memiliki owner.

```text
ADMIN
    └── Admin Pool Credential

MEMBER A
    ├── BYOK Credential #1
    └── BYOK Credential #2

MEMBER B
    └── BYOK Credential #1
```

Credential Member A tidak boleh digunakan Member B.

---

# 15. PROVIDER REGISTRY

Sistem harus memiliki registry provider.

Minimal siapkan adapter untuk provider besar yang relevan dengan use case project, misalnya:

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

Daftar ini adalah provider yang dipersiapkan oleh arsitektur, bukan berarti semua provider wajib diaktifkan sejak hari pertama.

Admin dapat:

```text
Enable
Disable
Configure
Test
Set Priority
```

Provider-specific capabilities juga harus disimpan.

Contoh:

```text
Provider
Model
Text
Vision
Image Input
Structured Output
Streaming
Tool Calling
```

---

# 16. CUSTOM PROVIDER

Ini adalah requirement penting.

Admin atau member harus dapat menambahkan provider custom.

Contoh:

```text
Provider Name:
My Custom AI

Base URL:
https://api.example.com/v1

Authentication:
Bearer Token

API Key:
**************

Model:
my-model

Chat Endpoint:
 /chat/completions
```

Jika provider kompatibel dengan OpenAI API, sistem sebaiknya dapat langsung menggunakannya melalui adapter OpenAI-compatible.

---

# 17. CUSTOM BASE URL

Base URL harus configurable.

Contoh:

```text
https://api.openai.com/v1
https://openrouter.ai/api/v1
https://api.groq.com/openai/v1
https://custom.example.com/v1
```

Jangan hard-code Base URL di Analysis Engine.

Gunakan:

```text
Provider Configuration
        ↓
Base URL
        ↓
Adapter
        ↓
AI Client
```

---

# 18. CUSTOM AUTHENTICATION

Custom provider harus mendukung beberapa bentuk authentication jika dibutuhkan:

```text
Bearer Token
API Key Header
API Key Query Parameter
Basic Auth
Custom Header
No Auth
```

Untuk custom headers:

```text
Header Name
Header Value
```

Sensitive values harus disimpan sebagai secret.

---

# 19. CUSTOM ENDPOINT MAPPING

Jangan mengasumsikan semua provider menggunakan endpoint yang sama.

Provider configuration dapat memiliki:

```text
Base URL
Chat Endpoint
Models Endpoint
Embeddings Endpoint
Image Endpoint
Video Endpoint
```

Namun V1 hanya perlu mengimplementasikan endpoint yang benar-benar digunakan.

Untuk core analysis, minimal:

```text
Chat / Generation Endpoint
```

---

# 20. MODEL REGISTRY

Model harus dipisahkan dari provider.

Contoh:

```text
Provider
    ↓
Model

OpenAI
    ├── Model A
    └── Model B

Anthropic
    ├── Model A
    └── Model B

Google
    ├── Model A
    └── Model B
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

Harga/token bersifat configurable karena dapat berubah.

---

# 21. MODEL ROUTING

Analysis Engine sebaiknya tidak bergantung pada satu model.

Contoh policy:

```text
Professional Analysis

Primary:
Model A

Fallback:
Model B

Fallback:
Model C
```

Atau:

```text
Image Analysis:
Vision-capable model

Video Analysis:
Multimodal-capable model

General Analysis:
Text model
```

---

# 22. TASK-BASED ROUTING

AI Service harus memahami jenis task.

Contoh:

```text
TASK: TOPIC_ANALYSIS
TASK: VIRAL_ANALYSIS
TASK: IMAGE_ANALYSIS
TASK: VIDEO_ANALYSIS
TASK: SCRIPT_GENERATION
TASK: SCENE_GENERATION
TASK: QUALITY_CONTROL
```

Setiap task dapat mempunyai model preference.

Contoh:

```text
IMAGE_ANALYSIS
    ↓
Vision Model

SCRIPT_GENERATION
    ↓
High-quality reasoning model

QUALITY_CONTROL
    ↓
High-quality reasoning model
```

---

# 23. AI REQUEST ROUTER

Arsitektur:

```text
Analysis Request
        ↓
Quota Service
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

# 24. CREDENTIAL RESOLUTION

Urutan:

```text
1. Determine member
2. Determine membership
3. Determine task
4. Determine requested model/capability
5. Check BYOK policy
6. Check member BYOK credentials
7. Check Admin Pool
8. Check provider health
9. Check quota
10. Select credential
11. Execute request
12. Record usage
```

---

# 25. QUOTA RESOLUTION

Sebelum request:

```text
Check Member Quota
        ↓
Check Provider Credential Quota
        ↓
Check Global Pool Quota
        ↓
Allow / Reject
```

Jika quota member habis:

```text
DO NOT CALL AI
```

Tampilkan:

```text
Your analysis quota has been exhausted.

Options:
- Wait until quota resets
- Upgrade membership
- Connect your own API key
```

Jika BYOK tersedia dan policy mengizinkan:

```text
Quota Admin Pool exhausted
        ↓
Use Member BYOK
```

---

# 26. FREE POOL POLICY

Admin dapat menentukan:

```text
Free Pool Enabled: YES

Quota Per Member:
10 analyses/month

Reset:
Monthly

Allowed Tasks:
- Topic Analysis
- Viral Analysis
- Script Generation

Blocked:
- High-cost tasks
```

Admin juga dapat menentukan maximum cost.

Contoh:

```text
Maximum Pool Cost Per Member:
$1/month
```

Jika cost estimation tersedia, router dapat mencegah penggunaan model yang terlalu mahal.

---

# 27. POOL BUDGET

Admin dapat menetapkan budget:

```text
Daily Pool Budget
Monthly Pool Budget
Per Member Budget
Per Provider Budget
Per Credential Budget
```

Contoh:

```text
Global Monthly AI Budget:
$100

Member Maximum:
$1

Provider Maximum:
$50
```

---

# 28. COST TRACKING

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
actual_cost (if available)
latency
status
error_code
created_at
```

Ini penting agar Admin dapat mengetahui biaya sebenarnya dari Free AI Pool.

---

# 29. ADMIN AI DASHBOARD

Admin dashboard harus memperlihatkan:

```text
AI PROVIDERS
Active Providers

API POOL
Total Credentials
Healthy
Degraded
Failed

USAGE
Requests Today
Requests This Month
Tokens Used
Estimated Cost

FREE POOL
Active Members
Credits Consumed
Budget Used

ERRORS
Rate Limits
Provider Errors
Credential Errors
```

---

# 30. CREDENTIAL MANAGEMENT UI

Contoh:

```text
AI PROVIDERS

Provider       Status    Credentials    Usage
------------------------------------------------
OpenAI         Active       3            42%
Anthropic      Active       2            31%
Google         Active       2            18%
OpenRouter     Active       1             9%
```

Klik provider:

```text
OpenAI

Credential #1
Status: Healthy
Priority: 1
Weight: 5
Usage: 38%
[Edit] [Test] [Disable]

Credential #2
Status: Healthy
Priority: 2
Weight: 3
Usage: 21%
[Edit] [Test] [Disable]
```

---

# 31. MEMBER BYOK UI

```text
MY AI API

[+ Add Provider]

Provider:
OpenAI

API Key:
**************

Model:
[Auto Detect / Select]

Usage Policy:
[Use for my analyses]

[Save & Test]
```

Setelah tersimpan:

```text
OpenAI
Connected
Last tested: ...
[Use as Primary]
[Disable]
[Remove]
```

---

# 32. BYOK VS ADMIN POOL

UI harus menjelaskan dengan jelas:

```text
ADMIN POOL

Provided by the platform.
Uses your membership quota.

BYOK

Uses your own API account and billing.
Does not consume Admin Pool quota.
```

Jika BYOK digunakan, biaya API provider menjadi tanggung jawab pemilik API key.

---

# 33. API TEST CONNECTION

Setiap credential harus memiliki:

```text
Test Connection
```

Test harus melakukan request minimal yang aman dan murah.

Hasil:

```text
CONNECTED
Provider reachable
Credential valid
Model available
Latency: 820ms
```

atau:

```text
FAILED
Invalid API key
```

Jangan menampilkan credential dalam error log.

---

# 34. RATE LIMIT HANDLING

Sistem harus membedakan:

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

Routing behavior harus berbeda.

Contoh:

```text
401
→ Disable credential / require revalidation

429
→ Temporarily deprioritize credential
→ Try another credential

5xx
→ Retry with backoff
→ Then failover

404 Model
→ Try another compatible model
```

---

# 35. IDEMPOTENCY

AI request yang gagal setelah provider menerima request harus ditangani hati-hati.

Gunakan:

```text
request_id
idempotency key where supported
```

Tujuannya mencegah duplicate processing jika retry terjadi.

---

# 36. AUDIT LOG

Admin harus dapat mengetahui:

```text
Who added credential
Who changed credential
Who disabled provider
Who changed quota
Who changed routing policy
Who changed pool budget
```

Member dapat melihat aktivitas credential miliknya sendiri secara terbatas.

---

# 37. DATABASE EXTENSION

Tambahkan entitas:

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

Jika ingin satu tabel credential generik:

```text
ai_credentials
    owner_type
    owner_id
    provider_id
    credential_type
```

---

# 38. RECOMMENDED CREDENTIAL MODEL

Konsep:

```text
AI Provider
    ↓
AI Credential
    ↓
AI Model
```

Credential memiliki:

```text
id
owner_type
owner_id
provider_id
name
encrypted_secret
base_url
auth_type
headers
status
priority
weight
daily_limit
monthly_limit
current_usage
last_error
last_success
created_at
updated_at
```

Jangan menyimpan secret plaintext jika secure/encrypted storage tersedia.

---

# 39. PROVIDER ADAPTER INTERFACE

Gunakan interface konseptual:

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

Setiap provider mempunyai adapter.

```text
OpenAIAdapter
AnthropicAdapter
GoogleAdapter
OpenRouterAdapter
GenericOpenAICompatibleAdapter
CustomAdapter
```

---

# 40. GENERIC OPENAI-COMPATIBLE PROVIDER

Ini sangat penting untuk mengurangi pekerjaan implementasi.

Jika provider mengikuti format API OpenAI-compatible:

```text
Custom Provider
        ↓
Generic OpenAI-Compatible Adapter
```

Admin cukup mengisi:

```text
Name
Base URL
API Key
Model
```

Jika provider memerlukan format khusus, gunakan custom adapter.

---

# 41. PROVIDER CAPABILITY MATRIX

Sistem harus mengetahui kemampuan provider/model.

Contoh:

| Capability | Provider/Model |
|---|---|
| Text | YES/NO |
| Vision | YES/NO |
| Structured Output | YES/NO |
| Streaming | YES/NO |
| Tool Calling | YES/NO |
| Image Generation | YES/NO |
| Video Generation | YES/NO |

Analysis Engine tidak boleh meminta capability yang tidak tersedia.

---

# 42. MULTIMODAL SUPPORT

Karena project menghasilkan konten gambar dan video, arsitektur harus siap untuk model multimodal.

Contoh task:

```text
Analyze Reference Image
Analyze Screenshot
Analyze Existing Video Metadata
Analyze Visual Concept
```

Namun jangan mengaktifkan fitur yang belum diperlukan.

Arsitektur cukup disiapkan agar dapat ditambahkan.

---

# 43. FALLBACK MATRIX

Contoh:

```text
TASK: IMAGE_ANALYSIS

Primary:
Vision Model A

Fallback:
Vision Model B

Fallback:
Vision Model C
```

Untuk video:

```text
TASK: VIDEO_ANALYSIS

Primary:
Multimodal Model A

Fallback:
Multimodal Model B
```

Untuk script:

```text
TASK: SCRIPT_GENERATION

Primary:
Reasoning Model A

Fallback:
Reasoning Model B
```

---

# 44. DO NOT MIX PAYMENT API POOL WITH AI API POOL

AI credentials dan payment credentials adalah dua sistem berbeda.

```text
AI Credential Management
        ↓
AI Providers / Models / Pool

Payment Credential Management
        ↓
Xendit / NOWPayments / Manual Transfer
```

Jangan menggunakan satu generic credential system yang membuat domain menjadi ambigu.

Secure secret infrastructure boleh digunakan bersama, tetapi domain/service layer tetap terpisah.

---

# 45. ADMIN OVERRIDE

Admin harus dapat mengatur:

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

Contoh:

```text
Emergency Mode

Provider:
OpenAI

Force Model:
Model X

Fallback:
Disabled
```

Ini berguna ketika provider tertentu bermasalah.

---

# 46. MEMBER POLICY

Admin dapat menentukan apakah member boleh:

```text
[YES] Use BYOK
[YES] Add multiple BYOK providers
[YES] Add custom provider
[NO] Override platform security settings
```

Custom provider dapat dinonaktifkan jika dianggap terlalu berisiko.

---

# 47. CUSTOM PROVIDER SECURITY

Custom Base URL merupakan attack surface.

Sistem harus mencegah:

- SSRF;
- akses localhost;
- private IP;
- internal network;
- cloud metadata endpoints;
- arbitrary internal ports;
- malicious redirects.

Base URL harus divalidasi.

Jika custom provider hanya boleh berupa public HTTPS endpoint, enforce:

```text
HTTPS required
Public hostname required
Private IP blocked
Loopback blocked
Localhost blocked
Metadata endpoint blocked
```

Admin dapat memiliki policy berbeda jika memang membutuhkan private infrastructure.

---

# 48. OBSERVABILITY

Admin perlu mengetahui alasan routing.

Contoh request log:

```text
Request #ABC123

Task:
SCRIPT_GENERATION

User:
Member #102

Selected Provider:
Anthropic

Credential:
Pool #3

Model:
Model X

Reason:
Primary credential healthy
Quota available
Highest priority

Fallback:
Not used

Latency:
2.4s

Tokens:
...
Cost:
...
```

Ini membuat sistem dapat diaudit dan diperbaiki.

---

# 49. USER EXPERIENCE

Member tidak perlu mengetahui kompleksitas routing jika menggunakan Admin Pool.

UI cukup:

```text
AI Access

● Platform AI
   7 / 10 analyses remaining

○ My API Key
   Connected
```

Jika BYOK aktif:

```text
Using:
My OpenAI API

Platform quota:
Untouched
```

---

# 50. CORE ARCHITECTURE

Arsitektur akhir:

```text
                    ANALYSIS REQUEST
                           │
                           ▼
                    QUOTA SERVICE
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
                  PROFESSIONAL CLASS
```

---

# 51. V1 PRIORITY

Untuk V1, implementasi tidak harus langsung mengaktifkan semua provider.

Prioritas:

### Tier 1

```text
Admin Pool
BYOK
Multiple Credentials
Quota
Failover
Generic OpenAI-Compatible Provider
```

### Tier 2

```text
Provider Registry
Model Registry
Cost Tracking
Weighted Routing
Health Monitoring
```

### Tier 3

```text
Advanced Cost Optimization
Automatic Model Selection
Advanced Budgeting
Dynamic Routing
```

---

# 52. DEFINITION OF DONE — AI PROVIDER SYSTEM

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
10. Admin dapat menyediakan Free AI Pool.
11. Free AI Pool memiliki quota per member.
12. Free AI Pool memiliki budget.
13. Member dapat menggunakan BYOK.
14. Member dapat memiliki lebih dari satu BYOK.
15. BYOK tidak mengurangi Admin Pool quota.
16. Member dapat memilih BYOK sebagai primary jika diizinkan.
17. Admin dapat menentukan routing policy.
18. Sistem mendukung provider registry.
19. Sistem mendukung model registry.
20. Sistem mendukung custom Base URL.
21. Sistem mendukung custom authentication.
22. Sistem memiliki generic OpenAI-compatible adapter.
23. Credential disimpan secara aman.
24. Credential tidak pernah masuk client-side response.
25. Credential tidak pernah masuk log.
26. Custom Base URL memiliki SSRF protection.
27. AI usage dicatat.
28. Error provider dicatat.
29. Health status credential dapat diketahui.
30. Admin dapat melihat penggunaan dan estimasi biaya.
31. Core Analysis Engine tidak bergantung pada provider tertentu.

---

# 53. KETENTUAN PENTING UNTUK AI BUILDER

Jangan mengimplementasikan AI integration sebagai:

```text
Analysis → OpenAI API langsung
```

Itu terlalu rigid.

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

Tujuannya agar project dapat berpindah provider tanpa mengubah Analysis Engine.

Jangan membuat API pool hanya sebagai daftar API key.

Pool harus mempunyai:

- routing;
- quota;
- health;
- priority;
- weight;
- failover;
- usage tracking;
- budget;
- ownership.

BYOK juga bukan sekadar field API key di profile.

BYOK harus menjadi credential yang dapat dipakai oleh Credential Resolver.

Custom Provider bukan sekadar input Base URL.

Custom Provider harus melalui:

- URL validation;
- SSRF protection;
- authentication configuration;
- provider adapter;
- capability detection;
- health check.

---

# 54. HASIL YANG DIINGINKAN

Dengan arsitektur ini, project dapat memiliki model bisnis:

```text
FREE MEMBER
     ↓
Admin AI Pool
     ↓
Limited quota
```

```text
PREMIUM MEMBER
     ↓
Larger Admin Pool quota
     +
Optional BYOK
```

```text
POWER USER
     ↓
BYOK
     ↓
Own provider/account
     ↓
Own provider billing
```

Dan platform dapat berkembang menjadi:

```text
                    AI PLATFORM
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       ADMIN POOL       BYOK        CUSTOM API
          │              │              │
     Multiple Keys   Multiple Keys   Custom Base URL
          │              │              │
          └──────────────┼──────────────┘
                         │
                  PROVIDER ROUTER
                         │
                  MODEL ROUTER
                         │
                  ANALYSIS ENGINE
                         │
                  PROFESSIONAL CLASS
```

---

# 55. FINAL PRODUCT PRINCIPLE

Core principle:

> **The product owns the AI orchestration layer, not the AI provider.**

Provider dapat berubah.

API key dapat berubah.

Model dapat berubah.

Harga provider dapat berubah.

Bahkan provider dapat ditambahkan atau dihapus.

Tetapi:

```text
Analysis Engine
Professional Class
Quota System
Member Experience
```

tetap stabil.

Itulah alasan sistem **BYOK + API Pool + Provider Adapter + Model Router** harus menjadi bagian dari arsitektur sejak awal, meskipun V1 hanya mengaktifkan beberapa provider.

# PROJECT DEVELOPMENT WORKFLOW & AI BUILDER OPERATING RULES

## Purpose

Dokumen ini adalah operating specification untuk AI Builder/Coding Agent yang mengerjakan project. Simpan di repository bersama dokumen spesifikasi produk.

Tujuannya:
- menjaga AI memahami tujuan project;
- mencegah implementasi menyimpang;
- menjadi persistent project memory;
- memastikan pengerjaan bertahap dan dapat diaudit;
- menjaga perubahan repository tetap terkendali.

---

## 1. SOURCE OF TRUTH

Recommended documentation:

```text
/docs/
├── 01_PROJECT_CONCEPT.md
├── 02_AI_CONTENT_INTELLIGENCE_SPEC.md
├── 03_AI_PROVIDER_BYOK_POOL_SPEC.md
├── 04_PRODUCT_BUSINESS_SPEC.md
└── 05_PROJECT_DEVELOPMENT_WORKFLOW.md
```

Jika nama file berbeda, prinsipnya sama.

Priority:

```text
Latest explicit user decision
        ↓
Latest approved project specification
        ↓
Existing repository implementation
        ↓
AI Builder assumption
```

Jika ada konflik, jangan memilih diam-diam. Identifikasi dan gunakan keputusan terbaru yang disetujui.

---

## 2. FIRST ACTION — READ BEFORE WORK

Sebelum implementation:

```text
STOP
↓
READ PROJECT DOCUMENTATION
↓
READ REPOSITORY STRUCTURE
↓
UNDERSTAND CURRENT IMPLEMENTATION
↓
IDENTIFY FEATURES
↓
IDENTIFY GAPS
↓
ONLY THEN PLAN
```

Jangan langsung coding berdasarkan prompt terakhir tanpa membaca dokumentasi yang relevan.

---

## 3. READ-ONLY AUDIT FIRST

Jika repository sudah memiliki implementation, tahap pertama adalah READ-ONLY AUDIT.

Periksa:

```text
Repository Structure
Architecture
Frontend
Backend
Database
Authentication
Authorization
AI Integration
Payment
Environment
Security
Tests
Build
Deployment
Documentation
Existing Checklist
```

Pada audit:

```text
DO NOT
- modify files
- delete files
- create replacement files
- commit
- push
```

---

## 4. EXISTING CHECKLIST RULE

Jika repository sudah memiliki checklist/audit checklist:

> Gunakan checklist yang sudah ada.

Jika perlu tambahan, update checklist tersebut.

Jangan membuat checklist baru hanya karena lebih nyaman, kecuali user meminta.

---

## 5. IMPLEMENTATION MAPPING

Setelah audit:

```text
SPECIFICATION
↓
REQUIRED FEATURE
↓
CURRENT IMPLEMENTATION
↓
GAP
↓
IMPLEMENTATION PLAN
```

Jangan langsung coding sebelum memahami gap dan dependency.

---

## 6. PHASED IMPLEMENTATION

Recommended:

```text
PHASE 0  Documentation & Audit
PHASE 1  Foundation
PHASE 2  Authentication & Membership
PHASE 3  AI Provider Infrastructure
PHASE 4  Analysis Engine
PHASE 5  Professional Content Blueprint
PHASE 6  Quality Control / Multi-pass AI
PHASE 7  Payment & Finance
PHASE 8  Admin Dashboard
PHASE 9  Security Hardening
PHASE 10 Testing & Production Readiness
```

Jangan mengerjakan seluruh subsystem sekaligus jika meningkatkan risiko regression.

---

## 7. CHANGE CONTROL

Sebelum perubahan:

```text
WHAT    — apa yang diubah?
WHY     — mengapa?
WHERE   — module/file mana?
IMPACT  — apa yang terdampak?
RISK    — apa risiko regression?
TEST    — bagaimana diverifikasi?
```

Untuk perubahan besar, buat implementation plan.

---

## 8. NO ASSUMPTION RULE

Jangan mengganti requirement berdasarkan preferensi AI Builder.

Contoh:

```text
Starter = Rp10.000
Pro     = Rp75.000
Power   = Rp150.000
```

Tidak boleh diubah tanpa keputusan user.

Jika requirement tidak diketahui:

```text
FLAG UNKNOWN
```

Jangan mengarang.

---

## 9. NO FEATURE CREEP

Jangan menambahkan fitur hanya karena:
- terlihat menarik;
- umum pada SaaS lain;
- mudah dibuat;
- AI Builder menganggapnya bagus.

Fitur baru harus memiliki:

```text
Business reason
User value
Technical impact
```

Jika tidak jelas, jangan implementasikan.

---

## 10. CORE PRODUCT PRIORITY

Prioritas:

```text
1. ANALYSIS QUALITY
2. PROFESSIONAL CONTENT BLUEPRINT
3. AI PIPELINE
4. MEMBERSHIP
5. AI PROVIDER / BYOK / POOL
6. PAYMENT
7. ADMIN
8. SUPPORTING UX
```

Jangan memprioritaskan fitur sekunder ketika Analysis Engine masih generik.

---

## 11. AI ENGINE IS THE CORE

```text
UI ≠ Core Product
Payment ≠ Core Product
AI API Integration ≠ Core Product
```

Core:

```text
Content Intelligence
+
Prompt Architecture
+
Multi-pass Reasoning
+
Production Planning
+
Quality Control
```

---

## 12. PROFESSIONAL OUTPUT STANDARD

"Professional Class" berarti kualitas dan kelengkapan output layak digunakan sebagai bahan produksi profesional.

Bukan kursus.

Bukan sekadar label marketing.

Production-ready berarti creator, designer, editor, director, scriptwriter, atau AI operator dapat menggunakan blueprint tanpa menebak maksud dasar.

---

## 13. CAROUSEL OUTPUT

Untuk carousel/image, minimal:

```text
Overall Strategy
Recommended Slide Count
Narrative Structure

EVERY SLIDE:
- Slide Number
- Purpose
- Narrative Role
- Main Message
- Headline
- Body Copy
- Visual Concept
- Composition
- Subject
- Background
- Lighting
- Typography Direction
- Image Prompt
- Continuity Notes
- Production Notes
```

Jangan hanya menghasilkan daftar image prompts.

---

## 14. VIDEO OUTPUT

Untuk video, minimal:

```text
Duration
Hook
Retention Strategy
Story Architecture

EVERY SCENE:
- Scene ID
- Duration
- Purpose
- Narration
- On-Screen Text
- Visual Description
- Image Prompt
- Video Prompt
- Camera
- Lens
- Camera Movement
- Subject Movement
- Environment Movement
- Lighting
- Mood
- Voice Direction
- Music
- SFX
- Transition
- Production Notes
```

---

## 15. VIRAL CONTENT RULE

Jangan menjanjikan:

> "Konten ini pasti viral."

Gunakan:

```text
Viral Potential
Attention Potential
Retention Potential
Shareability
Saveability
Comment Potential
```

Score wajib memiliki reasoning.

---

## 16. QUALITY OVER LENGTH

```text
MORE WORDS ≠ MORE PROFESSIONAL
```

Prioritas:

```text
Better reasoning
Better decisions
Better structure
Better visuals
Better production clarity
```

---

## 17. MULTI-PASS AI

Jika memungkinkan:

```text
Research
↓
Intelligence
↓
Strategy
↓
Format Architecture
↓
Production Planning
↓
Script
↓
Visual Assets
↓
QA
↓
Revision
↓
Final
```

Jangan memaksa satu AI request melakukan semua pekerjaan jika kualitas turun.

---

## 18. PARTIAL REGENERATION

Jika satu stage gagal:

```text
DO NOT REGENERATE EVERYTHING
```

Regenerate stage yang gagal dan dependent stages saja.

---

## 19. AI PROVIDER ABSTRACTION

Analysis Engine tidak boleh terikat langsung pada provider.

Gunakan:

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
```

Provider dapat diganti tanpa membongkar Analysis Engine.

---

## 20. BYOK / API POOL

Platform harus siap untuk:

```text
Admin AI Pool
Member BYOK
BYOK Pool
Multiple Providers
Custom Provider
Custom Base URL
Routing
Failover
Quota
Usage Metering
Cost Tracking
```

### Starter — Rp10.000

```text
Admin AI Pool
10 analysis/month
Limited models
```

### Pro — Rp75.000

```text
Larger Admin AI Pool
BYOK 1 API
More analysis
Better models
Priority processing
```

### Power — Rp150.000

```text
Larger Admin AI Pool
BYOK pool
Multiple providers
Custom API
Custom Base URL
Advanced routing
```

Tidak ada public Free Plan.

Admin dapat membuat complimentary member secara manual.

---

## 21. PAYMENT / FINANCE

Payment:

```text
Xendit
NOWPayments
Manual Transfer
```

Payment credentials harus terpisah dari AI credentials.

Transaction states:

```text
PENDING
PAID
FAILED
EXPIRED
CANCELLED
REFUNDED
```

Manual transfer memerlukan admin verification.

---

## 22. SECURITY

Wajib:

```text
Authentication
Authorization
Password Security
Secret Encryption
API Key Masking
No Credential Logging
Webhook Verification
Rate Limiting
Input Validation
SSRF Protection
Audit Logging
```

Custom Base URL harus memblokir minimal:

```text
localhost
127.0.0.0/8
private IP ranges
loopback
cloud metadata endpoints
```

---

## 23. TESTING

Setiap implementation stage harus memiliki verification.

Minimal:

```text
Unit Tests
Integration Tests
API Tests
Database Tests
Authentication Tests
Authorization Tests
AI Provider Tests
Payment Tests
Security Tests
Build Test
```

AI output menggunakan golden test cases.

---

## 24. GOLDEN TEST SET

Kategori minimal:

```text
History
Education
Technology
Business
Entertainment
Science
Lifestyle
Storytelling
Product
Documentary
```

Evaluasi:

```text
Strategic Quality
Audience Fit
Hook Quality
Differentiation
Story Quality
Visual Quality
Production Readiness
Factual Integrity
Overall Quality
```

Perubahan prompt/model tidak dianggap aman sebelum golden test dijalankan.

---

## 25. PROMPT VERSIONING

Production prompts memiliki version:

```text
topic_analysis_v1
audience_analysis_v1
viral_analysis_v2
carousel_engine_v1
video_engine_v1
image_prompt_v2
video_prompt_v2
qa_engine_v1
```

Simpan jika memungkinkan:

```text
Prompt Version
Model
Provider
Timestamp
```

---

## 26. COST CONTROL

Catat:

```text
Provider
Credential
Model
Tokens
Estimated Cost
Analysis ID
Member
Stage
```

Architecture mendukung:

```text
Member Quota
Provider Quota
Credential Quota
Global Pool Budget
```

---

## 27. DATABASE CHANGES

Sebelum schema change:

```text
Understand current schema
↓
Identify dependencies
↓
Create migration
↓
Test migration
↓
Verify impact
```

Jangan menghapus data/kolom penting untuk merapikan schema tanpa approval.

---

## 28. ENVIRONMENT & SECRETS

Jangan hardcode:

```text
API Keys
Payment Keys
Passwords
Tokens
Secrets
```

Jangan commit secret ke repository.

---

## 29. ERROR HANDLING

Error harus:

- jelas;
- actionable;
- tidak membocorkan secret;
- tercatat secara aman;
- memiliki fallback jika tersedia.

AI failure:

```text
Provider Failure
↓
Retry if appropriate
↓
Failover if policy allows
↓
Controlled Error
```

---

## 30. LOGGING

Log cukup untuk debugging tetapi tidak mencatat secret.

Contoh:

```text
Request ID
Analysis ID
Member ID
Provider
Model
Stage
Latency
Token Usage
Cost
Status
Error Category
```

Jangan log API key mentah.

---

## 31. GIT WORKFLOW

Urutan:

```text
READ
↓
AUDIT
↓
PLAN
↓
IMPLEMENT
↓
TEST
↓
REVIEW
↓
DOCUMENT
```

### Push rule

> Jangan melakukan push ke remote tanpa izin eksplisit user.

Izin satu push hanya berlaku untuk push tersebut.

Jangan menganggap:

```text
lanjutkan
kerjakan
perbaiki
selesaikan
```

sebagai izin push.

Jika pekerjaan siap dipush:

```text
STOP
↓
REPORT READY
↓
REQUEST EXPLICIT PUSH APPROVAL
```

---

## 32. CHANGE SUMMARY

Setelah implementation:

```text
Changed:
- ...

Added:
- ...

Removed:
- ...

Tests:
- ...

Remaining:
- ...

Risk:
- ...
```

Jangan menyatakan 100% selesai jika masih ada gap.

---

## 33. AUDIT COMPLETION STANDARD

Audit lengkap harus mencakup:

```text
Repository
Architecture
Frontend
Backend
Database
Authentication
Authorization
AI
Prompts
Provider Infrastructure
BYOK
API Pool
Quota
Payment
Finance
Security
Testing
Deployment
Documentation
```

Jika area tidak dapat diperiksa, nyatakan secara eksplisit.

---

## 34. DOCUMENTATION MAINTENANCE

Jika keputusan berubah:

1. identifikasi dokumen terdampak;
2. update dokumen;
3. catat perubahan;
4. pastikan implementation mengikuti versi terbaru.

Dokumentasi tidak boleh sengaja dibiarkan berbeda dengan implementation.

---

## 35. DECISION LOG

Keputusan penting dicatat:

```text
Date:
Decision:
Reason:
Impact:
Affected Components:
```

Contoh:

```text
Decision:
No public Free Plan.

Reason:
Product economics and positioning.

Impact:
Membership, quota, AI Pool, payment.
```

---

## 36. CONFLICT HANDLING

Jika:

```text
DOCUMENT A → X
DOCUMENT B → Y
```

Jangan memilih diam-diam.

Lakukan:

```text
Identify Conflict
↓
Determine Latest Approved Decision
↓
If Unclear, Flag It
↓
Do Not Silently Rewrite Requirements
```

---

## 37. AI BUILDER MEMORY RECOVERY

Jika memulai session baru atau kehilangan context:

```text
READ:
1. Project Concept
2. AI Content Intelligence Specification
3. AI Provider / BYOK / Pool Specification
4. Product Business Specification
5. This Development Workflow
6. Existing Checklist
7. Decision Log
```

Kemudian:

```text
Audit Current Repository State
↓
Compare Implementation Against Documentation
↓
Continue From Actual State
```

Jangan mengandalkan model memory saja.

> **Repository documentation adalah persistent project memory.**

---

## 38. NEVER REINVENT THE PROJECT

Jika lupa detail:

```text
DO NOT GUESS
```

Baca kembali dokumentasi.

Jika tetap tidak ditemukan:

```text
FLAG UNKNOWN
```

Jangan menciptakan requirement baru.

---

## 39. USER AUTHORITY

AI Builder boleh:

- menganalisa;
- merekomendasikan;
- menemukan risiko;
- menyarankan perbaikan.

AI Builder tidak boleh diam-diam:

- mengubah business model;
- mengubah pricing;
- menghapus feature requirement;
- mengubah core architecture;
- mengubah provider policy;
- menganggap perubahan sebagai approved.

---

## 40. READY-TO-CONTINUE CHECK

Sebelum melanjutkan pekerjaan dari session sebelumnya:

```text
[ ] Read project docs
[ ] Read development workflow
[ ] Read existing checklist
[ ] Audit repository state
[ ] Identify current phase
[ ] Identify completed work
[ ] Identify incomplete work
[ ] Identify known issues
[ ] Identify pending decisions
[ ] Identify unapproved changes
[ ] Define next action
```

Baru kemudian lanjut.

---

# FINAL OPERATING PRINCIPLE

```text
READ
↓
UNDERSTAND
↓
AUDIT
↓
PLAN
↓
IMPLEMENT
↓
TEST
↓
REVIEW
↓
DOCUMENT
↓
WAIT FOR APPROVAL WHEN REQUIRED
```

Bukan:

```text
PROMPT
↓
CODE IMMEDIATELY
↓
ASSUME
↓
PUSH
```

Jika ragu, kembali ke dokumentasi.

Jika dokumentasi tidak cukup, jangan menebak requirement.

Jika perubahan belum diotorisasi, jangan melakukan perubahan yang memerlukan otorisasi tersebut.

Jika implementation berbeda dari specification, identifikasi dan laporkan gap.

Jika pekerjaan belum benar-benar selesai, jangan menyatakan selesai.

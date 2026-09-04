# 05 — PROJECT DECISIONS

Dokumen ini adalah **living decision log** untuk menyimpan keputusan bisnis, produk, arsitektur, dan engineering yang telah disepakati.

## Rule

Jika keputusan baru bertentangan dengan dokumen lama, keputusan terbaru yang secara eksplisit disetujui user menjadi sumber kebenaran. Jangan mengubah keputusan penting berdasarkan asumsi.

## Decision 001 — No Public Free Plan

**Decision:** Platform tidak memiliki public Free Plan.

**Current plans:**
- Starter — Rp10.000/month
- Pro — Rp75.000/month
- Power — Rp150.000/month

**Reason:** positioning sebagai produk SaaS berbayar dan menjaga economics AI infrastructure.

**Impact:** membership, quota, AI Pool, payment, onboarding.

## Decision 002 — Starter Pricing

**Decision:** Starter = Rp10.000/bulan.

**Entitlement:** Admin AI Pool, 10 analysis/month, limited models.

## Decision 003 — Pro Pricing

**Decision:** Pro = Rp75.000/bulan.

**Entitlement:** larger Admin AI Pool, BYOK 1 API, more analysis, better models, priority processing.

## Decision 004 — Power Pricing

**Decision:** Power = Rp150.000/bulan.

**Entitlement:** larger Admin AI Pool, BYOK pool, multiple providers, custom API, custom Base URL, advanced routing.

## Decision 005 — Complimentary Members

**Decision:** Tidak ada public Free Plan, tetapi admin dapat membuat complimentary/special members secara manual.

Admin dapat menentukan custom quota, expiry, access, provider/model, BYOK access, dan status.

## Decision 006 — Professional Class Meaning

**Decision:** "Professional Class" bukan halaman kursus dan bukan fitur pembelajaran. Istilah tersebut berarti **standar kualitas output**.

Output harus professional-grade, lengkap, jelas, actionable, dan production-ready.

## Decision 007 — Core Product Differentiator

**Decision:** Diferensiasi utama bukan sekadar AI text generation.

Nilai utama adalah:
1. kualitas analysis;
2. strategic reasoning;
3. content blueprint;
4. production-ready detail;
5. blueprint image/carousel dan video.

## Decision 008 — Multi-Pass AI

**Decision:** AI tidak menggunakan satu prompt monolitik.

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

## Decision 009 — Professional Blueprint

Final output dapat mencakup:

1. Executive Summary
2. Topic Analysis
3. Audience Analysis
4. Content Opportunity
5. Viral Potential
6. Competitive Differentiation
7. Recommended Angle
8. Hook Options
9. Selected Hook
10. Content Objective
11. Story Architecture
12. Content Format Strategy
13. Carousel Plan
14. Video Plan
15. Complete Script
16. Scene Breakdown
17. Global Visual Bible
18. Character Profiles
19. Image Prompts
20. Video Prompts
21. Camera Direction
22. Voice Direction
23. Music Direction
24. Sound Effects
25. On-Screen Text
26. Captions
27. Transitions
28. Production Notes
29. Fact-Check Notes
30. Quality Control
31. Final Production Checklist

Section yang tidak relevan dengan format boleh dilewati.

## Decision 010 — Carousel Standard

Carousel bukan sekumpulan image yang berdiri sendiri.

AI harus menentukan jumlah slide, fungsi setiap slide, headline, body copy, visual concept, image prompt, composition, typography direction, continuity, dan production notes.

Setiap slide harus memiliki narrative purpose.

## Decision 011 — Video Standard

Video output bukan hanya script.

AI harus menghasilkan bila relevan: duration, hook, first frame, first 1–3 seconds, retention structure, open loops, micro-payoffs, scene breakdown, narration, on-screen text, visual direction, image prompt, video prompt, camera, lens, movement, lighting, audio, SFX, transition, dan production notes.

## Decision 012 — Viral Potential

AI harus menganalisis faktor attention dan sharing, termasuk Hook Strength, Curiosity, Novelty, Emotional Impact, Surprise, Relatability, Visual Stopping Power, Information Value, Story Potential, Retention Potential, Shareability, Saveability, Comment Potential, Audience Fit, Timeliness, Cultural Relevance, Differentiation, dan Production Feasibility.

**Important:** Sistem tidak boleh menjamin suatu konten akan viral.

## Decision 013 — Factual Integrity

AI tidak boleh mengarang facts, statistics, quotes, events, atau trends.

Informasi dibedakan antara:
- VERIFIED;
- INFERENCE;
- CREATIVE INTERPRETATION;
- SPECULATION;
- REQUIRES FACT CHECK.

Engagement tidak boleh mengorbankan factual integrity.

## Decision 014 — AI Provider Abstraction

Analysis Engine tidak boleh bergantung langsung pada provider tertentu.

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

## Decision 015 — Admin AI Pool

Admin dapat memiliki pool credential dari satu atau beberapa provider.

Pool mendukung multiple credentials, priority, weight, round robin, health-based routing, failover, rate-limit handling, dan circuit breaker.

## Decision 016 — BYOK

BYOK = Bring Your Own Key.

Creator: BYOK setelah quota Admin AI Pool habis, sehingga member dapat melanjutkan analysis menggunakan API miliknya sendiri.

BYOK dapat menggunakan credential provider yang didukung platform. Detail jumlah credential/provider yang dapat disimpan harus mengikuti entitlement implementasi dan tidak boleh diasumsikan lebih luas dari keputusan ini.

Credential wajib encrypted at rest, masked, tidak masuk log, dapat di-revoke, dan health checked.

## Decision 017 — Custom Base URL Security

Custom Base URL hanya boleh diproses melalui backend dengan SSRF protection.

Minimal: block localhost, loopback, private IP, link-local, cloud metadata; DNS/IP validation; redirect validation; HTTPS; timeout; response size limit.

## Decision 018 — Payment Providers

Payment platform mendukung:
- Xendit;
- NOWPayments;
- manual transfer.

Payment credentials harus terpisah dari AI credentials.

## Decision 019 — Payment Status

```text
PENDING
PAID
FAILED
EXPIRED
CANCELLED
REFUNDED
```

Manual transfer:

```text
Invoice
↓
Transfer
↓
Proof Upload
↓
Admin Verification
↓
Approve / Reject
↓
Membership Activation
```

## Decision 020 — Git Push Permission

AI Builder tidak boleh push ke remote repository tanpa explicit user approval.

Kata seperti "lanjutkan", "kerjakan", "perbaiki", atau "selesaikan" tidak otomatis berarti izin push.

## Decision 021 — Repository as Source of Truth

Saat mengerjakan project:
1. baca dokumen project;
2. baca workflow;
3. baca decision log;
4. inspect repository;
5. audit implementation;
6. baru lakukan perubahan.

Jangan mengandalkan memory model sebagai sumber kebenaran project.

## Decision 022 — No Feature Creep

Jangan menambahkan feature, plan, pricing, provider policy, architecture, atau business rule yang belum disetujui jika perubahan tersebut memengaruhi core product.

AI boleh memberikan rekomendasi, tetapi keputusan final berada pada user.

## Decision 023 — Quality Over Length

Output yang panjang tidak otomatis lebih baik.

Prioritas:

```text
Accuracy
↓
Strategic Quality
↓
Clarity
↓
Actionability
↓
Production Readiness
↓
Detail
↓
Style Polish
```

## Decision 024 — Partial Regeneration

Jika satu stage gagal, jangan selalu regenerate seluruh pipeline. Regenerate stage yang gagal dan dependents yang terdampak.

Tujuan: mengurangi cost/latency dan menjaga output yang sudah valid.

## Decision 025 — Prompt Versioning

Hidden prompts harus memiliki versioning agar output dapat dibandingkan, regression dideteksi, golden test digunakan, dan rollback memungkinkan.

## Decision 026 — Golden Test Set

AI quality diuji lintas kategori:
- History;
- Education;
- Technology;
- Business;
- Entertainment;
- Science;
- Lifestyle;
- Storytelling;
- Product;
- Documentary.

Metric dapat mencakup quality score, user rating, revision rate, generation cost, dan completion rate.

## Decision 027 — Security Hierarchy

Prompt hierarchy:

```text
System
↓
Engine
↓
Task
↓
User
```

User input tidak boleh override system-level safety, security, factuality, atau quality rules. External web content dianggap untrusted data dan tidak boleh diperlakukan sebagai system instruction.

## Decision 028 — Current Product Priority

Prioritas implementasi:
1. Analysis quality
2. Professional Content Blueprint
3. AI pipeline
4. Membership
5. AI Provider / BYOK / Pool
6. Payment
7. Admin
8. Supporting UX

## Decision 029 — Decision Log Maintenance

Setiap keputusan baru yang memengaruhi business model, pricing, membership, AI architecture, provider policy, payment, security, atau production workflow harus ditambahkan ke dokumen ini.

Format:

```markdown
## Decision XXX — Title

**Decision:** ...

**Reason:** ...

**Impact:** ...
```

Jangan menghapus keputusan lama tanpa alasan dan persetujuan.

## Decision 030 — Current Membership Plans Revision

**Decision:** Struktur membership terbaru menggantikan struktur Starter / Pro / Power sebelumnya.

**Current plans:**

### Starter — Rp30.000/bulan
- Admin AI Pool
- Quota 5 analysis/bulan
- Fitur analisa terbatas
- Tidak ada BYOK entitlement

### Growth — Rp90.000/bulan
- Admin AI Pool
- Quota 20 analysis/bulan
- Fitur analisa FULL
- Tidak ada BYOK entitlement yang ditetapkan oleh keputusan ini

### Creator — Rp270.000/bulan
- Admin AI Pool
- Quota 75 analysis/bulan
- Fitur analisa FULL
- BYOK tersedia setelah quota Admin AI Pool habis, sehingga member dapat melanjutkan menggunakan API miliknya sendiri

**Supersedes:** Decision 002, Decision 003, Decision 004, dan bagian plan lama yang bertentangan di dokumen project.

**Impact:** membership pricing, entitlement, analysis quota, AI Pool policy, BYOK availability, payment, onboarding, dan UI membership.

**Important:** Keputusan ini tidak mengubah keputusan No Public Free Plan atau aturan complimentary member kecuali dinyatakan secara eksplisit dalam keputusan baru.

## Decision 031 — Product Sitemap, Analysis Workspace, and Navigation

**Decision:** Struktur halaman dan fungsi utama Liteslice didokumentasikan dalam `docs/08_PRODUCT_SITEMAP_AND_INFORMATION_ARCHITECTURE.md`.

Untuk Member Area, **Create New Analysis** harus berbentuk multi-stage analysis workspace dengan progress navigation horizontal di bagian atas workspace, tepat di bawah header.

Tahapan yang digunakan:

```text
01 Source
→ 02 Intelligence
→ 03 Strategy
→ 04 Production
→ 05 Quality
→ 06 Final
```

Progress tersebut **bukan submenu vertical di sidebar**.

Source pada Stage 01 mendukung tiga mode konseptual:
- manual idea;
- article/video link;
- uploaded file.

Ketiga mode tersebut diarahkan ke downstream analysis pipeline yang sama setelah source ingestion/content extraction/source understanding.

Content format pada initial selection hanya:
- Carousel;
- Short Video;
- Long Video.

Field Platform dihapus dari initial format selection, dan pilihan Both tidak digunakan pada initial selection.

**Admin:** struktur Admin mencakup domain Finance untuk revenue, expenses, Profit & Loss, transactions, invoices, payments, serta AI cost reporting.

**Reason:** menjaga Create New Analysis tetap sederhana, memberikan pemahaman progres yang jelas tanpa membebani sidebar, menyatukan berbagai source input ke intelligence pipeline yang sama, dan memastikan struktur admin mencakup operational serta financial management.

**Impact:** information architecture, member navigation, analysis workspace UX, source ingestion flow, content format selection, admin navigation, finance reporting requirements, dan UI planning.

**Implementation note:** Decision ini menetapkan product/UX direction dan information architecture. Detail teknologi extraction, file support, database schema, accounting treatment, dan route implementation belum otomatis ditetapkan.

# Current Status

Dokumen ini merupakan baseline keputusan project. Jika terdapat konflik antara dokumen lama dan keputusan terbaru yang telah disetujui user, keputusan terbaru menjadi acuan dan dokumen terkait harus diperbarui agar kembali konsisten.

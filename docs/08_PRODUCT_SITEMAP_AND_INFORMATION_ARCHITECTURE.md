# 08 — PRODUCT SITEMAP & INFORMATION ARCHITECTURE

> **Status:** Product direction documented from the latest approved UX discussion. This document defines page/menu structure and the intended function of each area. It does not by itself mean the UI or backend has been implemented.

## 1. PURPOSE

Dokumen ini menjadi acuan struktur halaman, menu, dan alur navigasi Liteslice untuk Public Area, Member Area, dan Admin Area.

Prinsip utama:

- navigasi harus sederhana dan mudah dipahami;
- Create New Analysis adalah workspace bertahap, bukan sekadar form tunggal;
- progress Create New Analysis ditampilkan secara **horizontal di atas workspace, tepat di bawah header**, bukan sebagai submenu vertical di sidebar;
- sumber input yang berbeda harus masuk ke downstream analysis pipeline yang sama;
- content format pada initial analysis hanya terdiri dari Carousel, Short Video, dan Long Video;
- Platform tidak menjadi field pada Step 1/format selection;
- hasil analysis tetap mengikuti entitlement membership, termasuk Included / Preview / Locked untuk fitur intelligence yang belum tersedia pada plan tertentu;
- Admin memiliki area Finance untuk memantau revenue, expenses, AI cost, profit, dan financial reporting.

---

# 2. INFORMATION ARCHITECTURE

```text
LITESLICE
│
├── PUBLIC
│   ├── Home
│   ├── Features
│   ├── How It Works
│   ├── Pricing
│   ├── Showcase
│   ├── FAQ
│   ├── Login
│   ├── Register
│   ├── Checkout
│   ├── Terms
│   └── Privacy
│
├── MEMBER
│   ├── Dashboard
│   ├── Create New Analysis
│   │   └── Analysis Workspace
│   │       ├── 01 Source
│   │       ├── 02 Intelligence
│   │       ├── 03 Strategy
│   │       ├── 04 Production
│   │       ├── 05 Quality
│   │       └── 06 Final
│   ├── History
│   ├── Membership
│   └── Settings
│       └── BYOK
│
└── ADMIN
    ├── Dashboard
    ├── Members
    ├── Memberships
    ├── AI
    │   ├── AI Pool
    │   ├── Providers
    │   ├── Credentials
    │   ├── Models
    │   ├── Routing
    │   ├── Usage
    │   └── AI Costs
    ├── Finance
    │   ├── Overview
    │   ├── Revenue
    │   ├── Expenses
    │   ├── Profit & Loss
    │   ├── Transactions
    │   ├── Invoices
    │   └── Payments
    └── System
        ├── Analysis Jobs
        ├── QA
        ├── Golden Tests
        ├── Audit Logs
        └── Settings
```

---

# 3. PUBLIC AREA

## 3.1 Home

Fungsi:

- menjelaskan positioning Liteslice;
- menunjukkan transformasi dari ide menjadi Professional Content Blueprint;
- menjelaskan value utama: analysis, strategy, production planning, dan professional output;
- mengarahkan user ke Features, Showcase, Pricing, Register/Login.

Core visual/message:

```text
IDEA
↓
AI CONTENT INTELLIGENCE
↓
STRATEGY
↓
PROFESSIONAL BLUEPRINT
↓
READY TO PRODUCE
```

## 3.2 Features

Fungsi:

- menjelaskan kemampuan platform;
- memperlihatkan kedalaman intelligence dan production planning;
- menjelaskan perbedaan antara output Liteslice dan generic AI writer/script generator.

## 3.3 How It Works

Fungsi:

- menjelaskan workflow dari source/input sampai final blueprint;
- membantu user memahami bahwa AI tidak langsung membuat script;
- memperlihatkan tahapan analysis → strategy → production → QA.

## 3.4 Pricing

Fungsi:

- menampilkan membership saat ini;
- menjelaskan quota dan akses analysis;
- menjelaskan BYOK sesuai entitlement Creator;
- mengarahkan user ke checkout/register.

Current plans harus mengikuti `docs/06_CURRENT_MEMBERSHIP_PLANS.md` dan Decision 030.

## 3.5 Showcase

Fungsi:

- memperlihatkan contoh Professional Content Blueprint;
- memberikan visual proof mengenai kualitas output;
- membantu user memahami detail production-ready output yang sulit dijelaskan hanya dengan copy marketing.

## 3.6 FAQ

Fungsi:

- menjawab pertanyaan umum mengenai analysis, membership, quota, AI Pool, BYOK, payment, dan output.

## 3.7 Login / Register

Fungsi:

- autentikasi member;
- onboarding account;
- mengarahkan user ke Member Dashboard setelah berhasil masuk.

## 3.8 Checkout

Fungsi:

- memilih plan;
- membuat invoice/transaction;
- memproses payment provider yang tersedia;
- menangani status payment sesuai payment architecture.

## 3.9 Terms / Privacy

Fungsi:

- dokumen legal dan privacy policy platform.

---

# 4. MEMBER AREA

## 4.1 Dashboard

Dashboard adalah titik masuk utama setelah login.

Fungsi utama:

- menampilkan status membership;
- menampilkan sisa quota analysis;
- menyediakan CTA utama **Create New Analysis**;
- menampilkan analysis terbaru/history ringkas;
- menampilkan contextual upgrade information bila relevan;
- menampilkan status BYOK untuk Creator bila applicable.

Dashboard tidak boleh menjadi halaman yang terlalu kompleks. Core member experience tetap:

```text
Dashboard
↓
Create New Analysis
↓
Analysis Result / Workspace
↓
History
```

## 4.2 Create New Analysis

Create New Analysis adalah **analysis workspace multi-stage**.

Ini bukan satu form panjang dan bukan halaman yang memiliki submenu vertical di sidebar.

### Horizontal Progress Navigation

Progress navigation diletakkan:

```text
Header
↓
Horizontal Stage Navigation
↓
Current Workspace Content
```

Contoh:

```text
01 Source → 02 Intelligence → 03 Strategy → 04 Production → 05 Quality → 06 Final
```

Progress bersifat horizontal dan berada di bagian atas workspace, tepat di bawah header.

Tidak menggunakan:

```text
Sidebar
├── 01 Source
├── 02 Intelligence
├── 03 Strategy
└── ...
```

### Stage State

Setiap stage dapat memiliki state:

- **Active** — stage sedang dibuka/dikerjakan;
- **Completed** — stage telah selesai dan dapat ditinjau kembali;
- **Upcoming** — stage berikutnya;
- **Locked / unavailable** — hanya bila entitlement atau dependency memang membatasi akses.

### 4.2.1 Stage 01 — Source

Tujuan: menerima dan memahami sumber konten awal.

Input modes:

```text
IDEA
ARTICLE / VIDEO LINK
UPLOADED FILE
```

#### Manual Idea

User dapat mengetik ide/topik secara langsung.

#### Link

User dapat memasukkan link artikel atau video untuk dijadikan sumber analysis.

#### Upload

User dapat mengunggah file sebagai sumber analysis.

Semua mode input harus diarahkan ke konsep pipeline yang sama:

```text
IDEA / LINK / UPLOAD
↓
SOURCE INGESTION
↓
CONTENT EXTRACTION
↓
SOURCE UNDERSTANDING
↓
CONTENT ANALYSIS
↓
SAME AI INTELLIGENCE PIPELINE
↓
PROFESSIONAL CONTENT BLUEPRINT
```

Catatan: jenis file yang didukung, metode extraction, parser, transcription, OCR, dan detail teknologi belum ditetapkan oleh dokumen ini. Jangan mengasumsikan implementasi teknis sebelum ada keputusan/requirement tambahan.

### 4.2.2 Stage 02 — Intelligence

Tujuan: memahami topic dan content opportunity sebelum keputusan strategy dibuat.

Area intelligence dapat mencakup:

- Topic Analysis;
- Audience Insight;
- Context;
- Content Opportunity;
- Attention Analysis;
- Viral Potential;
- Competitive Differentiation;
- Unexpected Insight;
- factual grounding dan fact-check notes bila relevan.

Entitlement membership menentukan apakah bagian tertentu tampil sebagai Included, Preview, atau Locked sesuai feature access yang telah ditetapkan.

Starter tetap harus mendapatkan hasil yang berkualitas tinggi pada scope yang diberikan. Limitasi Starter adalah **scope/depth yang tersedia**, bukan penurunan kualitas reasoning dasar.

### 4.2.3 Stage 03 — Strategy

Tujuan: mengubah intelligence menjadi keputusan konten.

Dapat mencakup:

- Recommended Angle;
- Alternative Angles;
- Hook Recommendation;
- Hook Scoring;
- Content Objective;
- Story Architecture;
- Format Strategy.

Stage ini harus menjawab:

```text
What should we say?
Why this angle?
Why this hook?
How should the story progress?
Why is this format appropriate?
```

### 4.2.4 Stage 04 — Production

Tujuan: mengubah strategy menjadi blueprint produksi.

Initial content format taxonomy:

```text
Carousel
Short Video
Long Video
```

**Platform field tidak digunakan pada format selection ini.** User secara natural dapat menentukan platform distribusinya sendiri.

Tidak ada pilihan initial **Both** pada tahap format selection. Jika di masa depan dibutuhkan beberapa output format dari satu analysis, dapat dibuat sebagai variant/generation flow tersendiri tanpa menambah kompleksitas pada initial selection.

Production output dapat mencakup sesuai format:

### Carousel

- slide structure;
- narrative role;
- headline/body copy;
- visual concept;
- image prompt;
- composition;
- typography direction;
- continuity notes;
- production notes.

### Short Video

- duration;
- first-frame strategy;
- first 1–3 second hook;
- retention structure;
- scene breakdown;
- narration;
- on-screen text;
- visual direction;
- image/video prompts;
- camera/lens/movement;
- lighting;
- audio/SFX;
- transitions;
- production notes.

### Long Video

- longer narrative architecture;
- chapters/sections when relevant;
- hook/open loops;
- progression;
- micro-payoffs;
- climax;
- final payoff;
- scene/sequence blueprint;
- script and production details.

## 4.2.5 Stage 05 — Quality

Tujuan: memastikan hasil siap digunakan.

Quality layer dapat mencakup:

- factual integrity;
- fact-check notes;
- consistency;
- narrative coherence;
- production feasibility;
- prompt consistency;
- quality checks;
- revision/refinement bila entitlement tersedia.

Stage ini bukan sekadar spelling check. QA harus memvalidasi kualitas strategic dan production output sesuai pipeline.

## 4.2.6 Stage 06 — Final

Tujuan: menyajikan hasil akhir sebagai **Professional Content Blueprint**.

Fungsi:

- menampilkan final structured output;
- menyediakan review terakhir;
- menyediakan download/export action;
- mempertahankan section structure yang relevan dengan format yang dipilih.

Output final harus dapat dipahami production team tanpa mengharuskan mereka menebak maksud creator.

---

# 5. ANALYSIS RESULT & ENTITLEMENT UX

Analysis Workspace dan final result harus menggunakan prinsip visibility berikut:

```text
INCLUDED
↓
PREVIEW
↓
LOCKED
```

### Included

Feature tersedia penuh sesuai entitlement.

### Preview

User mendapatkan core value yang relevan, sementara depth tambahan diperlihatkan sebagai preview/contextual upgrade opportunity.

### Locked

Capability ditampilkan secara jelas sebagai capability yang tersedia pada tier tertentu, tetapi akses penuh belum tersedia.

Contoh contextual CTA:

- Unlock 2 alternative angles
- See full viral factor breakdown
- Unlock complete production blueprint
- Get advanced audience intelligence

Jangan menggunakan fake findings, fake scarcity, atau dark-pattern messaging.

Jika suatu intelligence stage memang tidak dijalankan karena entitlement, UI harus menyatakan bahwa full analysis mencakup capability tersebut, bukan berpura-pura bahwa sistem telah menemukan insight tertentu.

Arsitektur konseptual:

```text
AI Intelligence Engine
↓
Analysis Result
↓
Entitlement Resolver
↓
Output Visibility Policy
↓
UI Renderer
```

---

# 6. HISTORY

Fungsi:

- daftar analysis sebelumnya;
- search/filter bila dibutuhkan;
- membuka kembali analysis workspace/result;
- melihat status analysis;
- melanjutkan analysis yang belum final bila workflow mendukung;
- membuka final blueprint.

History adalah archive kerja member, bukan sekadar daftar transaksi.

---

# 7. MEMBERSHIP

Fungsi:

- menampilkan plan aktif;
- quota usage;
- renewal/expiry information;
- plan comparison;
- upgrade path;
- status entitlement;
- informasi BYOK sesuai plan.

Current membership authority tetap:

- Starter — Rp30.000/bulan, 5 analysis, limited analysis features;
- Growth — Rp90.000/bulan, 20 analysis, full analysis features;
- Creator — Rp270.000/bulan, 75 analysis, full analysis features, BYOK setelah Admin AI Pool quota habis.

Detail individual feature gating harus mengikuti keputusan product yang telah disetujui dan tidak boleh diinventasikan oleh UI.

---

# 8. SETTINGS

Fungsi umum:

- profile/account settings;
- preferences;
- language bila tersedia;
- security/account controls;
- AI credential settings sesuai entitlement.

## 8.1 BYOK

BYOK menjadi submenu/area settings yang hanya dapat digunakan oleh member dengan entitlement yang sesuai.

Creator dapat menggunakan API miliknya sendiri setelah quota Admin AI Pool habis sesuai Decision 030.

Credential security harus mengikuti aturan AI Provider/BYOK: encrypted at rest, masked, tidak masuk log, dapat di-revoke, dan health checked.

---

# 9. ADMIN AREA

Admin Area digunakan untuk operational control, AI infrastructure, membership, analysis operations, quality, audit, dan financial management.

## 9.1 Dashboard

Fungsi:

- Business Overview;
- active members;
- analysis volume;
- revenue;
- AI cost;
- profit;
- system health;
- operational alerts.

Dashboard menjadi ringkasan lintas domain, bukan pengganti halaman detail.

## 9.2 Members

Fungsi:

- melihat member;
- membership status;
- quota;
- account status;
- complimentary/special member management;
- custom entitlement sesuai authorization admin.

## 9.3 Memberships

Fungsi:

- plan configuration/visibility;
- membership lifecycle;
- quota policy;
- entitlement management;
- membership-related operational data.

Current public plans tetap mengikuti canonical membership documents.

---

# 10. ADMIN — AI

## 10.1 AI Pool

Fungsi:

- melihat pool credential;
- pool health;
- usage;
- budget/quota;
- failover status;
- routing behavior.

## 10.2 Providers

Fungsi:

- provider registry;
- provider capabilities;
- adapter configuration;
- provider status.

## 10.3 Credentials

Fungsi:

- admin AI credentials;
- credential status;
- health checks;
- usage/cost association;
- revoke/rotate controls.

## 10.4 Models

Fungsi:

- model registry;
- capability matrix;
- model availability;
- task suitability;
- pricing/cost metadata bila tersedia.

## 10.5 Routing

Fungsi:

- priority;
- weighted routing;
- round robin;
- least usage;
- health-based routing;
- failover;
- circuit breaker;
- task-based model/provider routing.

## 10.6 Usage

Fungsi:

- token usage;
- request volume;
- model usage;
- provider usage;
- member/analysis usage;
- error/rate-limit patterns.

## 10.7 AI Costs

Fungsi:

- estimated AI cost;
- actual AI cost bila tersedia;
- cost per provider/model;
- cost per analysis;
- cost allocation untuk financial reporting.

Arsitektur:

```text
AI Usage
↓
AI Cost Calculation
↓
Cost Ledger
↓
Financial Reporting
```

AI cost tidak boleh diperlakukan sebagai angka finance mentah tanpa cost calculation/ledger yang sesuai.

---

# 11. ADMIN — FINANCE

Finance adalah domain tersendiri dan tidak hanya menjadi bagian kecil dari Payment.

## 11.1 Finance Overview

Menampilkan:

- total revenue;
- total expenses;
- gross profit;
- net profit;
- profit margin;
- period comparison;
- major cost drivers;
- revenue trend;
- AI cost trend.

## 11.2 Revenue

Fungsi:

- revenue by plan;
- revenue by period;
- paid transactions;
- recurring membership revenue;
- complimentary/non-revenue accounts sebagai pembanding bila relevan.

Flow konseptual:

```text
Payment
↓
Transaction
↓
Revenue Recognition
↓
Financial Reporting
```

## 11.3 Expenses

Kategori minimal yang perlu dapat ditampung:

- AI infrastructure;
- hosting;
- database;
- storage;
- payment fees;
- operational expenses;
- other expenses.

Detail accounting treatment dapat ditetapkan pada tahap financial implementation dan tidak diasumsikan oleh dokumen ini.

## 11.4 Profit & Loss

Fungsi:

- revenue;
- cost of service / direct costs bila ditetapkan;
- gross profit;
- operating expenses;
- net profit;
- margin.

## 11.5 Transactions

Fungsi:

- payment transactions;
- status;
- amount;
- provider;
- invoice reference;
- member/membership reference;
- refund/cancellation state.

## 11.6 Invoices

Fungsi:

- invoice records;
- invoice status;
- amount;
- membership relation;
- payment relation.

## 11.7 Payments

Fungsi:

- payment provider status;
- pending/paid/failed/expired/cancelled/refunded;
- manual transfer verification;
- proof review;
- activation relationship.

Payment credentials tetap dipisahkan dari AI credentials.

---

# 12. ADMIN — SYSTEM

## 12.1 Analysis Jobs

Fungsi:

- monitoring analysis execution;
- stage status;
- failed jobs;
- retry/recovery;
- partial regeneration state.

## 12.2 QA

Fungsi:

- quality checks;
- validation results;
- regression indicators;
- output review.

## 12.3 Golden Tests

Fungsi:

- menjalankan/meninjau golden test set;
- quality comparison;
- regression detection;
- cost/completion metrics.

## 12.4 Audit Logs

Fungsi:

- admin actions;
- sensitive configuration changes;
- membership changes;
- credential events;
- payment verification;
- system events.

## 12.5 Settings

Fungsi:

- system configuration;
- operational settings;
- controlled feature configuration;
- security-related configuration sesuai authorization.

---

# 13. CORE NAVIGATION PRINCIPLES

## Member

Primary navigation harus tetap sederhana:

```text
Dashboard
Create New Analysis
History
Membership
Settings
```

Create New Analysis memiliki **internal horizontal progress navigation**, tetapi progress tersebut bukan bagian dari global sidebar navigation.

## Admin

Admin dapat memiliki sidebar karena domain operationalnya luas. Namun domain harus dikelompokkan agar tidak menjadi daftar panjang tanpa struktur:

```text
Dashboard
Members
Memberships
AI
Finance
System
```

AI, Finance, dan System kemudian memiliki submenu/domain navigation masing-masing.

---

# 14. SOURCE-TO-BLUEPRINT USER JOURNEY

```text
Member Dashboard
      ↓
Create New Analysis
      ↓
01 Source
  ├── Idea
  ├── Link
  └── Upload
      ↓
02 Intelligence
      ↓
03 Strategy
      ↓
04 Production
  ├── Carousel
  ├── Short Video
  └── Long Video
      ↓
05 Quality
      ↓
06 Final
      ↓
Professional Content Blueprint
      ↓
Download / Export
```

Source mode tidak mengubah prinsip downstream pipeline. Perbedaan input terjadi pada source ingestion/extraction, kemudian hasilnya masuk ke intelligence engine yang sama.

---

# 15. IMPLEMENTATION STATUS

| Area | Status | Catatan |
|---|---|---|
| Public sitemap | Product direction documented | Belum berarti semua route sudah implemented |
| Member sitemap | Product direction documented | Core navigation sudah ditetapkan |
| Admin sitemap | Product direction documented | Finance domain ditambahkan sebagai requirement |
| Create Analysis workspace | Product direction documented | Multi-stage workspace |
| Horizontal progress navigation | **Approved UX direction** | Di bawah header, bukan sidebar |
| Source: manual idea | Product requirement | Downstream ke pipeline yang sama |
| Source: link | Product requirement | Extraction implementation belum ditetapkan |
| Source: upload | Product requirement | File support/processing belum ditetapkan |
| Format: Carousel | Product requirement | Platform field dihapus |
| Format: Short Video | Product requirement | Platform field dihapus |
| Format: Long Video | Product requirement | Platform field dihapus |
| Initial format: Both | Removed from initial selection | Variant flow dapat dipertimbangkan terpisah |
| Intelligence entitlement UX | Product direction | Included / Preview / Locked |
| Admin Finance | Requirement documented | Full accounting implementation belum ada |
| Revenue / Expense / Profit reporting | Requirement documented | Belum berarti financial ledger sudah implemented |

---

# 16. NON-GOALS / OPEN DETAILS

Dokumen ini **tidak** menetapkan secara otomatis:

- framework frontend;
- exact URL routing implementation;
- database schema final;
- extraction provider/technology untuk URL/file;
- daftar MIME type upload;
- transcription/OCR implementation;
- accounting methodology;
- tax treatment;
- individual feature entitlement matrix di luar keputusan yang sudah disetujui;
- platform-specific publishing integration.

Detail tersebut harus melalui requirement/decision tambahan sebelum implementation jika berdampak pada core product atau architecture.

---

# 17. DEFINITION OF DONE FOR INFORMATION ARCHITECTURE

Information architecture dianggap terdokumentasi dengan benar apabila:

1. Public, Member, dan Admin areas memiliki struktur yang jelas;
2. setiap menu utama memiliki fungsi yang terdokumentasi;
3. Create New Analysis memiliki six-stage workspace;
4. progress Create New Analysis berada horizontal di bawah header;
5. tidak ada vertical submenu Create Analysis di sidebar;
6. Idea, Link, dan Upload memiliki downstream analysis flow yang sama;
7. format awal hanya Carousel, Short Video, dan Long Video;
8. Platform field tidak menjadi bagian dari initial format selection;
9. entitlement UX membedakan Included, Preview, dan Locked;
10. Admin memiliki domain Finance yang mencakup revenue, expenses, P&L, transactions, invoices, dan payments;
11. AI cost dapat mengalir dari usage → cost calculation → cost ledger → financial reporting;
12. status dokumentasi dibedakan dari status implementation.

# 18. CURRENT STATUS

Dokumen ini adalah acuan Information Architecture dan menu/function structure untuk tahap product design berikutnya. Implementasi UI/backend harus tetap mengikuti repository source of truth, project decisions, dan change-control workflow.

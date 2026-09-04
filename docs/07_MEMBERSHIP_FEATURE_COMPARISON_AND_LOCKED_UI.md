# 07 — MEMBERSHIP FEATURE COMPARISON & LOCKED INTELLIGENCE UI

## Status

Dokumen ini mendokumentasikan masalah product yang belum terselesaikan pada pembagian fitur Starter vs Full, serta rencana UX/UI untuk menampilkan feature comparison dan locked intelligence.

Dokumen ini menjadi **product design proposal yang disetujui** untuk arah UX/UI, tetapi pemetaan entitlement individual final tetap harus mengikuti keputusan membership yang ditetapkan secara eksplisit.

Current public membership authority:
- `docs/06_CURRENT_MEMBERSHIP_PLANS.md`
- `docs/05_PROJECT_DECISIONS.md` Decision 030

---

## 1. PRODUCT PROBLEM

Current membership decision hanya menetapkan:

```text
Starter
→ Limited analysis features

Growth
→ Full analysis features

Creator
→ Full analysis features
→ BYOK after Admin AI Pool quota exhausted
```

Namun detail fitur individual yang masuk ke kategori `Limited` belum ditentukan.

Masalah yang perlu diselesaikan:

1. Starter harus tetap menghasilkan output yang berkualitas tinggi dan terasa bernilai.
2. Starter tetap harus memiliki pembeda yang jelas terhadap Full.
3. Fitur Full tidak sebaiknya sekadar dihapus dari pengalaman Starter.
4. User Starter perlu dapat memahami bahwa platform sebenarnya memiliki intelligence yang lebih dalam.
5. UI harus menciptakan upgrade path yang jelas tanpa membuat Starter terasa seperti trial atau produk yang sengaja dibuat buruk.

---

## 2. PRODUCT PRINCIPLE

### Quality is not tier-gated

Tier rendah tidak boleh berarti kualitas AI lebih buruk.

```text
Starter
= High Quality + Limited Scope

Full
= High Quality + Full Intelligence Depth
```

Yang dibatasi adalah:

- breadth of analysis;
- depth of reasoning exposed;
- number of alternatives;
- comparative intelligence;
- production detail;
- advanced QA/revision;
- advanced output sections.

Yang **tidak** boleh sengaja diturunkan:

- factual integrity;
- basic strategic reasoning;
- usefulness;
- clarity;
- actionable recommendation;
- kualitas hook/angle utama;
- kualitas script yang diberikan.

---

## 3. STARTER EXPERIENCE PRINCIPLE

Setiap analisa Starter harus tetap memiliki **Wow Moment**.

Minimum expected value:

1. Unexpected Insight
2. Strong Recommended Angle
3. Strong Hook
4. Actionable Content Direction
5. Usable Script

Starter tidak boleh terasa seperti:

```text
Input
↓
Generic AI response
```

Tetapi tetap harus terasa seperti:

```text
Input
↓
Content Intelligence
↓
Insight
↓
Strategic Recommendation
↓
Usable Output
```

---

## 4. THREE ACCESS STATES

Feature/output access menggunakan tiga state UX utama:

### INCLUDED

Member mendapatkan hasil lengkap.

```text
🟢 Included
```

### PREVIEW

Member mendapatkan hasil inti atau ringkasan, sementara kedalaman tambahan ditampilkan sebagai preview.

```text
🟡 Preview
```

Tujuan:
- menjaga value Starter;
- menunjukkan adanya intelligence yang lebih dalam;
- menciptakan curiosity untuk upgrade.

### LOCKED

Detail output tidak diberikan kepada tier tersebut, tetapi UI tetap menunjukkan bahwa capability tersebut tersedia pada Full tier.

```text
🔒 Locked
```

Locked feature harus menjelaskan value yang akan terbuka, bukan sekadar menampilkan ikon gembok.

---

## 5. RECOMMENDED FEATURE COMPARISON

Ini adalah baseline product recommendation untuk desain membership UI. Ini **bukan** penggantian otomatis terhadap entitlement implementation sampai pemetaan final disetujui sebagai keputusan produk.

| Intelligence / Output | Starter | Growth | Creator |
|---|:---:|:---:|:---:|
| Basic Topic Analysis | Included | Included | Included |
| Core Audience Insight | Included | Included | Included |
| Unexpected Insight | Included | Included | Included |
| Recommended Angle | 1 best | Full | Full |
| Alternative Angles | Preview / Locked | Included | Included |
| Hook Recommendation | 1–2 best | Full | Full |
| Hook Scoring | Preview | Included | Included |
| Viral Potential | Summary | Full | Full |
| Viral Factor Breakdown | Locked | Included | Included |
| Competitive Differentiation | Locked | Included | Included |
| Audience Psychology | Basic | Advanced | Advanced |
| Story Architecture | Good/basic | Advanced | Advanced |
| Carousel Strategy | Basic | Full | Full |
| Detailed Slide Blueprint | Preview | Included | Included |
| Video Strategy | Basic | Full | Full |
| Detailed Scene Blueprint | Preview | Included | Included |
| Visual Direction | Basic | Full | Full |
| Image Prompts | Basic | Full | Full |
| Video Prompts | Locked | Included | Included |
| Global Visual Bible | Locked | Included | Included |
| Character Consistency Profile | Locked | Included | Included |
| Professional QA | Basic | Full | Full |
| Revision / Refinement | Locked | Included | Included |
| Professional Content Blueprint | Simplified | Full | Full |
| Analysis quota / month | 5 | 20 | 75 |
| BYOK after Admin Pool quota | No | No | Yes |

---

## 6. PRICING PAGE COMPARISON

Pricing page tidak perlu menampilkan seluruh internal intelligence pipeline.

Gunakan feature comparison yang ringkas dan mudah dipahami:

| Feature | Starter | Growth | Creator |
|---|:---:|:---:|:---:|
| AI Content Analysis | Limited | Full | Full |
| Audience Intelligence | Basic | Advanced | Advanced |
| Hook & Angle Generation | Limited | Advanced | Advanced |
| Viral Potential Analysis | Basic | Full | Full |
| Competitive Differentiation | Locked | Included | Included |
| Professional Content Blueprint | Simplified | Full | Full |
| AI Revision & Refinement | Locked | Included | Included |
| Analysis quota | 5/mo | 20/mo | 75/mo |
| BYOK after quota | — | — | Included |

Pricing page harus menjelaskan perbedaan value, bukan mengungkap seluruh technical pipeline.

---

## 7. LOCKED INTELLIGENCE UX

### Core principle

Jangan menggunakan pola:

```text
Feature unavailable ❌
```

Gunakan:

```text
Feature exists
↓
Partial result / preview
↓
Explain additional value
↓
Upgrade CTA
```

### Example — Recommended Angle

Starter dapat melihat:

```text
🎯 Recommended Angle

"Orang sebenarnya tidak membeli kopi — mereka membeli ritual."

Ini adalah angle utama yang paling relevan dengan topic dan audience.

🔒 2 alternative angles tersedia di Full

Lihat alternatif angle + comparison → Upgrade
```

User tetap mendapatkan angle yang berguna, tetapi mengetahui bahwa Full menyediakan exploration yang lebih dalam.

---

## 8. LOCKED VIRAL ANALYSIS UX

Starter dapat melihat summary:

```text
🔥 Viral Potential

High Potential

Topik memiliki kombinasi curiosity dan relatability yang kuat.

🔒 Full analysis mencakup 17 attention & sharing factors,
score, reasoning, weakness, dan recommended improvements.

Lihat Full Viral Analysis → Upgrade
```

Jangan menampilkan angka atau detail seolah-olah Full analysis telah diberikan jika entitlement Starter hanya memberikan summary.

---

## 9. LOCKED PRODUCTION BLUEPRINT UX

Untuk output carousel/video, Starter tetap mendapatkan arah produksi yang usable, tetapi detail advanced dapat dipreview.

Contoh:

```text
Slide 1
✓ Hook
✓ Headline
✓ Core visual direction

Slide 2
✓ Main message
✓ Basic visual direction

🔒 Full Blueprint
• complete slide architecture
• composition
• typography direction
• continuity notes
• production notes
• detailed image prompts

Unlock Full Blueprint → Upgrade
```

Tujuan bukan mengurangi kualitas slide yang terlihat, tetapi menunjukkan bahwa Full dapat mengurangi pekerjaan produksi lebih jauh.

---

## 10. LOCKED OUTPUT MUST BE HONEST

UI tidak boleh membuat klaim palsu seperti:

```text
"AI menemukan 20 insight"
```

jika sistem tidak benar-benar menjalankan atau menyimpan proses tersebut.

Locked UI harus merepresentasikan capability/entitlement secara jujur.

Jika engine memang menghasilkan data lengkap tetapi presentation layer menyembunyikannya berdasarkan entitlement, sistem boleh menampilkan preview yang berasal dari hasil tersebut.

Jika engine tidak menjalankan stage tersebut untuk Starter, gunakan wording capability seperti:

```text
"Full analysis includes competitive differentiation."
```

bukan:

```text
"We found 12 competitive gaps for you"
```

kecuali data tersebut benar-benar tersedia.

---

## 11. UPGRADE CTA PRINCIPLE

CTA harus kontekstual dengan locked intelligence.

Hindari CTA generik berulang:

```text
Upgrade now
```

Lebih baik:

```text
Unlock 3 alternative angles
```

```text
See full viral factor breakdown
```

```text
Unlock complete production blueprint
```

```text
Get advanced audience intelligence
```

CTA harus menjelaskan **apa yang didapat**, bukan hanya menyuruh upgrade.

---

## 12. RESULT PAGE INFORMATION ARCHITECTURE

Recommended structure:

```text
ANALYSIS RESULT
│
├── Executive Insight
│   ├── Core Insight
│   ├── Unexpected Insight
│   └── Main Recommendation
│
├── Audience
│   ├── Basic/Full Audience Insight
│   └── Locked Advanced Psychology (Starter)
│
├── Angle
│   ├── Recommended Angle
│   └── Locked Alternative Angles (Starter)
│
├── Hook
│   ├── Best Hook
│   └── Locked Hook Options / Scoring (Starter)
│
├── Viral Potential
│   ├── Summary
│   └── Locked Factor Breakdown (Starter)
│
├── Content Strategy
│   └── Locked Competitive Differentiation (Starter)
│
├── Production Blueprint
│   ├── Usable Basic Direction
│   └── Locked Advanced Details (Starter)
│
└── Upgrade Opportunities
```

Locked sections harus tetap visually integrated dengan result, bukan terasa seperti iklan terpisah.

---

## 13. DESIGN RULES

### Rule 1 — Never punish Starter quality

Starter harus tetap memberikan output yang dapat digunakan.

### Rule 2 — Show the ceiling

User Starter harus dapat melihat bahwa Full memiliki kemampuan yang lebih dalam.

### Rule 3 — Preview value, don't dump value

Preview harus cukup untuk menunjukkan manfaat tanpa memberikan seluruh entitlement Full.

### Rule 4 — Contextual upgrade

CTA muncul dekat dengan locked capability yang relevan.

### Rule 5 — Avoid dark patterns

Tidak boleh:
- misleading countdown;
- fake scarcity;
- fake AI findings;
- menutupi hasil Starter yang sebenarnya sudah menjadi entitlement;
- membuat output sengaja buruk agar user terpaksa upgrade.

### Rule 6 — Preserve factual integrity

Feature gating tidak boleh menyebabkan fakta menjadi kurang akurat atau misleading.

---

## 14. ENGINE VS PRESENTATION LAYER

Architecture sebaiknya memisahkan:

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

Tujuan:

- AI quality tetap konsisten;
- membership policy tidak tercampur dengan prompt logic;
- feature gating dapat berubah tanpa merombak engine;
- locked/preview/included dapat dikontrol secara terstruktur;
- audit entitlement lebih mudah.

Recommended conceptual model:

```text
Feature
├── capability
├── entitlement
├── visibility_state
├── preview_content
├── locked_description
└── upgrade_action
```

Implementasi database/API belum ditentukan oleh dokumen ini.

---

## 15. SUCCESS CRITERIA

UX dianggap berhasil jika:

### Starter

- user mendapatkan hasil yang terasa high-quality;
- user dapat mengambil keputusan konten dari hasil tersebut;
- user tidak merasa membeli versi AI yang buruk;
- user dapat melihat adanya intelligence yang lebih dalam.

### Growth / Creator

- user memahami bahwa Full bukan sekadar quota lebih besar;
- user melihat manfaat strategic depth;
- user mendapatkan complete production-oriented output;
- Creator memahami nilai BYOK setelah Admin AI Pool quota habis.

### Business

- plan differentiation mudah dipahami;
- upgrade reason muncul secara natural;
- locked features meningkatkan perceived value tanpa dark pattern;
- kualitas Starter tetap mendukung brand positioning Liteslice.

---

## 16. IMPLEMENTATION STATUS

Status saat dokumen ini dibuat:

```text
Membership pricing                → DECIDED
Starter limited / Full tiers     → DECIDED
Individual feature mapping       → PROPOSAL
Locked / Preview / Included UX   → APPROVED PRODUCT DIRECTION
Final entitlement matrix         → NOT YET IMPLEMENTED
UI implementation                → NOT YET IMPLEMENTED
Database entitlement schema       → NOT YET IMPLEMENTED
```

Jangan menganggap proposal pada dokumen ini sebagai implementasi yang sudah selesai.

Setiap perubahan pada entitlement individual yang memengaruhi business rule harus kembali dicatat di `docs/05_PROJECT_DECISIONS.md`.

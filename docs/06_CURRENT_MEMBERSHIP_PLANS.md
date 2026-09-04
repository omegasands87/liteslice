# CURRENT MEMBERSHIP PLANS

## Status

Dokumen ini adalah spesifikasi membership yang **berlaku saat ini** untuk Liteslice.

Sumber keputusan: **Decision 030 — Current Membership Plans Revision** pada `docs/05_PROJECT_DECISIONS.md`.

Dokumen ini menggantikan referensi plan Starter / Pro / Power lama yang masih muncul sebagai historical baseline pada dokumen terdahulu.

---

## 1. Public Free Plan

Tidak ada public Free Plan.

---

## 2. STARTER — Rp30.000/bulan

```text
STARTER
│
├── Admin AI Pool
├── 5 analysis / month
└── Limited analysis features
```

Entitlement:

- menggunakan Admin AI Pool;
- quota 5 analysis per bulan;
- fitur analisa terbatas;
- tidak memiliki entitlement BYOK berdasarkan keputusan membership saat ini.

---

## 3. GROWTH — Rp90.000/bulan

```text
GROWTH
│
├── Admin AI Pool
├── 20 analysis / month
└── FULL analysis features
```

Entitlement:

- menggunakan Admin AI Pool;
- quota 20 analysis per bulan;
- fitur analisa FULL;
- tidak memiliki entitlement BYOK yang ditetapkan oleh keputusan membership saat ini.

---

## 4. CREATOR — Rp270.000/bulan

```text
CREATOR
│
├── Admin AI Pool
├── 75 analysis / month
├── FULL analysis features
└── BYOK after Admin Pool quota is exhausted
```

Entitlement:

- menggunakan Admin AI Pool;
- quota 75 analysis per bulan;
- fitur analisa FULL;
- setelah quota Admin AI Pool habis, member dapat melanjutkan analysis menggunakan API miliknya sendiri melalui BYOK.

### BYOK behavior

```text
Creator
   ↓
Admin AI Pool
   ↓
Quota available?
   ├── YES → consume Creator quota
   └── NO  → BYOK may be used
```

BYOK pada Creator adalah mekanisme lanjutan setelah quota Admin AI Pool habis. Penggunaan API provider melalui BYOK menjadi tanggung jawab pemilik API credential.

---

## 5. QUOTA MODEL

V1 menggunakan:

```text
1 Analysis = 1 quota unit
```

Quota membership dan quota provider/credential tetap merupakan konsep terpisah.

```text
Member Quota
     ≠
Provider Credential Quota
```

Quota Admin AI Pool untuk membership dihitung secara individual per member.

---

## 6. FEATURE ACCESS

Membership memiliki dua level feature access yang telah diputuskan:

```text
Starter
→ Limited analysis features

Growth
→ Full analysis features

Creator
→ Full analysis features
```

Detail pemetaan fitur individual ke `limited` atau `full` belum ditentukan dalam keputusan membership ini dan **tidak boleh diasumsikan** oleh AI Builder.

Jika detail feature gating diperlukan, buat keputusan product baru sebelum implementation.

---

## 7. AI SOURCE POLICY

Semua plan menggunakan Admin AI Pool sebagai sumber AI platform:

```text
Starter → Admin AI Pool
Growth  → Admin AI Pool
Creator → Admin AI Pool → BYOK after quota exhausted
```

Admin AI Pool tetap tunduk pada provider quota, credential quota, health, routing, failover, dan global pool budget.

---

## 8. COMPLIMENTARY MEMBERS

Keputusan membership baru tidak menghapus aturan complimentary member.

Admin tetap dapat membuat member khusus secara manual dengan:

- custom quota;
- expiry;
- allowed access;
- provider/model;
- BYOK access;
- status.

Detail entitlement complimentary member dapat berbeda dari plan publik dan harus ditentukan oleh Admin.

---

## 9. PAYMENT IMPACT

Plan yang dapat dijual kepada public saat ini:

| Plan | Price / month | Admin AI Pool | Analysis quota | Feature access | BYOK |
|---|---:|---|---:|---|---|
| Starter | Rp30.000 | Yes | 5 | Limited | No |
| Growth | Rp90.000 | Yes | 20 | Full | No |
| Creator | Rp270.000 | Yes | 75 | Full | After Admin Pool quota exhausted |

Payment, invoice, transaction, renewal, expiry, dan membership activation harus menggunakan plan terbaru ini.

---

## 10. IMPLEMENTATION RULE

Jangan menggunakan struktur berikut sebagai current product requirement:

```text
Starter — Rp10.000
Pro     — Rp75.000
Power   — Rp150.000
```

Struktur tersebut adalah **superseded historical decision**.

Current requirement:

```text
Starter — Rp30.000 — 5 analysis — Limited
Growth  — Rp90.000 — 20 analysis — Full
Creator — Rp270.000 — 75 analysis — Full — BYOK after quota exhausted
```

Jika ditemukan konflik antara dokumen lama dan dokumen ini, gunakan `docs/05_PROJECT_DECISIONS.md` Decision 030 sebagai sumber keputusan terbaru dan jangan mengarang entitlement tambahan.

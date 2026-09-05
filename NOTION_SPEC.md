# 📋 AI Business Health Check — Spesifikasi Metodologi & Parameter Diagnostik (Notion Sync)

> **Dokumen Spesifikasi Lengkap (Ready to Copy to Notion)**  
> **Status:** Locked & Final (Groundwork Phase 0)  
> **Target Pengguna:** UMKM Mikro & Kecil (PP No. 7/2021) di Indonesia  
> **Arsitektur:** White-Box (Deterministic Rule Engine + Gemini AI Structured Narrative)  
> **Stakeholders:** Felix (Engineering & Methodology Lead), Rayyan (UI/UX Design Lead), Sriram (Advisor/Stakeholder)

---

## 📌 Ringkasan Eksekutif & Filosofi Arsitektur

Alat ini dirancang untuk mendiagnosis kesehatan operasional, finansial, pasar, dan tata kelola bisnis UMKM di Indonesia secara **transparan, objektif, dan terukur**.

```
[ Input Kuesioner Pengusaha (Hybrid UI) ]
                   │
                   ▼
┌─────────────────────────────────────────────────────────────┐
│          DETERMINISTIC SCORING ENGINE (Rule-Based)          │
│  - Bobot Balanced Scorecard (Fin 35%, Ops 25%, Mkt 20%, Gov 20%)  │
│  - Adaptasi Sektor (F&B, Retail, Jasa)                      │
│  - Diferensiasi Skala Usaha (Mikro vs Kecil - PP 7/2021)     │
│  - Circuit Breakers: Red Flags Detector (P0, P1, P2)        │
│  - Status Capping Logic (P0 → Max Status 'Needs Attention') │
└──────────────────────────────┬──────────────────────────────┘
                               │
            ┌──────────────────┴──────────────────┐
            ▼                                     ▼
   [ Visual Dashboard ]               [ Gemini AI Engine ]
   - Radar Chart (4 Dimensi)          - Strict Structured JSON Payload
   - Kartu Skor Komposit & Status     - 30-Day Phased Action Plan (W1-W4)
   - Emergency Red Flag Banners       - Context-Aware Interactive Chat Advisor
```

---

## 🏢 Bagian 1: Profil Bisnis & Klasifikasi Skala (PP No. 7/2021)

Sebelum masuk ke pertanyaan diagnostik, sistem mengumpulkan 5 data profil dasar untuk kalibrasi kuesioner:

| Field Input | Tipe Data | Pilihan / Format | Fungsi Sistem |
|---|---|---|---|
| **Nama Usaha** | Text | String (misal: *Kopi Seduh Kenangan*) | Personalisasi laporan |
| **Sektor Industri** | Dropdown | `F&B` \| `Retail / Dagang` \| `Jasa / Layanan` | Adaptasi pertanyaan dimensi Operasi & Margin |
| **Lama Beroperasi** | Dropdown | `< 1 tahun` \| `1 – 3 tahun` \| `> 3 tahun` | Evaluasi Red Flag P2 (NIB) & kematangan usaha |
| **Omzet Tahunan** | Dropdown / Nominal | `≤ Rp 2 Miliar` (Mikro) \| `> Rp 2 Miliar – Rp 15 Miliar` (Kecil) | Klasifikasi regulasi PP 7/2021 & kalibrasi ekspektasi |
| **Jumlah Karyawan** | Number / Dropdown | `1 – 4 orang` \| `5 – 19 orang` \| `≥ 20 orang` | Kalibrasi dimensi HR & Owner Dependency |

---

## 📐 Bagian 2: Formula Skoring Komposit & Bobot Dimensi

Skor komposit dihitung secara deterministik (skala 0 – 100):

$$\text{Total Score} = (S_{fin} \times 0.35) + (S_{ops} \times 0.25) + (S_{mkt} \times 0.20) + (S_{gov} \times 0.20)$$

### Matriks Kategori Kesehatan Bisnis

| Skor | Kategori | Label Warna | Makna Diagnostik |
|---|---|---|---|
| **80 – 100** | 🟢 **Healthy** | Hijau | Fondasi kokoh, arus kas sehat, siap ekspansi / pengajuan kredit bank. |
| **60 – 79** | 🟡 **Fairly Healthy** | Kuning | Beroperasi stabil, namun ada celah struktural yang perlu dibenahi. |
| **40 – 59** | 🟠 **Needs Attention** | Oranye | Rentan terhadap guncangan pasar/operasional; butuh perbaikan segera. |
| **0 – 39** | 🔴 **Critical** | Merah | Berada dalam bahaya kegagalan usaha atau insolvensi likuiditas. |

> ⚠️ **Aturan Circuit Breaker (Status Capping):**  
> Jika **Red Flag P0 (Bahaya Likuiditas)** aktif, kategori status usaha **dibatasi maksimal menjadi "Needs Attention" (Oranye)** meskipun skor total komposit berada di atas 60.

---

## 📝 Bagian 3: Spesifikasi Detail Kuesioner (Hybrid Input & Adaptif)

### Dimensi 1: Kesehatan Finansial ($S_{fin}$ — Bobot 35%)

$$S_{fin} = (\text{Pemisahan Rekening} \times 0.25) + (\text{Pencatatan Keuangan} \times 0.25) + (\text{Cash Runway} \times 0.30) + (\text{Margin Laba Kotor} \times 0.20)$$

#### 1.1 Pemisahan Rekening Bisnis & Pribadi (Bobot: 25%)
- **Pertanyaan:** *Apakah uang operasional usaha dipisahkan dari rekening/keuangan pribadi pemilik?*
- **Opsi Jawaban:**
  - `[100]` **Terpisah Sepenuhnya:** Memiliki rekening bank/dompet digital khusus usaha, pencatatan kas terisolasi.
  - `[0]` **Bercampur:** Uang hasil usaha dan kebutuhan pribadi masih memakai satu rekening/dompet yang sama.  
    🚩 *Memicu Red Flag P0 (Bahaya Likuiditas).*

#### 1.2 Metode Pencatatan Keuangan (Bobot: 25%)
- **Pertanyaan:** *Bagaimana cara bisnis mencatat transaksi pemasukan dan pengeluaran setiap hari?*
- **Opsi Jawaban (Disesuaikan Skala PP 7/2021):**
  - `[100]` **Software / Aplikasi Akuntansi Digital** (Moka, Mekari Jurnal, BukuKas, Qasir, Accurate, dll.)
  - `[75]` **Spreadsheet Terstruktur** (Microsoft Excel / Google Sheets dengan rekap rutin)
  - `[50]` *(Skala Mikro)* / `[35]` *(Skala Kecil)* **Buku Catatan Kas Manual / Nota Fisik**
  - `[0]` **Tidak ada pencatatan tertulis sama sekali**

#### 1.3 Cash Runway / Cadangan Kas Operasional (Bobot: 30%)
- **Model Input:** **Hybrid**
  - *Opsi Cepat (Default):*
    - `[100]` **Aman (≥ 3 bulan):** Kas cukup untuk membayar seluruh biaya rutin tanpa ada pemasukan sama sekali selama ≥ 3 bulan.
    - `[60]` **Cukup (1 – 2.9 bulan):** Kas cukup untuk 1 hingga 2.9 bulan operasional.
    - `[20]` **Kritis (< 1 bulan):** Kas operasional tersisa kurang dari 30 hari. 🚩 *Memicu Red Flag P0.*
  - *Kalkulator Mini Opsional (Jika tahu angka riil):*
    $$\text{Cash Runway (Bulan)} = \frac{\text{Total Kas \& Saldo Bank Siap Pakai (Rp)}}{\text{Total Pengeluaran Rutin / OPEX Bulanan (Rp)}}$$
    - Jika hasil $\ge 3.0 \rightarrow \text{Skor } 100$
    - Jika hasil $1.0 \le x < 3.0 \rightarrow \text{Skor } 60$
    - Jika hasil $< 1.0 \rightarrow \text{Skor } 20$ (Trigger P0)

#### 1.4 Margin Laba Kotor / Gross Profit Margin (Bobot: 20%)
- **Model Input:** **Hybrid & Adaptif per Sektor**
  - *Opsi Cepat (Default):*
    - `[100]` **Sehat & Sesuai Standar Industri**
      - F&B: Laba kotor $\ge 35\%$
      - Retail / Dagang: Laba kotor $\ge 20\%$
      - Jasa / Layanan: Laba kotor $\ge 40\%$
    - `[50]` **Tipis / Di Bawah Rata-rata** (F&B: 20-34%, Retail: 10-19%, Jasa: 20-39%)
    - `[20]` **Sangat Tipis / Tidak Tahu Pasti Margin Produknya**
  - *Kalkulator Mini Opsional:*
    $$\text{GPM (\%)} = \frac{\text{Harga Jual} - \text{HPP (Harga Pokok Penjualan)}}{\text{Harga Jual}} \times 100\%$$

---

### Dimensi 2: Operasional & Efisiensi ($S_{ops}$ — Bobot 25%)

$$S_{ops} = (\text{Dokumentasi SOP} \times 0.35) + (\text{Ketergantungan Owner} \times 0.35) + (\text{Manajemen Inventori / Kapasitas} \times 0.30)$$

#### 2.1 Dokumentasi & Pelaksanaan SOP (Bobot: 35%)
- **Pertanyaan:** *Apakah langkah kerja operasional inti (pembukaan gerai, pembuatan produk/layanan, penutupan) terdokumentasi?*
- **Opsi Jawaban:**
  - `[100]` **Ada SOP Tertulis & Dijalankan Konsisten:** Karyawan baru bisa langsung bekerja mengikuti panduan tertulis/video.
  - `[50]` **SOP Sebagian / Arahan Lisan Terbiasa:** Ada instruksi rutin tapi belum semua terdokumentasi rapi.
  - `[10]` **Tidak Ada SOP:** Alur kerja acak dan sepenuhnya bergantung pada ingatan masing-masing orang.

#### 2.2 Owner Dependency Index / Ketergantungan pada Pemilik (Bobot: 35%)
- **Pertanyaan:** *Berapa lama bisnis bisa tetap berjalan normal melayani pelanggan jika pemilik cuti / tidak masuk sama sekali?*
- **Opsi Jawaban:**
  - `[100]` **Mandiri (≥ 1 bulan):** Tim operasional bisa jalan tanpa kehadiran fisik atau intervensi harian pemilik.
  - `[50]` **Semi-Mandiri (1 minggu – 1 bulan):** Bisa jalan beberapa hari, tapi butuh pemilik untuk keputusan penting mingguan.
  - `[0]` **Sangat Tergantung (≤ 2 hari kerja):** Jika pemilik absen, operasional berhenti atau layanan langsung kacau.  
    🚩 *Memicu Red Flag P1 (Operational Bottleneck).*

#### 2.3 Manajemen Stok / Utilisasi Kapasitas — *Adaptif per Sektor* (Bobot: 30%)
- **Cabang A: Sektor F&B dan Retail / Dagang (Fokus: Kontrol Stok & Waste)**
  - *Pertanyaan:* *Bagaimana bisnis Anda mengontrol persediaan barang, stok kadaluarsa, atau sisa bahan terbuang?*
  - `[100]` **Stock Opname Rutin Terjadwal:** Ada pencatatan stok masuk-keluar berkala dan rasio bahan terbuang (*waste*) dicatat.
  - `[40]` **Pencatatan Seadanya:** Hanya mencatat saat belanja nota masuk, tanpa pengecekan stok fisik rutin.
  - `[0]` **Tidak Ada Kontrol Stok:** Sering kehabisan stok mendadak atau banyak barang rusak/hilang tanpa diketahui penyebabnya.
- **Cabang B: Sektor Jasa / Layanan (Fokus: Utilisasi Kapasitas & Jam Kerja)**
  - *Pertanyaan:* *Bagaimana bisnis Anda mengukur dan mengelola kapasitas waktu kerja (man-hours/booking)?*
  - `[100]` **Penjadwalan & Kapasitas Terukur:** Ada sistem reservasi/kalender kerja yang terencana dan tingkat keterisian jam kerja terpantau.
  - `[40]` **Penjadwalan Ad-hoc:** Menerima pekerjaan langsung tanpa mengukur beban kerja tim sehingga kadang *overload* atau kosong.
  - `[0]` **Tanpa Manajemen Kapasitas:** Waktu pengerjaan tidak terlacak dan sering terjadi *deadline* molor parah.

---

### Dimensi 3: Pasar & Pelanggan ($S_{mkt}$ — Bobot 20%)

$$S_{mkt} = (\text{Konsentrasi Pendapatan} \times 0.50) + (\text{Diversifikasi Kanal} \times 0.25) + (\text{Pelacakan Retensi Pelanggan} \times 0.25)$$

#### 3.1 Konsentrasi Pendapatan / Ketergantungan Klien (Bobot: 50%)
- **Pertanyaan:** *Apakah ada satu klien atau satu produk tunggal yang menyumbang lebih dari separuh total pendapatan bisnis Anda?*
- **Opsi Jawaban:**
  - `[100]` **Terdiversifikasi Baik:** Tidak ada satu pelanggan/klien pun yang menyumbang > 40% total omzet bisnis.
  - `[30]` **Sangat Terkonsentrasi:** Satu pelanggan utama menyumbang ≥ 50% pendapatan (kehilangan klien ini mengancam kelangsungan usaha).

#### 3.2 Diversifikasi Kanal Penjualan (Bobot: 25%)
- **Pertanyaan:** *Melalui kanal apa saja pelanggan membeli produk/layanan Anda?*
- **Opsi Jawaban:**
  - `[100]` **Multi-Channel Aktif:** Gabungan Offline (toko fisik/kantor) dan Online (GoFood/GrabFood/Shopee/WhatsApp Business/Website).
  - `[50]` **Single-Channel:** Hanya mengandalkan satu jalur saja (misal: hanya toko fisik tanpa kehadiran online, atau hanya Instagram DM).

#### 3.3 Pelacakan Retensi & Repeat Order (Bobot: 25%)
- **Pertanyaan:** *Apakah Anda melacak pelanggan yang datang kembali (*repeat buyer*) atau memiliki program retensi?*
- **Opsi Jawaban:**
  - `[100]` **Terlacak Aktif:** Memiliki database kontak pelanggan, grup loyalitas, atau program *repeat order* berkala.
  - `[40]` **Hanya Transaksional:** Menjual lepas tanpa mencatat data pelanggan berulang.

---

### Dimensi 4: Tata Kelola & SDM ($S_{gov}$ — Bobot 20%)

$$S_{gov} = (\text{Legalitas Usaha / NIB} \times 0.50) + (\text{Struktur SDM \& Kompensasi} \times 0.50)$$

#### 4.1 Legalitas Usaha & Kepatuhan Regulasi (Bobot: 50%)
- **Pertanyaan:** *Dokumen legalitas apa yang sudah dimiliki oleh bisnis Anda?*
- **Opsi Jawaban:**
  - `[100]` **Lengkap (NIB + Izin Sektoral / Sertifikasi):** Memiliki NIB (Nomor Induk Berusaha melalui OSS) ditambah izin edar/P-IRT/Sertifikat Halal/HAKI bila diwajibkan.
  - `[60]` **NIB Terdaftar:** Sudah memiliki NIB resmi dari OSS-RBA.
  - `[10]` **Belum Memiliki Legalitas Formal:** Belum memiliki NIB.  
    🚩 *Memicu Red Flag P2 jika usia usaha > 1 tahun.*

#### 4.2 Manajemen SDM & Skema Kompensasi — *Diferensiasi PP 7/2021* (Bobot: 50%)
- **Pertanyaan:** *Bagaimana pembagian tugas dan sistem pembayaran upah bagi tim/karyawan?*
- **Opsi Jawaban (Mikro vs Kecil):**
  - **Untuk Skala Mikro (1–4 staf):**
    - `[100]` **Peran Jelas:** Pembagian tugas harian jelas dan gaji/upah dibayarkan tepat waktu sesuai kesepakatan.
    - `[50]` **Peran Campur Aduk:** Semua orang mengerjakan segalanya tanpa kejelasan tanggung jawab utama.
  - **Untuk Skala Kecil (≥ 5 staf):**
    - `[100]` **Peran Spesifik + Skema Insentif/Bonus Performa:** Job description tertulis dan ada bonus jika target penjualan/KPI tercapai.
    - `[60]` **Gaji Tetap Saja:** Peran jelas namun tanpa insentif performa kerja.
    - `[20]` **Tanpa Struktur Peran & Sering Terjadi Konflik Tugas.**

---

## 🚩 Bagian 4: Matriks Pemicu Circuit Breaker (Red Flags)

Red flag dihitung **secara otomatis (rule-based)** sebelum payload dikirimkan ke model AI:

| Kode | Nama Red Flag | Kondisi Pemicu Matematis | Dampak Diagnostik Langsung | Intervensi Wajib di Rencana Aksi |
|---|---|---|---|---|
| 🔴 **P0** | **Liquidity Danger** (Bahaya Likuiditas) | `Pemisahan Rekening == Bercampur` **ATAU** `Cash Runway < 1 bulan` | **Status Cap**: Kategori kesehatan bisnis dibatasi maksimal **"Needs Attention"** terlepas dari nilai komposit. Muncul banner merah darurat. | **Wajib di Minggu ke-1**: Audit kas darurat harian, isolasi rekening rekening usaha, kurangi pengeluaran non-primer. |
| 🟠 **P1** | **Operational Bottleneck** (Hambatan Operasional) | `Owner Dependency <= 2 hari kerja` | Menandakan usaha berisiko tinggi terhadap *key-person risk*; usaha belum *scalable*. | **Wajib di Minggu ke-2**: Pembuatan SOP 3 tugas harian pemilik teratas & delegasi ke staf senior. |
| 🟡 **P2** | **Regulatory Risk** (Risiko Legalitas) | `Lama Beroperasi > 1 tahun` **DAN** `Legalitas == Belum Memiliki NIB` | Menutup akses ke pinjaman bank (KUR), tender pengadaan barang pemerintah, dan kemitraan rantai pasok formal. | **Wajib di Minggu ke-3**: Registrasi NIB mandiri gratis via sistem OSS-RBA (*Online Single Submission*). |

---

## 🤖 Bagian 5: Spesifikasi Integrasi Gemini AI (Strict JSON Schema)

### 5.1 Injected Context Payload (Dari Frontend ke Gemini API)

```json
{
  "business_profile": {
    "name": "Warung Berkah Jaya",
    "sector": "F&B",
    "business_age_years": 2,
    "annual_revenue_bracket": "Micro (<= 2B)",
    "scale_classification": "Mikro (PP No. 7/2021)",
    "employee_count": 3
  },
  "deterministic_scores": {
    "composite_score": 62.5,
    "composite_status": "Needs Attention",
    "status_capped_by_p0": true,
    "dimension_scores": {
      "financial": 45.0,
      "operations": 65.0,
      "market": 70.0,
      "governance": 75.0
    }
  },
  "active_red_flags": [
    {
      "code": "P0",
      "name": "Liquidity Danger",
      "reason": "Uang usaha bercampur dengan rekening pribadi & cash runway < 1 bulan."
    }
  ],
  "questionnaire_responses": {
    "account_separation": "Mixed (0)",
    "record_keeping": "Manual ledger (50)",
    "cash_runway": "< 1 month (20)",
    "gross_profit_margin": "Thin / Unknown (20)",
    "sop_status": "Partial (50)",
    "owner_dependency": "Max 1 week (50)",
    "inventory_control": "Regular stock opname (100)",
    "revenue_concentration": "No single client > 40% (100)",
    "sales_channels": "Multi-channel (100)",
    "customer_retention": "Untracked (40)",
    "licensing": "NIB registered (60)",
    "hr_structure": "Clear roles (100)"
  }
}
```

### 5.2 Structured Output JSON Schema (Respons Wajib dari Gemini)

Frontend mewajibkan Gemini merespons dalam format JSON murni yang sesuai dengan skema berikut:

```json
{
  "executive_summary": "Ringkasan kondisi bisnis dalam 2-3 kalimat profesional dan lugas.",
  "status_badge": "Needs Attention",
  "status_warning": "Status dibatasi karena terdeteksi bahaya likuiditas akut (P0).",
  "dimension_analysis": {
    "financial": {
      "score": 45.0,
      "analysis": "Penjelasan mengapa skor keuangan rendah tanpa mengotak-atik rumus."
    },
    "operations": {
      "score": 65.0,
      "analysis": "Penjelasan kondisi operasional dan kendala ketergantungan owner."
    },
    "market": {
      "score": 70.0,
      "analysis": "Evaluasi penetrasi pasar dan peluang penguatan retensi."
    },
    "governance": {
      "score": 75.0,
      "analysis": "Tinjauan kepatuhan legalitas dan kesiapan skala organisasi."
    }
  },
  "action_plan_30_days": [
    {
      "week": 1,
      "theme": "Stabilisasi Likuiditas & Kas Darurat",
      "priority_flag": "P0",
      "tasks": [
        {
          "id": "T1-1",
          "task_name": "Buka Rekening Terpisah untuk Usaha",
          "description": "Buka satu rekening digital gratis khusus penerimaan omzet usaha hari ini.",
          "estimated_days": 2,
          "impact": "Tinggi"
        },
        {
          "id": "T1-2",
          "task_name": "Audit Harian Pengeluaran Rutin",
          "description": "Catat seluruh pengeluaran kas harian untuk mengidentifikasi pos pemborosan.",
          "estimated_days": 5,
          "impact": "Kritis"
        }
      ]
    },
    {
      "week": 2,
      "theme": "Standarisasi Operasional & Delegasi",
      "priority_flag": "P1",
      "tasks": [
        {
          "id": "T2-1",
          "task_name": "Tuliskan 3 SOP Alur Kerja Terpenting",
          "description": "Dokumentasikan alur buka/tutup toko dan kontrol bahan baku dalam checklist sederhana 1 halaman.",
          "estimated_days": 4,
          "impact": "Sedang"
        }
      ]
    },
    {
      "week": 3,
      "theme": "Legalitas & Kepatuhan Usaha",
      "priority_flag": "P2",
      "tasks": [
        {
          "id": "T3-1",
          "task_name": "Registrasi NIB via Portal OSS-RBA",
          "description": "Lengkapi pendaftaran KBLI sesuai usaha untuk membuka peluang KUR bank.",
          "estimated_days": 3,
          "impact": "Tinggi"
        }
      ]
    },
    {
      "week": 4,
      "theme": "Pertumbuhan Pasar & Database Pelanggan",
      "priority_flag": null,
      "tasks": [
        {
          "id": "T4-1",
          "task_name": "Setup Database Loyalitas WhatsApp",
          "description": "Kumpulkan kontak 50 pelanggan pertama dan kirimkan kupon diskon repeat order.",
          "estimated_days": 5,
          "impact": "Sedang"
        }
      ]
    }
  ]
}
```

---

## 👥 Bagian 6: Checklist Tugas Tim Magang (Notion Sprint Board)

### 🎨 Track Rayyan (UI/UX Design Lead)
- [ ] Buat wireframe alur multi-step assessment form (Step 1 Profil, Step 2 Finansial, Step 3 Operasi, Step 4 Pasar, Step 5 Tata Kelola).
- [ ] Desain kartu input hybrid (opsi radio button bersih + toggle mini-calculator nominal).
- [ ] Desain Dashboard Hasil: Skor Gauge/Radar Chart, Banner Peringatan Merah P0, dan Kartu Rencana Aksi Mingguan (Minggu 1–4).
- [ ] Rancang antarmuka panel samping *Interactive AI Chat Advisor*.

### 💻 Track Felix & AI Pair (Engineering & Architecture Lead)
- [x] Finalisasi metodologi white-box, PP 7/2021, dan rumusan bobot Balanced Scorecard.
- [x] Susun kamus kuesioner lengkap, hybrid input mode, dan skema JSON integrasi Gemini.
- [ ] Buat file modul scoring engine deterministik murni (`scoring.js`) dengan 100% test coverage.
- [ ] Setup repository & deployment baseline Vercel CI/CD (Fase 1).
- [ ] Hubungkan form asesmen ke scoring engine dan panggil Gemini API dengan schema JSON.
- [ ] Pasang panel interaktif Context-Aware AI Chat dengan streaming responses.

### 👔 Track Sriram (Advisor / Stakeholder Alignment)
- [x] Verifikasi penyesuaian Balanced Scorecard untuk UMKM Indonesia.
- [ ] Review slide deck metodologi [presentation.html](file:///c:/Users/Felix/Documents/Magang/AI%20Business%20Health%20Check/presentation.html).
- [ ] Validasi bobot persentase 4 dimensi dan ambang batas Red Flag P0/P1/P2.

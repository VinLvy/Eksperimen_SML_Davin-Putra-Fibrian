# PRD — Sistem Machine Learning End-to-End (Target: Basic / Lulus Minimum)

**Status:** Draft untuk agentic coding (Antigravity)
**Target nilai:** 2 pts di setiap kriteria (total 8/16, rata-rata 2.0 → Bintang 3 / Level Basic / Lulus)
**Non-goal eksplisit:** Hyperparameter tuning, MLflow online (DagsHub), Docker build/push, GitHub Actions untuk preprocessing otomatis, Grafana alerting, metrik >3 di Prometheus.

Ganti semua placeholder `[Nama-siswa]` dan `[namadataset]` sebelum eksekusi. Dataset belum ditentukan — asumsikan tabular, klasifikasi, ukuran kecil-menengah, kompatibel Scikit-Learn. Jika dataset berbeda, sesuaikan `modelling.py` dan skema kolom.

---

## 1. Ringkasan Proyek

Membangun 3 repository GitHub terpisah + 1 folder bukti monitoring, sesuai ketentuan Dicoding kelas "Membangun Sistem Machine Learning". Semua dijalankan **tanpa Docker** dan **tanpa tuning**, mengandalkan:

- Python 3.12.7
- mlflow==2.19.0
- MLflow Tracking UI lokal (`127.0.0.1`)
- GitHub Actions (wajib, berjalan di runner GitHub — bukan beban laptop)
- Prometheus + Grafana native (binary/installer, bukan container)

## 2. Environment & Tech Stack

| Komponen | Versi/Tools |
|---|---|
| Python | 3.12.7 |
| MLflow | 2.19.0 |
| ML Framework | Scikit-Learn (default asumsi) |
| Model Serving | `mlflow model serve` |
| Monitoring | Prometheus (binary native) |
| Visualisasi | Grafana OSS (installer native) |
| CI | GitHub Actions (`ubuntu-latest` runner) |
| VCS | Git + GitHub (3 repo publik) |

## 3. Struktur Repository & Folder (Wajib, Basic Tier)

### Repo 1: `Eksperimen_SML_[Nama-siswa]` (Publik)
```
Eksperimen_SML_[Nama-siswa]/
├── [namadataset]_raw/
└── preprocessing/
    ├── Eksperimen_[Nama-siswa].ipynb
    └── [namadataset]_preprocessing/
```
Tidak perlu `.workflow` maupun `automate_[Nama-siswa].py` (itu tier Skilled/Advance).

### Folder 2: `Membangun_model/` (bagian dari submission ZIP, tidak perlu repo GitHub terpisah)
```
Membangun_model/
├── modelling.py
├── [namadataset]_preprocessing/
├── screenshoot_dashboard.jpg
├── screenshoot_artifak.jpg
└── requirements.txt
```
Tidak perlu `modelling_tuning.py` maupun `DagsHub.txt`.

### Repo 3: `Workflow-CI` (Publik)
```
Workflow-CI/
├── .github/workflows/ci.yml
└── MLProject/
    ├── MLProject
    ├── conda.yaml
    ├── modelling.py
    └── [namadataset]_preprocessing/
```

### Folder 4: `Monitoring dan Logging/` (bagian dari submission ZIP)
```
Monitoring dan Logging/
├── 1.bukti_serving/
├── 2.prometheus.yml
├── 3.prometheus_exporter.py
├── 4.bukti monitoring Prometheus/
│   ├── 1.monitoring_http_requests_total.jpg
│   ├── 2.monitoring_http_request_duration_seconds.jpg
│   └── 3.monitoring_model_prediction_errors_total.jpg
├── 5.bukti monitoring Grafana/
│   ├── 1.monitoring_http_requests_total.jpg
│   ├── 2.monitoring_http_request_duration_seconds.jpg
│   └── 3.monitoring_model_prediction_errors_total.jpg
└── 7.inference.py
```
Tidak perlu folder `6.bukti alerting Grafana` (itu Skilled/Advance).

### Struktur ZIP Final Submission
```
SMSML_[Nama-siswa].zip
├── Eksperimen_SML_[Nama-siswa].txt   (link repo GitHub Publik #1)
├── Membangun_model/
├── Workflow-CI.txt                    (link repo GitHub Publik #3)
└── Monitoring dan Logging/
```
**Kritis:** Kedua `.txt` berisi URL repo GitHub dengan visibilitas **Public**. Repo Private = otomatis reject.

## 4. Spesifikasi Fungsional per Kriteria

### Kriteria 1 — Eksperimen Dataset (Target 2 pts)
**Acceptance criteria:**
- [ ] Notebook `Eksperimen_[Nama-siswa].ipynb` mengikuti Template Eksperimen MSML.
- [ ] Cell data loading berjalan tanpa error.
- [ ] Cell EDA (distribusi, missing value, korelasi, dsb) ada dan berjalan.
- [ ] Cell preprocessing (cleaning, encoding, split) ada dan berjalan.
- [ ] Output dataset bersih disimpan ke `[namadataset]_preprocessing/`.
- [ ] Seluruh cell dieksekusi berurutan tanpa error (Restart & Run All harus sukses).

**Non-goal:** Tidak perlu `automate_[Nama-siswa].py` atau GitHub Actions preprocessing.

### Kriteria 2 — Modelling (Target 2 pts)
**Acceptance criteria:**
- [ ] `modelling.py` melatih model Scikit-Learn menggunakan dataset hasil preprocessing (bukan raw).
- [ ] `mlflow.sklearn.autolog()` dipanggil sebelum training.
- [ ] MLflow Tracking UI berjalan di `http://127.0.0.1:5000`, artefak tersimpan lokal (folder `mlruns/`).
- [ ] Screenshot `screenshoot_dashboard.jpg` (MLflow UI run list) dan `screenshoot_artifak.jpg` (isi artefak 1 run) diambil setelah training sukses.
- [ ] `requirements.txt` mencantumkan seluruh dependency yang dipakai.

**Non-goal:** Tidak ada hyperparameter tuning, tidak ada manual logging, tidak ada DagsHub.

### Kriteria 3 — Workflow CI (Target 2 pts)
**Acceptance criteria:**
- [ ] Folder `MLProject/` berisi `MLProject`, `conda.yaml`, `modelling.py`, dataset preprocessing.
- [ ] `.github/workflows/ci.yml` trigger otomatis (`push` ke `main` dan/atau `workflow_dispatch`).
- [ ] Workflow menjalankan `mlflow run .` di dalam job dan **minimal 1x sukses** (cek tab Actions, status hijau).
- [ ] Tidak ada secrets sensitif ter-hardcode di workflow file.

**Non-goal:** Tidak perlu upload artefak ke Drive/GitHub, tidak perlu Docker build.

### Kriteria 4 — Monitoring & Logging (Target 2 pts)
**Acceptance criteria:**
- [ ] Model di-serve lokal via `mlflow model serve -m <path_model> -p 5001 --env-manager=local`.
- [ ] Bukti serving (screenshot terminal + response test) disimpan di `1.bukti_serving/`.
- [ ] `prometheus_exporter.py` mengekspos **minimal 3 metrik berbeda** via endpoint `/metrics` (port 8000).
- [ ] `prometheus.yml` mengonfigurasi scrape target ke exporter tersebut.
- [ ] Prometheus native berjalan (`./prometheus --config.file=prometheus.yml`), screenshot 3 metrik di UI Prometheus (`localhost:9090`) → simpan di `4.bukti monitoring Prometheus/`.
- [ ] Grafana native berjalan, dashboard dibuat dengan **nama = username Dicoding**, memvisualisasikan 3 metrik yang sama → screenshot ke `5.bukti monitoring Grafana/`.
- [ ] `inference.py` berfungsi sebagai client uji coba yang memanggil endpoint serving.

**Non-goal:** Tidak perlu alerting Grafana, tidak perlu >3 metrik.

## 5. Urutan Eksekusi (Build Order untuk Agent)

1. Setup environment: `conda create -n mlflow-env python=3.12.7` → install `requirements.txt`.
2. Kerjakan notebook eksperimen (Kriteria 1) sampai menghasilkan dataset bersih.
3. Copy dataset bersih ke `Membangun_model/[namadataset]_preprocessing/` dan `Workflow-CI/MLProject/[namadataset]_preprocessing/`.
4. Jalankan `mlflow ui` lokal → jalankan `modelling.py` → ambil 2 screenshot (Kriteria 2).
5. Push `Workflow-CI/` ke repo baru → pastikan Actions tab hijau (Kriteria 3).
6. Serve model → jalankan `prometheus_exporter.py` → jalankan Prometheus native → jalankan Grafana native → buat dashboard dengan nama username Dicoding → ambil semua screenshot (Kriteria 4).
7. Susun ulang semua ke struktur ZIP final (Section 3) → cek dua repo GitHub berstatus **Public** → compress tanpa ZIP-dalam-ZIP.

## 6. Definition of Done (Full Checklist)

- [ ] 2 repo GitHub Publik (`Eksperimen_SML_[Nama-siswa]`, `Workflow-CI`) — bisa diakses tanpa login.
- [ ] GitHub Actions di repo `Workflow-CI` menunjukkan minimal 1 run sukses (centang hijau).
- [ ] MLflow run tersimpan lokal dengan autolog aktif, terbukti via 2 screenshot.
- [ ] Model ter-serve lokal, terbukti via screenshot request/response.
- [ ] Prometheus mengoleksi 3 metrik, Grafana memvisualisasikan 3 metrik yang sama dengan nama dashboard = username Dicoding.
- [ ] Struktur ZIP final sesuai Section 3, tanpa ZIP-dalam-ZIP.
- [ ] Tidak ada kriteria dengan 0 pts.

## 7. Risiko & Mitigasi

| Risiko | Mitigasi |
|---|---|
| Lupa nama dashboard Grafana pakai username Dicoding | Cek ulang sebelum screenshot final — ini penyebab reject eksplisit di dokumen "Lainnya". |
| Repo GitHub ter-set Private secara default | Set visibility Public saat/​setelah create repo, verifikasi via browser mode incognito. |
| Workflow CI gagal karena environment mismatch | Pin versi Python (3.12.7) dan mlflow (2.19.0) persis di `conda.yaml` dan `ci.yml`. |
| Notebook error saat "Restart & Run All" | Jalankan ulang dari kernel fresh sebelum final commit — reviewer kemungkinan menjalankan ulang. |

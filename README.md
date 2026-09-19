# Eksperimen_SML_Davin-Putra-Fibrian

Repository ini berisi eksperimen dataset untuk sistem machine learning klasifikasi Heart Disease.

## Struktur Folder
```
Eksperimen_SML_Davin-Putra-Fibrian/
├── heart_disease_raw/
│   └── heart_disease.csv
└── preprocessing/
    ├── Eksperimen_Davin-Putra-Fibrian.ipynb
    └── heart_disease_preprocessing/
        └── data_clean.csv
```

## Deskripsi Eksperimen
- **Dataset**: Heart Disease Classification Dataset (~1.025 baris, 13 fitur klinis).
- **Notebook**: `Eksperimen_Davin-Putra-Fibrian.ipynb` memuat:
  1. Data Loading & verifikasi struktur.
  2. Exploratory Data Analysis (EDA): Sebaran target, korelasi fitur, dan visualisasi distribusi.
  3. Preprocessing: Standarisasi fitur kontinu (`StandardScaler`) dan Stratified Split (80:20) untuk mencegah overfitting.
  4. Export data bersih ke `heart_disease_preprocessing/data_clean.csv`.

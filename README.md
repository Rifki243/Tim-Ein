# MBG Tweet Classification — README

## Case 1 BDC Internal 2026

### Deskripsi
Solusi multiclass text classification untuk mengklasifikasikan tweet tentang program Makan Bergizi Gratis (MBG) ke dalam 8 kelas.

### Pendekatan
Pipeline berbasis TF-IDF + Ensemble Machine Learning:

1. **Preprocessing**: lowercase, hapus URL/mention, normalisasi spasi
2. **Fitur**:
   - TF-IDF Word n-gram (1–3) &rarr; menangkap kata dan frasa penting
   - TF-IDF Char n-gram (3–5) &rarr; robust terhadap typo dan bahasa informal
   - Keyword features &rarr; lexicon domain-specific per kelas (8 kategori)
3. **Model**:
   - Logistic Regression (`class_weight='balanced'`)
   - LinearSVC via CalibratedClassifierCV (`class_weight='balanced'`)
4. **Ensemble**: Weighted average probabilitas (bobot dari CV score)
5. **Evaluasi**: 5-Fold Stratified Cross-Validation &rarr; Balanced Accuracy

### Dependensi
```
pip install scikit-learn pandas numpy openpyxl
```
Python 3.8+

### Cara Menjalankan
1. Pastikan file dataset ada di direktori yang sama dengan script:
   - `case_1_labeled_data.xlsx`
   - `case_1_text_to_predict.xlsx`
   - `case_1_template_sheet.xlsx`

2. Jalankan script utama:
```bash
python Ein_code.py
```

3. Output akan tersimpan sebagai `Ein.xlsx`

### Struktur File
```
├── Ein_code.py       # Script utama (preprocessing + training + inferensi)
├── Ein.xlsx          # File prediksi (output)
└── README.md         # Dokumentasi ini
```

### Metrik
- **Balanced Accuracy** (rata-rata recall per kelas)
- CV Score: ~0.608

### Catatan
- `class_weight='balanced'` digunakan secara eksplisit di semua model untuk mengoptimasi balanced accuracy pada distribusi kelas yang tidak seimbang (Ekonomi hanya 145 sampel vs Kualitas Pangan 1247 sampel)
- Char n-gram sangat membantu untuk bahasa Indonesia informal/twitter dengan banyak typo dan singkatan

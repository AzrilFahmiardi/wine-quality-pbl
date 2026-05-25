# Data Loading - Script Pembahasan

## 📋 Tujuan Section
Melakukan loading dataset red wine dan white wine dari repository GitHub dan menggabungkannya menjadi satu dataset yang unified.

## 📊 Output Analysis

### Dataset Dimensions:
```
Dataset red wine:    1,599 samples × 13 features
Dataset white wine:  4,898 samples × 13 features
Dataset gabungan:    6,497 samples × 13 features
```

### Key Insights:

**1. Class Imbalance (Red vs White)**
- Red wine hanya 24.6% dari total dataset
- White wine 75.4% dari total dataset
- Hal ini perlu diperhatikan dalam modeling karena data imbalance

**2. Feature Count**
- Masing-masing dataset memiliki 13 kolom
- 12 kolom original fisikokimia properties
- 1 kolom ditambahkan bernama 'type' untuk membedakan red/white

## 🔍 Fitur-Fitur yang Tersedia

Setiap dataset memiliki 11 fitur fisikokimia yang sama:
1. **fixed acidity** - Keasaman tetap yang tertanam dalam wine
2. **volatile acidity** - Jumlah asam asetat dalam wine
3. **citric acid** - Asam sitrat (biasanya dalam jumlah kecil)
4. **residual sugar** - Gula yang tersisa setelah fermentasi
5. **chlorides** - Konsentrasi garam dalam wine
6. **free sulfur dioxide** - Bentuk bebas SO2
7. **total sulfur dioxide** - Jumlah total SO2 (free + bound)
8. **density** - Kepadatan wine relatif terhadap air
9. **pH** - Tingkat keasaman (0-14 scale)
10. **sulphates** - Aditif SO2 yang berfungsi sebagai antimikroba
11. **alcohol** - Persentase kandungan alkohol
12. **quality** - Rating kualitas (target variable)
13. **type** - Jenis wine (Red/White) - ditambahkan saat loading

## 💡 Proses Loading

### Step-by-Step:
1. **URL Definition**: Dataset diambil dari GitHub repository
2. **Load Red Wine**: `pd.read_csv()` dengan separator `;` (bukan `,`)
3. **Add Type Column**: Column 'type' = 'Red' ditambahkan untuk identifikasi
4. **Load White Wine**: Sama dengan red wine, tapi dengan type = 'White'
5. **Concatenation**: `pd.concat()` menggabungkan kedua dataset

### Catatan Penting:
- Separator yang digunakan adalah `;` bukan `,` 
- Ini umum untuk dataset dari CSV berbahasa Eropa
- Index direset dengan `ignore_index=True` untuk menghindari duplikasi index

## 🎯 Rangkuman
Section ini berhasil loading 6,497 records wine quality dari 2 jenis wine (red dan white) dengan 13 features total.

## ✅ Checklist
- [x] Red wine dataset loaded (1,599 records)
- [x] White wine dataset loaded (4,898 records)
- [x] Tipe wine ditambahkan sebagai identifier
- [x] Dataset digabungkan dengan benar
- [x] Shape verified (6,497 × 13)

---
**Next Section**: Exploratory Overview - Informasi umum tentang dataset

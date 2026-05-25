# Statistik Deskriptif - Script Pembahasan

## 📋 Tujuan Section
Menampilkan statistik deskriptif lengkap untuk semua fitur numerik menggunakan `describe()` method pandas.

## 📊 Output Analysis

```
Statistik Deskriptif Dataset Wine Quality (6,497 samples):

                   fixed acidity  volatile acidity  citric acid  ...  alcohol  quality
count                   6497.000           6497.000     6497.000      6497.000  6497.000
mean                       7.215               0.340        0.319        10.492     5.818
std                        1.296               0.165        0.145         1.193     0.873
min                        3.800               0.080        0.000         8.000     3.000
25% (Q1)                   6.400               0.230        0.250         9.500     5.000
50% (Median)               7.000               0.290        0.310        10.300     6.000
75% (Q3)                   7.700               0.400        0.390        11.300     6.000
max                       15.900               1.580        1.660        14.900     9.000
```

## 🔬 Analisis Detail Per Feature

### 1. **Fixed Acidity** (Mean: 7.215, Std: 1.296)
- **Range**: 3.8 - 15.9 g/L
- **Interpretation**: 
  - Distribusi relatif normal dengan spread yang reasonable
  - Median 7.0 ≈ Mean 7.215 → sedikit skewed ke kanan
  - Variasi sedang (Std/Mean = 0.18)

### 2. **Volatile Acidity** (Mean: 0.340, Std: 0.165)
- **Range**: 0.08 - 1.58 g/L
- **Interpretation**:
  - Shorter range daripada fixed acidity
  - Median 0.29 < Mean 0.34 → right-skewed distribution
  - Volatile acidity berpengaruh pada taste (too much = vinegar taste)

### 3. **Citric Acid** (Mean: 0.319, Std: 0.145)
- **Range**: 0.0 - 1.66 g/L
- **Interpretation**:
  - Min value 0.0 menunjukkan beberapa wine tidak mengandung citric acid
  - Narrow range relatif
  - Biasanya ditambahkan untuk menambah freshness

### 4. **Residual Sugar** (Mean: 5.443, Std: 4.758)
- **Range**: 0.6 - 65.8 g/L
- **Interpretation**:
  - ⚠️ WIDE range! (Std > Mean × 0.8)
  - Median 3.0 << Mean 5.443 → severely right-skewed
  - Mencerminkan perbedaan jenis wine (dry vs sweet)
  - Outlier potential tinggi

### 5. **Chlorides** (Mean: 0.056, Std: 0.035)
- **Range**: 0.009 - 0.611 g/L
- **Interpretation**:
  - Very narrow range
  - Right-skewed (Median 0.047 < Mean 0.056)
  - Represents salt content (normally kept minimal)

### 6. **Free Sulfur Dioxide** (Mean: 30.525, Std: 17.749)
- **Range**: 1.0 - 289.0 mg/L
- **Interpretation**:
  - Wide range indicates variable preservation strategy
  - Important for preventing oxidation
  - Right-skewed distribution

### 7. **Total Sulfur Dioxide** (Mean: 115.745, Std: 56.522)
- **Range**: 6.0 - 440.0 mg/L
- **Interpretation**:
  - Much wider range than free SO2
  - Includes both free and bound sulfur dioxide
  - High variation in wine preservation approach

### 8. **Density** (Mean: 0.995, Std: 0.003)
- **Range**: 0.987 - 1.039
- **Interpretation**:
  - ⚠️ Very small std (0.003) → tight clustering
  - Related to alcohol and residual sugar content
  - Precision measurement needed for this metric

### 9. **pH** (Mean: 3.219, Std: 0.161)
- **Range**: 2.72 - 4.01
- **Interpretation**:
  - Narrow range (acidic scale for wine: 2.5-4.5)
  - Gaussian-like distribution
  - Most wines consistently acidic (pH < 4)

### 10. **Sulphates** (Mean: 0.531, Std: 0.149)
- **Range**: 0.22 - 2.0 g/L
- **Interpretation**:
  - Narrow range overall
  - Added as antimicrobial and antioxidant
  - Right-skewed distribution

### 11. **Alcohol** (Mean: 10.492, Std: 1.193)
- **Range**: 8.0 - 14.9%
- **Interpretation**:
  - Reasonable spread
  - Median 10.3 ≈ Mean 10.492 → relatively symmetric
  - Most wines in 9-11% alcohol range

### 12. **Quality** (Target Variable, Mean: 5.818, Std: 0.873)
- **Range**: 3 - 9 (scale)
- **Interpretation**:
  - ⚠️ **Imbalanced target!** 
  - Median = Mode = 6 (most common rating)
  - Concentrated between 5-6 (80% of data)
  - Few high-quality (8-9) dan low-quality (3) wines
  - **Challenge untuk classification**: imbalanced class distribution

## 📈 Statistical Insights

### Distributional Characteristics:
| Feature | Skewness | Variability | Concern |
|---------|----------|-------------|---------|
| Residual Sugar | Very High | High | Outliers likely |
| Free/Total SO2 | High | High | Preprocessing needed |
| Citric Acid | Medium | Low | Min value = 0 |
| Quality | Medium | Low | ⚠️ Imbalanced target |
| Density | Very Low | Very Low | Tight range |
| pH | Low | Very Low | Stable feature |

### Key Takeaways:
1. **Feature Scaling**: Features memiliki magnitude sangat berbeda → standardization perlu
2. **Outliers**: Residual sugar dan SO2 features kemungkinan punya outliers
3. **Target Imbalance**: Quality skewed ke 5-6 → perlu stratified sampling
4. **Data Quality**: Tidak ada missing values ✅

## 🎯 Rangkuman
Statistik deskriptif mengungkapkan bahwa dataset memiliki high feature scale variation, right-skewed distributions pada beberapa features, dan imbalanced target variable yang memerlukan preprocessing khusus.

## ✅ Checklist
- [x] Descriptive statistics generated
- [x] Feature scale variations identified
- [x] Skewness patterns analyzed
- [x] Target imbalance noted
- [x] Ready untuk data quality assessment

---
**Next Section**: Data Quality Assessment - Missing values dan duplicate analysis

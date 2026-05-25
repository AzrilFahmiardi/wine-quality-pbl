# Feature Distributions - Script Pembahasan

## 📋 Tujuan Section
Menganalisis distribusi dari setiap fitur menggunakan visualisasi (histogram, KDE plots) untuk memahami pola dan karakteristik data.

## 📊 Sub-Sections Overview

Section ini terbagi menjadi 3 bagian utama:
1. **Distribusi Target Variable (Quality)**
2. **Distribusi Fitur Fisikokimia** (11 input features)
3. **Perbandingan Distribusi Antar Tipe Wine** (Red vs White)

---

## 1. Distribusi Target Variable (Quality) 🎯

### Data Overview:
```
Quality Score Range: 3 - 9
Mean: 5.818
Median: 6.000
Mode: 6 (most common)
Std: 0.873
```

### Distribution Pattern:
```
Frequency:
3: ▓ (30 samples)
4: ▓▓▓ (200 samples)
5: ████████░░ (1,457 samples)
6: ████████████ (2,836 samples)
7: ████░░░░░░░ (1,079 samples)
8: ▓▓░░░░░░░░░ (193 samples)
9: ▓░░░░░░░░░░ (5 samples)
```

### Key Characteristics:

**Imbalanced Distribution:**
- **Concentrated in middle**: 80% samples berkisar 5-6
- **Few extreme values**: 
  - Low quality (3-4): 3.5% of data
  - High quality (8-9): 3% of data
- **"J-shaped" or "left-truncated"**: More wines dengan quality 6 vs quality 3

### Implications untuk Modeling:

#### Problem: Class Imbalance
```
Distribution skewness:
- Quality 5: ~22.4% (underrepresented low)
- Quality 6: ~43.6% (overrepresented middle)
- Quality 7: ~16.6% (underrepresented high)
- Other:     ~17.4% (rare classes)

Model Risk: Bias toward predicting quality 6
```

#### Solutions:
```
1. Stratified K-Fold Cross-Validation
   - Maintain quality distribution in each fold
   
2. Class Weight Balancing
   - Weight rare classes (3-4, 8-9) higher
   
3. Oversampling Minority Classes
   - Use SMOTE untuk generate synthetic samples
   
4. Undersampling Majority Class
   - Reduce samples dari quality 6
   
5. Threshold Adjustment
   - Tune decision boundary untuk rare classes
```

### Recommendation:
**Use stratified k-fold with class weights** untuk balance antara data quantity dan quality distribution accuracy.

---

## 2. Distribusi Fitur Fisikokimia 🧪

### 11 Features Analysis:

#### A. **Fixed Acidity**
- Distribution: Approximately normal/Gaussian
- Shape: Bell curve centered ~7.0
- Outliers: Nilai > 10 relatively rare
- Implication: Natural feature, ready for modeling

#### B. **Volatile Acidity**
- Distribution: Right-skewed
- Peak: 0.2-0.3
- Long tail: Hingga 1.58
- Implication: Most wines low, some with high acetic acid

#### C. **Citric Acid**
- Distribution: Bimodal (two peaks)
- Peak 1: 0.0 (wines tanpa citric acid)
- Peak 2: 0.3-0.4
- Implication: Binary decision (added or not)

#### D. **Residual Sugar**
- Distribution: Highly right-skewed/log-normal
- Peak: 1-2 (dry wines)
- Long tail: Up to 65.8 (sweet wines)
- Implication: **Strong outliers**, need log-transformation

#### E. **Chlorides**
- Distribution: Right-skewed
- Peak: 0.04-0.05
- Tail: Up to 0.61
- Implication: Salt content tightly controlled

#### F. **Free Sulfur Dioxide**
- Distribution: Right-skewed
- Peak: 10-30
- Range: 1-289
- Implication: Variable preservation strategy

#### G. **Total Sulfur Dioxide**
- Distribution: Right-skewed
- Peak: 100-130
- Range: 6-440
- Implication: Much wider range than free SO2

#### H. **Density**
- Distribution: Approximately normal
- Range: 0.987-1.039 (very narrow!)
- Peak: 0.995-0.997
- Implication: Tightly clustered, stable feature

#### I. **pH**
- Distribution: Approximately normal
- Peak: 3.1-3.3
- Range: 2.7-4.0 (typical wine pH)
- Implication: Stable acidity levels

#### J. **Sulphates**
- Distribution: Right-skewed
- Peak: 0.4-0.6
- Tail: Up to 2.0
- Implication: Antimicrobial agent, mostly standard

#### K. **Alcohol**
- Distribution: Approximately normal
- Peak: 9-11%
- Range: 8-14.9%
- Implication: Most wines similar alcohol content

### Distribution Summary Table:

| Feature | Shape | Skewness | Action Needed |
|---------|-------|----------|--------------|
| Fixed Acidity | Normal | Low | None |
| Volatile Acidity | Right-skewed | Medium | Consider log-transform |
| Citric Acid | Bimodal | High | Consider categorical encoding |
| Residual Sugar | Highly Skewed | Very High | **Log-transform essential** |
| Chlorides | Right-skewed | Medium | Consider log-transform |
| Free SO2 | Right-skewed | Medium | Consider log-transform |
| Total SO2 | Right-skewed | Medium | Consider log-transform |
| Density | Normal | Low | None |
| pH | Normal | Low | None |
| Sulphates | Right-skewed | Medium | Consider log-transform |
| Alcohol | Normal | Low | None |

---

## 3. Perbandingan Distribusi Antar Tipe Wine 🍷🍾

### Red Wine vs White Wine Comparison:

#### Karakteristik:
```
Red Wine:     1,599 samples (24.6%)
White Wine:   4,898 samples (75.4%)
```

#### Expected Differences:

| Feature | Red Wine | White Wine | Significance |
|---------|----------|-----------|--------------|
| Fixed Acidity | Higher | Lower | Red punya more acids |
| Volatile Acidity | Potentially Higher | Lower | Red oxidize faster |
| Citric Acid | Often Present | Varied | Red usually has some |
| Residual Sugar | Typically Lower | Can be Higher | White often sweeter |
| Alcohol | Potentially Higher | Lower | Red fermentation deeper |
| Quality | Varied | Varied | Independent variable |

### Modeling Implications:

```
Separate Models vs Single Model:

Option 1: Single Unified Model
├─ Pros:
│  ├─ More training data
│  └─ Captures general wine quality patterns
├─ Cons:
│  ├─ May miss red/white specific patterns
│  └─ Feature distributions different
│
└─ Mitigation: Include 'type' as feature with one-hot encoding

Option 2: Separate Models per Type
├─ Pros:
│  ├─ Captures type-specific patterns
│  └─ Better feature distribution fit
├─ Cons:
│  ├─ Reduced training data (~1,600 for red)
│  └─ Harder to predict unknown type first
│
└─ Use if: Classification boundaries significantly different

RECOMMENDATION: Single model dengan 'type' as feature
```

## 🎯 Rangkuman

Feature Distribution Analysis mengungkapkan:

1. **Target Variable (Quality)**:
   - Imbalanced dengan clustering di 5-6
   - Perlu stratified sampling dan class weighting

2. **Input Features**:
   - Right-skewed features: Residual Sugar, SO2, Volatile Acidity, Chlorides
   - Need log-transformation untuk normalization
   - Normal features: Fixed Acidity, Density, pH, Alcohol → ready as-is

3. **Wine Type Differences**:
   - Red dan White memiliki karakteristik fisikokimia berbeda
   - Best approach: Single model dengan type sebagai feature

## ✅ Action Checklist

- [x] Quality distribution analyzed (imbalanced)
- [x] Feature distributions examined (multiple shapes)
- [x] Skewness patterns identified
- [x] Transformation needs identified (log-transform candidates)
- [x] Red vs White distributions compared
- [x] Ready untuk correlation analysis

---
**Next Section**: Correlation Analysis - Matriks korelasi antar features dan dengan target variable

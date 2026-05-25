# Correlation Analysis - Script Pembahasan

## 📋 Tujuan Section
Menganalisis hubungan linear antar features (Pearson correlation) untuk identifikasi:
1. Features yang berkorelasi kuat dengan quality (target)
2. Multicollinearity antar input features
3. Feature importance untuk modeling

## 📊 Sub-Sections Overview

Section ini terbagi menjadi 3 bagian:
1. **Matriks Korelasi** - Correlation matrix antar semua features
2. **Heatmap Korelasi** - Visual representation dengan color-coded matrix
3. **Korelasi dengan Target Variable (Quality)** - Sorted feature importance

---

## 1. Matriks Korelasi (Pearson Correlation) 📊

### Konsep:
```
Pearson Correlation Coefficient (r):
- Range: -1 hingga +1
- +1: Perfect positive correlation
-  0: No correlation
- -1: Perfect negative correlation

Formula: r = Cov(X,Y) / (σx * σy)
Interpretation Level:
- |r| > 0.7: Strong correlation
- 0.4 < |r| ≤ 0.7: Moderate correlation
- 0.2 < |r| ≤ 0.4: Weak correlation
- |r| ≤ 0.2: Very weak/negligible correlation
```

### Expected Correlations dalam Wine Data:

#### Features yang Berkorelasi Satu Sama Lain:

```
High Correlations:
├─ Fixed Acidity ↔ Density (positive)
│  └─ More acids = denser wine
│
├─ Fixed Acidity ↔ pH (negative)
│  └─ More acid = lower pH
│
├─ Alcohol ↔ Density (negative)
│  └─ More alcohol = less dense
│
├─ Free SO2 ↔ Total SO2 (positive)
│  └─ Free adalah bagian dari total
│
├─ Residual Sugar ↔ Density (positive)
│  └─ More sugar = denser
│
└─ Alcohol ↔ Residual Sugar (correlation varies)
   └─ Depends on fermentation completeness
```

### Multicollinearity Concern:

**Multicollinearity**: Features highly correlated dengan feature lain
- **Problem**: Redundant information, reduces model interpretability
- **Risk Level**:
  - r > 0.8: High multicollinearity
  - 0.5 < r ≤ 0.8: Moderate multicollinearity
  - r ≤ 0.5: Low multicollinearity

**Handling Multicollinearity**:
```
1. Identify highly correlated pairs (|r| > 0.7)
2. Options:
   a) Keep both (if both useful for prediction)
   b) Remove one (reduce dimensionality)
   c) Create composite feature (PCA)
   d) Keep during training, check VIF (Variance Inflation Factor)
```

---

## 2. Heatmap Korelasi 🔥

### Interpretation:

A heatmap visualizes correlation matrix dengan:
- **Warna merah/panas**: Positive correlation
- **Warna biru/dingin**: Negative correlation
- **Warna putih**: No correlation

### Key Patterns to Look For:

```
Pattern 1: Strong Positive (Red Blocks)
└─ Features yang naik bersamaan

Pattern 2: Strong Negative (Blue Blocks)
└─ Saat satu naik, yang lain turun

Pattern 3: No Pattern (White/Gray Blocks)
└─ Independent features

Important: Diagonal selalu = 1 (perfect self-correlation)
```

---

## 3. Korelasi dengan Target Variable (Quality) 🎯

### Hasil Lengkap (Sorted by Strength):

```
Feature Correlation with Quality (Sorted):
==================================================
alcohol                   : 0.4443   ← STRONGEST
density                   : 0.3059
volatile acidity          : 0.2657
chlorides                 : 0.2007
citric acid               : 0.0855
fixed acidity             : 0.0767
free sulfur dioxide       : 0.0555
total sulfur dioxide      : 0.0414
sulphates                 : 0.0385
residual sugar            : 0.0370
pH                        : 0.0195   ← WEAKEST
```

### Detailed Analysis:

#### TIER 1: Strong Features (|r| > 0.30)

**1. Alcohol (r = 0.4443)** ⭐⭐⭐⭐⭐
- Strongest predictor of quality
- Interpretation: Higher alcohol → generally higher rated quality
- Model impact: **HIGH** - include always
- Mechanism: Alcohol affects taste perception, completeness of fermentation

**2. Density (r = 0.3059)** ⭐⭐⭐
- Second strongest predictor
- Interpretation: Lower density → slightly higher quality
- Model impact: **MODERATE-HIGH**
- Caveat: Highly correlated dengan alcohol (-), so multicollinearity concern
- Note: Density reflects alcohol + residual sugar content

---

#### TIER 2: Moderate Features (0.20 < |r| < 0.30)

**3. Volatile Acidity (r = 0.2657)** ⭐⭐⭐
- Moderate predictor
- Interpretation: Lower volatile acidity → higher quality
- Model impact: **MODERATE**
- Mechanism: Volatile acidity creates vinegary taste (negative for quality)
- Actionable: Feature engineering opportunity (highlight acetic acid levels)

**4. Chlorides (r = 0.2007)** ⭐⭐⭐
- Borderline moderate predictor
- Interpretation: Lower chlorides → slightly higher quality
- Model impact: **MODERATE** (barely in tier 2)
- Mechanism: Salt content should be minimal for good wine taste

---

#### TIER 3: Weak Features (|r| < 0.20)

**5. Citric Acid (r = 0.0855)**
- Weak predictor
- Interpretation: Minimal correlation dengan quality
- Model impact: **LOW**
- Note: Bimodal distribution membuat correlation weak

**6. Fixed Acidity (r = 0.0767)**
- Weak predictor
- Interpretation: Linear relationship tidak kuat
- Model impact: **LOW**
- Reason: Fixed acidity important untuk taste, tapi quantity less predictive

**7. Free Sulfur Dioxide (r = 0.0555)**
- Very weak predictor
- Model impact: **VERY LOW**
- Note: Optimization level (not too much, not too little) → non-linear

**8. Total Sulfur Dioxide (r = 0.0414)**
- Very weak predictor
- Model impact: **VERY LOW**
- Similar to free SO2, optimal range complicates correlation

**9. Sulphates (r = 0.0385)**
- Very weak predictor
- Model impact: **VERY LOW**
- Note: Often added uniformly → low variance

**10. Residual Sugar (r = 0.0370)**
- Very weak predictor
- Model impact: **VERY LOW**
- Insight: Wine quality independent dari sweetness level
- Implication: Red dan white wines dengan different sugar levels rated similarly

**11. pH (r = 0.0195)** ⭐ (Weakest)
- Essentially no correlation
- Model impact: **NEGLIGIBLE**
- Reason: Most wines dalam narrow pH range (2.7-4.0) → low variance

---

## 🔬 Feature Importance Summary

### For Modeling Decisions:

```
Feature Selection Strategy:

Must-Have Features (Tier 1):
├─ alcohol           [REQUIRED]
└─ density           [CONSIDER multicollinearity]

Important Features (Tier 2):
├─ volatile acidity  [INCLUDE]
└─ chlorides         [INCLUDE]

Optional Features (Tier 3):
├─ citric acid       [INCLUDE if no overfitting]
├─ fixed acidity     [INCLUDE for completeness]
├─ free SO2          [OPTIONAL]
├─ total SO2         [OPTIONAL - redundant with free SO2]
├─ sulphates         [OPTIONAL]
├─ residual sugar    [OPTIONAL]
└─ pH                [CONSIDER REMOVING]
```

### Multicollinearity Check:

```
Potential High Correlations:
├─ alcohol ↔ density       [Strong negative expected]
│  └─ Action: Check VIF, may drop one
│
├─ free SO2 ↔ total SO2   [Strong positive expected]
│  └─ Action: Consider keeping only total SO2
│
├─ fixed acidity ↔ pH     [Negative expected]
│  └─ Action: One measure of acidity, may be redundant
│
└─ residual sugar ↔ density [Positive expected]
   └─ Action: Check correlation strength
```

### Model Building Recommendation:

```python
# Initial Model Setup:
STRONG_FEATURES = ['alcohol', 'volatile_acidity', 'chlorides', 'density']

# Option A: Start Simple (Best Practice)
features = STRONG_FEATURES
# Result: Simpler model, easier interpretation, avoids overfitting

# Option B: Include All
features = all_features_except_weak_ones
# Result: More data but higher multicollinearity risk

# Option C: Feature Engineering
features = STRONG_FEATURES + [create_composite_features()]
# Result: Domain-specific features capturing wine chemistry
```

---

## 🎯 Rangkuman

Correlation Analysis mengungkapkan:

1. **Strong Predictors**:
   - Alcohol (r=0.44) adalah strongest predictor
   - Density (r=0.31) secondary predictor
   - Together explain ~20% of quality variance (R²)

2. **Moderate Predictors**:
   - Volatile Acidity (r=0.27) dan Chlorides (r=0.20)
   - Reasonable predictive power

3. **Weak Predictors**:
   - Citric acid, Fixed acidity, SO2 compounds, Sulphates, Residual Sugar, pH
   - |r| < 0.09 → minimal linear relationship

4. **Modeling Implications**:
   - No single feature dominates (highest r=0.44)
   - Need ensemble approach (multiple moderate features)
   - Multicollinearity likely between alcohol/density/sugar/density
   - Non-linear relationships possible (SO2 optimal range)

---

## ✅ Action Checklist

- [x] Pearson correlations calculated
- [x] Feature-feature correlations analyzed
- [x] Feature-target correlations ranked
- [x] Multicollinearity concerns identified
- [x] Feature importance prioritized
- [x] Ready untuk outlier detection and treatment

---

**Next Section**: Outlier Detection and Analysis - Identifikasi anomalous values menggunakan IQR method

# Spotify Pop Genres Analysis - Summary

## i. Exploratory Data Analysis (EDA)

### Data Visualization Conducted:

1. **Genre Distribution Analysis**
   - Analyzed 10 pop genre categories from 114 total genres in the dataset
   - Pop genres: cantopop, indie-pop, j-pop, k-pop, mandopop, pop, pop-film, power-pop, synth-pop
   - Total of 9,002 tracks across these pop genres

2. **Distribution Visualizations**
   - **Box plots** for each genre showing popularity distribution
   - **Scatter plots** comparing danceability vs popularity for each genre
   - **IQR (Interquartile Range) analysis** for popularity across all pop genres
   - Combined visualizations for all pop genres together

3. **Key Features Analyzed**
   - **Acousticness**: Measure of acoustic sound (0-1 scale)
   - **Valence**: Musical positiveness/happiness (0-1 scale)
   - **Loudness**: Overall loudness in decibels (-60 to 0 dB)
   - **Danceability**: How suitable a track is for dancing (0-1 scale)
   - **Popularity**: Track popularity score (0-100)

### Data Splitting:

**Train/Validation/Test Split: 60% / 20% / 20%**
- **Training set**: 5,401 samples (60%)
- **Validation set**: 1,800 samples (20%)
- **Test set**: 1,801 samples (20%)
- Used **stratified splitting** to maintain genre proportions across all splits
- Random state = 42 for reproducibility

---

## ii. Data Pre-processing and Feature Engineering

### Pre-processing Steps:

1. **Data Cleaning**
   - Removed unnecessary columns: `track_id`, `album_name`, `Unnamed: 0`
   - Converted `explicit` column from boolean to integer (0/1)
   - Checked for missing values (Result: ✓ No missing values found)

2. **Feature Standardization**
   - Applied **StandardScaler** to normalize numeric features
   - Mean = 0, Standard Deviation = 1
   - Critical for logistic regression and PCA

3. **Data Type Verification**
   - Verified all numeric features are proper data types
   - Generated descriptive statistics for all features

### Feature Engineering:

1. **Feature Binning** (in `feature_engineering.ipynb`)
   - Created categorical bins for continuous features:
     - **Acousticness bins**: Low (0-0.33), Medium (0.33-0.67), High (0.67-1.0)
     - **Valence bins**: Negative (0-0.33), Neutral (0.33-0.67), Positive (0.67-1.0)
     - **Loudness bins**: Quiet (-60 to -20 dB), Medium (-20 to -10 dB), Loud (-10 to 0 dB)
     - **Danceability bins**: Low (0-0.33), Medium (0.33-0.67), High (0.67-1.0)
   
2. **Analysis of Binned Features**
   - Visualized distribution of each binned feature
   - Analyzed relationship between binned features and popularity
   - Identified patterns: louder songs tend to be more popular

3. **Duration Conversion** (for regression analysis)
   - Converted duration from milliseconds to seconds for interpretability

---

## iii. Regression Analysis (Linear Regression)

### Application:

**Target Variable**: Danceability (continuous, 0-1 scale)

**Approach**:
1. **Feature Selection**: Used SelectKBest with f_regression to select top 10 predictors
2. **Selected Features**: popularity, explicit, speechiness, liveness, valence, tempo, time_signature, and one-hot encoded genres (k-pop, pop, power-pop)
3. **Models Tested**:
   - Linear Regression
   - Ridge Regression (L2 regularization)

### Results:

| Model | R² | RMSE | MAE |
|-------|-----|------|-----|
| Linear Regression | 0.3418 | 0.1133 | 0.0889 |
| Ridge Regression | 0.3417 | 0.1133 | 0.0889 |

### Insights from Analysis:

1. **Model Performance**:
   - Low R² (~34%) indicates the model is **underfitting**
   - Linear relationships insufficient to capture danceability patterns
   - Actual vs Predicted plot shows significant scatter around the diagonal

2. **Feature Importance**:
   - **Valence** was selected as one of the top 10 predictors
   - **Popularity** included in feature selection
   - Genre encodings (k-pop, pop, power-pop) were important
   - Suggests non-linear relationships exist between features and danceability

3. **Regularization**:
   - **Ridge regression showed NO improvement** over basic linear regression
   - Regularization unnecessary because:
     - Model is underfitting (not overfitting)
     - Simple model already struggles to capture patterns
     - Adding penalty further restricts model's ability to fit data
   
4. **Key Learning**:
   - Danceability has complex, **non-linear relationships** with predictors
   - More sophisticated models (Random Forest, Neural Networks) would likely perform better
   - Feature engineering (polynomial features, interactions) could help

---

## iv. Logistic Regression Analysis

### Application:

**Target Variable**: Mode (binary classification - Major key = 1, Minor key = 0)

**Approach**:
1. **Initial Analysis**: Fit logistic regression with all 12 numeric features (standardized)
2. **Feature Selection**: Identified top 2 features by coefficient magnitude:
   - **Danceability**: coefficient = -0.2499
   - **Energy**: coefficient = -0.2124
3. **Final Model**: Trained logistic regression using only danceability and energy
4. **Cross-Validation**: 5-fold stratified cross-validation

### Results:

**Training Set Performance** (with threshold = 0.66):
- Accuracy: ~67-70%
- True Positive Rate (Sensitivity): ~67%
- True Negative Rate (Specificity): ~68%

**Validation Set Performance**:
- **AUC Score**: 0.592
- ROC curve analysis completed

**Cross-Validation Results** (5-fold):
- **Mean AUC**: 0.592
- **Mean Accuracy**: 0.543
- Optimal threshold identified at 0.668

### Insights from Analysis:

1. **Feature Importance**:
   - **Danceability** and **Energy** are the most important predictors of musical mode
   - Both have negative coefficients:
     - Higher danceability → more likely Minor key
     - Higher energy → more likely Minor key
   - This makes musical sense: minor keys often associated with more energetic, danceable pop music

2. **Model Performance**:
   - Modest predictive power (AUC ~0.59)
   - Better than random guessing (AUC 0.5) but not highly discriminative
   - Mode (major/minor) is influenced by many subtle musical factors beyond danceability and energy

3. **Regularization**:
   - **No explicit regularization applied** (used solver='liblinear' without penalty parameter tuning)
   - Model is relatively simple (only 2 features)
   - Regularization would be more relevant with more features or overfitting concerns
   - Performance suggests underfitting rather than overfitting

4. **Key Learnings**:
   - Musical mode has measurable relationship with audio features
   - **Danceability** appears as important feature in both regression (predicting danceability itself) and classification (predicting mode)
   - Model performance indicates additional features or non-linear models could improve predictions
   - Cross-validation shows consistent performance across folds

5. **Comparison to Regression**:
   - Logistic regression performed better for its task (binary classification) than linear regression did for predicting danceability
   - Both models suggest non-linear relationships exist in the data
   - Feature importance can be directly interpreted from coefficients after standardization

---

## Summary of Findings Across All Analyses:

1. **EDA revealed** distinct patterns across 10 pop genres with significant variation in popularity and audio features
2. **Feature engineering** created interpretable categorical features that showed relationships with popularity
3. **Linear regression** struggled with danceability prediction (R² = 0.34), suggesting non-linear relationships
4. **Logistic regression** showed modest success (AUC = 0.59) in mode prediction using danceability and energy
5. **Key features** (acousticness, valence, loudness, danceability, popularity) all contribute differently to various prediction tasks
6. **Regularization** was not beneficial in either case due to model underfitting rather than overfitting
7. **Genre differences** important - one-hot encoding helped in regression, stratified splitting preserved distributions

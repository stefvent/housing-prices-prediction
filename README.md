# House Price Prediction

A machine learning project that predicts house prices based on property features such as size, number of rooms, location, and amenities. Built using Linear Regression and Random Forest Regression.

---

## Dataset

- **Source:** [Kaggle Housing Prices Dataset](https://www.kaggle.com/datasets/yasserh/housing-prices-dataset)
- **Size:** 545 rows, 13 columns
- **Target variable:** `price`

---

## Features

| Feature | Description |
|---|---|
| `price` | Sale price of the house (target) |
| `area` | Size of the house in square feet |
| `bedrooms` | Number of bedrooms |
| `bathrooms` | Number of bathrooms |
| `stories` | Number of floors |
| `parking` | Number of parking spaces |
| `mainroad` | Connected to main road (yes/no) |
| `guestroom` | Has a guest room (yes/no) |
| `basement` | Has a basement (yes/no) |
| `hotwaterheating` | Has hot water heating (yes/no) |
| `airconditioning` | Has air conditioning (yes/no) |
| `prefarea` | Located in a preferred area (yes/no) |
| `furnishingstatus` | Furnishing level (furnished/semi-furnished/unfurnished) |

---

## Project Structure

```
├── Housing.csv                    # Dataset
├── HousingPricesPrediction.ipynb  # Main Jupyter Notebook
├── requirements.txt               # Python dependencies
└── README.md
```

---

## Methodology

### Preprocessing
All preprocessing lives inside a scikit-learn `Pipeline`, so it is fit on training data only (no test-set leakage):
- Missing-value imputation (numerical → median, categorical → mode)
- One-hot encoded categorical variables
- Standardized numerical features

### Feature Engineering
- `total_rooms` = bedrooms + bathrooms
- `area_per_room` = area / (total_rooms + 1)

### Models
- **Mean-price baseline** — `DummyRegressor`, the bar any model must beat
- **Linear Regression** — simple, interpretable model
- **Random Forest Regression** — to capture non-linear relationships

### Evaluation
- 80/20 train/test split (MAE, RMSE, R²)
- 5-fold cross-validation to check that model comparisons are not a fluke of one split
- Standardized Linear Regression coefficients to rank feature impact

---

## Results

**Held-out test set (20%, 109 houses)**

| Model | R² | MAE (% of mean price) | RMSE (% of mean price) |
|---|---|---|---|
| Mean-price baseline | — | 34.9% | 45.3% |
| Linear Regression | 0.66 | 19.3% | 26.2% |
| Random Forest | 0.61 | 20.8% | 28.0% |

**5-fold cross-validation (all 545 houses)**

| Model | R² (mean ± std) | MAE (% of mean price) | RMSE (% of mean price) |
|---|---|---|---|
| Linear Regression | 0.64 ± 0.08 | 16.7% | 22.6% |
| Random Forest | 0.64 ± 0.04 | 16.6% | 22.8% |

Linear Regression cuts the average prediction error by about 45% compared with the mean-price baseline. It scored higher than Random Forest on the single test split, but cross-validation shows the two models are effectively tied. With equal accuracy, Linear Regression is preferred because its coefficients are directly interpretable. The small dataset (545 rows) likely limits the benefit of more complex models.

### Key Findings
- `area` is the strongest predictor of price
- Amenities and location add large premiums: air conditioning, hot water heating, and being in a preferred area
- Unfurnished houses sell at a discount relative to furnished ones
- No single feature dominates — multiple features together drive price

---

## Requirements

Python 3.9+ with the packages in `requirements.txt`. Install with:
```bash
pip install -r requirements.txt
```

---

## Usage

1. Clone the repository
```bash
git clone https://github.com/stefvent/housing-prices-prediction.git
cd housing-prices-prediction
```

2. Install dependencies
```bash
pip install -r requirements.txt
```

3. Open and run the notebook (the dataset `Housing.csv` is included)
```bash
jupyter notebook HousingPricesPrediction.ipynb
```

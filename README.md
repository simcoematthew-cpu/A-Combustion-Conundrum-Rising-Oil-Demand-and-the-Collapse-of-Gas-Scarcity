[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![XGBoost](https://img.shields.io/badge/ML-XGBoost-orange.svg)](https://xgboost.readthedocs.io/)

## Overview

This project investigates a critical vulnerability in U.S. energy markets: **how crashes in oil prices can trigger severe natural gas supply shocks**, causing gas prices to spike dramatically and hurting American consumers.

The Permian Basin produces roughly 28% of major U.S. shale gas — but this gas is a *byproduct* of oil drilling. When oil prices collapse, drilling stops, and this massive gas supply disappears from the market regardless of gas demand.

Using machine learning (XGBoost) and economic modeling, we quantify this "Oil-Induced Gas" risk and demonstrate how interconnected energy markets create hidden vulnerabilities for consumers.


## Key Findings

| Finding | Value |
|---------|-------|
| Permian's share of major U.S. shale gas production | ~28% |
| Oil's share of Permian drilling revenue | ~90% |
| Predicted gas price increase if Permian supply removed | 100-140% |
| WTI-Bottleneck correlation (supports "uncorking" thesis) | Positive |

### The Core Thesis

1. **Associated Gas Economics**: Permian Basin producers drill primarily for oil, which provides ~90% of revenue. Natural gas is essentially a byproduct that gets produced regardless of gas prices — producers are **price-insensitive to gas**.

2. **The Hidden Vulnerability**: Because Permian gas production is driven by *oil* economics, not gas economics, a severe oil price crash would halt drilling and remove ~28% of major U.S. shale gas supply — **even if gas demand remains strong**.

3. **Consumer Impact**: This supply shock would cause gas prices to spike by 100-140%, dramatically increasing heating and electricity costs for American households and businesses.

4. **Secondary Effect — The "Uncorking"**: As a side observation, the project also tracks how falling oil prices reduce pipeline congestion (the "uncorking" effect), as less associated gas flows through capacity-constrained Permian infrastructure.

## Project Structure

```
├── OIG_Complete.ipynb      # Main analysis notebook (14 cells)
├── data/
│   ├── Permian.csv         # Permian Basin production data
│   ├── Appalachia.csv      # Appalachian Basin production data
│   ├── Haynesville.csv     # Haynesville Shale production data
│   ├── WTI.csv             # West Texas Intermediate oil prices
│   ├── Henry_Price.csv     # Henry Hub natural gas prices
│   └── ngshistory.csv      # EIA natural gas storage data
├── README.md
└── requirements.txt
```

## Methodology

### Phase 1: Supply Shock Analysis

- Merged production data from three major U.S. shale basins (Permian, Appalachia, Haynesville)
- Calculated market share evolution over time
- Analyzed revenue composition to explain producer behavior

### Phase 2: Midstream Bottleneck Analysis

- Integrated EIA regional storage data (South Central region as Permian proxy)
- Created "Bottleneck Proxy" metric: `Production / Storage`
- Demonstrated correlation between oil prices and pipeline congestion
- This secondary analysis shows how reduced drilling eases infrastructure constraints

### Phase 3: Machine Learning Counterfactual

- **Model**: XGBoost Regressor
- **Target**: Henry Hub natural gas prices
- **Features**: 
  - WTI oil prices
  - Lagged production values (1, 3, 6 months)
  - Rolling averages (3, 6 months)
  - Seasonal indicators (month)
- **Validation**: Temporal train/test split (pre/post 2023)

### Counterfactual Simulation

We simulate a "No Permian" scenario by:
1. Setting Permian production features to zero
2. Adjusting total supply accordingly
3. Predicting prices under this counterfactual
4. Comparing to actual prices

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/oil-induced-gas.git
cd oil-induced-gas

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## Requirements

```
pandas>=1.5.0
numpy>=1.21.0
matplotlib>=3.5.0
seaborn>=0.11.0
xgboost>=1.6.0
scikit-learn>=1.0.0
```

## Usage

1. **Configure data path** in Cell 0:
```python
data_path = r"your/data/path/here"
```

2. **Run all cells** sequentially in Jupyter Notebook or JupyterLab

3. **Key outputs**:
   - Visualizations (production trends, correlations, market share, counterfactual prices)
   - Summary tables (model comparison, sensitivity analysis)
   - Exported CSV with all calculated features

## Notebook Structure

| Cell | Purpose |
|------|---------|
| 0 | Configuration and imports |
| 1 | Data loading and merging (production + prices + storage) |
| 2 | Exploratory visualization: Production vs. Prices |
| 3 | Correlation heatmap |
| 4 | Oil vs. Gas revenue analysis (proves price insensitivity) |
| 5 | Midstream bottleneck analysis (secondary "uncorking" effect) |
| 6 | Feature engineering (lags, rolling windows) |
| 7 | XGBoost model training |
| 8 | Market share stacked area chart (shows systemic risk) |
| 9 | Counterfactual simulation (quantifies consumer impact) |
| 10 | Sensitivity analysis (PED scenarios) |
| 11 | Model comparison table |
| 12 | Final counterfactual visualization |
| 13 | Export and summary |

## Visualizations

### 1. Market Share Evolution
Shows the Permian Basin's growing dominance in U.S. shale gas production — illustrating the increasing systemic risk.
<img width="1126" height="563" alt="image" src="https://github.com/user-attachments/assets/b5949e4f-b64f-4f6b-a36e-30e5c25cc18c" />

### 2. The "Permian Subsidy" (Counterfactual Analysis)
The key visualization: actual gas prices vs. predicted prices without Permian supply. **The gap represents how much more consumers would pay if oil prices crashed and Permian drilling stopped.**

### 3. Oil Price vs. Bottleneck Proxy
Secondary analysis showing correlation between WTI prices and pipeline congestion metrics.

## Economic Model

We apply price elasticity of demand (PED) analysis to translate supply shocks into price impacts:

```
Price Change = Supply Shock / |PED|
```

| PED Assumption | Interpretation | Price Impact (28% shock) |
|----------------|----------------|--------------------------|
| -0.15 | Very Inelastic (Winter) | ~187% |
| -0.20 | Moderately Inelastic | ~140% |
| -0.25 | Less Inelastic | ~112% |

**Translation**: At a base price of $2.75/MMBtu, removing Permian supply could spike prices to $6.60/MMBtu or higher — a devastating impact on consumer heating and electricity bills.

## Data Sources

- **Production Data**: U.S. Energy Information Administration (EIA) Drilling Productivity Report
- **Price Data**: EIA spot prices (WTI, Henry Hub)
- **Storage Data**: EIA Weekly Natural Gas Storage Report

## Limitations

- Model assumes historical relationships persist into the future
- Does not account for LNG export dynamics or international factors
- Regional storage is a proxy for Permian-specific bottlenecks (Waha hub prices would be more precise)
- Short-run elasticity estimates vary in academic literature
- Does not model potential supply response from other basins if Permian production falls

## Future Work

- [ ] Incorporate Waha Hub spot prices for direct basis spread analysis
- [ ] Add LNG export capacity as a feature
- [ ] Model supply response from Appalachia/Haynesville if Permian drops
- [ ] Extend to quarterly forecasting
- [ ] Test alternative ML models (LSTM, Prophet)
- [ ] Analyze historical oil crash episodes (2014-2016, 2020) for validation

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- U.S. Energy Information Administration for public data access
- Academic literature on natural gas price elasticity

## Contact

For questions or collaboration inquiries, please open an issue on this repository.

---

*This project demonstrates a critical vulnerability in U.S. energy markets: American consumers' natural gas bills are hostage to oil market dynamics, because so much of our gas supply is a byproduct of oil drilling in the Permian Basin.*

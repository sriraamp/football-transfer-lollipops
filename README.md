# Football Transfer Lollipops (2000–2025)

This R visualization explores the world record football transfers from 2000 to 2025, adjusted for European football market inflation (9% annually). Each transfer is visualized as a "lollipop" showing:

- 🎨 Selling club (bottom half of the stick)
- 🎯 Buying club (top half)
- 🏷️ Inflation-adjusted fee (in 2025 euros)

The project uses `ggplot2`, `ggrepel`, `tidyr`, and `forcats` to produce a clean, annotated chart with club-specific color coding.

## 📊 Key Features
- Inflation-adjusted fees based on football market inflation
- Player names sorted by fee
- Clear dual-color lollipops
- Legend mapping each club to its color
- Readable fee annotations

## 📁 Structure
football-transfer-lollipops/
├── transfer_lollipop.R # Main R script
├── README.md # Project overview
├── plots/ # Exported images (e.g., PNGs)
└── data/ # (Optional) Raw transfer data

## 📷 Sample Output

![Sample Plot](plots/lollipop_transfers.png)

## ⚙️ Dependencies
- ggplot2
- ggrepel
- tidyr
- dplyr
- forcats

## 🏁 Getting Started
1. Clone the repo  
2. Run `transfer_lollipop.R` in RStudio  
3. Adjust export or aesthetics as needed

# 📊 Wallet Analysis (Base) - Dashboard Documentation

This dashboard provides a comprehensive look at an individual wallet's activity on the **Base (Layer 2)** network. It covers gas consumption, transaction frequency across different token standards, and detailed NFT trade performance tracking using Seaport 1.6 data.

## 🚀 Overview

The **Wallet Analysis (Base)** dashboard is designed for users to input a single wallet address and instantly gain insights into their on-chain behavior, spending habits, and trading profitability.

### Key Features

* **Gas Tracking:** Analyze ETH spent on transaction fees over time.
* **Multi-Standard Activity:** Breakdown of activity across Native ETH, ERC-20, ERC-721, and ERC-1155.
* **NFT P/L Analysis:** Specific tracking for Seaport 1.6 (OpenSea) trades, accounting for partial fills and gas costs.

---

## 📈 Component Details

### 1. [Base] Gas Spend Counter

This chart tracks the daily and cumulative ETH spent on gas by the input wallet.

* **Logic:** It filters `base.transactions` where the wallet is the sender (`from`), calculates the ETH cost (), and applies a window function for the cumulative total.
* **Use Case:** Ideal for identifying "heavy" usage days and monitoring long-term network costs.

### 2. [Base] Transaction Counter

A granular breakdown of all interactions the wallet has had with the Base network.

* **Data Sources:** Joins data from `base.transactions`, `base.traces`, and event tables for `erc20`, `erc721`, and `erc1155`.
* **Metrics:** Displays daily counts for top-level transactions, internal traces, and token transfers.
* **Scoping:** Uses `COALESCE` to ensure dates align across all transaction types for an accurate "Total Transaction Count" per day.

### 3. [Base] Daily and Total ERC-721 NFTs Sold

A focused view on outbound NFT activity.

* **Logic:** Counts `evt_Transfer` events from the `erc721_base` schema where the wallet is the sender.
* **Visualization:** Provides a scannable bar/line chart of daily sales vs. lifetime sales.

### 4. [Base] OpenSea NFT Trade (Net P/L)

The most complex component, this query calculates the **actual net cash flow** for NFT trades via Seaport 1.6.

* **Seaport Logic:** It captures ETH sent to and from the Seaport contract, correctly handling partial-fill refunds which are often missed by basic transfer monitors.
* **Cost Basis:** Allocates gas fees to each specific trade to provide a **Net P/L** (Profit/Loss) figure rather than just Gross Revenue.
* **Parameters:** Users can filter this specific view by `Start Date` and `End Date`.

---

## 🛠 How to Use

1. **Enter Wallet Address:** Paste the target address (0x...) into the `wallet address` parameter field at the top of the dashboard.
2. **Set Date Range:** (Optional) Use the Seaport date parameters to focus on a specific trading window.
3. **Run:** Click the "Run" button to refresh the visualizations.

---

## 📝 Technical Notes

* **Network:** Base (Chain ID: 8453)
* **SQL Engine:** Dune SQL
* **Precision:** Gas and ETH values are cast to `DECIMAL(10,5)` or similar to ensure human-readable chart scales while maintaining accuracy.

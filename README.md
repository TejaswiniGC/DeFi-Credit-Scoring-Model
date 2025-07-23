# Aave V2 Wallet Credit Scoring

## Project Overview

This project implements a rule-based credit scoring model for wallets interacting with the Aave V2 decentralized finance (DeFi) protocol. The goal is to evaluate wallet behavior based on on-chain transaction data and assign a credit score from 0 to 1000. This score serves as an indicator of a wallet's reliability and risk, differentiating between healthy, responsible usage and potentially risky or exploitative behavior.

## Methodology & Architecture

The scoring system is built on a simple, transparent, and interpretable rule-based model. The process consists of three main stages:

1.  **Data Loading and Preprocessing:**
    * The raw transaction data, provided as a JSON file, is loaded into a Pandas DataFrame.
    * The nested `actionData` field is flattened to create a single, clean table for analysis.
    * Irrelevant columns are dropped to focus on key transaction details.

2.  **Feature Engineering:**
    * The preprocessed data is aggregated by `userWallet` to create a set of behavioral features.
    * Key features include:
        * `total_tx_count`: Total number of transactions.
        * `wallet_age_days`: The duration of a wallet's activity on the protocol.
        * `total_deposit_usd`: Total value of deposited assets in USD.
        * `total_repay_usd` and `total_borrow_usd`: Total values of repaid and borrowed assets.
        * `repayment_ratio_usd`: The ratio of total repayments to total borrowings, a key indicator of creditworthiness.
        * `num_liquidations_as_borrower`: A count of liquidation events, which are a major negative signal.

3.  **Rule-Based Scoring:**
    * A base score of 500 is assigned to each wallet.
    * This score is then adjusted based on the engineered features:
        * **Positive Indicators:** Features like `wallet_age_days`, `total_deposit_usd`, and a high `repayment_ratio_usd` positively influence the score.
        * **Negative Indicators:** Features like `num_liquidations_as_borrower` and a high `borrow_to_deposit_ratio` heavily penalize the score.
    * The final score is clamped to a range of 0 to 1000.

## Project Files

* `MainScript.ipynb`: The Python script that runs the entire pipeline from data loading to score generation.
* `user-wallet-transactions.json`: The raw input data file.
* `wallet_scores.csv`: The output file containing the final credit score for each unique wallet.
* `analysis.md`: A detailed report on the score distribution and an examination of high- and low-scoring wallet behaviors.

## How to Run the Script

This project is designed to be run in a Google Colab notebook for ease of use, with no local setup required.

1.  **Open Google Colab:** Go to [colab.research.google.com](https://colab.research.google.com/) and create a new notebook.
2.  **Upload the Data:**
    * On the left-hand side, click the "Files" icon (folder icon).
    * Click the "Upload to session storage" icon (up arrow) and upload the `user-wallet-transactions.json` file.
    * **Note:** The file must be named exactly `user-wallet-transactions.json` for the script to find it.
3.  **Paste the Code:** Copy the entire Python script from `[Your_Main_Script_Name].py` and paste it into a code cell in the notebook.
4.  **Run the Code:** Click the "Run" button (play icon) or press `Ctrl + Enter`.
5.  **Retrieve the Output:** After the script finishes, a new file named `wallet_scores.csv` will appear in the "Files" section. Right-click and select "Download" to save it to your local machine.

## Things to Note (Colab Specifics)

* **Temporary Storage:** The files you upload to the Colab files section are stored in a temporary virtual machine. They will be deleted when the runtime disconnects. Make sure to download your output file before closing the notebook.
* **Dependencies:** The script relies on standard libraries like `pandas`, `numpy`, and `matplotlib`. Colab comes with these pre-installed, so you do not need to run `pip install` commands unless you introduce a new, non-standard library.
* **Debugging:** The Colab environment is useful for debugging. If you encounter an error, you can easily inspect the state of your variables and DataFrames in separate cells.

---
_This README provides a clear and concise guide to the project. For a deeper dive into the results and model validation, please refer to the `analysis.md` file._
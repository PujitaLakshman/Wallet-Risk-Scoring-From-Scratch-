Ethereum Wallet Risk Scoring using RiskProp + XGBoost
Overview
This project identifies potentially risky Ethereum wallets by analyzing their transaction history and interaction behavior using both graph-based propagation and machine learning techniques. The methodology includes:
- RiskProp: A graph-based method that simulates how risk or trust spreads across wallets based on their connections.
- XGBoost: A machine learning model that learns patterns in transaction behavior to classify wallets.

The final output is a risk score (0 to 1000) for each wallet, where higher values indicate higher risk.
1. Data Collection
Fetch Transaction History: Retrieving the transaction data for each provided wallet address from Compound V2 or V3 protocol using the Covalent API.
PROCESS:
The Covalent API enables access to blockchain data like Ethereum transactions via web requests.

1. Wallets Input: A list of wallet addresses (e.g., 0xfe5a...) is stored in a CSV file. We use the fetch_csv() function to read them.
2. API Setup:
   • API_KEY = "your_api_key" (obtain it by signing up at covalenthq.com)
   • CHAIN_ID = 1 (Ethereum Mainnet)
   • Input/output file paths are defined.

   For each wallet, we call:
   https://api.covalenthq.com/v1/{CHAIN_ID}/address/{wallet}/transactions_v2/?key={API_KEY}

3. Data Fetching: This URL is hit for every wallet in the CSV. The API returns JSON data.
4. Saving Output: We use fetch_transaction_data() to retrieve the data and save it to a local .json file for later use.
What the data we fetched actually means:
Basic Transaction Info
Field	Meaning
block_signed_at	When the transaction was recorded on blockchain
tx_hash	Unique ID of the transaction
from_address, to_address	Who sent and who received the transaction
value	Amount of ETH (in wei) sent
value_quote	USD value of that ETH at that time
successful	Was the transaction successful? (true/false)
Gas (Transaction Fee) Info
Log events: Token transfers or approvals during the transaction
2. Data Preprocessing
Clean and organize the transaction data by parsing sender, receiver, and value. Create two dictionaries:
- outgoing_transactions: where a wallet sends tokens
- incoming_transactions: from where it receives tokens
These help build a transaction graph.
3. Data Exploration (Exploratory Data Analysis)
Analyze wallet behavior, distribution of transaction values, frequency of transfers, and detect outliers.
4. Feature Selection and Feature Engineering
Generate De-anonymous Score based on:
- Number of connections a wallet has
- Whether it mostly sends or receives tokens
- Combine these into a score for each transaction
5. Training and Testing Data Split
Split the data into training and testing sets. If labels are available (e.g., 0 = safe, 1 = risky), use them to train the XGBoost model.
6. Model Selection
Use XGBoost Classifier due to its strong performance with structured data and interpretability of results.
7. Model Training and Evaluation
Train the model on the training dataset. Evaluate performance using metrics like:
- Accuracy
- Precision
- Recall
- F1 Score
8. Improving Model by Improving Scoring Rate (0–1000)
Improve scoring by refining:
- RiskProp’s scoring logic
- Feature set (include wallet age, interaction frequency, etc.)
- Adjust thresholds dynamically
- Tune hyperparameters of XGBoost for better accuracy

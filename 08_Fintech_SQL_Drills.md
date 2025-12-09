# 💰 Fintech SQL Drills: The "CoFee" Database

Since KeyValue works heavily in Fintech (Payments), you **must** know how to write queries for Ledgers, Transactions, and Reporting. Complex `JOIN`s and `Window Functions` are fair game.

---

## 🏗️ The Schema
*   **Users** (`user_id`, `name`, `email`, `created_at`)
*   **Wallets** (`wallet_id`, `user_id`, `balance`, `currency`)
*   **Transactions** (`txn_id`, `sender_wallet_id`, `receiver_wallet_id`, `amount`, `status`, `timestamp`)

---

## ⚡ Drills (Write the query before peeking)

### 1. The "Double Spending" Check (Self Join)
**Scenario:** Find all users who made two transactions to the **same receiver** within **1 minute** of each other (Potential bug/fraud).
```sql
SELECT 
    t1.sender_wallet_id, 
    t1.receiver_wallet_id, 
    t1.timestamp
FROM Transactions t1
JOIN Transactions t2 
    ON t1.sender_wallet_id = t2.sender_wallet_id 
    AND t1.receiver_wallet_id = t2.receiver_wallet_id
    AND t1.txn_id != t2.txn_id
WHERE 
    ABS(EXTRACT(EPOCH FROM (t1.timestamp - t2.timestamp))) < 60;
```
*Tip: `EXTRACT(EPOCH...)` is Postgres specific. In MySQL use `TIMESTAMPDIFF(SECOND, ...)`.*

### 2. The "Rich List" (Rank Window Function)
**Scenario:** Find the top 3 richest users in *each* currency.
```sql
WITH RankedWallets AS (
    SELECT 
        u.name, 
        w.balance, 
        w.currency,
        DENSE_RANK() OVER (PARTITION BY w.currency ORDER BY w.balance DESC) as rank
    FROM Wallets w
    JOIN Users u ON w.user_id = u.user_id
)
SELECT * FROM RankedWallets WHERE rank <= 3;
```
*Why `DENSE_RANK`? If two people have same balance, they both get rank 1. `RANK` skips numbers (1, 1, 3).*

### 3. The "Daily Volume" Report (Group By + Date Trunc)
**Scenario:** Calculate total money moved per day for the last 7 days.
```sql
SELECT 
    DATE(timestamp) as txn_date, 
    SUM(amount) as total_volume,
    COUNT(*) as total_count
FROM Transactions
WHERE 
    status = 'SUCCESS' 
    AND timestamp >= NOW() - INTERVAL '7 days'
GROUP BY 1
ORDER BY 1 DESC;
```

### 4. The "Missing Money" (Left Join)
**Scenario:** Find Users who have created a wallet but NEVER made a transaction (Sender).
```sql
SELECT u.name
FROM Users u
JOIN Wallets w ON u.user_id = w.user_id
LEFT JOIN Transactions t ON w.wallet_id = t.sender_wallet_id
WHERE t.txn_id IS NULL;
```

### 5. The "Running Balance" (Cumulative Sum)
**Scenario:** Show the ledger for a specific wallet: `txn_id`, `amount`, and `running_balance` after each transaction.
```sql
SELECT 
    txn_id, 
    amount, 
    SUM(amount) OVER (ORDER BY timestamp ASC) as running_balance
FROM Transactions
WHERE sender_wallet_id = 'w_123'
ORDER BY timestamp ASC;
```

### 6. The "ACID" Trap (Transaction Block)
**Scenario:** How do you move money safely?
*   *Wrong:* Two separate updates.
*   *Correct:*
```sql
BEGIN;

-- 1. Deduct from Sender (Check balance first!)
UPDATE Wallets SET balance = balance - 500 WHERE wallet_id = 'A' AND balance >= 500;

-- 2. Add to Receiver
UPDATE Wallets SET balance = balance + 500 WHERE wallet_id = 'B';

-- 3. Record Log
INSERT INTO Transactions (sender, receiver, amount) VALUES ('A', 'B', 500);

COMMIT;
```

---

## 🧠 Theory Cheatsheet for Fintech DBs
1.  **Isolation Levels:** "Serializable" is safest but slow. "Read Committed" is standard.
2.  **Locking:** `SELECT ... FOR UPDATE` locks the row so no one else can read/write it until you commit. Critical for preventing race conditions (e.g., two people spending same available balance).
3.  **Audit Columns:** Every table needs `created_at`, `updated_at`, and `created_by`. Never delete rows (`soft_delete = true`).

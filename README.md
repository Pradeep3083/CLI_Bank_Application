# 🏦 CLI Bank Application (Java/JDBC)

This is a simple command-line interface (CLI) banking application built in Java, utilizing **Java Database Connectivity (JDBC)** to manage user accounts and transactions in a **MySQL** database.

## ✨ Features

The application provides core banking functionalities via a command-line menu:

* **Secure Authentication:** User accounts are stored securely using **BCrypt** password hashing.
* **Account Creation:** Users can sign up with an initial deposit.
* **Login/Logout:** Secure access control for banking operations.
* **Core Transactions:**
    * Deposit funds into the account.
    * Withdraw funds, with balance checks to prevent overdrafts.
* **View Data:** Check current balance and view the last 10 transaction records.
* **Data Integrity:** All transactions (Deposit/Withdrawal) are handled using JDBC transactions (`setAutoCommit(false)`) to ensure **atomicity** (all or nothing).

---

## 🛠️ Technology Stack

* **Language:** Java (JDK 11+)
* **Database:** MySQL
* **Connectivity:** JDBC API
* **Dependencies:**
    * `mysql-connector-java`: JDBC Driver
    * `jbcrypt`: Secure password hashing library
* **Build Tool:** Maven

---

## ⚙️ Setup Instructions

### 1. Database Configuration

You must have a running **MySQL** instance.

1.  **Create Database and Tables:** Execute the following SQL script to set up the necessary schema:

    ```sql
    CREATE DATABASE IF NOT EXISTS cli_bank_db;
    USE cli_bank_db;

    CREATE TABLE Accounts (
        account_id INT AUTO_INCREMENT PRIMARY KEY,
        username VARCHAR(50) NOT NULL UNIQUE,
        password_hash VARCHAR(60) NOT NULL,
        balance DECIMAL(10, 2) NOT NULL DEFAULT 0.00
    );

    CREATE TABLE Transactions (
        transaction_id INT AUTO_INCREMENT PRIMARY KEY,
        account_id INT NOT NULL,
        type VARCHAR(10) NOT NULL,
        amount DECIMAL(10, 2) NOT NULL,
        transaction_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        FOREIGN KEY (account_id) REFERENCES Accounts(account_id)
    );
    ```

2.  **Update Connection Details:** Edit the `DBConnection.java` file and update the `URL`, `USER`, and `PASS` constants to match your local MySQL configuration.

### 2. Project Build (Maven)

1.  Navigate to the project root directory in your terminal.
2.  Build the project using Maven:

    ```bash
    mvn clean install
    ```

---

## ▶️ How to Run

1.  Ensure the database is running and configured.
2.  Run the main class (`BankAppCLI.java` or `Main.java` if you kept your original package structure) from your IDE or using the compiled JAR:

    ```bash
    # Example using Maven exec plugin (if configured)
    mvn exec:java -Dexec.mainClass="com.bankapp.cli.BankAppCLI"
    
    # Or run from your IDE (Recommended)
    ```

The application will start, and you will be presented with the main menu to **Create New Account** or **Login**.

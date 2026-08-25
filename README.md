# Java-Order-System

A Java Swing point-of-sale (POS) desktop app for a food/beverage business — order entry, payment, receipt export, and sales reporting — backed by a local SQLite database, with role-based access for Cashier and Admin accounts.

**A ready-to-use build is available in [`/dist`](./dist) — `app_OrderEnv.jar` (with its `lib` folder) requires only a Java Runtime.**

## Description

The app presents a menu of meal, beverage, and dessert items (each with an image, price, and quantity entry). A logged-in cashier adds items to a running order, processes payment (with change calculation), and the order is saved to the database. Logged-in Admin accounts get an additional sales dashboard to review, export, or clear historical order records.

## Features

**Login**
- Username/password login checked against a `tblAccounts` database table
- Two account types with different access: **CASHIER** (order entry) and **ADMIN** (sales reporting)

**Ordering (Cashier)**
- Menu of meal, beverage, and dessert items, each ordered with a quantity (max 10 per item)
- Running order table with live total
- Cancel a single item or cancel the whole in-progress order
- Process payment — validates the amount covers the total and calculates change
- Cancel/change a payment before confirming

**Sales Reporting (Admin)**
- List of past sales by date
- View all order line-items and total sales for a selected date
- Remove an individual order record
- Clear **all** order records (with a confirmation prompt, since it's irreversible)
- Export the currently viewed sales table to a `receipt_.txt` file — either to a chosen folder or the Desktop by default

## Tech Stack

- **Java** (Swing/AWT for the GUI)
- **SQLite** (via `sqlite-jdbc`) — the database actually used at runtime (`sqliteorder.sqlite`)
- `rs2xml.jar` — for binding SQL `ResultSet`s directly to Swing tables
- Built with **NetBeans** (Ant-based build, see `build.xml` / `nbproject/`)

Note: a MySQL JDBC driver (`mysql-connector-java`) and a MySQL schema (`db/mysqlord.sql`) are also included in the repo, but the application code (`DBConnection.java`) connects to the SQLite database — the MySQL files appear to be from an earlier/alternate design.

## Prerequisites

- **Java Runtime Environment (JRE)** — to run the packaged jar
- **Java Development Kit (JDK)** + NetBeans (or another Java IDE/Ant) — only needed if building from source

## Installation

Clone the repository:

```bash
git clone https://github.com/paoradox/Java-Order-System.git
cd Java-Order-System
```

To build from source, open the project in NetBeans (or run Ant directly using `build.xml`).

## Usage

### Option 1: Run the packaged build (recommended)

From [`/dist`](./dist):

```bash
java -jar "app_OrderEnv.jar"
```

Keep the `lib` folder alongside the jar — it contains the required SQLite JDBC driver and other dependencies referenced in the jar's manifest.

### Option 2: Run from source

Build and run via NetBeans, or compile with Ant using `build.xml`.

### Workflow

1. **Log in** with a username and password (stored in the `tblAccounts` table).
2. **As Cashier:** select menu items and quantities to build an order, then process payment — the order is saved and a receipt can be exported.
3. **As Admin:** select a date from the sales list to view that day's orders and total, remove individual records, export the view to a text file, or clear all records.

## Configuration

- The database connection points to `sqliteorder.sqlite` (SQLite), expected in the application's working directory, with tables including `tblAccounts`, `tblOrders_temp`, and `tblOrders_perm`.
- A sample/starting database file (`sqliteorder.sqlite`) is included at the repo root and in `/dist`; a backup/alternate copy is archived in `stock_db/sqliteorder.7z`.

## Project Structure

```
Java-Order-System/
├── dist/
│   ├── app_OrderEnv.jar        # Packaged runnable jar
│   ├── lib/                    # Bundled dependency jars (SQLite JDBC, rs2xml, etc.)
│   ├── sqliteorder.sqlite       # Database used by the packaged build
│   └── README.TXT               # NetBeans build-output notes
├── src/
│   └── jpaj_package/
│       ├── App_Env.java         # Main application window and all business logic
│       ├── App_Env.form         # NetBeans GUI form definition
│       ├── DBConnection.java    # SQLite connection helper
│       └── *.png                # Menu item images and toolbar icons
├── img/                         # Menu item images and toolbar icons (duplicate of src copies)
├── db/
│   ├── mysqlord.sql             # MySQL schema (earlier/alternate design, not used at runtime)
│   ├── mysql-connector-java-5.1.45-bin.jar
│   ├── sqlite-jdbc-3.21.0.jar
│   └── rs2xml.jar
├── stock_db/
│   └── sqliteorder.7z           # Archived/backup copy of the SQLite database
├── build/                       # Compiled classes (Ant build output)
├── nbproject/                   # NetBeans project configuration
├── build.xml                    # Ant build script
├── manifest.mf                  # Jar manifest
└── sqliteorder.sqlite            # Working copy of the database (repo root)
```

## License

Not specified.

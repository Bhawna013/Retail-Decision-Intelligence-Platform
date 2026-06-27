# Data Dictionary — Retail Pricing & Merchandise Analytics Platform

This data dictionary outlines the schema for the normalized retail database derived from the supply chain dataset, augmented with engineered financial and inventory metrics to support pricing optimization and planning analytics.

---

## 1. Table: `products`
Contains master data for all merchandise items available on the platform.

| Column Name | Data Type | Key | Description | Example |
| :--- | :--- | :---: | :--- | :--- |
| `product_id` | INT | PK | Unique identifier for each product | `1360` |
| `product_name` | VARCHAR(100) | | The commercial name of the item | `Smart watch` |
| `category_id` | INT | FK | Links to the `categories` table | `45` |
| `product_price` | DECIMAL(10,2) | | The baseline retail selling price before promotional discounts | `329.99` |
| `mrp` | DECIMAL(10,2) | | *Engineered:* Maximum Retail Price (Catalog value simulated at 1.1x–1.5x of price) | `429.99` |
| `cost_price` | DECIMAL(10,2) | | *Engineered:* Product procurement/manufacturing cost (Simulated at 55%–80% of price) | `197.50` |

---

## 2. Table: `orders`
Contains header-level records for every customer transaction.

| Column Name | Data Type | Key | Description | Example |
| :--- | :--- | :---: | :--- | :--- |
| `order_id` | INT | PK | Unique identifier for the order | `180517` |
| `customer_id` | INT | FK | Links to the `customers` table | `20755` |
| `order_date` | DATETIME | | Timestamp of when the order was placed | `2026-06-27 13:55:00` |
| `order_status` | VARCHAR(30) | | Status of the order (COMPLETE, PENDING, CANCELED, SUSPECTED_FRAUD) | `COMPLETE` |
| `market` | VARCHAR(20) | | High-level global market region | `LATAM` |

---

## 3. Table: `order_items`
Line-item detail breakdown for products within an order.

| Column Name | Data Type | Key | Description | Example |
| :--- | :--- | :---: | :--- | :--- |
| `order_item_id` | INT | PK | Unique identifier for the transaction row | `451201` |
| `order_id` | INT | FK | Links to the `orders` table | `180517` |
| `product_id` | INT | FK | Links to the `products` table | `1360` |
| `quantity` | INT | | Number of units purchased in this line item | `1` |
| `sales` | DECIMAL(10,2) | | Gross sales value (calculated as `product_price * quantity`) | `329.99` |
| `discount_amount` | DECIMAL(10,2) | | Total value deducted from the gross sale via discounts | `16.50` |
| `net_revenue` | DECIMAL(10,2) | | Final order amount paid by customer (`sales - discount_amount`) | `313.49` |
| `order_profit` | DECIMAL(10,2) | | Actual profit generated from the item sale after costs | `101.22` |

---

## 4. Table: `inventory`
*Engineered Table* — Tracks stock metrics to evaluate replenishment risks, inventory turnover, and dead stock scenarios.

| Column Name | Data Type | Key | Description | Example |
| :--- | :--- | :---: | :--- | :--- |
| `product_id` | INT | PK, FK | Links directly to the `products` table | `1360` |
| `current_stock` | INT | | Number of physical units currently available in the warehouse | `245` |
| `safety_stock` | INT | | Minimum buffer stock required to prevent stockouts | `50` |
| `reorder_point` | INT | | Stock threshold level that triggers a replenishment order | `85` |
| `lead_time_days` | INT | | Average number of days required for supplier delivery | `5` |

---

## 5. Table: `customers`
Contains demographic and segmentation data for registered marketplace buyers.

| Column Name | Data Type | Key | Description | Example |
| :--- | :--- | :---: | :--- | :--- |
| `customer_id` | INT | PK | Unique identifier for the customer | `20755` |
| `customer_fname` | VARCHAR(50) | | Customer's first name | `Mary` |
| `customer_segment` | VARCHAR(30) | | Market segment classification (Consumer, Corporate, Home Office) | `Consumer` |
| `customer_city` | VARCHAR(50) | | City of the customer's billing address | `Caguas` |
| `customer_state` | VARCHAR(10) | | State code of the customer's billing address | `PR` |





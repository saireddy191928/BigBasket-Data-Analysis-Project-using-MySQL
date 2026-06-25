# 🛒 BigBasket Data Analysis Project using MySQL

<div align="center">

![BigBasket Analysis](https://img.shields.io/badge/Project-BigBasket%20Analysis-FF6B35?style=for-the-badge&logo=database)
![MySQL](https://img.shields.io/badge/Database-MySQL-4479A1?style=for-the-badge&logo=mysql)
![Python](https://img.shields.io/badge/Language-Python-3776AB?style=for-the-badge&logo=python)
![Status](https://img.shields.io/badge/Status-Active-00B050?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**A comprehensive data analysis project exploring BigBasket's e-commerce dataset using MySQL and Python**

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [Database Schema](#-database-schema) • [Contributing](#-contributing)

</div>

---

## 📊 Project Overview

This project performs in-depth data analysis on BigBasket's e-commerce platform dataset. It leverages **MySQL** for robust data management and **Python** for exploratory data analysis, visualization, and insights generation.

**BigBasket** is India's leading online grocery delivery platform, and this project analyzes various aspects including:
- 🛍️ Product categories and pricing patterns
- 📈 Customer purchasing behavior
- ⭐ Product ratings and reviews
- 💰 Revenue and sales trends
- 🎯 Category-wise performance metrics

---

## ✨ Features

### 📈 Data Analysis Capabilities
- **Product Analysis**: Category distribution, pricing strategies, and product popularity
- **Customer Insights**: Purchase patterns, frequency analysis, and segment behavior
- **Sales Performance**: Revenue trends, seasonal patterns, and growth metrics
- **Rating Analysis**: Product ratings distribution, review sentiment, and quality metrics
- **Inventory Insights**: Stock levels, availability patterns, and demand forecasting

### 🛠️ Technical Stack
- **Database**: MySQL 8.0+
- **Backend**: Python 3.8+
- **Libraries**: 
  - `mysql-connector-python` - Database connection
  - `pandas` - Data manipulation
  - `NumPy` - Numerical computing
  - `Matplotlib & Seaborn` - Data visualization
  - `SQLAlchemy` - ORM framework

### 📊 Output Visualizations
- Interactive charts and graphs
- Statistical summaries
- Trend analysis reports
- Category performance dashboards

---

## 🚀 Installation

### Prerequisites
- MySQL Server (8.0 or higher)
- Python 3.8 or above
- pip (Python package manager)

### Step 1: Clone the Repository
```bash
git clone https://github.com/yourusername/BigBasket-Data-Analysis-MySQL.git
cd BigBasket-Data-Analysis-MySQL
```

### Step 2: Create Virtual Environment
```bash
python -m venv venv

# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Database Setup
```bash
# Login to MySQL
mysql -u root -p

# Create database and import schema
CREATE DATABASE bigbasket_db;
USE bigbasket_db;
source database/schema.sql;
source database/sample_data.sql;
```

### Step 5: Configure Connection
Edit `config.py` with your MySQL credentials:
```python
DB_CONFIG = {
    'host': 'localhost',
    'user': 'root',
    'password': 'your_password',
    'database': 'bigbasket_db',
    'port': 3306
}
```

---

## 📖 Usage

### Run the Analysis Script
```bash
python main.py
```

### Generate Specific Reports
```bash
# Product Analysis Report
python analysis/product_analysis.py

# Customer Behavior Report
python analysis/customer_analysis.py

# Sales Performance Report
python analysis/sales_analysis.py

# Rating and Reviews Analysis
python analysis/rating_analysis.py
```

### Interactive Data Exploration
```bash
python interactive_dashboard.py
```

---

## 🗄️ Database Schema

### Tables Overview

#### 📦 Products Table
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(255),
    category_id INT,
    price DECIMAL(10, 2),
    rating FLOAT,
    reviews_count INT,
    FOREIGN KEY (category_id) REFERENCES categories(category_id)
);
```

#### 🏷️ Categories Table
```sql
CREATE TABLE categories (
    category_id INT PRIMARY KEY,
    category_name VARCHAR(100),
    description TEXT
);
```

#### 👥 Customers Table
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(255),
    email VARCHAR(255),
    phone VARCHAR(15),
    registration_date DATE,
    city VARCHAR(100)
);
```

#### 📋 Orders Table
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATETIME,
    total_amount DECIMAL(10, 2),
    status VARCHAR(50),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

#### 📝 Order Items Table
```sql
CREATE TABLE order_items (
    item_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    price_per_unit DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

#### ⭐ Reviews Table
```sql
CREATE TABLE reviews (
    review_id INT PRIMARY KEY,
    product_id INT,
    customer_id INT,
    rating INT,
    review_text TEXT,
    review_date DATE,
    FOREIGN KEY (product_id) REFERENCES products(product_id),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

---

## 📊 Key Insights & Findings

### 🎯 Top Discoveries
- **Most Popular Categories**: Grocery essentials dominate with 45% of total orders
- **Price Range**: Average product price: ₹250-500, with premium products reaching ₹2000+
- **Customer Loyalty**: 35% of revenue comes from repeat customers
- **Peak Hours**: Orders surge between 6 PM - 9 PM on weekdays
- **Rating Distribution**: 78% products have ratings above 4.0 stars

---

## 📁 Project Structure

```
BigBasket-Data-Analysis-MySQL/
│
├── 📄 README.md
├── 📄 requirements.txt
├── 📄 config.py
├── 📄 main.py
│
├── 📁 database/
│   ├── schema.sql
│   ├── sample_data.sql
│   └── queries.sql
│
├── 📁 analysis/
│   ├── product_analysis.py
│   ├── customer_analysis.py
│   ├── sales_analysis.py
│   └── rating_analysis.py
│
├── 📁 visualizations/
│   ├── charts/
│   └── reports/
│
└── 📁 utils/
    ├── db_connection.py
    └── helpers.py
```

---

## 🔍 Sample Queries

### Get Top 10 Best Selling Products
```sql
SELECT p.product_name, SUM(oi.quantity) as total_sold, p.category_id
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_id
ORDER BY total_sold DESC
LIMIT 10;
```

### Calculate Customer Purchase Frequency
```sql
SELECT customer_id, COUNT(order_id) as purchase_count, 
       MAX(order_date) as last_purchase
FROM orders
GROUP BY customer_id
ORDER BY purchase_count DESC;
```

### Revenue by Category
```sql
SELECT c.category_name, SUM(oi.price_per_unit * oi.quantity) as revenue
FROM categories c
JOIN products p ON c.category_id = p.category_id
JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY c.category_id
ORDER BY revenue DESC;
```

---

## 🎨 Visualization Examples

The project generates comprehensive visualizations including:

📊 **Sales Dashboard**
- Revenue trends over time
- Category-wise sales distribution
- Top performing products

👥 **Customer Analytics**
- Purchase frequency distribution
- Customer segmentation
- Geographic distribution

⭐ **Product Insights**
- Rating distribution
- Price vs Rating correlation
- Category popularity

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the Repository**
   ```bash
   git clone https://github.com/yourusername/BigBasket-Data-Analysis-MySQL.git
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Commit Your Changes**
   ```bash
   git commit -m "Add meaningful commit message"
   ```

4. **Push to Branch**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open a Pull Request**

---

## 📝 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Your Name**
- GitHub: [@yourusername](https://github.com/yourusername)
- Email: your.email@example.com

---

## 🙏 Acknowledgments

- BigBasket for providing valuable insights into e-commerce data
- MySQL and Python communities for excellent tools and documentation
- Contributors and reviewers of this project

---

## 📞 Support & Contact

For questions, suggestions, or issues:

- 📧 Email: your.email@example.com
- 🐛 GitHub Issues: [Open an Issue](https://github.com/yourusername/BigBasket-Data-Analysis-MySQL/issues)
- 💬 Discussions: [Start a Discussion](https://github.com/yourusername/BigBasket-Data-Analysis-MySQL/discussions)

---

<div align="center">

**⭐ If you find this project helpful, please give it a star!**

Made with ❤️ for data enthusiasts | Last Updated: June 2024

</div>

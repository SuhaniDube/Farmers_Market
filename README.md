# Farmer's Market

A web application that connects local farmers with merchants through a simple bidding marketplace. Farmers list fresh produce with a base price; merchants browse listings and place competitive bids to get the best deals.

## Features

- **User accounts** — Register as a **farmer** or **merchant**, with email, name, and phone number
- **Product listings** — Farmers can add, edit, and delete products with images, categories, quantity, and pricing
- **Bidding system** — Merchants place bids above the base price on products they want
- **Dashboard** — Farmers see their own listings; merchants see all available products
- **Product discovery** — Browse, search, filter by category, and sort by price or date on the home page
- **Admin tools** — View all users and clean up duplicate bids (admin account only)

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | [Flask](https://flask.palletsprojects.com/) 2.0 |
| Database | SQLite via [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/) |
| Authentication | [Flask-Login](https://flask-login.readthedocs.io/) |
| Frontend | Bootstrap 5, Font Awesome, vanilla JavaScript |
| Forms | Flask-WTF |

## Project Structure

```
farmer market (final)/
├── main.py                 # Application entry point
├── init_db.py              # Reset and recreate the database
├── view_db.py              # Print database contents to the terminal
├── requirements.txt        # Python dependencies
├── app/
│   ├── __init__.py         # App factory and database setup
│   ├── models.py           # User, Product, and Bid models
│   ├── views.py            # Main routes and API endpoints
│   ├── auth.py             # Login, sign-up, and logout
│   ├── static/
│   │   ├── css/style.css
│   │   ├── js/main.js
│   │   └── img/products/   # Uploaded product images
│   └── templates/          # Jinja2 HTML templates
└── migrations/
    └── add_phone_number.py # Migration script for phone field
```

## Getting Started

### Prerequisites

- Python 3.8 or later
- pip

### Installation

1. Clone or download this repository and open a terminal in the project folder.

2. Create and activate a virtual environment (recommended):

   ```bash
   python -m venv venv

   # Windows
   venv\Scripts\activate

   # macOS / Linux
   source venv/bin/activate
   ```

3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

   To use the `view_db.py` utility, also install `tabulate`:

   ```bash
   pip install tabulate
   ```

### Running the Application

```bash
python main.py
```

The app starts at **http://localhost:5000** with debug mode enabled.

The SQLite database (`farmers_market.db`) is created automatically on first run if it does not already exist.

### Resetting the Database

To drop all tables and recreate an empty database:

```bash
python init_db.py
```

### Viewing Database Contents

```bash
python view_db.py
```

## User Roles

| Role | Capabilities |
|------|--------------|
| **Farmer** | Add, edit, and delete their own product listings |
| **Merchant** | Browse all products and place bids above the base price |

After signing up, users are redirected to the dashboard. Farmers can create new products from **Product → New**; merchants can open any product and submit a bid.

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/latest-products` | Returns the 6 most recently added products |
| GET | `/api/products` | Filter and sort products (`category`, `search`, `sort` query params) |
| POST | `/product/<id>/bid` | Place a bid on a product (merchants only) |
| POST | `/product/<id>/delete` | Delete a product (owner only) |

**Sort options for `/api/products`:** `price_low`, `price_high`, or newest (default).

## Admin Access

Admin routes are restricted to the account with email `admin@example.com`:

- `/admin/users` — View all registered users
- `/admin/cleanup-duplicate-bids` — Remove duplicate bids (same user and amount on a product)

To use admin features, register or update a user with that email address.

## Configuration

Before deploying to production, update the secret key in `app/__init__.py`:

```python
app.config['SECRET_KEY'] = 'your-secret-key'

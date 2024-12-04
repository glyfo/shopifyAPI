# Shopify API Configuration with a Virtual Environment

This guide explains how to configure and use the Shopify API within a Python virtual environment on macOS.

## Prerequisites

Before starting, ensure the following:

- **Python 3.6+** is installed (preferably using Homebrew or another package manager).
- **pip** is up-to-date.
- Access to a Shopify store with API credentials.

---

## Steps to Configure the Shopify API

### 1. Create a Virtual Environment

Navigate to your project directory and create a virtual environment:

```bash
python3 -m venv myenv
```

Activate the virtual environment:

```bash
source myenv/bin/activate
```

Your terminal prompt should now show `(myenv)` to indicate the environment is active.

---

### 2. Upgrade `pip`

Inside the virtual environment, upgrade `pip` to the latest version:

```bash
pip install --upgrade pip
```

---

### 3. Install the Shopify API Library

Install the official Shopify API library:

```bash
pip install shopifyapi
```

---

### 4. Verify Installation

Confirm that the Shopify API library is installed:

```bash
pip show shopifyapi
```

---

### 5. Set Up Environment Variables

Store your Shopify API credentials securely. Create a `.env` file in your project directory with the following content:

```env
SHOPIFY_API_KEY=your_api_key_here
SHOPIFY_PASSWORD=your_password_here
SHOPIFY_STORE_URL=your_store_url_here
```

Use a library like `python-dotenv` to load these variables into your script. Install it with:

```bash
pip install python-dotenv
```

---

### 6. Write a Test Script

Create a Python script (e.g., `shopify_test.py`) to test the API connection:

```python
import os
import shopify
import logging

# Configure logging
logging.basicConfig(level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s")

def setup_environment():
    """Set up environment configuration for Shopify API."""
    return {
        "API_KEY": os.getenv("SHOPIFY_API_KEY"),
        "API_SECRET": os.getenv("SHOPIFY_API_SECRET"),
        "SHOP_NAME": os.getenv("SHOPIFY_SHOP_NAME"),
        "TOKEN": os.getenv("SHOPIFY_ACCESS_TOKEN"),
        "API_VERSION": "2024-07",
    }

def setup_shopify_session(config):
    """
    Establish a Shopify session using provided configuration.
    """
    try:
        shop_url = f"https://{config['SHOP_NAME']}.myshopify.com"
        shopify.Session.setup(api_key=config['API_KEY'], secret=config['API_SECRET'])
        session = shopify.Session(shop_url, config['API_VERSION'], config['TOKEN'])
        shopify.ShopifyResource.activate_session(session)
        logging.info("Shopify session successfully established.")
    except Exception as e:
        raise ConnectionError(f"Failed to set up Shopify session: {e}")

def main():
    try:
        logging.info("Initializing environment configuration...")
        config = setup_environment()

        logging.info("Setting up Shopify session...")
        setup_shopify_session(config)

```

Run the script to ensure everything is working:

```bash
python shopify_test.py
```

If successful, the script will print the name of your Shopify store.

---

### 7. Deactivate the Virtual Environment

When finished working, deactivate the virtual environment:

```bash
deactivate
```

---

## Optional: Save Dependencies

To make your environment reproducible, save installed dependencies to a `requirements.txt` file:

```bash
pip freeze > requirements.txt
```

Later, you can recreate the environment with:

```bash
pip install -r requirements.txt
```

---

## Notes

- Always keep your API credentials secure.
- Use virtual environments for every project to avoid dependency conflicts.

For more information on the Shopify API, visit the [Shopify API Documentation](https://shopify.dev/docs/api).


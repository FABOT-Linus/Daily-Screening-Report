name: Daily V4 Hybrid Screener and Email

on:
  schedule:
    # 22:15 UTC = 5:15 PM EST (Standard Time)
    # Runs Monday (1) through Friday (5)
    - cron: '15 22 * * 1-5'
  
  # Allows you to click "Run workflow" manually from the Actions tab
  workflow_dispatch:

jobs:
  run-screener:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'
          
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install pandas numpy requests yfinance tradingview-screener
          
      - name: Run V4 Screener and Send Email
        env:
          # Alpaca API Credentials (pulled from GitHub Secrets)
          ALPACA_API_KEY: ${{ secrets.ALPACA_API_KEY }}
          ALPACA_SECRET_KEY: ${{ secrets.ALPACA_SECRET_KEY }}
          
          # Email Credentials (pulled from GitHub Secrets)
          SMTP_USER: ${{ secrets.SMTP_USER }}
          SMTP_APP_PASSWORD: ${{ secrets.SMTP_APP_PASSWORD }}
          
          # Hardcoded Recipient Email
          REPORT_RECIPIENT_EMAIL: "linus.chen1984@gmail.com"
        run: |
          python hybrid_v4_screener.py
# Data

The raw data files are not stored in this repository. FRED marks the IMF beef price series as copyrighted, so instead of copying the files, this page explains exactly where to download them and how to check that you have the same data I used.

## The three files

Put all three files in this `data/` folder (or upload them to the Colab Files panel) with these exact names.

| File name | Series | What it is | Unit | Frequency | Original source |
|---|---|---|---|---|---|
| `PBEEFUSDM.csv` | [PBEEFUSDM](https://fred.stlouisfed.org/series/PBEEFUSDM) | IMF global price of beef: Australian and New Zealand frozen boneless lean beef, US import price | US cents per pound | Monthly | International Monetary Fund |
| `EXUSAL.csv` | [EXUSAL](https://fred.stlouisfed.org/series/EXUSAL) | Australian dollar exchange rate, monthly average | US dollars per 1 Australian dollar | Monthly | Federal Reserve Board |
| `APU0000703112.csv` | [APU0000703112](https://fred.stlouisfed.org/series/APU0000703112) | Average retail price of 100% ground beef, US city average | US dollars per pound | Monthly | US Bureau of Labor Statistics |

## How to download

1. Open the series page on FRED (links above).
2. To reproduce my results exactly, set the chart's date range to match the version I used (see below). For the full history, click **Max**.
3. Click **Download**, then **CSV (data)**. Keep the file name as it is.
4. Do not open the files in Excel and save them again. Excel can change the dates into your local format (for example `01-09-2026`), and the notebook reads one exact format, `YYYY-MM-DD`. To look inside a file, use a text editor.

## The version I used

| File | Period | FRED "Updated" date | MD5 fingerprint |
|---|---|---|---|
| `PBEEFUSDM.csv` | May 2010 to September 2026 | 22 October 2026 | [from experiment_log.csv] |
| `EXUSAL.csv` | [from notebook Step 2 output] | [from the FRED page] | [from experiment_log.csv] |
| `APU0000703112.csv` | [from notebook Step 2 output] | 10 November 2026 | [from experiment_log.csv] |

Downloaded on [date].

FRED data can be revised, and new months are added each month. If your download is newer, your numbers will differ slightly from mine. That is expected.

## How to check that you have the same files

The notebook records an MD5 fingerprint of each file in `outputs/experiment_log.csv`. Compute the fingerprint of your download and compare:

```bash
# Windows (Command Prompt)
certutil -hashfile PBEEFUSDM.csv MD5

# macOS
md5 PBEEFUSDM.csv

# Linux
md5sum PBEEFUSDM.csv
```

If the fingerprint matches, the file is identical to mine, byte for byte.

## File format

Each file has two columns:

```text
observation_date,PBEEFUSDM
2010-05-01,157.75
2010-06-01,144.8055556
...
```

- Dates are the first day of each month, in `YYYY-MM-DD` format.
- A missing value may appear as an empty cell or as `.`. The notebook reads both as missing, never as zero.

## Known issues in the data

| Series | Issue | How I handled it |
|---|---|---|
| Retail ground beef | No value for October 2025. BLS did not collect prices during the US government shutdown. | Left blank, not filled |
| Retail ground beef | No value for October 2012. The source shows `.` and gives no reason. | Left blank, not filled |
| Beef price | 5 monthly moves above 10% | Kept. I checked one against the FRED chart and it is in the source. |
| AUD/USD | 2 monthly moves above 10% | Kept |
| Retail ground beef | 3 monthly moves above 10% | Kept |

The checks themselves are in notebook Step 3, and their results are on the Checks sheet of `outputs/beef_market_dashboard.xlsx`.

## Citation

- International Monetary Fund, Global price of Beef [PBEEFUSDM], retrieved from FRED, Federal Reserve Bank of St. Louis.
- Board of Governors of the Federal Reserve System (US), U.S. Dollars to Australian Dollar Spot Exchange Rate [EXUSAL], retrieved from FRED, Federal Reserve Bank of St. Louis.
- U.S. Bureau of Labor Statistics, Average Price: Ground Beef, 100% Beef (Cost per Pound/453.6 Grams) in U.S. City Average [APU0000703112], retrieved from FRED, Federal Reserve Bank of St. Louis.

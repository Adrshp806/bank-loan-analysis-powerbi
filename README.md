# Bank Loan Analysis — Power BI + SQL

A focused Power BI project that examines loan performance and borrower profiles using SQL-derived KPIs and interactive visuals.

## What you’ll see
- Summary KPIs: total applications, funded amount, amount received, avg. interest, avg. DTI  
- Good vs Bad loans: counts, amounts, and percentage split  
- Trends and segmentation: monthly trend, state map, term, employment length, purpose, home ownership  
- Clean SQL logic + presentation deck for walkthroughs

## Repo contents
> Flat layout on purpose so you can open things quickly.
- **Bank_Loan_data.pbix** — the dashboard (open in Power BI Desktop)  
- **bank_loan_data.csv** — sample dataset for local testing  
- **BANK LOAN REPORT QUERY DOCUMENT.odt** — SQL notes/queries used for KPIs  
- **Bank_Loan_data.pptx** and **Microsoft-Power-BI-Storytelling.pptx** — slide decks for demos and storytelling

## Quick start
1. Open **Bank_Loan_data.pbix** in Power BI Desktop.  
2. If it prompts for data source, point it to **bank_loan_data.csv** (same folder).  
3. Refresh. You should see the Summary, Overview, and Details pages.

## Data model (at a glance)
Single fact table: `bank_loan_data`  
Key columns used:
- `id`, `issue_date` (date), `loan_amount`, `total_payment`
- `int_rate` (as decimal like 0.1325), `dti` (decimal)
- `loan_status` (`Fully Paid`, `Current`, `Charged Off`)
- `address_state`, `term`, `emp_length`, `purpose`, `home_ownership`

## KPI definitions
- **Total Loan Applications** = `COUNT(id)`  
- **Total Funded Amount** = `SUM(loan_amount)`  
- **Total Amount Received** = `SUM(total_payment)`  
- **Average Interest Rate (%)** = `AVG(int_rate) * 100`  
- **Average DTI (%)** = `AVG(dti) * 100`  
- **Good Loans** = `loan_status IN ('Fully Paid','Current')`  
- **Bad Loans** = `loan_status = 'Charged Off'`

## Example SQL (portable)
```sql
-- Total applications
SELECT COUNT(id) AS total_applications FROM bank_loan_data;

-- Good loan percentage
SELECT (COUNT(CASE WHEN loan_status IN ('Fully Paid','Current') THEN 1 END) * 100.0) / COUNT(*) AS good_loan_pct
FROM bank_loan_data;

-- Monthly trend (SQL Server)
SELECT
  DATEFROMPARTS(YEAR(issue_date), MONTH(issue_date), 1) AS month_start,
  DATENAME(MONTH, issue_date) AS month_name,
  COUNT(*) AS total_apps,
  SUM(loan_amount) AS total_funded,
  SUM(total_payment) AS total_received
FROM bank_loan_data
GROUP BY DATEFROMPARTS(YEAR(issue_date), MONTH(issue_date), 1), DATENAME(MONTH, issue_date)
ORDER BY month_start;
```

## Dashboard preview
Add 2–4 screenshots to the repo and reference them here:
```markdown
![Summary](docs/dashboard_screenshots/summary.png)
![Overview](docs/dashboard_screenshots/overview.png)
![Details](docs/dashboard_screenshots/details.png)
```
If you don’t want folders yet, drop PNGs next to the PBIX and link them directly.

## How to present this
- Use **Bank_Loan_data.pptx** for a quick walkthrough:
  1) business problem → 2) KPIs → 3) visuals → 4) insights → 5) next steps.  
- Keep one slide with assumptions and data limitations.

## Make it yours
- Add a short **project description** in the GitHub sidebar.
- Optional badges at the top:
```markdown
![Power BI](https://img.shields.io/badge/Tool-PowerBI-yellow)
![SQL Server](https://img.shields.io/badge/Database-SQLServer-red)
![License: MIT](https://img.shields.io/badge/License-MIT-green)
```

## Roadmap
- [ ] Add a tiny anonymized sample (1–2k rows) for faster clones  
- [ ] Export 3 crisp screenshots  
- [ ] Add a `/sql` file with all queries in one place  
- [ ] Optional: publish to Power BI Service and link here

## License
MIT — do whatever, just add credit where sensible.

## Contact
**<Your Name>** — <email or LinkedIn>

---
name: vendor-concentration-analysis
description: Systematic methodology for detecting contractor favoritism and bid rigging through spending pattern analysis. Use when investigating suspicious concentration of government contracts with specific vendors, potential pay-to-play schemes, or procurement corruption. Includes Herfindahl-Hirschman Index calculations, threshold gaming detection, and relationship mapping between vendors and officials.
---

# Vendor Concentration Analysis

## Purpose

Detect and document patterns of contractor favoritism, bid rigging, and procurement corruption by analyzing government spending concentration among vendors. This methodology provides systematic investigation protocols using public records to identify suspicious relationships between government officials and preferred contractors.

## When to Use This Skill

Use this skill when investigating:
- Disproportionate spending with single vendor or small group of vendors
- Suspicious lack of competitive bidding despite high contract values
- Contracts split just below competitive bidding thresholds
- Same vendors winning contracts across multiple departments
- Vendors with connections to government officials
- Emergency or sole-source procurements that bypass competition
- Procurement patterns suggesting coordinated bid rigging

**Red flags that trigger this investigation:**
- One vendor receiving >50% of department's total spending
- Multiple contracts awarded to same vendor just below bidding threshold
- Vendor owners/employees contributing to officials' campaigns
- Officials voting on contracts involving their associates
- Sudden vendor preference changes after elections
- Geographic concentration (all vendors from official's district)

## Core Concept: Spending Concentration as Corruption Signal

Government procurement should be competitive, transparent, and serve public interest. When spending concentrates with few vendors, it suggests:

1. **Favoritism:** Officials steering contracts to preferred vendors
2. **Bid rigging:** Vendors coordinating to eliminate competition
3. **Pay-to-play:** Campaign contributions buying contract preferences
4. **Kickbacks:** Officials receiving benefits for contract awards
5. **Threshold gaming:** Deliberately splitting contracts to avoid oversight

Vendor concentration analysis exposes these patterns through systematic spending data review and statistical analysis.

## Four-Phase Investigation Protocol

### Phase 1: Data Collection

**Objective:** Gather comprehensive spending data for analysis period

**Public records to obtain:**

**Primary sources:**
- Check register / warrant register (all payments made)
- Purchase order log (all approved purchases)
- Contract register (formal agreements)
- Budget documents (allocated vs actual spending)
- Procurement method documentation (competitive/sole-source/emergency)

**Supporting sources:**
- Vendor registration database
- Campaign contribution records (local and state)
- Property ownership records (for vendor principals)
- Business entity registrations (LLC/corporation ownership)
- Official financial disclosures (Form 700 or equivalent)

**Data elements needed:**
- Vendor name (standardized - watch for multiple business entities)
- Payment amount
- Payment date
- Department/agency making purchase
- Purchase description/category
- Procurement method (bid, quote, sole-source, emergency)
- Contract number (if applicable)
- Purchase order number

**Time period:**
- Minimum: 2 fiscal years (shows patterns vs anomalies)
- Recommended: 3-5 fiscal years (tracks changes over time)
- Comprehensive: Since current administration took office

**Data format considerations:**
- PDF check registers require OCR and manual cleanup
- Excel/CSV preferred for analysis
- Standardize vendor names (variations need consolidation)
- Convert all amounts to common currency/fiscal year

### Phase 2: Concentration Measurement

**Objective:** Quantify spending concentration using statistical methods

**Method 1: Top Vendor Share Analysis**

Calculate percentage of total spending going to top vendors:

```
Top 1 vendor share = (Top vendor total / Department total spending) × 100
Top 5 vendor share = (Sum of top 5 vendors / Department total spending) × 100
Top 10 vendor share = (Sum of top 10 vendors / Department total spending) × 100
```

**Interpretation thresholds:**
- Top vendor >30%: Moderate concentration, investigate further
- Top vendor >50%: High concentration, likely favoritism
- Top vendor >70%: Extreme concentration, strong corruption signal
- Top 5 vendors >80%: Oligopoly pattern, possible bid rigging

**Method 2: Herfindahl-Hirschman Index (HHI)**

Industry-standard market concentration measure:

```
HHI = Σ(s₁² + s₂² + s₃² + ... + sₙ²)

where sᵢ = vendor i's market share as percentage
```

**Calculation steps:**
1. Calculate each vendor's percentage of total spending
2. Square each percentage
3. Sum all squared percentages

**Example:**
- Vendor A: 40% of spending → 40² = 1,600
- Vendor B: 30% of spending → 30² = 900
- Vendor C: 20% of spending → 20² = 400
- Vendor D: 10% of spending → 10² = 100
- HHI = 1,600 + 900 + 400 + 100 = 3,000

**Interpretation:**
- HHI < 1,500: Competitive market
- HHI 1,500-2,500: Moderate concentration
- HHI > 2,500: High concentration (FTC merger concern threshold)
- HHI > 5,000: Extreme concentration, investigation warranted

**Method 3: Gini Coefficient**

Measures inequality in spending distribution:

```
Gini = (Σᵢ Σⱼ |xᵢ - xⱼ|) / (2n² × x̄)

where:
xᵢ = spending amount to vendor i
n = number of vendors
x̄ = mean spending per vendor
```

**Simplified calculation:**
1. Rank vendors by spending (lowest to highest)
2. Calculate cumulative percentage of vendors
3. Calculate cumulative percentage of spending
4. Plot Lorenz curve (cumulative vendors vs cumulative spending)
5. Gini = (Area between curve and equality line) / (Total area under equality line)

**Interpretation:**
- Gini = 0: Perfect equality (all vendors get equal spending)
- Gini = 1: Perfect inequality (one vendor gets everything)
- Gini > 0.6: High inequality, concentration concern
- Gini > 0.8: Extreme inequality, likely favoritism

**Method 4: Time Series Analysis**

Track concentration metrics over time:

```
Year-over-year HHI change = HHI(year n) - HHI(year n-1)
```

**Red flags:**
- Sudden HHI increase after election
- Gradual concentration over multiple years
- Concentration increase coinciding with official taking office
- Cyclical patterns (concentration before elections, dispersion after)

### Phase 3: Threshold Gaming Detection

**Objective:** Identify deliberate contract splitting to avoid competitive bidding requirements

**How threshold gaming works:**

Government procurement requires competitive bidding above certain thresholds (e.g., $50,000). Corrupt officials split contracts into smaller purchases to stay just below threshold while funneling work to preferred vendors.

**Detection method 1: Just-Below-Threshold Clustering**

Analyze distribution of contract amounts:

```
For contracts with same vendor:
- Count contracts in range: (Threshold - 20%) to (Threshold - 1%)
- Compare to expected random distribution
- Calculate statistical significance
```

**Example:**
- Threshold: $50,000
- Target range: $40,000-$49,999
- Vendor X has 15 contracts in this range
- Expected (random distribution): 2-3 contracts
- Statistical significance: p < 0.001 (highly suspicious)

**Detection method 2: Sequential Contract Analysis**

Identify suspiciously timed sequential contracts:

```
For each vendor, find contracts where:
- Contract N amount: $48,000 (just below threshold)
- Contract N+1 date: Within 30 days of Contract N
- Contract N+1 amount: $47,000 (also just below threshold)
- Both contracts: Same or related services
```

**Red flag:** Multiple sequential contracts that could have been combined into single competitive procurement.

**Detection method 3: Service Category Fragmentation**

Analyze whether similar services artificially separated:

```
Example suspicious pattern:
- "Janitorial supplies Part 1": $45,000
- "Janitorial supplies Part 2": $46,000
- "Janitorial supplies Part 3": $44,000
Total: $135,000 (should have triggered competitive bidding)
```

**Documentation for evidence:**
- Scatter plot showing contract amounts vs threshold line
- Table of sequential contracts with timing and amounts
- Service category analysis showing fragmentation
- Statistical significance calculations

### Phase 4: Relationship Mapping

**Objective:** Identify connections between vendors and government officials

**Investigation 1: Campaign Finance Connections**

Cross-reference vendor data with campaign contributions:

**Data sources:**
- Federal: OpenFEC API (for federal candidates)
- State: State campaign finance databases
- Local: County/city clerk campaign reports

**Analysis queries:**

```sql
-- Find vendors who also contributed to officials
SELECT
    v.vendor_name,
    SUM(v.contract_amount) as total_contracts,
    c.official_name,
    SUM(c.contribution_amount) as total_contributions,
    COUNT(c.contribution_date) as contribution_count
FROM vendors v
JOIN contributions c ON v.owner_name = c.contributor_name
GROUP BY v.vendor_name, c.official_name
ORDER BY total_contracts DESC
```

**Red flags:**
- Large contributions followed by contract awards
- Officials voting on contracts from their donors
- Contributions from vendor employees/family members
- Contribution timing (shortly before contract awards)

**Investigation 2: Business Entity Ownership**

Trace vendor ownership to identify hidden relationships:

**Secretary of State research:**
- LLC/corporation formation documents
- List of members, managers, officers, directors
- Registered agent information
- Business addresses

**Cross-reference with:**
- Official financial disclosures (business investments)
- Property records (shared addresses/ownership)
- Other vendor registrations (same principals operating multiple entities)

**Red flags:**
- Official owns interest in vendor company
- Official's family members as vendor principals
- Multiple vendor entities with same owners (avoiding concentration detection)
- Vendor address matches official's property

**Investigation 3: Employment and Service Relationships**

Identify indirect financial relationships:

**Research areas:**
- Official employed by vendor (or vice versa)
- Official's law firm represents vendor
- Official's consulting business serves vendor
- Vendor employs official's family members

**Public records sources:**
- LinkedIn and professional profiles
- Business registrations (DBA filings)
- Form 700 disclosure of positions held
- News articles and press releases

**Investigation 4: Geographic and Social Network Analysis**

Map non-obvious connections:

**Geographic analysis:**
- Vendor locations relative to official's district
- Clustering of preferred vendors in official's neighborhood
- All vendors using same business address (front companies)

**Social network indicators:**
- Shared nonprofit board memberships
- Joint investments in other businesses
- Common social/professional associations
- Shared legal representation

## Evidence Package Development

**Organize findings into structured report:**

### Executive Summary
- Department/agency analyzed
- Time period covered
- Key concentration metrics (HHI, top vendor shares)
- Number of red flags identified
- Estimated financial impact
- Recommended actions

### Section 1: Spending Concentration Analysis

**Metrics table:**
```
Metric                     Value      Interpretation
─────────────────────────  ─────────  ─────────────────
Top vendor share           67%        Extreme concentration
Top 5 vendor share         89%        Oligopoly pattern
HHI                        4,200      High concentration
Gini coefficient           0.82       Extreme inequality
```

**Visualization:**
- Pie chart: Top 10 vendors vs all others
- Lorenz curve: Cumulative vendor vs spending distribution
- Time series: HHI trend over fiscal years
- Bar chart: Top 10 vendors by total spending

### Section 2: Threshold Gaming Evidence

**Analysis table:**
```
Vendor Name        Contracts  Avg Amount  Just-Below-Threshold  Statistical Sig.
─────────────────  ─────────  ──────────  ───────────────────  ────────────────
ABC Contractors    23         $47,500     18 (78%)             p < 0.0001
XYZ Services       15         $48,200     12 (80%)             p < 0.001
```

**Sequential contract examples:**
- Document specific instances with dates and amounts
- Show cumulative totals that exceed thresholds
- Demonstrate pattern across multiple vendors

### Section 3: Relationship Documentation

**Connection matrix:**
```
Vendor              Campaign $  Business      Property    Employment  Total
                                Interest      Connection              Connections
──────────────────  ──────────  ────────────  ──────────  ──────────  ───────────
ABC Contractors     $15,000     LLC member    Same addr   None        3
XYZ Services        $8,500      None          None        Spouse      2
```

**Detailed narratives:**
For each significant relationship, provide:
- Nature of connection (contribution, ownership, employment)
- Timeline (when established relative to contracts)
- Financial magnitude (contribution amounts, contract values)
- Voting records (did conflicted officials recuse?)
- Supporting documentation (FEC records, Form 700, etc.)

### Section 4: Financial Impact Assessment

**Overpayment calculation:**

Compare prices paid to preferred vendors vs market rates or competitive bids:

```
Service Category        Preferred    Competitive  Difference  Quantity  Overpayment
                        Vendor Rate  Bid Rate
─────────────────────  ───────────  ───────────  ──────────  ────────  ───────────
Janitorial services    $45/hour     $32/hour     $13/hour    1,000 hr  $13,000
Office supplies        15% markup   5% markup    10% delta   $100K     $10,000
```

**Total impact:**
- Annual overpayment estimate
- Multi-year total (over contract period)
- Opportunity cost (what could have been purchased instead)

### Section 5: Recommendations

**Immediate actions:**
- Terminate or renegotiate identified contracts
- Demand refunds for overpayments
- Refer to district attorney for investigation
- File ethics complaints for conflicted officials

**Process improvements:**
- Require competitive bidding for all contracts >$X
- Prohibit contract splitting (aggregate related purchases)
- Mandate conflict of interest disclosures before votes
- Implement vendor rotation policies
- Enhance procurement oversight

**Policy changes:**
- Lower competitive bidding thresholds
- Require pre-approval for sole-source contracts
- Ban campaign contributions from contractors
- Establish cooling-off periods (contribution → contract award)
- Create independent procurement review board

## Analysis Tools and Techniques

### Spreadsheet Analysis Template

**Vendor spending summary:**
```
Vendor Name | FY 2020 | FY 2021 | FY 2022 | Total | % of Dept | HHI Contribution
─────────── | ─────── | ─────── | ─────── | ───── | ───────── | ────────────────
ABC Co      | 100,000 | 150,000 | 200,000 | 450K  | 45%       | 2,025
XYZ Inc     | 50,000  | 75,000  | 100,000 | 225K  | 22.5%     | 506
...
TOTALS      | 250,000 | 350,000 | 450,000 | 1,050K| 100%      | HHI = 3,500
```

**Pivot table configurations:**
- Rows: Vendor name
- Columns: Fiscal year
- Values: Sum of payment amounts
- Filters: Department, procurement method, service category

### Statistical Software

**R script for HHI calculation:**
```r
# Calculate Herfindahl-Hirschman Index
calculate_hhi <- function(vendor_spending) {
  total_spending <- sum(vendor_spending)
  market_shares <- (vendor_spending / total_spending) * 100
  hhi <- sum(market_shares^2)
  return(hhi)
}

# Example usage
spending <- c(450000, 225000, 150000, 100000, 75000, 50000)
hhi <- calculate_hhi(spending)
print(paste("HHI:", round(hhi, 0)))
```

**Python for threshold clustering analysis:**
```python
import pandas as pd
import numpy as np
from scipy import stats

def detect_threshold_gaming(contracts, threshold=50000, tolerance=0.20):
    """
    Detect contracts suspiciously clustered just below threshold
    """
    lower_bound = threshold * (1 - tolerance)
    upper_bound = threshold * 0.99

    # Contracts in suspicious range
    suspicious = contracts[
        (contracts['amount'] >= lower_bound) &
        (contracts['amount'] <= upper_bound)
    ]

    # Expected count (random distribution)
    total_contracts = len(contracts)
    range_width = upper_bound - lower_bound
    total_range = contracts['amount'].max() - contracts['amount'].min()
    expected_count = total_contracts * (range_width / total_range)

    # Statistical significance
    actual_count = len(suspicious)
    chi_square = ((actual_count - expected_count)**2) / expected_count
    p_value = stats.chi2.sf(chi_square, df=1)

    return {
        'actual_count': actual_count,
        'expected_count': expected_count,
        'p_value': p_value,
        'significant': p_value < 0.05
    }
```

## Common Patterns and Case Studies

### Pattern 1: The Dominant Contractor

**Characteristics:**
- Single vendor receives >50% of department spending
- Vendor wins all competitive bids (suspiciously high success rate)
- Other vendors stop bidding (bid rigging signal)
- Contract amounts incrementally increase each renewal

**Detection:**
- Top vendor share >50%
- HHI >3,000
- Declining number of unique vendors over time
- No new vendor entry despite high margins

**Example red flags:**
- Vendor wins 15 of 15 bids over 3 years
- Competitors submit pro forma bids then withdraw
- Contract specifications written to favor incumbent

### Pattern 2: The Rotating Oligopoly

**Characteristics:**
- 3-5 vendors dominate spending (>80% total)
- Vendors take turns winning contracts
- Pricing remains consistently high across all vendors
- No new competition despite profitable contracts

**Detection:**
- Top 5 vendor share >80%
- Contract rotation pattern (A wins Q1, B wins Q2, C wins Q3...)
- Price uniformity across vendors
- Geographic market allocation (each vendor gets specific area)

**Example red flags:**
- All bids within 1% of each other
- Losing bidders don't protest decisions
- Same vendors rebid but never undercut each other

### Pattern 3: The Shell Game

**Characteristics:**
- Multiple vendor entities with same owners
- Different business names but same address/phone
- Contracts split among related entities
- Total spending concentrated when entities aggregated

**Detection:**
- Secretary of State entity search showing common principals
- Identical or sequential business addresses
- Same contact information across "different" vendors
- Contract award patterns suggesting coordination

**Example red flags:**
- ABC Landscaping, ABC Lawn Service, ABC Grounds (all same owner)
- Each entity stays just below bidding threshold
- Combined total represents market dominance

### Pattern 4: The Pay-to-Play Cycle

**Characteristics:**
- Vendor contributes to official's campaign
- Shortly after, vendor awarded lucrative contract
- Vendor contributes again before contract renewal
- Pattern repeats across multiple election cycles

**Detection:**
- Timeline analysis: contribution → contract award lag
- Contribution amounts correlating with contract values
- Patterns specific to certain officials (donors get contracts)
- Contribution cessation when contracts not renewed

**Example red flags:**
- $5,000 contribution in June, $500,000 contract in July
- Only contributors receive sole-source awards
- Campaign finance reports show vendor employees bundling

## Legal and Regulatory Framework

### Federal Requirements (for grant recipients)

**2 CFR Part 200.318-320 - Procurement Standards**

Applies to state/local governments receiving federal funds:
- Must maintain written procurement procedures
- Must conduct procurements providing full and open competition
- Contracts >$250,000 require competition (unless sole-source justified)
- Must maintain oversight to ensure contractor compliance

**Violations to document:**
- Procurement bypassing competitive requirements
- Inadequate sole-source justifications
- Lack of written procurement procedures
- Inadequate oversight of contractor performance

### State Requirements

**California Government Code examples:**

**§1090 - Conflicts of Interest**
- Officials prohibited from being financially interested in contracts
- Violation is felony
- Contract voidable if official has prohibited interest

**§54202 - Competitive Bidding**
- Contracts exceeding threshold require competitive bidding
- Public notice and sealed bid requirements
- Award to lowest responsible bidder

### Municipal Code Requirements

Check local ordinances for:
- Competitive bidding thresholds (often lower than state)
- Local preference programs (may permit favoritism)
- Emergency procurement authorizations
- Sole-source justification requirements

## Success Criteria

Investigation is successful when it produces:

- **Quantified concentration:** Specific HHI, Gini, and top-vendor metrics
- **Statistical significance:** P-values showing patterns aren't random
- **Documented relationships:** Connections between vendors and officials
- **Financial impact:** Dollar amount of overpayment/favoritism
- **Legal violations:** Specific procurement law breaches with citations
- **Actionable recommendations:** Concrete next steps for accountability

**The standard:**
Evidence package should be sufficient for:
- District attorney to open investigation
- Media to publish investigative story
- Civil lawsuit for recovery of overpayments
- Ethics commission to pursue violations
- Public pressure for procurement reform

## Resources and References

**Data sources:**
- Government check registers and warrant registers
- Procurement/contract management systems
- Campaign finance databases (FEC, state, local)
- Secretary of State business entity databases
- Property assessor records

**Statistical tools:**
- Excel/Google Sheets (basic calculations)
- R or Python (advanced statistical analysis)
- Tableau/Power BI (visualization)
- SQL databases (large dataset analysis)

**Regulatory references:**
- 2 CFR Part 200 (federal procurement standards)
- State government codes (procurement requirements)
- Municipal procurement ordinances
- Ethics and conflict of interest laws

**Industry standards:**
- GAAS (Generally Accepted Auditing Standards)
- GASB (Governmental Accounting Standards Board)
- FTC merger guidelines (HHI thresholds)

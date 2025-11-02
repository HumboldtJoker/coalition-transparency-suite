---
name: cost-allocation-plan-review
description: Methodology for detecting Cost Allocation Plan manipulation and federal fund fraud. Use when investigating how governments allocate indirect costs to federal grants, potential CAP gaming to maximize federal reimbursement, or misallocation of costs. James (forensic accountant) identified this as often-exploited weak point in government finances. Covers 2 CFR Part 200 compliance, allocation base reasonableness, and comparative analysis.
---

# Cost Allocation Plan Review

## Purpose

Detect manipulation of Cost Allocation Plans (CAPs) that allows governments to fraudulently maximize federal grant reimbursements. CAPs determine how indirect costs (overhead, administration) are charged to federal programs - making them prime targets for fraud through inflated or misallocated costs.

## When to Use This Skill

Use this skill when investigating:
- Suspiciously high indirect cost rates charged to federal programs
- Changes in allocation methodology that increase federal reimbursement
- Indirect costs allocated to programs without actual benefit
- Personnel costs reallocated to federally-funded positions
- Comparative rates significantly above similar jurisdictions

**Red flags:**
- Indirect cost rate increases >10% year-over-year
- All indirect costs suddenly "allocable" to federal programs
- Allocation bases that don't reflect actual cost drivers
- CAP changes coinciding with budget shortfalls
- Lack of supporting documentation for allocation methodology

## Core Concept: CAP as Federal Reimbursement Mechanism

**What is a Cost Allocation Plan?**

Federal grants prohibit direct billing of certain overhead costs (HR, accounting, facilities, etc.). Instead, governments create CAPs showing systematic methodology for allocating these "indirect costs" across all programs (federal and non-federal) based on reasonable allocation bases.

**How CAPs create fraud opportunity:**

1. **Inflated indirect cost pools:** Adding costs that should be direct or unallowable
2. **Manipulated allocation bases:** Using metrics that overstate federal program usage
3. **Selective allocation:** Only allocating costs beneficial for federal reimbursement
4. **Ghost employees:** Charging personnel costs for positions that don't exist
5. **Cost shifting:** Moving local costs to federally-funded cost pools

**The incentive:** Every dollar of indirect cost allocated to federal programs = federal reimbursement. Local budgets benefit by shifting costs to federal grants.

## Three-Phase Investigation Protocol

### Phase 1: CAP Document Review

**Obtain the Cost Allocation Plan:**
- Official CAP submitted to federal agencies (annual document)
- Supporting schedules showing cost pool calculations
- Allocation base data (statistics used for allocation)
- Prior year CAPs for comparison
- Audit reports or federal monitoring findings

**Key sections to analyze:**

**1. Indirect Cost Pools**

Document what costs are included:
- Central Services (HR, IT, Purchasing, Finance)
- Facilities (rent, utilities, maintenance)
- General Administration (executive, legal, clerk)
- Vehicle/Equipment usage
- Employee benefits

**Review for:**
- Unallowable costs (lobbying, fundraising, entertainment)
- Costs that should be direct-charged (program-specific)
- Duplicate costs (charged both direct and indirect)
- Inflated amounts (comparison to actual expenditures)

**2. Allocation Bases**

Identify methodology for distributing costs:
- Full-Time Equivalents (FTE) - personnel count
- Salaries & Wages - total compensation
- Direct costs - program expenditures
- Square footage - space utilization
- Transactions - volume of services

**Evaluate reasonableness:**
- Does base reflect actual benefit received?
- Are measurements accurate and documented?
- Do all programs use the service being allocated?
- Has base been manipulated to favor federal programs?

**Example manipulation:**
- Using "total expenditures" as base when federal programs spend more (inflates their allocation)
- Should use "FTE count" instead (more accurately reflects benefit)

**3. Indirect Cost Rate Calculation**

```
Indirect Cost Rate = Total Indirect Cost Pool / Allocation Base

Example:
HR Department costs: $500,000
Allocation base (FTE): 100 employees
Rate: $500,000 / 100 = $5,000 per FTE

Federal program has 20 FTE → receives $100,000 allocation
```

**Review for:**
- Mathematical accuracy
- Consistency with documented methodology
- Changes from prior year (and justification)
- Comparison to industry standards

### Phase 2: Compliance Testing

**Regulatory framework: 2 CFR Part 200 (OMB Uniform Guidance)**

**Key requirements:**

**§200.405 - Allocable Costs**
A cost is allocable to federal program if:
- Program benefits from the cost
- Cost can be assigned according to benefits received
- Cost is necessary to overall operation (including federal programs)

**Violation examples:**
- Allocating entire legal department to federal program when most work is for local matters
- Charging facilities costs to federal program housed in free donated space
- Allocating vehicle costs when federal program doesn't use vehicles

**§200.413 - Direct vs Indirect Costs**

**Direct costs:** Can be specifically identified with particular program
**Indirect costs:** Benefit multiple programs, cannot be easily identified with one

**Manipulation to detect:**
- Reclassifying direct costs as indirect to include in allocation
- Charging same cost both direct and indirect
- Moving costs between categories to maximize federal reimbursement

**§200.414 - Indirect Cost Rates**

Governments can use:
- **De minimis rate:** 10% of modified total direct costs (no documentation required)
- **Negotiated rate:** Based on actual CAP, negotiated with federal cognizant agency
- **Cost rate proposal:** Annual submission showing cost pools and bases

**Red flags:**
- Claiming de minimis but actually charging higher rate
- Negotiated rate not approved by cognizant agency
- Rates exceeding federally-negotiated maximum

**Testing procedures:**

**Test 1: Allocation Base Accuracy**

Verify allocation statistics are correct:
```
Claimed FTE count: 120
Actual FTE (from payroll): 95
Variance: 26% overstatement
Impact: Indirect costs overstated by 26%
```

**Test 2: Cost Pool Inclusion**

Sample costs from pool and verify allowability:
- Select 10-20 transactions from HR cost pool
- Review supporting documentation
- Verify cost is allowable, allocable, and reasonable
- Calculate error rate and extrapolate

**Test 3: Direct Cost Comparison**

Identify costs charged both direct and indirect:
```
Find:
- Federal grant budget showing "legal services" as direct cost
- Same legal costs also in central services indirect pool
Result: Double-charging federal program
```

### Phase 3: Comparative Analysis

**Benchmark indirect cost rates:**

**Similar jurisdiction comparison:**

Obtain CAPs from comparable governments:
- Similar population size
- Similar budget size
- Same state (subject to same regulations)
- Geographic proximity

Compare rates:
```
Jurisdiction          HR Rate    Finance Rate   Facilities Rate   Total Indirect %
────────────────────  ─────────  ────────────  ────────────────  ────────────────
County A (subject)    $6,500     $4,200        $8,000            28%
County B (similar)    $4,000     $3,000        $5,500            18%
County C (similar)    $4,500     $3,200        $6,000            19%
County D (similar)    $4,200     $2,800        $5,800            17%

Subject variance:     +55%       +38%          +38%              +55%
```

**Red flag:** Subject jurisdiction rates significantly above peers suggests inflation.

**Year-over-year trend analysis:**

```
Fiscal Year   Indirect Rate   Change      Federal Grants   Reimbursement
────────────  ──────────────  ──────────  ───────────────  ─────────────
FY 2019       18%             baseline    $5,000,000       $900,000
FY 2020       22%             +4%         $5,200,000       $1,144,000
FY 2021       26%             +4%         $5,500,000       $1,430,000
FY 2022       28%             +2%         $6,000,000       $1,680,000

Total increase: +10 percentage points (+55%)
Additional federal reimbursement (vs FY 2019 rate): $780,000
```

**Investigation question:** What changed to justify 55% rate increase? Or is this cost shifting to maximize federal dollars?

**State/federal guidance comparison:**

Many states publish recommended/maximum indirect cost rates:
- California: CAP handbook with rate ranges by service
- Federal cognizant agencies: Negotiated rate agreements

**Compare to guidance:**
```
Service Area      Subject Rate   State Max   Federal Negotiated   Variance
────────────────  ────────────  ──────────  ───────────────────  ────────
HR/Personnel      $6,500        $4,000      $4,200               +55%
Accounting        $4,200        $3,000      $3,100               +35%
```

## Common CAP Fraud Schemes

**Scheme 1: The Phantom Employee**

Add non-existent positions to federal program staffing:
- Creates FTE count inflation
- Federal program charged for "employees" who don't exist
- Local budget relieved of salary costs

**Detection:**
- Compare FTE count in CAP to actual payroll records
- Cross-reference employee names/positions
- Verify position descriptions match actual work

**Scheme 2: The Cost Shift**

Systematically move local costs to federal cost pools:
- Reclassify general fund employees as "grant management"
- Claim local projects benefit federal programs
- Allocate full cost of services barely used by federal programs

**Detection:**
- Review job descriptions of "reallocated" positions
- Interview staff about actual duties
- Analyze service usage (do federal programs actually use?)

**Scheme 3: The Base Game**

Manipulate allocation bases to favor federal programs:
- Use expenditure-based allocation when federal programs spend more
- Switch from reasonable base (FTE) to inflated base (dollars)
- Count federal program activities multiple times

**Detection:**
- Evaluate whether base truly reflects benefit received
- Compare current base to prior years (why change?)
- Test alternative bases (does choice maximize federal dollars?)

**Scheme 4: The Unallowable Inclusion**

Include costs federal regulations prohibit:
- Lobbying expenses
- Political contributions
- Entertainment and fundraising
- Idle facilities/equipment
- Fines and penalties

**Detection:**
- Sample transactions from cost pools
- Review against 2 CFR Part 200.420-475 (unallowable costs)
- Calculate total unallowable amount and impact

## Investigation Output

**Evidence package structure:**

**1. Executive Summary**
- Subject government and federal grants received
- CAP indirect cost rate (current vs historical vs peers)
- Key findings (manipulation methods identified)
- Financial impact (excess federal reimbursement)
- Recommended actions

**2. CAP Analysis**
- Document review findings
- Compliance violations identified
- Allocation base reasonableness assessment
- Cost pool inclusion testing results

**3. Comparative Benchmarking**
- Peer jurisdiction rate comparison
- Year-over-year trend analysis
- Variance from state/federal guidelines
- Statistical significance of differences

**4. Fraud Schemes Documented**
- Specific instances with supporting evidence
- Transaction samples showing violations
- Calculations showing financial impact
- Patterns suggesting intentional vs negligent

**5. Recommendations**
- Immediate: Suspend federal reimbursements pending review
- Recovery: Calculate and demand refund of excess reimbursements
- Referral: Report to federal OIG, state auditor
- Reform: Require independent CAP audit, lower rates to compliant levels

## Resources

**Federal regulations:**
- 2 CFR Part 200 Subpart E (Cost Principles)
- Federal agency-specific grant requirements
- OMB Circular A-87 (predecessor guidance, still referenced)

**State guidance:**
- California: "Handbook for Cost Allocation Plans and Indirect Cost Proposals" (Controller's Office)
- Other states: Check state controller/comptroller websites

**Standards:**
- GAAS (Generally Accepted Auditing Standards)
- GASB (Governmental Accounting Standards Board)
- Single Audit requirements (2 CFR Part 200 Subpart F)

**Comparative data:**
- Federal Audit Clearinghouse (single audit reports)
- State associations of counties/cities
- Similar jurisdiction CAPs (FOIA request)

---
name: anti-palantir-osint-pipeline
description: Automated government transparency data collection and analysis system. Use when establishing ongoing surveillance of government meetings, votes, campaign finance, and official relationships. Integrates Legistar meeting scraping, OpenFEC campaign finance data, PostgreSQL unified storage, Neo4j relationship mapping, and Spiderfoot deep investigation modules for comprehensive transparency intelligence.
---

# Anti-Palantir OSINT Pipeline

## Purpose

Build and operate automated open-source intelligence (OSINT) infrastructure that continuously collects, stores, and analyzes government transparency data. The Anti-Palantir inverts traditional surveillance - instead of monitoring citizens, it monitors those in power using exclusively public records and open data sources.

This system enables activists, journalists, and transparency advocates to systematically track government activities, voting patterns, campaign finance relationships, and conflicts of interest at scale.

## When to Use This Skill

Use this skill when:
- Establishing transparency monitoring for a jurisdiction (city, county, state)
- Investigating systematic corruption requiring cross-referencing multiple data sources
- Building evidence of relationships between votes and campaign contributions
- Tracking voting patterns and legislative activity over time
- Conducting deep background investigations on public officials
- Creating transparency dashboards for public accountability
- Automating what would otherwise be manual FOIA request workflows

**Project phases requiring this skill:**
- Initial setup: Installing and configuring the pipeline components
- Data collection: Scraping meetings, downloading campaign finance data
- Integration: Cross-referencing data sources for relationship discovery
- Analysis: Identifying corruption patterns and conflicts of interest
- Presentation: Creating public-facing dashboards and reports

## Core Concept: Surveillance Points Up

The Anti-Palantir embodies a fundamental principle: **surveillance technology should point up the power hierarchy, not down**.

Traditional surveillance systems (like Palantir) are designed to monitor citizens - tracking movements, communications, associations, and behaviors to identify threats to those in power.

The Anti-Palantir reverses this:
- **Subjects:** Public officials, not private citizens
- **Data sources:** Public meetings, campaign finance, voting records, not private communications
- **Purpose:** Government accountability, not population control
- **Access:** Open to everyone, not restricted to law enforcement
- **Transparency:** Methods and findings public, not secret databases

This is opposition technology - using the same technical approaches (data aggregation, relationship mapping, pattern analysis) but for democratic accountability instead of authoritarian control.

## Technical Architecture

### Four-Layer Stack

**Layer 1: Data Collection**
- **Legistar Scraper:** Automated harvesting of meeting minutes, votes, documents
- **OpenFEC API:** Federal campaign finance data retrieval
- **Manual Integration Points:** State/local campaign finance (varies by jurisdiction)

**Layer 2: Unified Storage**
- **PostgreSQL:** Structured storage for meetings, votes, contributions, officials
- **Schema:** Normalized tables with foreign key relationships
- **Indexing:** Optimized for cross-reference queries

**Layer 3: Relationship Mapping**
- **Neo4j:** Graph database for network analysis
- **Nodes:** Officials, donors, organizations, legislation, votes
- **Edges:** Voted-for, contributed-to, employed-by, lobbied-for
- **Queries:** Pattern matching for corruption detection

**Layer 4: Investigation & Presentation**
- **Spiderfoot:** Deep background investigation (200+ OSINT modules)
- **The Harvester:** Domain/email enumeration for official networks
- **Dashboard:** Public-facing transparency portal
- **Reports:** Automated corruption pattern alerts

### Data Flow

```
Legistar meetings → PostgreSQL → Neo4j → Analysis/Dashboard
                          ↓
OpenFEC contributions → PostgreSQL → Neo4j → Relationship mapping
                          ↓
Spiderfoot OSINT → PostgreSQL → Context enrichment
```

## Component 1: Legistar Scraper

### What is Legistar?

Legistar is the dominant meeting management software for local governments in the US. Thousands of cities, counties, and special districts use Legistar to publish:
- Meeting agendas and minutes
- Legislation and ordinances
- Voting records
- Meeting videos and transcripts
- Committee assignments
- Public hearing notices

### Installation

**Deprecated library:** `python-legistar` (no longer maintained)
**Current library:** `scraper-legistar`

```bash
pip install scraper-legistar
```

### Usage Patterns

**Basic scraping workflow:**

```python
from legistar.scraper import LegistarScraper

# Initialize scraper for jurisdiction
scraper = LegistarScraper('humboldt')  # humboldt.legistar.com

# Get recent events
events = scraper.events(
    since='2024-01-01',
    until='2024-12-31'
)

# Get event details
for event in events:
    event_detail = scraper.event(event['EventId'])

    # Extract votes
    votes = event_detail.get('EventItems', [])
    for item in votes:
        legislation = item.get('EventItemMatterId')
        vote_data = item.get('EventItemRollCallFlag')

    # Extract attendees
    attendees = event_detail.get('EventInSiteURL')
```

**Key data points to collect:**
- Event ID, date, body name (Board of Supervisors, City Council, etc.)
- Legislation ID, title, type (ordinance, resolution, contract)
- Vote results (aye/nay/abstain by person)
- Meeting videos/transcripts (if available)
- Attachments and supporting documents

### PostgreSQL Schema for Legistar Data

```sql
CREATE TABLE jurisdictions (
    jurisdiction_id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    legistar_url VARCHAR(255),
    state VARCHAR(2)
);

CREATE TABLE bodies (
    body_id SERIAL PRIMARY KEY,
    jurisdiction_id INT REFERENCES jurisdictions(jurisdiction_id),
    name VARCHAR(255),  -- "Board of Supervisors"
    description TEXT
);

CREATE TABLE events (
    event_id INT PRIMARY KEY,  -- Legistar EventId
    body_id INT REFERENCES bodies(body_id),
    event_date TIMESTAMP,
    event_time VARCHAR(50),
    event_location TEXT,
    minutes_status VARCHAR(50),
    video_url TEXT
);

CREATE TABLE legislation (
    matter_id INT PRIMARY KEY,  -- Legistar MatterId
    jurisdiction_id INT REFERENCES jurisdictions(jurisdiction_id),
    matter_type VARCHAR(100),
    matter_status VARCHAR(100),
    title TEXT,
    intro_date DATE,
    final_action DATE
);

CREATE TABLE event_items (
    event_item_id SERIAL PRIMARY KEY,
    event_id INT REFERENCES events(event_id),
    matter_id INT REFERENCES legislation(matter_id),
    agenda_sequence INT,
    agenda_note TEXT,
    minutes_note TEXT,
    action_text TEXT
);

CREATE TABLE officials (
    person_id INT PRIMARY KEY,  -- Legistar PersonId
    full_name VARCHAR(255),
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(255),
    website TEXT,
    photo_url TEXT
);

CREATE TABLE votes (
    vote_id SERIAL PRIMARY KEY,
    event_item_id INT REFERENCES event_items(event_item_id),
    person_id INT REFERENCES officials(person_id),
    vote_result VARCHAR(20),  -- "Aye", "Nay", "Abstain", "Absent"
    vote_date TIMESTAMP
);

CREATE TABLE attachments (
    attachment_id SERIAL PRIMARY KEY,
    matter_id INT REFERENCES legislation(matter_id),
    name VARCHAR(255),
    hyperlink TEXT,
    document_type VARCHAR(100)
);
```

### Scraping Strategy

**Initial historical load:**
1. Identify jurisdiction's Legistar URL (e.g., `humboldt.legistar.com`)
2. Scrape all events from past 2-5 years (establish baseline)
3. Collect all legislation, votes, and officials
4. Download critical attachments (contracts, staff reports)

**Ongoing monitoring:**
1. Schedule daily scrape for new events (cron job or scheduled task)
2. Monitor for new legislation introduced
3. Track vote results as meetings occur
4. Alert on votes matching watchlist criteria

**Rate limiting and etiquette:**
- Respect Legistar servers: 1-2 second delays between requests
- Scrape during off-peak hours (nights/weekends)
- Cache responses to avoid repeat requests
- Monitor for IP blocking and adjust as needed

## Component 2: OpenFEC Campaign Finance API

### What is OpenFEC?

The Federal Election Commission provides free API access to:
- Individual contributions to federal candidates
- Committee disbursements and expenditures
- Candidate financial summaries
- PAC and Super PAC activity
- Independent expenditures

### API Access

**No authentication required** for most endpoints (public data)

**Rate limits:** 1,000 requests/hour

**Base URL:** `https://api.open.fec.gov/v1/`

### Key Endpoints

**Candidate search:**
```
GET /candidates/search/?name={candidate_name}&state={CA}
```

**Individual contributions to candidate:**
```
GET /schedules/schedule_a/?committee_id={committee_id}&min_date={2020-01-01}
```

**Committee information:**
```
GET /committee/{committee_id}/
```

**Example Python usage:**

```python
import requests

# Search for candidate
response = requests.get(
    'https://api.open.fec.gov/v1/candidates/search/',
    params={
        'name': 'Huffman, Jared',
        'state': 'CA',
        'office': 'H'  # House
    }
)
candidates = response.json()['results']

# Get candidate's committee
committee_id = candidates[0]['principal_committees'][0]['committee_id']

# Get contributions
response = requests.get(
    'https://api.open.fec.gov/v1/schedules/schedule_a/',
    params={
        'committee_id': committee_id,
        'min_date': '2020-01-01',
        'per_page': 100
    }
)
contributions = response.json()['results']

for contrib in contributions:
    contributor_name = contrib['contributor_name']
    amount = contrib['contribution_receipt_amount']
    date = contrib['contribution_receipt_date']
    employer = contrib['contributor_employer']
```

### PostgreSQL Schema for Campaign Finance

```sql
CREATE TABLE candidates (
    candidate_id VARCHAR(20) PRIMARY KEY,  -- FEC candidate ID
    name VARCHAR(255),
    party VARCHAR(50),
    office VARCHAR(10),  -- 'H', 'S', 'P'
    state VARCHAR(2),
    district VARCHAR(10),
    incumbent_flag BOOLEAN
);

CREATE TABLE committees (
    committee_id VARCHAR(20) PRIMARY KEY,  -- FEC committee ID
    name VARCHAR(255),
    committee_type VARCHAR(50),
    designation VARCHAR(50),
    party VARCHAR(50)
);

CREATE TABLE candidate_committees (
    candidate_id VARCHAR(20) REFERENCES candidates(candidate_id),
    committee_id VARCHAR(20) REFERENCES committees(committee_id),
    is_principal BOOLEAN,
    PRIMARY KEY (candidate_id, committee_id)
);

CREATE TABLE contributions (
    contribution_id SERIAL PRIMARY KEY,
    committee_id VARCHAR(20) REFERENCES committees(committee_id),
    contributor_name VARCHAR(255),
    contributor_city VARCHAR(100),
    contributor_state VARCHAR(2),
    contributor_zip VARCHAR(10),
    contributor_employer VARCHAR(255),
    contributor_occupation VARCHAR(255),
    contribution_date DATE,
    amount DECIMAL(12,2),
    receipt_type VARCHAR(50)
);

CREATE TABLE disbursements (
    disbursement_id SERIAL PRIMARY KEY,
    committee_id VARCHAR(20) REFERENCES committees(committee_id),
    recipient_name VARCHAR(255),
    disbursement_date DATE,
    amount DECIMAL(12,2),
    purpose TEXT,
    recipient_city VARCHAR(100),
    recipient_state VARCHAR(2)
);
```

### State/Local Campaign Finance

**Challenge:** No unified API like OpenFEC

**State-by-state approaches:**

**California:** Cal-Access database
- Download bulk data: `http://campaignfinance.cdn.sos.ca.gov/`
- Parse CSV files for contributions and expenditures
- Match candidates to local offices

**Other states:** Check secretary of state websites
- Some provide APIs or bulk downloads
- Others require manual CSV downloads
- Worst case: Screen scraping individual filings

## Component 3: Neo4j Relationship Mapping

### Why Graph Database?

Government corruption is fundamentally about **relationships**:
- Official X voted YES on contract
- Contractor Y contributed to Official X's campaign
- Contractor Y employs Official X's spouse

Traditional SQL requires complex joins to discover these patterns. Graph databases make relationship queries natural and performant.

### Installation

**Neo4j Community Edition** (free, open source)

```bash
# Docker approach (recommended)
docker run -p 7474:7474 -p 7687:7687 \
    -e NEO4J_AUTH=neo4j/password \
    neo4j:latest
```

Access web interface: `http://localhost:7474`

### Graph Schema

**Node types:**
- `:Official` - Elected officials and appointed department heads
- `:Donor` - Individual contributors or organizations
- `:Legislation` - Bills, resolutions, contracts, ordinances
- `:Organization` - Companies, PACs, nonprofits, government agencies
- `:Event` - Meetings where votes occur

**Relationship types:**
- `(:Official)-[:VOTED_FOR]->(:Legislation)`
- `(:Donor)-[:CONTRIBUTED_TO]->(:Official)`
- `(:Official)-[:EMPLOYED_BY]->(:Organization)`
- `(:Organization)-[:BENEFITED_FROM]->(:Legislation)`
- `(:Official)-[:ATTENDED]->(:Event)`

### Loading Data from PostgreSQL to Neo4j

**Python approach using py2neo:**

```python
from py2neo import Graph, Node, Relationship
import psycopg2

# Connect to both databases
neo4j_graph = Graph("bolt://localhost:7687", auth=("neo4j", "password"))
pg_conn = psycopg2.connect("dbname=transparency user=postgres")

# Load officials
cursor = pg_conn.cursor()
cursor.execute("SELECT person_id, full_name, email FROM officials")
for row in cursor:
    official = Node("Official",
                    person_id=row[0],
                    name=row[1],
                    email=row[2])
    neo4j_graph.merge(official, "Official", "person_id")

# Load legislation
cursor.execute("SELECT matter_id, title, matter_type FROM legislation")
for row in cursor:
    legislation = Node("Legislation",
                      matter_id=row[0],
                      title=row[1],
                      matter_type=row[2])
    neo4j_graph.merge(legislation, "Legislation", "matter_id")

# Load votes (creates relationships)
cursor.execute("""
    SELECT v.person_id, ei.matter_id, v.vote_result, v.vote_date
    FROM votes v
    JOIN event_items ei ON v.event_item_id = ei.event_item_id
    WHERE v.vote_result = 'Aye'
""")
for row in cursor:
    official = neo4j_graph.nodes.match("Official", person_id=row[0]).first()
    legislation = neo4j_graph.nodes.match("Legislation", matter_id=row[1]).first()

    if official and legislation:
        vote = Relationship(official, "VOTED_FOR", legislation,
                           vote_date=row[3])
        neo4j_graph.create(vote)

# Load contributions (creates donor nodes and relationships)
cursor.execute("""
    SELECT contributor_name, contributor_employer,
           candidate_id, amount, contribution_date
    FROM contributions c
    JOIN candidate_committees cc ON c.committee_id = cc.committee_id
""")
for row in cursor:
    donor = Node("Donor",
                 name=row[0],
                 employer=row[1])
    neo4j_graph.merge(donor, "Donor", "name")

    # Find corresponding official (match candidate to official)
    official = neo4j_graph.nodes.match("Official", name=row[2]).first()

    if official:
        contribution = Relationship(donor, "CONTRIBUTED_TO", official,
                                   amount=row[3],
                                   date=row[4])
        neo4j_graph.create(contribution)
```

### Corruption Detection Queries

**Pattern 1: Votes following contributions**

```cypher
// Find officials who voted for contracts from companies whose employees donated
MATCH (donor:Donor)-[c:CONTRIBUTED_TO]->(official:Official)
MATCH (official)-[v:VOTED_FOR]->(legislation:Legislation)
WHERE donor.employer CONTAINS legislation.title
  AND c.date < v.vote_date
RETURN official.name, donor.employer, legislation.title,
       c.amount, c.date, v.vote_date
ORDER BY c.amount DESC
```

**Pattern 2: Voting bloc analysis**

```cypher
// Find officials who consistently vote together
MATCH (official1:Official)-[:VOTED_FOR]->(leg:Legislation)<-[:VOTED_FOR]-(official2:Official)
WHERE official1.person_id < official2.person_id
WITH official1, official2, COUNT(leg) as shared_votes
WHERE shared_votes > 50
RETURN official1.name, official2.name, shared_votes
ORDER BY shared_votes DESC
```

**Pattern 3: Donor influence networks**

```cypher
// Find donors who contribute to multiple officials voting together
MATCH (donor:Donor)-[:CONTRIBUTED_TO]->(official:Official)-[:VOTED_FOR]->(leg:Legislation)
WITH donor, leg, COLLECT(official.name) as officials, COUNT(official) as official_count
WHERE official_count >= 3
RETURN donor.name, leg.title, officials, official_count
ORDER BY official_count DESC
```

## Component 4: Spiderfoot Deep Investigation

### What is Spiderfoot?

Spiderfoot is an open-source OSINT automation framework with 200+ modules for:
- Domain reconnaissance
- Email address discovery
- Social media profiling
- Corporate registration lookups
- Dark web monitoring
- Data breach exposure

### Installation

```bash
git clone https://github.com/smicallef/spiderfoot.git
cd spiderfoot
pip3 install -r requirements.txt
python3 sf.py -l 127.0.0.1:5001
```

Access web interface: `http://localhost:5001`

### Use Cases for Government Transparency

**Official background investigation:**
Target: Official name, email address
Modules:
- Email address discovery (find personal accounts)
- Social media profiling (Facebook, LinkedIn, Twitter)
- Corporate affiliations (business registrations)
- Domain ownership (personal websites, businesses)

**Organization investigation:**
Target: Company name, domain
Modules:
- WHOIS lookups (ownership, registration dates)
- DNS records (infrastructure, email servers)
- Breach exposure (employee credentials in leaks)
- Related entities (subsidiaries, partnerships)

### Integration with Anti-Palantir Pipeline

**Automated enrichment workflow:**

1. New official enters PostgreSQL database
2. Trigger Spiderfoot scan with official's email/name
3. Collect results (social media, affiliations, domains)
4. Store enriched data back to PostgreSQL
5. Create Neo4j nodes for discovered organizations/connections

**Python automation:**

```python
import requests

# Start Spiderfoot scan
response = requests.post('http://localhost:5001/startscan', json={
    'scanname': 'Official Investigation: Jane Doe',
    'scantarget': 'janedoe@example.com',
    'modulelist': 'sfp_accounts,sfp_company,sfp_emailformat',
    'typelist': 'EMAILADDR,AFFILIATE,COMPANY_NAME'
})

scan_id = response.json()['id']

# Poll for results
results = requests.get(f'http://localhost:5001/scaneventresults?id={scan_id}')
```

## Component 5: The Harvester

### Purpose

The Harvester performs email and subdomain enumeration through:
- Search engines (Google, Bing)
- Public sources (PGP key servers, Shodan)
- DNS queries (brute force subdomains)

### Installation

```bash
git clone https://github.com/laramies/theHarvester.git
cd theHarvester
pip3 install -r requirements.txt
```

### Usage for Official Email Discovery

```bash
# Find all email addresses at government domain
python3 theHarvester.py -d humboldtgov.org -b all

# Find subdomains (discover departments)
python3 theHarvester.py -d humboldtgov.org -b dns
```

**Results parsing:**

```python
# Parse Harvester output
with open('harvester_results.xml') as f:
    # Extract emails
    emails = extract_emails(f)

    # Match to officials database
    for email in emails:
        # Update PostgreSQL with discovered emails
        cursor.execute("""
            UPDATE officials
            SET email = %s
            WHERE email ISNULL
              AND LOWER(full_name) LIKE LOWER(%s)
        """, (email, f'%{name_from_email(email)}%'))
```

## Deployment Architecture

### Production Setup

**Server requirements:**
- 4 CPU cores minimum
- 16GB RAM (Neo4j and PostgreSQL)
- 500GB storage (for meeting videos/documents)
- Ubuntu 22.04 LTS recommended

**Docker Compose deployment:**

```yaml
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: transparency
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  neo4j:
    image: neo4j:5
    environment:
      NEO4J_AUTH: neo4j/${NEO4J_PASSWORD}
    volumes:
      - neo4j_data:/data
    ports:
      - "7474:7474"
      - "7687:7687"

  scraper:
    build: ./scrapers
    depends_on:
      - postgres
    environment:
      DATABASE_URL: postgresql://postgres:${DB_PASSWORD}@postgres/transparency
    command: python legistar_scraper.py --continuous

  dashboard:
    build: ./dashboard
    depends_on:
      - postgres
      - neo4j
    ports:
      - "8080:8080"
    environment:
      DATABASE_URL: postgresql://postgres:${DB_PASSWORD}@postgres/transparency
      NEO4J_URI: bolt://neo4j:7687

volumes:
  postgres_data:
  neo4j_data:
```

### Automated Scraping Schedule

**Cron jobs for data collection:**

```cron
# Legistar scraping - daily at 2 AM
0 2 * * * cd /opt/anti-palantir && python scrapers/legistar_scraper.py

# OpenFEC sync - weekly on Sundays at 3 AM
0 3 * * 0 cd /opt/anti-palantir && python scrapers/openfec_scraper.py

# Neo4j rebuild - weekly on Sundays at 5 AM
0 5 * * 0 cd /opt/anti-palantir && python scripts/rebuild_graph.py

# Dashboard data refresh - hourly
0 * * * * cd /opt/anti-palantir && python dashboard/refresh_stats.py
```

## Analysis Workflows

### Workflow 1: Contract Vote + Contribution Analysis

**Objective:** Find officials who voted on contracts where contractor employees contributed to their campaigns

**Steps:**
1. Identify contract legislation in Legistar (matter_type = 'Contract')
2. Extract contractor name from contract title/attachments
3. Query OpenFEC for contributions from contractor employees to officials
4. Match voting officials to contribution recipients
5. Calculate time lag (contribution date → vote date)
6. Assess pattern (multiple instances? Large amounts?)

**Cypher query:**

```cypher
MATCH (donor:Donor)-[c:CONTRIBUTED_TO]->(official:Official)
MATCH (official)-[v:VOTED_FOR]->(contract:Legislation)
WHERE contract.matter_type = 'Contract'
  AND donor.employer = contract.contractor_name
  AND c.date < v.vote_date
WITH official, contract, donor, c, v,
     duration.between(c.date, v.vote_date).months as months_between
WHERE months_between < 12
RETURN official.name,
       contract.title,
       donor.name,
       donor.employer,
       c.amount,
       c.date as contribution_date,
       v.vote_date,
       months_between
ORDER BY c.amount DESC
```

### Workflow 2: Voting Bloc + Common Donors

**Objective:** Identify groups of officials who vote together and share campaign donors

**Steps:**
1. Calculate voting similarity matrix (Jaccard similarity)
2. Identify official pairs with >80% voting agreement
3. Find donors who contributed to both officials in pair
4. Assess whether voting bloc serves donor interests

**Analysis code:**

```python
# Calculate voting similarity
similarity_query = """
    SELECT
        o1.person_id as official1,
        o2.person_id as official2,
        COUNT(DISTINCT v1.matter_id) as shared_votes,
        COUNT(DISTINCT v1.matter_id) * 1.0 /
            (SELECT COUNT(DISTINCT matter_id)
             FROM votes
             WHERE person_id IN (o1.person_id, o2.person_id)) as similarity
    FROM officials o1
    CROSS JOIN officials o2
    JOIN votes v1 ON o1.person_id = v1.person_id
    JOIN votes v2 ON o2.person_id = v2.person_id
        AND v1.matter_id = v2.matter_id
        AND v1.vote_result = v2.vote_result
    WHERE o1.person_id < o2.person_id
      AND v1.vote_result = 'Aye'
    GROUP BY o1.person_id, o2.person_id
    HAVING COUNT(DISTINCT v1.matter_id) > 20
      AND similarity > 0.8
"""

# Find common donors
common_donors_query = """
    SELECT
        c1.contributor_name,
        c1.contributor_employer,
        SUM(c1.amount) as total_to_official1,
        SUM(c2.amount) as total_to_official2,
        SUM(c1.amount + c2.amount) as total_contributions
    FROM contributions c1
    JOIN contributions c2
        ON c1.contributor_name = c2.contributor_name
        AND c1.committee_id = %s  -- official1's committee
        AND c2.committee_id = %s  -- official2's committee
    GROUP BY c1.contributor_name, c1.contributor_employer
    ORDER BY total_contributions DESC
"""
```

### Workflow 3: Facility Lease + Property Ownership

**Objective:** Cross-reference facility leases with official property ownership

**Steps:**
1. Extract facility lease legislation from Legistar
2. Identify property address from lease documents
3. Query county assessor for property owner
4. Cross-reference with Form 700 financial disclosures
5. Match official voting records on lease approval

**Integration query:**

```sql
SELECT
    l.matter_id,
    l.title as lease_description,
    p.property_address,
    p.owner_name,
    o.full_name as official_name,
    v.vote_result,
    f.disclosure_year,
    f.property_description
FROM legislation l
JOIN property_records p ON l.property_address = p.address
JOIN officials o ON p.owner_name ILIKE '%' || o.last_name || '%'
JOIN votes v ON o.person_id = v.person_id
JOIN form_700_disclosures f ON o.person_id = f.person_id
WHERE l.matter_type = 'Lease Agreement'
  AND v.vote_result = 'Aye'
  AND f.property_description ILIKE '%' || p.property_address || '%'
```

## Dashboard and Reporting

### Public-Facing Dashboard Features

**Home page:**
- Recent votes (last 30 days)
- Top campaign contributors (current quarter)
- Meeting calendar (upcoming government meetings)
- Alerts (new conflicts of interest detected)

**Official profiles:**
- Voting record (with vote explanations if available)
- Campaign finance summary (top donors, total raised)
- Financial disclosure highlights (property, business interests)
- Attendance record (missed meetings)
- Committee assignments

**Legislation tracker:**
- Bill status and history
- Full text and attachments
- Voting record (who voted how)
- Related contributions (donors with interest in legislation)
- Public comment period status

**Relationship explorer:**
- Interactive network graph (officials, donors, organizations)
- Filter by contribution amount, vote type, date range
- Click to explore connections

**Reports:**
- Monthly transparency report (votes, contributions, conflicts)
- Quarterly investigation summaries
- Annual accountability scorecard

### Alert System

**Automated alerts for:**
- New contributions >$1,000 to officials
- Sole-source contracts >$100,000
- Officials voting on matters where donors have interest
- Missed votes or meeting absences
- New financial disclosure filings
- Emergency procurement declarations

**Delivery methods:**
- Email digests (daily, weekly)
- RSS feeds (by topic, official, or organization)
- Twitter/social media bots
- Public Slack/Discord channels

## Legal and Ethical Considerations

### Data Sources: All Public

**What the Anti-Palantir collects:**
- Government meeting records (Legistar, public websites)
- Campaign finance data (FEC, state disclosure databases)
- Financial disclosures (Form 700, ethics filings)
- Property ownership (county assessor public records)
- Public domain information (WHOIS, social media)

**What the Anti-Palantir does NOT collect:**
- Private communications
- Personal financial accounts
- Location tracking
- Browsing history
- Social network private data
- Anything requiring authentication/hacking

### Responsible Disclosure

**Before publishing corruption findings:**
1. Verify all facts against source documents
2. Calculate financial impacts with transparent methodology
3. Provide context (is this pattern or isolated incident?)
4. Give officials opportunity to respond
5. Distinguish policy disagreements from corruption
6. Publish source data for independent verification

**Publication standards:**
- State facts, not speculation
- Quantify harms with specific dollar amounts
- Cite legal violations with statutory references
- Include official responses in full
- Maintain source documents for legal challenges

### Defamation Protection

**How to avoid defamation claims:**
- Stick to documented facts from public records
- Avoid characterizing intent ("corrupt" vs "received contributions")
- Use qualifiers ("appears to" vs "is")
- Provide complete context
- Allow officials to correct factual errors

**Opinion vs assertion:**
- ✓ "Official X voted for a contract from Company Y, whose employees contributed $25,000 to X's campaign"
- ✗ "Official X is corrupt and sold their vote to Company Y"

## Success Metrics

**System operational health:**
- Scraper uptime (target: >95%)
- Data freshness (meetings added within 24 hours)
- Database integrity (no orphaned records)
- Query performance (dashboard loads <2 seconds)

**Investigation impact:**
- Corruption patterns identified per quarter
- Media coverage of published findings
- Government policy changes resulting from transparency pressure
- Officials declining contributions after exposure
- Contracts canceled or rebid after scrutiny

**Public engagement:**
- Dashboard monthly active users
- Alert subscriber count
- Social media reach for transparency findings
- Community requests for new jurisdictions

## Future Enhancements

**Automation opportunities:**
- Natural language processing of meeting transcripts
- Automatic contract extraction from PDF attachments
- Machine learning for vote prediction
- Sentiment analysis of public comments
- Image recognition for identifying officials in photos

**Additional data sources:**
- Lobbyist disclosure databases
- Corporate SEC filings (for contractor financials)
- Tax assessor data (for official property values)
- Employment records (for official outside income)
- Nonprofit tax returns (for board affiliations)

**Advanced analytics:**
- Social network analysis (community detection in voting blocs)
- Time series analysis (contribution patterns over election cycles)
- Anomaly detection (unusual votes or contributions)
- Predictive modeling (forecast votes based on donors)

## Resources and References

**Technical documentation:**
- Legistar scraper: https://github.com/opencivicdata/python-legistar
- OpenFEC API: https://api.open.fec.gov/developers/
- Neo4j graph database: https://neo4j.com/docs/
- Spiderfoot OSINT: https://www.spiderfoot.net/documentation/

**Legal frameworks:**
- FEC campaign finance law: https://www.fec.gov/legal-resources/
- Freedom of Information Act: https://www.foia.gov/
- State public records laws: https://www.rcfp.org/open-government-guide/

**Investigative resources:**
- NICAR (investigative journalism with data): https://www.ire.org/nicar/
- ProPublica data guides: https://www.propublica.org/datastore/
- Investigative Reporters & Editors: https://www.ire.org/

**Similar projects:**
- OpenSecrets (campaign finance): https://www.opensecrets.org/
- FollowTheMoney (state campaigns): https://www.followthemoney.org/
- GovTrack (federal legislation): https://www.govtrack.us/

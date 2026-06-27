# 🧬 BioToolkit — Clinical Trial Finder + CRISPR Visualizer
### Complete Project Brain Document

---

## 📌 Project Overview

- **Project Name:** BioToolkit (you can rename it)
- **Type:** Multi-tool Biotech Web Platform
- **Tools Inside:**
  - Tool 1 → Clinical Trial Finder
  - Tool 2 → CRISPR Guide RNA Visualizer
- **Target Users:** Biotech students, researchers, patients, curious people
- **Cost:** ₹0 — completely free to build and deploy
- **Resume Value:** Very high — shows API integration + biology knowledge + web skills

---

## 🛠️ Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | HTML + CSS + JavaScript | Simple, no framework needed |
| Styling | Tailwind CSS (CDN) | Fast, modern UI, free |
| Logic | Vanilla JavaScript | CRISPR tool needs pure logic |
| API | ClinicalTrials.gov API v2 | Free, no key, no card |
| Hosting | Vercel or Netlify | Free forever tier |
| Code Generator | Antigravity | Writes code for you |

> No backend needed. Everything runs in the browser.

---

## 📁 File Structure

```
biotoolkit/
│
├── index.html           → Landing/Home page
├── crispr.html          → CRISPR Visualizer tool page
├── trials.html          → Clinical Trial Finder tool page
│
├── css/
│   └── style.css        → Custom styles (colors, fonts, layout)
│
├── js/
│   ├── crispr.js        → All CRISPR logic
│   └── trials.js        → API call + display logic
│
└── assets/
    └── logo.png         → Optional logo
```

---

## 🏠 Page 1 — Landing Page (index.html)

### What it contains:
- **Navbar** → Logo + links to both tools
- **Hero Section** → Project name, tagline, two big buttons
- **Tool Cards** → Two cards explaining each tool
- **Footer** → Your name, GitHub link, disclaimer

### Hero Tagline Ideas:
- "Biotech tools for everyone — free, fast, and open"
- "Explore genes, find trials, understand biology"

---

## 🔬 Page 2 — CRISPR Guide RNA Visualizer (crispr.html)

### What it does:
- User pastes a **DNA sequence** (A, T, G, C letters)
- App scans for all possible **CRISPR cut sites**
- Shows each guide RNA sequence (20 letters)
- Highlights **PAM sites** in color
- Shows **where the cut happens** on the sequence
- Displays a summary (how many sites found, etc.)

---

### How CRISPR Logic Works (for Antigravity prompts):

#### Concept:
- CRISPR-Cas9 uses a **guide RNA (gRNA)** to find a target in DNA
- It looks for a **PAM sequence = NGG** (N = any base, G = Guanine)
- The guide RNA is the **20 nucleotides BEFORE the NGG**
- The cut happens **3 bases before the PAM** (between position 17 and 18)

#### Step-by-step Algorithm:
```
1. Take user's DNA sequence (convert to uppercase)
2. Remove all non-ATGC characters
3. Scan sequence from left to right
4. At every position, check if position+20 and position+21 = "GG" (PAM = xGG)
5. If yes → extract 20 nucleotides before that position = guide RNA
6. Record: guide RNA, PAM site, position, cut site location
7. Repeat for ALL positions in the sequence
8. Also scan the REVERSE COMPLEMENT strand (CRISPR works on both strands)
9. Display all found guide RNAs in a table with color highlighting
```

#### Color Coding for Display:
- 🟢 **Green highlight** → Guide RNA (20nt)
- 🔴 **Red highlight** → PAM site (NGG)
- 🔵 **Blue marker** → Cut site location
- Grey → Rest of sequence

#### Reverse Complement Logic:
```
Original:   5' - A T G C - 3'
Complement:      T A C G
Reverse:    3' - G C A T - 5'  →  read as 5' TACG 3'

Rules:
A → T
T → A
G → C
C → G
Then reverse the whole string
```

---

### CRISPR Page UI Layout:

```
[ Navbar ]

[ Page Title: CRISPR Guide RNA Visualizer ]
[ Short explanation of what this tool does ]

[ Text area: Paste your DNA sequence here ]
[ Example sequence button ] [ Analyze Button ]

[ Results Section ]
  [ Sequence Display with color highlights ]
  [ Summary: X guide RNAs found on forward strand, Y on reverse ]
  [ Table of all guide RNAs ]
    Columns: #, Position, Guide RNA (20nt), PAM, Strand, Cut Site
  [ Download results as CSV button (optional) ]

[ Info Section: What is CRISPR? (collapsible) ]
```

---

### CRISPR Features List:
- [ ] Paste any DNA sequence (validate only ATGC)
- [ ] Show error if sequence too short (needs at least 23 chars)
- [ ] Scan forward strand for NGG PAM
- [ ] Scan reverse complement for NGG PAM
- [ ] Display results in a color-highlighted sequence view
- [ ] Table with all guide RNAs, positions, PAM sites
- [ ] Count total sites found
- [ ] Load example sequence button
- [ ] Clear button
- [ ] GC content % of each guide RNA (bonus)

---

## 🏥 Page 3 — Clinical Trial Finder (trials.html)

### What it does:
- User types any **disease or condition** (e.g., "lung cancer", "diabetes")
- App calls **ClinicalTrials.gov API**
- Shows all matching active trials worldwide
- User can **filter** by phase, status, country
- Click on any trial to see full details

---

### API Details — ClinicalTrials.gov v2

#### Base URL:
```
https://clinicaltrials.gov/api/v2/studies
```

#### Example API Call:
```
https://clinicaltrials.gov/api/v2/studies?query.cond=lung+cancer&pageSize=20&format=json
```

#### Key Parameters:
| Parameter | What it does | Example |
|---|---|---|
| query.cond | Search by disease/condition | query.cond=diabetes |
| query.term | General keyword search | query.term=immunotherapy |
| filter.overallStatus | Filter by trial status | filter.overallStatus=RECRUITING |
| pageSize | Results per page | pageSize=20 |
| format | Response format | format=json |
| pageToken | For next page (pagination) | pageToken=abc123 |

#### Status Filter Options:
- `RECRUITING` → Actively looking for patients
- `COMPLETED` → Trial is done
- `NOT_YET_RECRUITING` → Coming soon
- `ACTIVE_NOT_RECRUITING` → Running but full

#### Response Structure (JSON):
```json
{
  "studies": [
    {
      "protocolSection": {
        "identificationModule": {
          "nctId": "NCT012345",
          "briefTitle": "Trial Name Here",
          "officialTitle": "Full Official Title"
        },
        "statusModule": {
          "overallStatus": "RECRUITING",
          "startDateStruct": { "date": "2024-01" }
        },
        "descriptionModule": {
          "briefSummary": "What this trial is about..."
        },
        "conditionsModule": {
          "conditions": ["Lung Cancer"]
        },
        "designModule": {
          "phases": ["PHASE2"],
          "studyType": "INTERVENTIONAL"
        },
        "contactsLocationsModule": {
          "locations": [
            {
              "facility": "Hospital Name",
              "city": "New York",
              "country": "United States"
            }
          ]
        },
        "eligibilityModule": {
          "eligibilityCriteria": "Must be 18+, no prior treatment...",
          "minimumAge": "18 Years",
          "maximumAge": "75 Years",
          "sex": "ALL"
        }
      }
    }
  ],
  "nextPageToken": "abc123"
}
```

---

### Clinical Trials Page UI Layout:

```
[ Navbar ]

[ Page Title: Clinical Trial Finder ]
[ Short explanation ]

[ Search Bar: Type a disease or condition... ] [ Search Button ]
[ Filters Row: Status dropdown | Phase dropdown | Country input ]

[ Results Area ]
  [ Result count: "Showing 20 of 145 trials for Diabetes" ]
  [ Trial Cards - one per trial ]
    Each card shows:
    - Trial Title
    - Status badge (green=Recruiting, grey=Completed, etc.)
    - Phase (Phase 1 / Phase 2 / Phase 3)
    - Condition
    - Start Date
    - Location (city, country)
    - [ View Details button ] → expands or opens modal

[ Trial Detail Modal/Expanded View ]
  - Full title
  - NCT ID (with link to clinicaltrials.gov)
  - Full summary
  - Eligibility criteria
  - All locations
  - Contact info (if available)

[ Load More button ] (pagination using nextPageToken)
```

---

### Clinical Trial Finder Features List:
- [ ] Search by condition/disease name
- [ ] Filter by status (Recruiting, Completed, etc.)
- [ ] Filter by phase (Phase 1, 2, 3, 4)
- [ ] Filter by country
- [ ] Card layout for results
- [ ] Status color badges
- [ ] Trial detail expand/modal
- [ ] Direct link to official ClinicalTrials.gov page (using NCT ID)
- [ ] Pagination (load more button)
- [ ] Loading spinner while fetching
- [ ] Error message if no results or API fails
- [ ] Popular searches (quick chips: "Cancer", "Diabetes", "Alzheimer's")

---

## 🎨 Design Plan

### Color Palette:
| Color | Hex | Use |
|---|---|---|
| Deep Blue | #0D1B2A | Background / Navbar |
| Electric Teal | #00C2CB | Primary accent, buttons |
| Soft White | #F4F4F4 | Text, cards |
| Green | #22C55E | Recruiting badge |
| Red | #EF4444 | Error / stopped trial |
| Yellow | #FACC15 | PAM highlight in CRISPR |

### Fonts:
- **Headings:** Inter Bold or Space Grotesk
- **Body:** Inter Regular
- Source: Google Fonts (free)

### UI Style:
- Dark theme (looks modern and scientific)
- Card-based layout
- Smooth hover animations
- Mobile responsive

---

## 🔧 Build Order (Step by Step)

### Phase 1 — Setup (Day 1)
- [ ] Create project folder with file structure
- [ ] Create index.html with navbar and hero
- [ ] Add Tailwind CSS via CDN
- [ ] Add Google Fonts link

### Phase 2 — CRISPR Tool (Day 2-3)
- [ ] Build crispr.html layout
- [ ] Write crispr.js PAM scanning logic (forward strand)
- [ ] Add reverse complement logic
- [ ] Build color-highlighted sequence display
- [ ] Build results table
- [ ] Test with real DNA sequences

### Phase 3 — Clinical Trials Tool (Day 4-5)
- [ ] Build trials.html layout
- [ ] Write API fetch in trials.js
- [ ] Build trial card component
- [ ] Add filters (status, phase)
- [ ] Add trial detail modal
- [ ] Add pagination
- [ ] Test with real diseases

### Phase 4 — Polish (Day 6-7)
- [ ] Connect all pages via navbar
- [ ] Make everything mobile responsive
- [ ] Add loading spinners
- [ ] Add error handling
- [ ] Test all features
- [ ] Add your name and GitHub to footer

### Phase 5 — Deploy (Day 8)
- [ ] Push code to GitHub (free)
- [ ] Connect GitHub to Vercel
- [ ] Deploy in one click
- [ ] Share the live link on resume

---

## 🧪 Test Sequences for CRISPR Tool

Use these to test your CRISPR visualizer:

```
Test 1 (short, has PAM sites):
ATGCGATCGATCGATCGATCGGNGGCTAGCTAGCATCGATCGATCGGNGG

Test 2 (real BRCA1 gene fragment):
ATGGATTTATCTGCTCTTCGCGTTGAAGAAGTACAAAATGTCATTAATGCTATGCAGAAAATCTTAGAGTGTCCCATCTGTCTGGAGTTGATCAAGGAACCTGTCTCCACAAAGTGTGACCACATATTTTGCAAATTTTGCATGCTGAAACTTCTCAACCAGAAGAAAGGGCCTTCACAGTGTCCTTTATGTAAGAATGATATAACCAAAAGGG

Test 3 (minimal):
AAAAAAAAAAAAAAAAAAAAANGG
```

---

## 📝 Antigravity Prompt Templates

### For CRISPR Page:
```
Build an HTML page with CSS and JavaScript for a CRISPR Guide RNA Visualizer.
User pastes a DNA sequence in a textarea. On clicking Analyze:
1. Validate that input only contains A, T, G, C characters
2. Scan the sequence for all NGG PAM sites (N = any base, G = Guanine)
3. For each NGG found, extract the 20 nucleotides immediately before it as the guide RNA
4. Record position, guide RNA (20nt), PAM, cut site (between nt 17 and 18)
5. Also scan the reverse complement of the sequence
6. Display: color-highlighted sequence (green=guide RNA, yellow=PAM site, blue=cut site)
7. Display a table of all guide RNAs with columns: #, Position, Guide RNA, PAM, Strand, Cut Site
8. Show count of total guide RNAs found
Use dark theme with teal accent colors. Make it clean and modern.
```

### For Clinical Trials Page:
```
Build an HTML page with CSS and JavaScript for a Clinical Trial Finder.
User types a disease name and clicks Search.
Fetch data from: https://clinicaltrials.gov/api/v2/studies?query.cond=DISEASE&pageSize=20&format=json
Display results as cards showing: trial title, status (color badge), phase, condition, location, start date.
Add filters for status (RECRUITING, COMPLETED) and trial phase.
Add a Load More button for pagination using nextPageToken from API response.
Clicking a card shows a modal with full details: official title, NCT ID, full summary, eligibility criteria, all locations, link to clinicaltrials.gov.
Add loading spinner, error handling, and popular search chips.
Use dark theme with teal accent colors.
```

---

## 📣 How to Present This on Resume

```
BioToolkit | Biotech Web Platform                              [Year]
- Built a dual-tool biotech platform with CRISPR Guide RNA Visualizer
  and Clinical Trial Finder using HTML, CSS, JavaScript
- Implemented custom bioinformatics algorithm to scan DNA sequences
  for Cas9 PAM sites and generate guide RNAs on both strands
- Integrated ClinicalTrials.gov REST API to fetch real-time global
  trial data with filtering and pagination
- Deployed on Vercel — live at: [your-link.vercel.app]
Technologies: HTML, CSS, JavaScript, REST API, ClinicalTrials.gov API
```

---

## 🔗 Useful Links

| Resource | Link |
|---|---|
| ClinicalTrials.gov API Docs | https://clinicaltrials.gov/data-api/api |
| ClinicalTrials.gov API Test | https://clinicaltrials.gov/api/v2/studies?query.cond=cancer&pageSize=5&format=json |
| Tailwind CSS CDN | https://cdn.tailwindcss.com |
| Google Fonts | https://fonts.google.com |
| Deploy on Vercel | https://vercel.com |
| Deploy on Netlify | https://netlify.com |
| NCBI Gene Database | https://www.ncbi.nlm.nih.gov/gene |

---

## ⚠️ Important Disclaimers to Add on Website

- "This tool is for educational purposes only"
- "Not a substitute for professional medical advice"
- "Clinical trial data sourced from ClinicalTrials.gov"
- "CRISPR analysis is for learning only — consult a researcher for actual experiments"

---

*Brain document prepared for BioToolkit project — 2nd Year BTech Biotech*

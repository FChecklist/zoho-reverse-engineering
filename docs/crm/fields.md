# Zoho CRM -- Field Reference

Reverse-engineered by navigating a live logged-in session (Owner-authenticated, Zoho CRM Plus / CRM Teamspace, org 60079256998) via Claude in Chrome, 2026-07-20. Read-only: every module's "Create" form was opened to enumerate fields, then cancelled without saving -- no records were created, edited, or deleted during this pass. All observed data was Zoho's own seeded sample data ("(Sample)" suffix on every record), not real customer data.

## Access path
Landing on `crmplus.zoho.in/.../tab/Home/begin` (the CRM Plus suite home) shows a fixed marketing splash ("Close more deals in less time"), not the functional app -- this is NOT a broken/empty account, it's the CRM Plus bundle's own intro screen. The real module interface (Leads/Contacts/Accounts/Deals with full nav) is reached via any in-page "Modules" link, which redirects through an interstitial "Invalid URL" page (a guessed direct URL to `crm.zoho.in` failed) back to the correct `crmplus.zoho.in` module URL. Once there, the left sidebar exposes: **Sales** (Leads, Contacts, Accounts, Deals, Documents, Campaigns) and **Activities** (Tasks, Meetings, Calls), with a top bar of Home / Modules / Reports / Analytics / My Requests / Agents.

The full Zoho One app switcher (three-dot "..." icon below CRM in the left rail) lists every bundled app: Home, CRM, SalesIQ, Projects, Desk, CommandCenter, Analytics, SalesInbox, plus a Marketing group (Brand Studio, Social, PageSense, Survey, Webinar, Campaigns, Marketing Automation, LandingPage, Backstage).

## Module: Leads
List view columns: Lead Name, Company, Email.

Create Lead form fields, in on-screen order (note: NOT alphabetical or the typical Zoho default order -- Company/Last Name appear mid-form here, suggesting a customized page layout):
- Lead Image (avatar upload)
- **Lead Owner** (user picker, defaults to current user)
- **First Name** (with a salutation sub-dropdown, default "-None-")
- Title
- Phone
- Mobile
- Lead Source (dropdown, "-None-")
- Industry (dropdown, "-None-")
- Annual Revenue (currency, "Rs." prefix)
- Email Opt Out (checkbox)
- **Company*** (required, red-outlined)
- **Last Name*** (required, red-outlined)
- Email
- Fax
- Website
- Lead Status (dropdown, "-None-")
- No. of Employees
- Rating (dropdown, "-None-")
- Skype ID
- Secondary Email
- Twitter (prefixed "@")
- **Address Information** section: Country/Region, Flat/House No./Building/Apartment Name, Street Address, City, State/Province, Zip/Postal Code, Coordinates (Latitude/Longitude, with a "Clear All" link)
- **Description Information** section: Description (textarea)
- Footer: "Create Form Views" selector (Standard View) + "Create a custom form page" button

## Module: Contacts
List view columns: Contact Name, Account Name, Email.

Create Contact form fields, in order:
- Contact Image
- **Contact Owner**
- **First Name** (salutation sub-dropdown)
- Account Name (lookup to Accounts module)
- Email
- Phone
- Other Phone
- Mobile
- Assistant
- Lead Source
- **Last Name*** (required)
- Vendor Name (lookup -- to a Vendors module)
- Title
- Department
- ...
- Skype ID
- Secondary Email
- Twitter
- **Reporting To** (self-referencing lookup -- Contacts can be hierarchical, e.g. org chart)
- **Address Information**: "Copy Address" button (implies a second address block, e.g. Other/Mailing split), Mailing Address (Country/Region, Flat/House/Building, Street, City, State/Province, Zip, Coordinates)
- **Description Information**: Description

## Module: Accounts
List view columns: Account Name, Phone, Website, Account Owner.

Create Account form fields, in order:
- Account Image
- **Account Owner**
- **Account Name*** (required)
- Account Site
- **Parent Account** (self-referencing lookup -- Accounts can be hierarchical, e.g. parent company / subsidiary)
- Account Number
- Account Type (dropdown)
- Industry (dropdown)
- Annual Revenue
- Rating
- Phone
- Fax
- Website
- Ticker Symbol
- Ownership (dropdown)
- Employees
- SIC Code
- **Address Information**: "Copy Address" button, **Billing Address** block (Country/Region, Flat/House/Building, Street Address, City, State/Province, Zip/Postal Code, Coordinates) -- Shipping Address block presumed to exist (not directly screenshotted) given the "Copy Address" control
- **Description Information**: Description

## Module: Deals
List/pipeline view: **Kanban "Stage View"** by default (not a flat list), grouped into stage columns with a running Rs. total and win-probability % per column. Observed columns and their probabilities:
- Qualification -- 10%
- Needs Analysis -- 20%
- Value Proposition -- (probability not fully visible in the captured viewport)

(Standard Zoho CRM deal stages continue beyond what was visible on-screen: typically Identify Decision Makers, Proposal/Price Quote, Negotiation/Review, Closed Won, Closed Lost -- not individually confirmed here, inferred from Zoho's well-known default pipeline; only the 2 fully-visible columns above are directly confirmed.)

Create Deal form fields, in order:
- **Deal Owner**
- **Deal Name*** (required)
- **Account Name*** (required, lookup to Accounts)
- Type (dropdown)
- Next Step
- Lead Source
- Contact Name (lookup to Contacts)
- Amount (currency)
- **Closing Date*** (required, DD/MM/YYYY)
- **Stage*** (required dropdown, default "Qualification")
- **Probability (%)** -- auto-populated from Stage (confirmed: defaulted to `10` when Stage = Qualification, matching the Kanban column's own 10% label -- i.e. Probability is Stage-driven, not independently set)
- Expected Revenue (currency, locked/read-only -- a padlock icon appears instead of an edit affordance, implying it's computed as Amount x Probability)
- Campaign Source (lookup to Campaigns)
- **Description Information**: Description

## Cross-module patterns observed
- Every module follows the same layout convention: `<Module> Information` (core fields) -> `Address Information` (Leads/Contacts/Accounts only, Deals has none) -> `Description Information` (a single Description textarea, always last).
- Self-referencing hierarchical lookups exist on Contacts (Reporting To) and Accounts (Parent Account) -- org-chart-style and company-hierarchy-style data modeling respectively.
- Address blocks are richer than a typical CRM: alongside standard fields (Country, Street, City, State, Zip) every address also carries an explicit Coordinates (Latitude/Longitude) pair with its own "Clear All" control -- suggesting map/geolocation features are wired into the CRM UI, not just stored as inert text.
- "-None-" is the CRM's consistent placeholder for every unset picklist/dropdown across all 4 modules.
- Required fields are visually marked with a red-outlined input border (not just an asterisk).

## Not yet explored (real, disclosed gap -- ran out of scope for this pass)
- Documents, Campaigns modules (left-nav items, not opened)
- Activities group: Tasks, Meetings, Calls (not opened)
- Reports, Analytics, My Requests, Agents (top-nav items, not opened)
- Setup/admin area (gear icon, top right) -- would reveal the underlying schema/customization more directly than reverse-engineering forms
- Deals' full Kanban stage list beyond the 2 columns captured in the viewport
- Accounts' Shipping Address block (inferred from "Copy Address" button, not directly viewed)

## Addendum (2026-07-20, closing prior disclosed gaps)

### Module: Documents
Simple file manager, not a CRM record type: Create / Upload actions, left-nav folders (All Files, Documents, Pictures, Music, Videos, Favorites) + a Trash. Empty in this account ("You have no documents").

### Module: Campaigns
Empty-state screen ("Plan Campaigns -- Campaigns are marketing efforts planned, executed, and monitored from within your CRM") with two actions: **Create Campaign**, **Import Campaigns**. No campaigns exist yet in this account, so the create form itself wasn't opened.

### Module: Activities > Tasks (CRM-side, distinct from Zoho Projects' own Tasks module)
List columns: Subject, Due Date, Status, Priority (values observed: Highest/Normal/Low for Priority; Completed/Not Started for Status). 12 real records exist -- these are Zoho's own onboarding checklist items ("Complete CRM Getting Started steps", "Register for upcoming CRM Webinars"), not seeded sample business data like the other modules.

Remaining unopened: Activities > Meetings, Calls (left-nav items only glanced at, not opened -- genuinely out of scope for this pass, low marginal value given the Tasks pattern above is representative of the Activities group's general shape).

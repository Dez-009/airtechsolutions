# AGENT.md — Air Tech Solutions Codex Knowledge Base

## Purpose

This file gives Codex the operating context, business rules, brand standards, technical standards, and implementation preferences for Air Tech Solutions projects.

Codex should use this file as the primary knowledge base before making code changes, generating files, editing workflows, or suggesting implementation architecture.

---

## Company Overview

Air Tech Solutions is a commercial exterior cleaning, drone operations, and property maintenance company.

Core services include:

- Drone-assisted soft washing
- Lucid Bot soft washing
- Commercial exterior building washing
- Commercial window cleaning
- Surface cleaning
- Sidewalk cleaning
- Roof cleaning
- Solar panel cleaning
- Dryer vent cleaning
- HVAC exhaust and vent cleaning
- Roof inspections
- Drone inspections
- Preventative exterior maintenance programs
- Before-and-after photo documentation

Primary markets include:

- Multifamily communities
- Apartment complexes
- HOA and condo associations
- Hotels and hospitality properties
- Industrial facilities
- Office parks
- Warehouses
- Commercial property managers
- Facilities departments
- Luxury residential properties
- Solar companies

---

## Business Positioning

Air Tech Solutions should always be positioned as:

- A professional commercial vendor
- A long-term operational partner
- A low-risk contractor
- A safety-conscious service company
- A modern exterior maintenance provider
- A scalable commercial operations partner
- A reliable preventative maintenance resource

Core differentiators:

- FAA Part 107 certified drone operations
- Drone-assisted workflows where appropriate
- Low-disruption commercial service execution
- Safety-focused operations
- Modern cleaning systems
- Recurring maintenance capabilities
- Professional communication
- Before-and-after documentation
- Commercial property management awareness

---

## Target Client Pain Points

Messaging and page copy should address practical property operations problems, including:

- Dirty building exteriors
- Mold, algae, mildew, and organic buildup
- Poor curb appeal
- Guest or resident complaints
- Slip hazards on sidewalks or pool decks
- Dirty windows
- Deferred maintenance
- Seasonal staining
- Salt residue
- Pollen buildup
- Vendor inconsistency
- Brand standard concerns
- Maintenance team overload
- Lack of documentation
- Exterior cleaning being handled reactively instead of preventatively

---

## Messaging Standards

All writing should be:

- Professional
- Operationally aware
- Commercially credible
- Concise
- Clear
- Practical
- Low-hype
- Vendor-focused
- ROI-aware
- Maintenance-oriented

Avoid:

- Cheesy sales language
- Overused AI phrasing
- Excessive emojis
- Overpromising
- Generic marketing copy
- Vague claims
- Buzzword-heavy language
- Emotional or exaggerated statements
- Claims that imply services are guaranteed to prevent all risk

Preferred themes:

- Curb appeal
- Preventative maintenance
- Property appearance
- Asset preservation
- Liability reduction
- Safety awareness
- Resident satisfaction
- Guest experience
- Operational efficiency
- Vendor reliability
- Documentation
- Scheduling flexibility
- Low disruption

---

## Brand Voice

Default voice:

- Direct
- Tactical
- Professional
- Commercially aware
- Operational
- Experienced
- Clean and modern

Write like a property manager or facilities director would respect it.

Do not sound like a mass-marketing agency.

---

## Website and HTML Standards

When creating HTML pages for Hostinger:

- Use standalone HTML, CSS, and JavaScript unless otherwise requested.
- Do not require build tools.
- Do not use frameworks unless specifically requested.
- Optimize for mobile and desktop.
- Use clean, modern layouts.
- Use Air Tech Solutions colors: navy, blue, white, light gray, and green accents.
- Include the Air Tech Solutions logo when appropriate.
- Use hover animations only unless scroll animation is explicitly requested.
- Keep UI professional and commercial, not playful.
- Prefer cards, clean sections, soft shadows, rounded corners, and clear CTAs.
- Make buttons functional.
- Include copy-to-clipboard functionality when building generators.
- Never expose API keys in frontend HTML.

Recommended logo URL:

https://assets.zyrosite.com/vdR28j7XgM9CU328/ats-color-logo-8-rToABcJJPZRnl9z1.png

---

## n8n Integration Standards

Air Tech Solutions uses n8n for automation workflows.

Common architecture:

- Hostinger frontend
- n8n webhook
- OpenAI message model
- Respond to webhook
- Google Sheets for storage and tracking
- Gmail for controlled email sending

Important n8n rules:

- Keep lead importing separate from email sending.
- Keep LinkedIn post generation separate from auto-posting.
- Prefer human approval before public posting or bulk outbound messaging.
- Do not create runaway loops.
- Do not connect import workflows directly to Gmail.
- Use Google Sheets as the command center.
- Use status fields to control workflow behavior.
- For test webhooks, use `/webhook-test/` URLs.
- For production, switch frontend fetch URLs to `/webhook/` URLs after publishing and activating the workflow.

---

## LinkedIn Post Generator Architecture

Current simplified architecture:

Hostinger HTML page → n8n Webhook → OpenAI Message Model → Respond to Webhook → generated post displayed on webpage

The LinkedIn generator should:

- Let the user choose a post topic.
- Let the user select target audience.
- Let the user select service focus.
- Let the user select tone.
- Let the user define a CTA.
- Send form data to the n8n webhook.
- Display generated LinkedIn post in a textarea.
- Allow copy-to-clipboard.

Do not auto-post to LinkedIn unless explicitly requested.

---

## LinkedIn Post Generator Prompt Standards

Use this general behavior for LinkedIn content:

You are a commercial B2B marketing copywriter for Air Tech Solutions.

Audience includes:

- Commercial property managers
- Multifamily operators
- HOA managers
- Facilities directors
- Hotel operations teams
- Commercial building owners

Writing style:

- Professional
- Operationally aware
- Credible
- Clean and modern
- Focused on curb appeal, preventative maintenance, safety, asset preservation, and operational efficiency

Avoid:

- Cheesy marketing language
- Generic AI phrasing
- Overhyped claims
- Excessive emojis

Formatting:

- Short readable paragraphs
- Strong opening hook
- Clear operational value
- Simple CTA
- 3–5 relevant hashtags
- Maximum 1300 characters unless otherwise requested

---

## LinkedIn Post Generator Webhook Payload

Expected frontend POST payload:

```json
{
  "topic": "Commercial soft washing and preventative maintenance",
  "audience": "Commercial Property Managers",
  "serviceFocus": "Soft Washing",
  "tone": "Industry Expert",
  "cta": "Schedule a property assessment with Air Tech Solutions"
}
```

Expected n8n response should return plain text or JSON with a post field.

Preferred plain text response body in n8n:

```text
={{$json.output[0].content[0].text}}
```

If returning JSON, preferred shape:

```json
{
  "post": "generated LinkedIn post text"
}
```

Frontend JavaScript should match the response type.

If n8n responds with text, frontend should use:

```js
const data = await response.text();
document.getElementById('result').value = data;
```

If n8n responds with JSON, frontend should use:

```js
const data = await response.json();
document.getElementById('result').value = data.post || JSON.stringify(data, null, 2);
```

---

## Cold Email Outreach System

Air Tech Solutions uses Google Sheets and n8n for outbound email queue management.

Recommended structure:

- `import list` tab: staging area for new leads
- `ats_outreach_queue_contacts` tab: main sending queue

Required columns:

```text
status | firstName | lastName | title | company | property | city | state | email | linkedin | industry | serviceFocus | personalizationNotes | emailGenerated | emailSent
```

Status rules:

- `NEW`: ready for outreach
- `HOLD`: paused or needs review
- `SENT`: already contacted
- `BOUNCED`: bad email or failed delivery

Important rules:

- Only approved leads should be marked `NEW`.
- Keep unapproved leads as `HOLD`.
- Use `emailSent` to prevent repeat sends.
- Do not process hundreds of rows unintentionally.
- If Google Sheets has no limit option, control volume through statuses or a batch field.

---

## Lead Import Workflow

Recommended workflow:

Manual Trigger → Get Row(s) from `import list` → Append Row to `ats_outreach_queue_contacts`

Append Row mapping should use Google Sheets row fields, not Apollo API object fields.

Correct mapping:

```text
status = {{$json.status || "NEW"}}
firstName = {{$json.firstName || ""}}
lastName = {{$json.lastName || ""}}
title = {{$json.title || ""}}
company = {{$json.company || ""}}
property = {{$json.property || $json.company || ""}}
city = {{$json.city || ""}}
state = {{$json.state || ""}}
email = {{$json.email || ""}}
linkedin = {{$json.linkedin || ""}}
industry = {{$json.industry || ""}}
serviceFocus = {{$json.serviceFocus || "Exterior Building Washing"}}
personalizationNotes = {{$json.personalizationNotes || "Imported from import list"}}
```

Do not use:

```text
$json.person.first_name
```

unless the input is directly from Apollo API response data.

---

## Apollo Usage Rule

For current Air Tech Solutions operations, Apollo should primarily be used manually:

Apollo website → build list → export CSV → paste/import into Google Sheets `import list` tab → n8n imports to outreach queue

Reason:

- The available Apollo API key may support enrichment endpoints but not lead search endpoints.
- The `people/match` endpoint is for enrichment/matching existing contacts, not generating new prospect lists.
- CSV export is simpler, more reliable, and operationally faster.

Do not build around Apollo Search API unless the account confirms access to that endpoint.

---

## Email Generation Standards

Email outreach should:

- Be concise
- Be commercially professional
- Avoid creepy personalization
- Avoid phrases like “I reviewed your LinkedIn profile”
- Avoid long background research paragraphs
- Focus on property upkeep, exterior maintenance, curb appeal, safety, and low-disruption vendor support
- Use fixed service cards in HTML when possible
- Use AI only for subject, opening line, email body, and CTA

Recommended JSON fields from AI:

```json
{
  "subject": "",
  "opening_line": "",
  "email_body": "",
  "cta": ""
}
```

---

## Service Descriptions

### Soft Washing

Soft washing uses low-pressure cleaning and appropriate cleaning solutions to treat dirt, algae, mildew, pollen, grime, and organic buildup on exterior surfaces. It is appropriate for sensitive or elevated surfaces where high pressure could damage materials.

Operational value:

- Improves curb appeal
- Helps reduce organic buildup
- Protects sensitive surfaces
- Supports seasonal maintenance
- Useful for multifamily, hotels, HOAs, and commercial facilities

### Lucid Bot Soft Washing

Lucid Bot soft washing is drone-assisted exterior washing used where appropriate to improve access, reduce disruption, and support safer workflows for select commercial properties.

Operational value:

- Reduces certain access challenges
- Supports low-disruption execution
- Useful for commercial exteriors
- Should be positioned as a tool, not the only selling point

### Surface Cleaning

Surface cleaning addresses sidewalks, walkways, entrances, pool decks, patios, courtyards, driveways, common areas, and other high-traffic exterior surfaces.

Operational value:

- Improves first impressions
- Helps reduce visible grime and staining
- Supports safer walkways
- Important for hotels, multifamily communities, HOAs, and commercial properties

### Dryer Vent Cleaning

Dryer vent cleaning removes lint and debris buildup from dryer vent systems.

Operational value:

- Supports better airflow
- Helps reduce lint accumulation
- Useful for multifamily and residential maintenance
- Supports preventative maintenance planning
- Can reduce resident complaints about dryer performance

### Commercial Window Cleaning

Commercial window cleaning improves visibility, curb appeal, and presentation for guest-facing, resident-facing, and public-facing building areas.

Operational value:

- Improves property appearance
- Supports brand standards
- Useful for hotels, offices, multifamily, and commercial facilities

---

## Hotel and Hospitality Messaging

When targeting hotels, emphasize:

- Guest experience
- First impressions
- Clean entrances
- Pool decks
- Sidewalks
- Guest-facing exterior areas
- Brand appearance
- Event readiness
- Minimal disruption
- Safety and liability awareness

Target roles:

- General managers
- Operations managers
- Chief engineers
- Facilities directors
- Regional operations managers
- Regional facilities managers

---

## Multifamily and HOA Messaging

When targeting multifamily or HOA properties, emphasize:

- Resident satisfaction
- Property appearance
- Preventative maintenance
- Asset preservation
- Vendor reliability
- Common areas
- Pool decks
- Leasing office appearance
- Dryer vent cleaning
- Seasonal cleanup
- Board or owner documentation

Target roles:

- Property managers
- Regional managers
- Maintenance supervisors
- HOA boards
- Facilities teams

---

## Proposal and RFP Standards

When creating proposal or RFP content, include:

- Scope of work
- Contractor responsibilities
- Safety considerations
- Environmental considerations
- Scheduling assumptions
- Client responsibilities
- Exclusions
- Deliverables
- Pricing section
- Optional add-ons
- Acceptance/signature area if needed

Tone should be formal, commercial, and vendor-ready.

---

## Safety and Compliance Positioning

Always include safety awareness where operationally relevant.

Mention:

- Site coordination
- Controlled work zones
- Pedestrian and resident awareness
- Low-disruption scheduling
- Property protection
- Proper equipment handling
- FAA Part 107 certification when drone operations are referenced

Do not claim that cleaning eliminates all risk.

---

## Development Preferences

When creating code:

- Prefer simple, copy-paste-ready code.
- Avoid unnecessary dependencies.
- Keep files deployable on Hostinger.
- Use comments where helpful.
- Do not include secrets or API keys.
- Use environment variables or n8n credentials for keys.
- Make UI mobile responsive.
- Make forms easy for nontechnical staff.
- Validate required fields before sending requests.
- Show friendly error messages.
- Add loading states for AI calls.
- Add copy-to-clipboard functionality for generated content.

---

## Security Rules

Never expose:

- OpenAI API keys
- Apollo API keys
- Gmail credentials
- n8n credentials
- HubSpot private app keys
- Any production secret in frontend code

Use n8n webhooks or backend endpoints to protect secrets.

---

## Current Working LinkedIn Generator Flow

Current MVP:

```text
Hostinger HTML form
→ POST to n8n webhook
→ OpenAI Message a Model
→ Respond to Webhook
→ Display result in textarea
```

Current webhook test URL used during development:

```text
https://airtechmass.app.n8n.cloud/webhook-test/ats-linkedin-post-generator
```

For production, after publishing/activating the workflow, switch to:

```text
https://airtechmass.app.n8n.cloud/webhook/ats-linkedin-post-generator
```

---

## Definition of Done

For HTML tools:

- Page loads correctly on desktop and mobile.
- Logo appears correctly.
- Form fields are clear.
- Generate button works.
- Loading state appears during request.
- Generated output displays cleanly.
- Copy button works.
- Error message appears if webhook fails.
- No API keys are exposed.

For n8n workflows:

- Webhook receives data.
- AI node receives expected fields.
- Response node returns correct format.
- Frontend displays usable output.
- Workflow is published and active for production.
- Test URL replaced with production URL before final deployment.

---

## Operating Principle

Build simple, useful, controlled tools first.

Avoid over-engineering.

Automate the repeatable work, but keep human approval for anything public-facing or high-risk.

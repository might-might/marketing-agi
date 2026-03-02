# Call Transcript Analysis Rulebook

## Document Metadata

- **Version:** 1.0.0
- **Creation Date:** 2025-03-01
- **Status:** Quick-and-Dirty
- **Purpose:** Define rules for processing voice call transcripts to extract structured knowledge

---

## Purpose

This rulebook defines the standardized process for analyzing voice call transcripts (from Fireflies or other transcription applications) to extract structured knowledge about client needs, objections, and feedback. The extracted information will be integrated into the project's knowledge base for product development and client relationship management.

---

## Input

**Source Files Location:**
- Identification of the student name or student project folder. Students store their projects in the `/student-projects/`. You should identify which project or call recording are we dealing with.
- Original transcript files are stored in: `/student-projects/[student-name]/customer-calls/call-recordings/` folder
- Format: Text transcripts (markdown, plain text, or json) with speaker identification (when available)
- Identify the preferred language of the student. If it is English write everything in English, if it is Russian, write and generate text in Russian.
- Context: Client calls, sales calls, product discussions

**File Organization:**
- **Original Transcripts:** `/student-projects/[student-name]/customer-calls/call-recordings/` - All original transcript files go here
- **Summary Files:** `/student-projects/[student-name]/customer-calls/call-summaries/` - All generated summary files go here
- **Database:** `/student-projects/[student-name]/genotype/call-database.md` - Central knowledge base (root folder)

---

## Extraction Rules

### 0. Speaker Identification: Client vs Employee

**CRITICAL RULE:** Before extracting any quote, need, or objection, you MUST identify whether the speaker is a real client (external to the organization) or our employee or sales person.

**Why This Matters:**
- **Client quotes are the most valuable** - This is the true "Voice of Customer" - exactly how clients express their needs in their own words
- **Employee statements are less valuable** - Even when confirmed by clients, they represent our interpretation, not the client's direct voice
- **This distinction is critical** for understanding what clients actually need vs what we think they need

**Identification Process:**

1. **Load Employee List:**
   - Open `/student-projects/[student-name]/employees/employee-list.md`
   - Check speaker name against this list if the transcipt provides this information

2. **Identify Speaker Type:**
   - **If speaker name matches employee list:** Mark as "Employee"
   - **If speaker name does NOT match employee list:** Mark as "Client"
   - **If speaker name unclear:** Check context (company affiliation, role description) to determine

3. **Source Type Classification:**

   **Type A: Client Direct Quote (MOST VALUABLE)**
   - Quote/need/objection stated directly by the client
   - This is the true Voice of Customer
   - Mark as: `Source Type: Client Direct Quote`
   - **Example:** Client says "We need a chatbot that can handle multiple languages simultaneously"

   **Type B: Employee Statement with Client Confirmation (LESS VALUABLE BUT STILL NEEDED)**
   - Employee states a need/problem: "Do you want [X]?" or "Would it be helpful if [Y]?"
   - Client confirms with "Yes", "That's right", "Exactly", or similar confirmation
   - This is less valuable because it's our interpretation, but still useful
   - Mark as: `Source Type: Employee Statement with Client Confirmation`
   - **Example:** Employee: "Would it be helpful if avatars could grow with athletes over time?" Client: "Yes, that's exactly what we need"

4. **Documentation Requirement:**
   - **ALWAYS** include `Source Type` field in all extracted quotes, needs, and objections
   - **ALWAYS** verify speaker against employee list before extraction
   - **ALWAYS** note in database entry whether this is client direct quote or employee statement with confirmation

**Example Identification:**
```
Speaker: "Alex Smith"
Check: Matches employee list → Employee
Quote: "Would it be helpful if avatars could be deployed on-premises?"
Client Response: "Yes, that's exactly what we need"
Source Type: Employee Statement with Client Confirmation
```

```
Speaker: "Mark Barrow"
Check: Does NOT match employee list → Client
Quote: "We need avatars that can handle multiple languages simultaneously"
Source Type: Client Direct Quote
```

---

### 1. Client Quotes Extraction

**Rule:** Extract every important statement as an exact quote, but **prioritize client direct quotes** over employee statements.

**Criteria for "Important":**
- Statements about product needs or requirements
- Statements about preferences or priorities
- Statements expressing concerns or objections
- Statements about business context or use cases

**Priority Order:**
1. **Client direct quotes** (most valuable - true Voice of Customer)
2. **Employee statements with client confirmation** (less valuable but still needed)

**Output Format:**
- **Quote:** Exact text as spoken
- **Speaker:** Name/identifier of speaker (e.g., "John said...")
- **Source Type:** "Client Direct Quote" OR "Employee Statement with Client Confirmation"
- **Context:** Brief description of conversation context
- **Interpretation:** Short explanation of what the quote means
- **Client Confirmation:** (If Source Type is Employee Statement) - Exact client response confirming the statement

**Example 1 - Client Direct Quote:**
```
Quote: "We need avatars that can handle multiple languages simultaneously"
Speaker: John (Client)
Source Type: Client Direct Quote
Context: Discussion about international deployment
Interpretation: Client requires multilingual avatar capabilities for global rollout
```

**Example 2 - Employee Statement with Client Confirmation:**
```
Quote: "Would it be helpful if avatars could grow with athletes over time?"
Speaker: Alex Grimwood (Employee)
Source Type: Employee Statement with Client Confirmation
Client Confirmation: "Yes, that's exactly what we need"
Context: Discussion about athlete development tracking
Interpretation: Employee identified need for avatar growth tracking, client confirmed
```

---

### 2. Product Needs Extraction

**Rule:** Extract every statement where a need, want, or requirement is expressed about the product (avatars). **MUST identify source type** (client direct vs employee statement with confirmation).

**Patterns to Identify:**
- "We want [X]" (client direct)
- "We need [X]" (client direct)
- "We require [X]" (client direct)
- "It would be great if [X]" (client direct)
- "We're looking for [X]" (client direct)
- "Do you want [X]?" (employee statement - check for client confirmation)
- "Would it be helpful if [Y]?" (employee statement - check for client confirmation)
- "What if [Z]?" (employee statement - check for client confirmation)

**Focus:** Only extract needs related to **avatars** or the core product. Exclude:
- Meeting scheduling requests
- General business needs unrelated to product
- Administrative requests

**Source Type Identification (MANDATORY):**
1. **Check speaker name** against employee list (`/student-projects/[student-name]/employees/employee-list.md`)
2. **If client speaks directly:** Mark as "Client Direct Quote" (MOST VALUABLE)
3. **If employee speaks and client confirms:** Mark as "Employee Statement with Client Confirmation" (LESS VALUABLE BUT STILL NEEDED)
4. **If employee speaks but NO client confirmation:** DO NOT extract as a need (this is speculation, not validated need)

**Output Format:**
- **Need Statement:** Exact quote
- **Source Type:** "Client Direct Quote" OR "Employee Statement with Client Confirmation"
- **Speaker:** Who expressed the need (with role: Client or Employee)
- **Need Category:** Categorized need (e.g., "Multilingual Support", "Customization", "Performance")
- **Reason/Explanation:** Why they need this (if provided)
- **Client Confirmation:** (If Source Type is Employee Statement) - Exact client response confirming the need

**Example 1 - Client Direct Quote (MOST VALUABLE):**
```
Need Statement: "We want avatars that can handle multiple languages simultaneously because we're expanding to Europe"
Source Type: Client Direct Quote
Speaker: John (Client)
Need Category: Multilingual Support
Reason: International expansion to European markets
```

**Example 2 - Employee Statement with Client Confirmation (LESS VALUABLE BUT STILL NEEDED):**
```
Need Statement: "Would it be helpful if avatars could grow with athletes over time?"
Source Type: Employee Statement with Client Confirmation
Speaker: Alex Grimwood (Employee)
Client Confirmation: "Yes, that's exactly what we need"
Need Category: Customization / Growth Tracking
Reason: Client confirmed need for avatar growth tracking alongside athlete development
```

---

### 3. Explanations and Rationale Extraction

**Rule:** Extract explanations and reasons provided for needs or requests.

**Patterns to Identify:**
- "We want [X] because [Y]"
- "We need [X] since [Y]"
- "The reason we need [X] is [Y]"
- Contextual explanations following need statements

**Output Format:**
- **Linked Need:** Reference to the need statement
- **Explanation:** Exact quote or summary of the explanation
- **Business Context:** What business situation drives this need

---

### 4. Objections and Uncertainties Extraction

**Rule:** Extract every objection, concern, uncertainty, or hesitation expressed. **MUST identify source type** (client direct vs employee statement with confirmation).

**Patterns to Identify:**
- "We're not sure about [X]" (client direct)
- "We're concerned about [X]" (client direct)
- "We don't understand [X]" (client direct)
- "We're worried that [X]" (client direct)
- "We don't see how [X]" (client direct)
- Hesitation or questioning statements
- Employee asking "Are you concerned about [X]?" with client confirmation

**Source Type Identification (MANDATORY):**
1. **Check speaker name** against employee list (`/student-projects/[student-name]/employees/employee-list.md`)
2. **If client expresses objection directly:** Mark as "Client Direct Quote" (MOST VALUABLE)
3. **If employee identifies concern and client confirms:** Mark as "Employee Statement with Client Confirmation" (LESS VALUABLE BUT STILL NEEDED)

**Output Format:**
- **Objection Quote:** Exact text of the objection/concern
- **Source Type:** "Client Direct Quote" OR "Employee Statement with Client Confirmation"
- **Objection Type:** Categorized type (e.g., "Technical Concern", "Cost Concern", "Feature Gap", "Understanding Gap")
- **Speaker:** Who raised the objection (with role: Client or Employee)
- **Context:** Conversation context when objection was raised
- **Client Confirmation:** (If Source Type is Employee Statement) - Exact client response confirming the concern

**Example 1 - Client Direct Quote (MOST VALUABLE):**
```
Objection Quote: "I'm not sure if avatars can handle our volume of concurrent users"
Source Type: Client Direct Quote
Objection Type: Technical Concern / Scalability
Speaker: Sarah (Client)
Context: Discussion about enterprise deployment
```

**Example 2 - Employee Statement with Client Confirmation (LESS VALUABLE BUT STILL NEEDED):**
```
Objection Quote: "Are you concerned about latency with avatar interactions?"
Source Type: Employee Statement with Client Confirmation
Client Confirmation: "Yes, we had issues with latency last year"
Objection Type: Performance Concern
Speaker: Alex Grimwood (Employee)
Context: Discussion about event demonstrations
```

---

## Knowledge Base Mapping

### Target Database

**Primary Knowledge Base:** `/student-projects/[student-name]/genotype/call-database.md`

This database stores all extracted needs, objections, and quotes with:
- Unique identifiers (NEED-001, OBJ-001, QUOTE-001)
- Every need shold be named as "I want [need]" to specify the voice of the customer, what he or she really needs or wants
- Every objection should be named as "I don't want [problem]" or "I am concerned about [problem]"
- Frequency tracking (how many times mentioned)
- Source call references (which calls/clients mentioned it)
- **Source Type** (Client Direct Quote OR Employee Statement with Client Confirmation) - **CRITICAL FIELD**
- Exact quotes and variations
- Categorization and metadata

**Source Type Documentation in Database:**

When creating or updating database entries, **ALWAYS** include source type information:

1. **In "Source Calls" section:** Add source type indicator
   - Format: `[Call-ID] - [YYYY-MM-DD] - [Client Name] - [Speaker Name] - [Source Type]`
   - Example: `Call-2026-001 - 2026-02-03 - New Lead - Mark Brown - Client Direct Quote`

2. **In "Exact Quotes" section:** Add source type to each quote
   - Format: `"[Exact quote]" - [Call-ID], [Speaker] - [Source Type]`
   - Example: `"We need a chatbot that can handle multiple languages" - Call-2026-001, Mark Brown - Client Direct Quote`
   - Example: `"Would it be helpful if the chatbot could ask questions?" - Call-2026-001, Alex Smith - Employee Statement with Client Confirmation`

3. **Add "Source Type Summary" field** to each entry:
   - Shows breakdown: "X Client Direct Quotes, Y Employee Statements with Confirmation"
   - Helps identify which entries are most valuable (more client direct quotes = more valuable)

4. **Priority Marking:**
   - Entries with ONLY "Client Direct Quotes" = HIGHEST PRIORITY (true Voice of Customer)
   - Entries with mix of both = MEDIUM PRIORITY (validated but includes our interpretation)
   - Entries with ONLY "Employee Statements with Confirmation" = LOWER PRIORITY (still valuable but less direct)

---

### Mapping to Existing Knowledge

**Rule:** For each extracted item (need, objection, quote), check the Voice of Customer Database for existing entries before creating new ones.

**Mapping Process:**

1. **Open Voice of Customer Database:** Load `/student-projects/[student-name]/genotype/call-database.md`
2. **Identify Current Call's Client:** Extract client name from transcript metadata or content

3. **Check Existing Needs:**
   - Search database for needs with similar core meaning
   - **Matching Criteria:** Same core requirement = same entry (even if phrased differently)
   - **If Match Found:**
     - **Check if this need already has a source from the same client:**
       - **If same client already recorded:** 
         - **DO NOT** update entry (skip - prevents duplicate for same client)
         - **DO NOT** increment frequency
         - **DO NOT** add new source call reference
         - This need is already recorded for this client
       - **If different client(s) only:**
         - Update existing entry:
           - Increment frequency counter (+1)
           - Add new source call reference (Call-ID, date, client, speaker)
           - Add new quote variation if different phrasing
           - Update "Last Mentioned" date
           - Add new reason/explanation if provided
   - **If No Match Found:**
     - Create new need entry with next available ID (NEED-001, NEED-002, etc.)
     - Set frequency: 1
     - Add all source information
     - Categorize appropriately

4. **Check Existing Objections:**
   - Search database for objections with similar core concern
   - **Matching Criteria:** Same core concern = same entry (even if phrased differently)
   - **If Match Found:**
     - **Check if this objection already has a source from the same client:**
       - **If same client already recorded:**
         - **DO NOT** update entry (skip - prevents duplicate for same client)
         - **DO NOT** increment frequency
         - **DO NOT** add new source call reference
         - This objection is already recorded for this client
       - **If different client(s) only:**
         - Update existing entry:
           - Increment frequency counter (+1)
           - Add new source call reference
           - Add new quote variation if different phrasing
           - Update "Last Mentioned" date
   - **If No Match Found:**
     - Create new objection entry with next available ID (OBJ-001, OBJ-002, etc.)
     - Set frequency: 1
     - Add all source information
     - Categorize appropriately

5. **Check Existing Quotes:**
   - For important quotes, check if exact same quote exists
   - **If Exact Match Found:**
     - **Check if this quote already has a source from the same client:**
       - **If same client already recorded:**
         - **DO NOT** update entry (skip - prevents duplicate for same client)
         - **DO NOT** increment frequency
         - **DO NOT** add new source call reference
         - This quote is already recorded for this client
       - **If different client(s) only:**
         - Increment frequency counter
         - Add new source call reference
   - **If Similar Meaning but Different Wording:**
     - Create separate quote entry (QUOTE-001, QUOTE-002, etc.)
     - Link to related need/objection if applicable

6. **Update Database:**
   - Save all updates to `/student-projects/[student-name]/genotype/call-database.md`
   - Update statistics section
   - Update index by client (if new client)
   - Update index by call

**Example Mapping Result:**

**Example 1: New need from new client**
```
Extracted Need: "We need avatars that can handle multiple languages simultaneously"
Current Call Client: Siemens

Database Check: Found NEED-003 "I want chatbot speaking multiple languages"
Existing Sources: Call-2025-010 - 2025-01-10 - Apple - Larry Werber

Action: Update NEED-003 (different client)
- Frequency: 1 → 2
- Added Source: Call-2025-015 - 2025-01-13 - Siemens - John Smith
- Added Quote Variation: "We need a chatbot that can handle multiple languages simultaneously"
- Updated Last Mentioned: 2025-01-13
- Added Reason: "Expanding to European markets"
```

**Example 2: Same need from same client (deduplication)**
```
Extracted Need: "We need multilingual avatar support"
Current Call Client: Adidas

Database Check: Found NEED-003 "I want chatbot speaking multiple languages"
Existing Sources: 
- Call-2025-010 - 2025-01-10 - Apple - Larry Werber
- Call-2025-015 - 2025-01-13 - Coca-Cola - John Smith

Action: SKIP (same client already recorded)
- DO NOT increment frequency
- DO NOT add new source
- This need is already recorded for Adidas

```

---

### Matching Logic Guidelines

**For Needs:**
- Focus on core requirement, not exact wording
- Examples of same need:
  - "We need multilingual support" = "Avatars should work in multiple languages" = "Can avatars handle different languages?" = **SAME NEED**
  - "We need faster response times" = "Avatars should respond quicker" = **SAME NEED**

**For Objections:**
- Focus on core concern, not exact wording
- Examples of same objection:
  - "We're worried about scalability" = "Can it handle our volume?" = "I'm concerned about performance at scale" = **SAME OBJECTION**
  - "We don't understand how it works" = "Can you explain the technology?" = **SAME OBJECTION**

**For Quotes:**
- Exact same text = increment frequency
- Similar meaning but different words = create separate entry, but link to related need/objection

---

## Processing Workflow

### Step 1: Transcript Ingestion & Speaker Identification
- Load transcript file from `call-recordings/` folder
- Identify speakers (if available)
- **CRITICAL: Load employee list** from `/student-projects/[student-name]/employees/employee-list.md` if this file exists.
- **For each speaker identified:**
  - Check speaker name against employee list
  - Mark as "Employee" if name matches list
  - Mark as "Client" if name does NOT match list
  - Note: If speaker name unclear, check context (company affiliation, role) to determine
- Parse transcript structure
- Note: Original transcript files should be placed in `call-recordings/` folder before processing

### Step 2: Quote Extraction with Source Type Identification
- Scan transcript for important statements (from both clients and employees)
- **For each quote:**
  - Check speaker against employee list (from Step 1)
  - **If Client:** Mark as "Client Direct Quote" (MOST VALUABLE)
  - **If Employee:** Check if client confirmed with "Yes", "That's right", etc.
    - **If confirmed:** Mark as "Employee Statement with Client Confirmation" (LESS VALUABLE BUT STILL NEEDED)
    - **If NOT confirmed:** DO NOT extract (this is speculation, not validated)
- Extract exact quotes with speaker attribution and source type
- Add context and interpretation
- Include client confirmation quote if source type is "Employee Statement with Client Confirmation"

### Step 3: Needs Extraction with Source Type Identification
- Identify product-related needs (avatars only) from both clients and employees
- **For each need statement:**
  - Check speaker against employee list (from Step 1)
  - **If Client:** Mark as "Client Direct Quote" (MOST VALUABLE)
  - **If Employee:** Check if client confirmed the need
    - **If confirmed:** Mark as "Employee Statement with Client Confirmation" (LESS VALUABLE BUT STILL NEEDED)
    - **If NOT confirmed:** DO NOT extract as a need (this is speculation, not validated need)
- Extract need statements with explanations and source type
- Categorize needs
- Include client confirmation quote if source type is "Employee Statement with Client Confirmation"

### Step 4: Objections Extraction with Source Type Identification
- Identify concerns, uncertainties, objections from both clients and employees
- **For each objection:**
  - Check speaker against employee list (from Step 1)
  - **If Client:** Mark as "Client Direct Quote" (MOST VALUABLE)
  - **If Employee:** Check if client confirmed the concern
    - **If confirmed:** Mark as "Employee Statement with Client Confirmation" (LESS VALUABLE BUT STILL NEEDED)
    - **If NOT confirmed:** DO NOT extract as an objection (this is speculation, not validated concern)
- Extract objection quotes with source type
- Categorize objection types
- Include client confirmation quote if source type is "Employee Statement with Client Confirmation"

### Step 5: Knowledge Base Mapping with Source Type Documentation
- Open `/student-projects/[student-name]/genotype/call-database.md`
   - Update statistics section
- **Identify current call's client** from transcript metadata or content
- For each extracted need:
  - **Include source type** (Client Direct Quote OR Employee Statement with Client Confirmation) in mapping
  - Search database for matching need (same core meaning)
  - **Client Deduplication Check:**
    - If matching need found AND it already has a source from the same client:
      - **DO NOT** create duplicate entry
      - **DO NOT** increment frequency
      - **DO NOT** add new source call reference
      - Skip this need (already recorded for this client)
    - If matching need found BUT no source from this client:
      - Update existing entry (increment frequency, add source WITH source type)
      - **IMPORTANT** If in the call transcript you observe the **Poll** when the speaker asks the participants to raise hands or send something to the chat to gather information about how many people have this exact need, then analyze this piece of dialog and understand how many votes this need had collected, and increment the frequency based on this number.
      - **Document source type** in "Source Calls" and "Exact Quotes" sections
    - If no matching need found:
      - Create new need entry with next ID
      - **Include source type** in all relevant fields (Source Calls, Exact Quotes)
      - **Add "Source Type Summary"** field showing breakdown
- For each extracted objection:
  - **Include source type** (Client Direct Quote OR Employee Statement with Client Confirmation) in mapping
  - Search database for matching objection (same core concern)
  - **Client Deduplication Check:**
    - If matching objection found AND it already has a source from the same client:
      - **DO NOT** create duplicate entry
      - **DO NOT** increment frequency
      - **DO NOT** add new source call reference
      - Skip this objection (already recorded for this client)
    - If matching objection found BUT no source from this client:
      - Update existing entry (increment frequency, add source WITH source type)
      - **Document source type** in "Source Calls" and "Exact Quotes" sections
    - If no matching objection found:
      - Create new objection entry with next ID
      - **Include source type** in all relevant fields (Source Calls, Exact Quotes)
      - **Add "Source Type Summary"** field showing breakdown
- For each extracted quote:
  - **Include source type** (Client Direct Quote OR Employee Statement with Client Confirmation) in mapping
  - Check if exact quote exists
  - **Client Deduplication Check:**
    - If exact quote found AND it already has a source from the same client:
      - **DO NOT** create duplicate entry
      - **DO NOT** increment frequency
      - **DO NOT** add new source call reference
      - Skip this quote (already recorded for this client)
    - If exact quote found BUT no source from this client:
      - Update frequency and add source WITH source type
      - **Document source type** in quote entry
    - If exact quote not found:
      - Create new quote entry with next ID
      - **Include source type** in quote entry
- Save all updates to `/student-projects/[student-name]/genotype/call-database.md`
   - Update statistics section
- Update database statistics and indexes

**Important:** The deduplication rule ensures that each client's needs and objections are recorded only once in the knowledge base, even if mentioned in multiple calls. Only add new needs/objections/quotes if they are genuinely new for that client, or if the same need/objection comes from a different client.

### Step 6: Summary File Generation
- **Create summary file:** `summary-[original-filename].md` in `call-recordings/` folder
  - Example: If transcript is `call-recordings/Mark-Brown-Adidas-bot-03-02-26-3dbbf513-4c3d.md`
  - Summary file will be: `call-summaries/summary-Mark-Brown-Adidas-bot-03-02-26-3dbbf513-4c3d.md`
- Include in summary file:
  - **Customer/Client:** Who the call was with (extract from transcript title or content)
  - **Date and Time:** When the call took place (if available in transcript)
  - **Call Overview:** One paragraph summary describing what the call was about
  - **Extracted Needs:** List of all needs extracted from the call
  - **Extracted Objections/Doubts:** List of all objections, concerns, or uncertainties extracted
- Link to source transcript file (relative path: `call-recordings/[filename].md`)
- Reference database entry IDs (NEED-XXX, OBJ-XXX) if entries were created/updated

### Step 7: Knowledge Base Updates Documentation
- Document which database entries were modified/created
- Record mapping results showing:
  - Which entries were updated (existing entries)
  - Which entries were created (new entries)
  - Frequency changes

---

## Output Format

### Summary File Format

**File Name:** `summary-[original-transcript-filename].md`
**File Location:** `call-summaries/` folder

**Required Content:**

1. **Header Section:**
   - **Customer/Client:** [Name of client/customer from transcript]
   - **Call Date:** [Date if available, format: YYYY-MM-DD]
   - **Call Time:** [Time if available]
   - **Source Transcript:** [Relative path: `call-recordings/[filename].md`]

2. **Call Overview:**
   - One paragraph summary describing:
     - What the call was about
     - Main topics discussed
     - Purpose of the conversation
     - Key context

3. **Extracted Needs:**
   - List all product-related needs extracted from the call
   - Format: Bulleted list with need statement, category, and source type
   - Include database entry ID if mapped (e.g., NEED-001)
   - **ALWAYS indicate source type:** [Client Direct Quote] or [Employee Statement with Client Confirmation]
   - Example:
     - "Multilingual avatar support" (Category: Multilingual Support, Source: Client Direct Quote) - NEED-003
     - "Avatar growth tracking" (Category: Customization, Source: Employee Statement with Client Confirmation) - NEED-005

4. **Extracted Objections/Doubts:**
   - List all objections, concerns, or uncertainties extracted
   - Format: Bulleted list with objection statement, type, and source type
   - Include database entry ID if mapped (e.g., OBJ-001)
   - **ALWAYS indicate source type:** [Client Direct Quote] or [Employee Statement with Client Confirmation]
   - Example:
     - "Uncertainty about avatar scalability" (Type: Technical Concern, Source: Client Direct Quote) - OBJ-002
     - "Concern about latency" (Type: Performance Concern, Source: Employee Statement with Client Confirmation) - OBJ-004

5. **Database Updates:**
   - Brief note on which database entries were created or updated
   - Reference to `/student-projects/[student-name]/genotype/call-database.md` for full details

**Example Summary File Structure:**
```markdown
# Summary: [Call Title]

**Customer/Client:** Mark Brown / Adidas Chatbot
**Call Date:** 2026-02-03
**Call Time:** 3:34 PM
**Source Transcript:** call-recordings/Mark-Smith-Project-03-02-26-3dbbf513-4c3d.md

## Call Overview

[One paragraph describing what the call was about, main topics, purpose, and key context]

## Extracted Needs

- [Need statement] (Category: [Category], Source: [Client Direct Quote | Employee Statement with Client Confirmation]) - NEED-XXX
- [Need statement] (Category: [Category], Source: [Client Direct Quote | Employee Statement with Client Confirmation]) - NEED-XXX

## Extracted Objections/Doubts

- [Objection statement] (Type: [Type], Source: [Client Direct Quote | Employee Statement with Client Confirmation]) - OBJ-XXX
- [Objection statement] (Type: [Type], Source: [Client Direct Quote | Employee Statement with Client Confirmation]) - OBJ-XXX

## Database Updates

[Brief note on database entries created/updated. See /student-projects/[student-name]/genotype/call-database.md for details.]
```

---

### Internal Processing Data (Not in Summary File)

**For internal tracking and database updates:**

1. **Call Metadata:**
   - Call date and time
   - Participants
   - Call duration
   - Source transcript file path

2. **Extracted Quotes:**
   - Array of quote objects (quote, speaker, context, interpretation)

3. **Extracted Needs (Full Details):**
   - Array of need objects (statement, category, reason, speaker, KB mapping)

4. **Extracted Objections (Full Details):**
   - Array of objection objects (quote, type, speaker, context, KB mapping)

5. **Knowledge Base Updates:**
   - New entries created in `/student-projects/[student-name]/genotype/call-database.md`
   - Existing entries updated (frequency increments, new sources added)
   - Mapping results (which entries matched, which are new)
   - Database entry IDs referenced (NEED-XXX, OBJ-XXX, QUOTE-XXX)

6. **Frequency Summary:**
   - Needs mentioned multiple times
   - Objections mentioned multiple times
   - Trend indicators

---

## Quality Criteria

**Completeness:**
- All important statements extracted (both client direct quotes and employee statements with client confirmation)
- No product-related needs missed
- All objections captured
- Source type identified for every quote, need, and objection

**Accuracy:**
- Quotes are exact (no paraphrasing)
- **Source type correctly identified** - speaker checked against employee list
- **Client confirmation accurately captured** for employee statements
- Context accurately reflects conversation flow
- Mapping to knowledge base is correct

**Consistency:**
- Same need/objection mapped to same database entry across calls
- Same database entry ID used for same need/objection
- Categorization is consistent
- Frequency counting is accurate
- Source references are complete and traceable
- **Source type consistently documented** in all database entries
- **Employee list checked** for every speaker identification

**Source Type Validation:**
- **CRITICAL:** Every quote, need, and objection MUST have source type identified
- Speaker names MUST be checked against employee list (`/student-projects/[student-name]/employees/employee-list.md`)
- Client direct quotes are MOST VALUABLE and should be prioritized
- Employee statements without client confirmation should NOT be extracted
- Employee statements with client confirmation are LESS VALUABLE but still needed

---

## Related Documents

- **Voice of Customer Database:** `/student-projects/[student-name]/genotype/call-database.md` - Central knowledge base for all extracted needs, objections, and quotes


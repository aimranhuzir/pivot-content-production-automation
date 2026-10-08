# Pivot AI Content Production Automation

An AI-assisted content production workflow designed to streamline the process of turning structured content ideas into ready-to-review social media content and visual production directions.

The workflow was designed for the **Pivot Project Malaysia** content process and implemented using **n8n, Google Sheets, and Google Gemini**.

## Project Overview

Content production can become repetitive and difficult to manage when ideas, copywriting, visual planning, review, and publishing preparation are handled across disconnected steps.

This project creates a structured workflow that connects these stages into a single automated process.

The workflow takes a structured content idea and automatically:

1. Assigns a unique Content ID
2. Stores the content plan in Google Sheets
3. Generates an AI-assisted social media post
4. Generates structured video direction
5. Generates structured image direction
6. Creates a human review record
7. Maintains a sequential content counter for future submissions

The result is a repeatable **content planning → AI generation → human review** pipeline.

---

## Business Problem

A content team may need to repeatedly perform several manual tasks for every new post:

* Record content ideas
* Maintain consistent content fields
* Create social media copy
* Develop visual concepts
* Prepare video direction
* Track content IDs
* Move content into review
* Maintain records for future publishing

Without a structured process, these steps can create unnecessary manual work and make content difficult to track.

This project explores how workflow automation and AI can reduce repetitive work while keeping **human review and approval** in the process.

---

## Solution

The workflow separates the content process into structured stages.

```text
Content Idea
     ↓
Structured Input
     ↓
Parse Content Data
     ↓
Generate Content ID
     ↓
Save Content Plan
     ↓
Update Content Counter
     ↓
AI Content Generation
     ↓
Format AI Output
     ↓
Human Review
     ↓
Publishing Preparation
```

The workflow uses Google Sheets as the operational data layer and n8n as the automation layer.

AI is used to assist with content generation, while the final content remains subject to human review.

---

## Workflow Architecture

### 1. Content Input

Content ideas are submitted as structured JSON containing fields such as:

* Topic
* Audience
* Key Message
* Supporting Points
* CTA
* Platform
* Content Pillar
* Status

The input is intentionally structured so that downstream automation receives predictable data.

### 2. Content ID Generation

Instead of scanning the entire content planning sheet to determine the next ID, the workflow maintains a dedicated counter in a `System Config` sheet.

For example:

```text
Next Content Number: 6
        ↓
Generated Content ID: PIV-006
        ↓
Next Content Number: 7
```

This provides a simple state-management mechanism for sequential content IDs.

### 3. Content Planning

The structured content plan is appended to the `Content Planning` sheet.

The planning record contains:

```text
Content ID
Topic
Audience
Key Message
Supporting Points
CTA
Platform
Content Pillar
Status
```

### 4. AI Content Generation

Google Gemini receives the structured content plan and generates three separate outputs:

* Social media post
* Video direction
* Image direction

The AI output is structured as JSON so that n8n can process each component programmatically.

### 5. Output Formatting

A JavaScript Code node transforms the structured AI response into fields that are easier for humans to review.

For example, video direction is converted into a readable format containing:

* Format
* Duration
* Mood
* Visual concept
* Shot list
* Visual elements
* Avoid list

Image direction is similarly formatted into structured production guidance.

### 6. Human Review

The generated outputs are written into the `Content Review` sheet.

Each record begins with:

```text
Review Status: Pending
```

A reviewer can then evaluate and edit:

* Social media copy
* Video direction
* Image direction
* Visual requirements
* Review notes

This keeps a human approval step between AI generation and publishing.

---

## Data Structure

The workflow uses four Google Sheets:

| Sheet              | Purpose                                                           |
| ------------------ | ----------------------------------------------------------------- |
| `Content Planning` | Stores structured content ideas and planning information          |
| `Content Review`   | Stores AI-generated content and human review fields               |
| `Publishing Queue` | Stores approved content prepared for publishing                   |
| `System Config`    | Maintains workflow configuration and sequential content numbering |

This separation keeps planning, generation/review, publishing, and system state logically separated.

---

## Technologies

* **n8n** — Workflow automation and orchestration
* **Google Sheets** — Structured operational data storage
* **Google Gemini** — AI-assisted content generation
* **JavaScript** — Data transformation and structured output formatting
* **JSON** — Data exchange between workflow stages
* **GitHub** — Documentation and version control

---

## Key Automation Concepts Demonstrated

This project demonstrates several practical automation and data-handling concepts:

### Workflow orchestration

Connecting multiple business process stages into a single automated workflow.

### Structured data processing

Using predictable JSON structures to move information between workflow stages.

### State management

Maintaining a sequential content counter independently from the main content table.

### Data transformation

Using JavaScript to parse and restructure AI-generated JSON into operational fields.

### AI integration

Using an LLM as part of a controlled business workflow rather than as a standalone chatbot.

### Human-in-the-loop design

Keeping human review between automated content generation and publishing.

### Process standardisation

Creating consistent fields, statuses, IDs, and review stages for every content item.

---

## Example Workflow Output

A successful workflow run produces a content record such as:

```text
Content ID: PIV-005
Topic: The Skills You Build Today Can Open Tomorrow's Opportunities
Platform: LinkedIn
Review Status: Pending
```

The corresponding review record contains:

```text
AI Social Post
AI Video Direction
AI Image Direction
Review Status
```

This allows the content team to move from a structured idea to a review-ready content package without manually performing each intermediate generation step.

---

## Testing

The workflow was tested using multiple content submissions.

Testing included:

* Sequential Content ID generation
* Content planning insertion
* Counter incrementing
* Preservation of original planning data
* AI content generation
* JSON parsing
* Video direction formatting
* Image direction formatting
* Content Review record creation
* End-to-end workflow execution

The workflow successfully processed test content through the planning, AI generation, and review stages.

---

## Design Considerations

The workflow was intentionally designed around a few principles:

**Structured inputs over free-form data**

Predictable fields make downstream automation more reliable.

**AI assistance rather than uncontrolled generation**

The AI receives defined content parameters and produces structured outputs.

**Human review before publishing**

AI-generated content is treated as a production draft rather than an automatically published final asset.

**Separate system state**

The content counter is maintained separately from the content table rather than repeatedly scanning existing records.

**Modular workflow stages**

Each stage has a defined responsibility, making the workflow easier to test, troubleshoot, and extend.

---

## Future Improvements

Potential future improvements include:

* Automated visual generation
* Automated publishing handoff
* Social media platform integrations
* Approval notifications
* Content performance tracking
* Automated content reporting
* Additional content channels
* Database-backed content management
* Automated content scheduling
* Improved error handling and retry logic

---

## My Contribution

I designed the workflow structure, defined the content data model and business process, implemented and tested the automation in n8n, and used AI-assisted development to help build and troubleshoot parts of the implementation.

The project was developed as a practical automation solution rather than as a purely theoretical demonstration.

---

## Project Status

**Working prototype / portfolio project**

The core workflow for structured content intake, content ID generation, planning storage, AI content generation, and human review has been implemented and tested.


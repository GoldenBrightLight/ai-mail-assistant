# AI Mail Assistant

**An AI-powered email management and response assistant built with n8n, Gmail, Google Gemini, and Telegram.**

AI Mail Assistant is a workflow-based automation project designed to reduce the manual effort involved in reading, analysing, and responding to emails.

The system uses AI to understand incoming messages, generate context-aware draft replies, and deliver them to Telegram for human review before sending.

Instead of automatically responding to every email, AI Mail Assistant follows a **human-in-the-loop approach**, keeping the user in control of outgoing communication.

## Overview

Managing emails involves several repetitive tasks: reviewing new messages, identifying their intent, deciding whether a response is necessary, and composing replies.

AI Mail Assistant automates parts of this process while preserving human oversight.

The project is implemented using two interconnected n8n workflows:

1. **Email Processing** — retrieves and analyses incoming emails, generates draft replies, and sends information to Telegram.
2. **Telegram Controller** — processes user actions, manages draft approval and editing, and handles the subsequent email workflow.

The application is currently an early-stage project intended for learning, experimentation, and further development.

## Key Features

### AI-Powered Email Analysis

Incoming emails are processed by Google Gemini to:

- Identify the sender's intent.
- Categorise messages.
- Assess their priority.
- Determine whether a reply is necessary.
- Generate a concise summary.
- Prepare a context-aware draft response.

### Human-in-the-Loop Approval

AI-generated replies are submitted to Telegram rather than being sent immediately.

The user can:

- **Approve** a proposed reply.
- **Edit** a draft before sending.
- **Reject** an unwanted response.

This design helps prevent unintended outgoing messages and gives the user final control over communication.

### Telegram-Based Management

Telegram acts as the primary interface for reviewing AI-generated drafts and interacting with the email automation.

The Telegram Controller handles user interactions and coordinates the relevant workflow actions.

### Email Record Management

The project uses n8n Data Tables to maintain records associated with draft replies and their processing status.

This enables the system to retrieve pending drafts and manage subsequent user actions.

## System Architecture

AI Mail Assistant consists of two main workflows.

### Workflow 1 — Email Processing

```text
Gmail Trigger
     |
Fetch Email
     |
Prepare AI Input
     |
Google Gemini
     |
Parse AI Response
     |
Reply Required?
     |
     +-- Yes --> Save Email Record
     |                |
     |                v
     |          Telegram Approval
     |
     +-- No --> Telegram Notification
```

**Responsibilities:**

- Detect incoming Gmail messages.
- Retrieve message content.
- Prepare structured input for the AI model.
- Analyse the email and generate a proposed response.
- Route the result according to whether a reply is required.
- Store pending drafts for subsequent processing.

### Workflow 2 — Telegram Controller

```text
Telegram Trigger
       |
Route Telegram Events
       |
Validate User / Action
       |
       +-- Approve --> Send Reply
       |
       +-- Reject  --> Reject Draft
       |
       +-- Edit    --> Update Draft
       |
       +-- Other   --> Handle Relevant State
```

**Responsibilities:**

- Receive Telegram commands and callback events.
- Check whether actions originate from an authorised user.
- Process approval, rejection, and editing requests.
- Retrieve and update stored email records.
- Coordinate the sending of approved responses.
- Provide feedback through Telegram.

## Screenshots

The repository includes screenshots of the n8n workflows in the `screenshots/` directory.

These illustrate the email-processing pipeline and Telegram-based response management.

## Technology Stack

| Technology | Purpose |
|---|---|
| **n8n** | Workflow automation and orchestration |
| **Gmail API** | Email retrieval and sending |
| **Google Gemini** | Natural language analysis and draft generation |
| **Telegram Bot API** | User interaction and approval |
| **JavaScript** | Data processing and workflow logic |
| **n8n Data Tables** | Persistent storage of email records and draft statuses |

## Getting Started

### Prerequisites

To run AI Mail Assistant, you will need:

- An n8n Cloud or self-hosted n8n instance.
- A Google account with Gmail access.
- Gmail credentials configured in n8n.
- Access to a supported Google Gemini model and its API credentials.
- A Telegram bot created using BotFather.
- A Telegram account authorised to control the bot.

### Installation

**1. Download the workflows**

Clone the repository or download its ZIP archive.

```bash
git clone https://github.com/GoldenBrightLight/ai-mail-assistant.git
```

**2. Import into n8n**

Open your n8n instance and import both workflow JSON files from the `workflows/` directory.

**3. Configure credentials**

Create and select your own credentials for:

- Gmail OAuth2
- Google Gemini
- Telegram Bot API

The repository does not provide reusable credentials.

**4. Configure database storage**

Create the required n8n Data Table using the fields referenced by the imported workflows.

Ensure that the relevant Data Table nodes point to your own table.

**5. Configure Telegram access**

Update the Telegram destination Chat ID and the authorised user checks to match your account.

**6. Review the workflow configuration**

Check node parameters, expressions, model availability, and the mappings between both workflows.

**7. Test before activation**

Use test emails to verify the approval, rejection, editing, and sending paths.

Do not enable automated processing of real correspondence until the workflows have been configured and tested successfully.

## Security Considerations

Email automation involves sensitive information and external services.

AI Mail Assistant is designed around several important principles:

**Human approval:** Generated drafts require user approval before being sent in the current workflow design.

**Authorised interactions:** Telegram actions should be restricted to explicitly authorised accounts.

**Credential separation:** API keys, authentication tokens, and private account identifiers must not be committed to the repository.

**Untrusted email content:** Incoming emails are treated as untrusted input. The AI prompt instructs the model not to follow instructions embedded in emails that attempt to alter its behaviour.

**Data privacy:** Email contents and drafts may be processed by third-party services. Users should consider applicable privacy requirements before connecting sensitive accounts.

**Operational safety:** The project is experimental and has not undergone an independent security audit.

Prompt instructions alone do not provide complete protection against prompt injection or other malicious input.

## Current Limitations

AI Mail Assistant is an early-stage implementation and has several limitations:

- It currently relies on Gmail for email integration.
- AI analysis can produce inaccurate classifications or draft content.
- The system is not yet designed to maintain persistent conversational memory across unrelated email threads.
- Advanced sender filtering and blocklist management are not yet implemented.
- Multi-user account management is not yet available.
- The workflows require manual configuration when imported into a different n8n instance.

These limitations will help guide future development.

## Roadmap

The following features are planned but are **not part of the current release**:

- [ ] JavaScript-based email pre-filtering to reduce unnecessary AI calls.
- [ ] Sender blocking and unblocking through Telegram.
- [ ] Telegram controls for viewing full email content.
- [ ] Email body cleaning to reduce unnecessary tokens.
- [ ] Improved AI classification for important notifications.
- [ ] Configurable levels of automation and approval.
- [ ] Improved error handling and monitoring.
- [ ] Support for additional email providers.
- [ ] Optional conversation history and contextual memory.

## Project Goals

This project was developed as a practical exploration of:

- Business process automation.
- AI integration with external APIs.
- Event-driven workflows.
- Backend-oriented data processing.
- Human-in-the-loop system design.
- Security and reliability in automation systems.

The long-term goal is to develop a more reliable, configurable, and cost-efficient email assistant while maintaining a modular architecture.

## License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.

## Disclaimer

AI Mail Assistant is an independent educational and experimental project.

It is not affiliated with Google, Telegram, or n8n. Users are responsible for configuring their own credentials, reviewing generated responses, and ensuring that their use of the system complies with relevant service terms and privacy requirements.

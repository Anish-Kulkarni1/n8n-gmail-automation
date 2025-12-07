# AI-powered Automated Gmail Follow-up System using n8n

## Abstract

This article presents an automated method of sending follow-up emails or messages on threads in Gmail that do not yield a response. In this workflow, TRACKING_LABEL is a label in Gmail that helps in tracking conversations, and the AI model determines whether a thread has been ghosted. When a thread is found to be ghosted, a follow-up message is sent within the thread. The application runs on a schedule once every four days.

## Overview of Operations
- A scheduled trigger starts the execution every four days.
- Gmail threads with the label TRACKING_LABEL are retrieved.
- Evaluates every thread retrieved with an AI model.
- The AI model identifies who sent the most recent message and labels it as one of two classes:

```
GHOSTED
NOT_GHOSTED
```

- If the classification is GHOSTED, then a follow-up reply is generated and posted within the same thread.
- TRACKING_LABEL is removed from the thread after processing.

## Core Processing Logic
- Processing is limited to Gmail threads containing TRACKING_LABEL.
- If the latest message in the thread was sent by the user and there was no response after it, the thread is marked as GHOSTED.
- If the recipient has replied, then the thread is marked as NOT_GHOSTED.
- Follow-up messages are issued only when the status is GHOSTED.
- The label is removed after a decision is made in order to avoid duplicate processing.

## Technological Framework
- Automation Platform: n8n
- Gmail API - Email Access
- Ghost-Detection Mechanism: Large Language Model
- Deployment: Docker

## Workflow Components
- schedule trigger
- Gmail Get Many Threads
- AI Classification of Messages
- Edit fields
- IF condition
- Gmail Reply to Thread
- Gmail Remove Label

## Naming Conventions
- TRACKING_LABEL needs to be applied manually within Gmail.
- Processing is only applied to threads carrying this label.
- The label is automatically removed after the decision outcome.

## AI Output Specification
- The AI output is a JSON object structured as follows:

```
"id": "19af93f43df51c55",
"ghosting_status": "GHOSTED",
```

The permissible values for ghosting_status are:

```
GHOSTED
NOT_GHOSTED
```

## Follow-up characteristics
- The follow-up reply is issued within the original Gmail thread so that the contextual continuity remains preserved.
- No new email threads are created.
- All the contextual information is preserved automatically. Security Considerations Gmail credentials should be kept secure within the n8n environment. The API keys for AI should be stored securely in n8n credential stores. Secrets should not be hard-coded into the workflow.

# Deployment Guide

## Quick Start (Under 1 Hour)

### Prerequisites
- n8n cloud account (free trial available at n8n.io)
- OpenAI API key (platform.openai.com)
- Google account (for Google Sheets)

### Step 1: Create the Google Sheet (5 minutes)
1. Go to sheets.google.com
2. Create a new spreadsheet named "AI Business Inquiries"
3. Add these headers in Row 1:
   - A: Timestamp
   - B: Category
   - C: Customer Name
   - D: Urgency
   - E: Summary
   - F: Draft Response
   - G: Original Message

### Step 2: Set Up n8n Workflow (30 minutes)
1. Create a new workflow in n8n
2. Add **Webhook** node (POST method)
3. Add **OpenAI - Message a Model** node
   - Connect OpenAI API credential
   - Model: GPT-4o-mini
   - System message: Use prompt from `prompts/system_prompt.py`
   - User message: `{{ $json.body.message }}`
4. Add **Google Sheets - Append Row** node
   - Connect Google Sheets credential
   - Select your spreadsheet and sheet
   - Map columns using expressions from `docs/workflow_config.md`

### Step 3: Test (5 minutes)
1. Click "Execute Workflow" in n8n
2. Send a POST request to the webhook test URL:
```json
{
  "message": "Hi, my name is Erik. I'd like to book a table for 4 people this Saturday at 7pm."
}
```
3. Verify data appears in Google Sheets

### Step 4: Connect to Your Website (15 minutes)
- Add a contact form that sends POST requests to the webhook production URL
- Or integrate with your CRM, email system, or chat platform

## Customization

### Changing Categories
Edit the system prompt in `prompts/system_prompt.py` to add/remove/modify categories:
- Default: BOOKING, PRICING, COMPLAINT, GENERAL
- Example additions: PARTNERSHIP, FEEDBACK, CANCELLATION, SUPPORT

### Changing Urgency Rules
Modify the urgency section in the system prompt to match your business priorities.

### Adding Email Notifications
Add a fourth node in n8n (Email or Slack) to notify your team when HIGH urgency inquiries arrive.

## Cost Estimation

| Volume | Monthly API Cost | n8n Plan |
|--------|-----------------|----------|
| 100 inquiries/month | ~$0.03 | Free tier |
| 500 inquiries/month | ~$0.15 | Free tier |
| 1,000 inquiries/month | ~$0.30 | Starter ($24/mo) |
| 5,000 inquiries/month | ~$1.50 | Pro ($60/mo) |

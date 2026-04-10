# Workflow Configuration Reference
# AI Business Inquiry Processor
# Author: Arjun Ponnaganti (arjunworks.se)

## Workflow Nodes

### Node 1: Webhook (Trigger)
- Type: Webhook
- HTTP Method: POST
- Purpose: Receives incoming customer inquiries from contact forms, CRMs, or direct API calls

### Node 2: OpenAI - Message a Model
- Model: GPT-4o-mini
- Resource: Text
- Operation: Message a Model
- Message 1 (System): See prompts/system_prompt.py
- Message 2 (User): {{ $json.body.message }}

### Node 3: Google Sheets - Append Row
- Operation: Append Row
- Document: AI Business Inquiries
- Sheet: Sheet1

## Column Mapping (Google Sheets Expressions)

| Column           | Expression                                                    |
|------------------|---------------------------------------------------------------|
| Timestamp        | {{ $now.toISO() }}                                            |
| Category         | {{ JSON.parse($json.output[0].content[0].text).category }}    |
| Customer Name    | {{ JSON.parse($json.output[0].content[0].text).customer_name }}|
| Urgency          | {{ JSON.parse($json.output[0].content[0].text).urgency }}     |
| Summary          | {{ JSON.parse($json.output[0].content[0].text).summary }}     |
| Draft Response   | {{ JSON.parse($json.output[0].content[0].text).draft_response }}|
| Original Message | {{ $('Webhook').item.json.body.message }}                     |

## API Cost Estimation

- Model: GPT-4o-mini
- Average tokens per request: ~300 input + ~150 output = ~450 total
- Cost per request: ~$0.0003
- 1000 inquiries/month: ~$0.30/month
- Extremely cost-effective for any business size

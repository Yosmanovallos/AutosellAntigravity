# PROJECT CONTEXT — Read this before any action

## What we're building
An automated dropshipping system for Medellín, Colombia. A customer sees an ad,
sends a WhatsApp message, an AI bot handles them, takes the order, charges via Wompi,
automatically contacts the real supplier, generates a shipping label, and notifies the customer.

The user (me) holds NO inventory. I'm a pure digital intermediary.

## My profile
- I code Python and JavaScript at intermediate level
- I'm in Medellín, Colombia
- System: Windows 11 + WSL2 Ubuntu 22.04
- Operating budget: $20 USD/month (Claude Pro) + $5-10 USD/month future (VPS)
- Ad budget: $100-200 USD/month once the system works

## Decided tech stack (do NOT debate unless I ask)
- n8n self-hosted with Docker Compose (workflow orchestrator)
- PostgreSQL (database)
- Caddy (reverse proxy with automatic HTTPS)
- WhatsApp Cloud API official from Meta (channel)
- Wompi (Colombia payment gateway)
- Claude Haiku 4.5 (conversational brain, via API)
- Gemini 2.5 Flash-Lite (cheap classification)
- ngrok (to expose webhooks during local development)

## Project phases (7 sequential phases)
0. Environment preparation (Docker, WSL, Git, external accounts)
1. n8n self-hosted + PostgreSQL running with HTTPS
2. WhatsApp Cloud API connected (functional webhook)
3. Conversational agent with Claude (no payment yet)
4. Product catalog + supplier logic
5. Wompi payment gateway (payment link + confirmation webhook)
6. Supplier orchestrator (automatic contact + fallback)
7. Shipping carrier + automatic tracking

## OPERATING RULES — Critical, never violate them

1. **ONE PHASE AT A TIME**: never advance to the next phase without my explicit permission.
   When you finish a phase, stop and tell me: "Phase X completed. Should I verify with you before continuing?"

2. **MINIMUM TOKEN CONSUMPTION**:
   - DO NOT read files you don't need for the current task
   - DO NOT reread the same file twice in the same session
   - Use `grep` or `glob` before reading entire files
   - If a file is large, read only the relevant line range
   - DO NOT explain things to me that are already in this PROJECT.md
   - Be concise in responses; code first, short explanation after

3. **FREQUENT COMMITS**: after each significant step within a phase, commit:
   `git add . && git commit -m "phase-X: short description"`
   This protects progress if something fails.

4. **NEVER HARDCODE SECRETS**: 
   - All API keys go in `infra/.env` (NOT committed, it's in .gitignore)
   - If you need a secret value, ask me, DO NOT make it up

5. **VALIDATE BEFORE ADVANCING**: each phase has a verification checklist at the end.
   - Execute each checklist item
   - Show me the output
   - Wait for my confirmation before moving to the next phase

6. **IF SOMETHING FAILS, STOP**: don't try to "fix it and continue". Stop, tell me exactly
   what failed, what you tried, and wait for my input. Better 5 minutes lost than 2 hours
   on the wrong path.

7. **QUESTION MODE**: when you have real doubts (architectural decision, value you don't
   know, missing credential), ask. DO NOT assume. A short question saves hours later.

8. **MINIMUM DOCUMENTATION**: when finishing each phase, add or update `docs/phase-X-name.md`
   with: what was done, key commands used, problems encountered and solutions.
   DO NOT write long docs — short bullets.

9. **DO NOT EXECUTE ANYTHING DESTRUCTIVE** without confirming: `rm -rf`, `docker volume rm`,
   `DROP TABLE`, etc. Ask first.

10. **AUTOMATIC CHECKPOINT**: at the start of each new session, read `docs/STATUS.md`
    (if it exists) to know where we left off. If it doesn't exist, create it in Phase 0.

## Folder structure we'll respect



intercomunicador-medellin/
├── PROJECT.md               # This file
├── README.md                # Public project status
├── docs/
│   ├── STATUS.md            # ⭐ Where we are — ALWAYS keep updated
│   ├── phase-0-setup.md
│   ├── phase-1-n8n.md
│   └── ...
├── infra/
│   ├── docker-compose.yml
│   ├── .env                 # NOT in git, secrets
│   ├── .env.example         # YES in git, template
│   └── caddy/Caddyfile
├── n8n/workflows/           # Workflow JSON exports
├── scripts/                 # Auxiliary scripts (backup, deploy)
├── catalog/                 # Product and supplier JSONs
└── .gitignore


## Expected response format
- Confirm which phase you're executing
- List the steps you'll take (3-7 items max)
- Execute step by step, showing result of each
- If something isn't obvious to me, explain in 1-2 lines
- At the end: validation checklist + question "Should I advance to the next?"

## What you should NOT do
- DO NOT install global packages without warning me
- DO NOT modify system files (~/.bashrc, /etc/, etc.) without warning me
- DO NOT create junk files "just in case"
- DO NOT use weird or very new libraries — prefer mainstream and stable
- DO NOT write unit tests unless I ask (after Phase 7, yes)
- DO NOT refactor code that already works
- DO NOT give me multiple options when there's already a technical decision above

Ready. When you finish reading this, respond only with:
"PROJECT.md read. Ready for Phase 0. Shall we start?"


🎯 PER-PHASE PROMPTS
Use these prompts one at a time, in order. After Claude Code confirms a phase is done and you've validated everything works, paste the next one.
Phase 0 Prompt — Environment preparation
Execute Phase 0: Environment preparation.

I already have installed:
- WSL2 Ubuntu 22.04
- Docker Desktop with WSL integration enabled
- VS Code with WSL extension
- Git configured
- Node 22 via nvm
- Python 3 base

What YOU must do:
1. Verify all the above tools work (run verification commands and show me the
   output of each)
2. Create the project folder structure according to PROJECT.md
3. Create .gitignore with correct exclusions (secrets, data, logs, etc.)
4. Create initial README.md with phase checklist
5. Create docs/STATUS.md with current project state
6. Initialize git and make first commit
7. Create private GitHub repo using `gh repo create` and push it

You should NOT:
- Install Docker, Node, or any of that (already done)
- Touch my WhatsApp or Meta (I do that manually, I'll let you know when ready)
- Ask me for API keys yet (we use them in Phase 2)

At the end, give me the Phase 0 validation checklist with each item marked and
show me what's in docs/STATUS.md.

Stop there.
Phase 1 Prompt — n8n self-hosted with Docker
Execute Phase 1: n8n self-hosted + PostgreSQL + Caddy.

Goal: n8n running at http://localhost:5678 with PostgreSQL as DB,
accessible via local HTTPS with a fake domain like n8n.local using Caddy
with internal certificates.

Expected steps:
1. Read docs/STATUS.md to confirm where we left off
2. Create infra/docker-compose.yml with services: n8n, postgres, caddy
3. Create infra/.env.example (template WITHOUT real values)
4. Create infra/.env with real values (ask me for secure passwords if you don't
   generate them yourself)
5. Create infra/caddy/Caddyfile for reverse proxy with internal TLS
6. Start services with `docker compose up -d`
7. Verify logs of each container
8. Confirm n8n responds
9. MINIMAL guide in docs/phase-1-n8n.md (10 lines maximum)
10. Commit and push

n8n configuration:
- Language: Spanish
- Timezone: America/Bogota
- DB: PostgreSQL 16 alpine
- Persistence: Docker volume, NOT bind mount to host
- Encryption key: ask me for one or generate with `openssl rand -hex 32`
- Basic Auth: user `admin`, strong password you generate
- Webhook URL: for now http://localhost:5678 (in Phase 2 we change it to ngrok)

When done:
- Show me the output of `docker compose ps`
- Show me that I can open http://localhost:5678 (screenshots not needed, but
  confirm the login screen appears)
- Update docs/STATUS.md
- Commit with message "phase-1: n8n stack running"

Stop.
Phase 2 Prompt — WhatsApp Cloud API
Execute Phase 2: Connect WhatsApp Cloud API.

Prerequisite that I must have done (verify by asking me before starting):
- Meta Business Manager account created
- WhatsApp Business app created in Meta for Developers
- Dedicated phone number added and verified
- Temporary access token (24 hours) generated
- Phone Number ID copied
- Business Account ID copied

If I HAVE NOT done the above, stop and give me step-by-step guidance on what to
do in developers.facebook.com (with exact current menu names — search web if you
need) so that I do it. DO NOT try to do it yourself.

When I confirm I have the tokens:
1. Add variables to infra/.env (META_WHATSAPP_TOKEN, META_PHONE_NUMBER_ID, etc.)
2. Install ngrok in WSL if not present: `curl ... ngrok`
3. Create ngrok account if I don't have one (free) and configure authtoken
4. Run ngrok pointing to localhost:5678 (n8n)
5. Copy the ngrok public HTTPS URL
6. Create basic workflow in n8n: webhook trigger → respond
   - You create the workflow via n8n API (don't expect me to do it in manual UI)
   - Or give me the exported JSON and I import it
7. Register the webhook URL in Meta app as WhatsApp webhook
8. Configure verify token (secure random string)
9. Subscribe to events: messages
10. Make me send a test message from MY personal WhatsApp TO the WhatsApp test
    number (not the other way yet — Meta restricts outgoing sending without
    approved templates)
11. Verify the webhook received the event (n8n logs)

When done:
- Workflow "whatsapp-webhook-receiver.json" exported to n8n/workflows/
- Diagram in docs/phase-2-whatsapp.md of how an incoming message flows
- The 24h token has expiration — at the end give me instructions to generate
  permanent System User Token
- Commit and push

Stop.
Phase 3 Prompt — Conversational agent with Claude
Execute Phase 3: Conversational agent with Claude.

Goal: when a WhatsApp message arrives, n8n calls Claude Haiku 4.5,
Claude responds, and n8n sends the response back to the customer via WhatsApp.

No payment, no real products yet — just conversation.

Steps:
1. Verify Phase 2 still works (active webhook, ngrok running)
2. Add ANTHROPIC_API_KEY to infra/.env
3. Design the conversational agent's system prompt. Characteristics:
   - Tone: friendly, Colombian, NOT formal, maximum 2 emojis
   - Goal: identify what product they want, color/size/variant, take data
     (name, city, address, alternative phone)
   - If they ask about something outside catalog, says they'll verify and lets
     a human follow up
   - When they have ALL the data, respond with structured JSON at the end that
     n8n can parse:
 [ORDER_CONFIRMED]
 {"product":"...", "variant":"...", "name":"...", "city":"...", 
  "address":"...", "phone":"..."}
 [/ORDER_CONFIRMED]
4. Save the system prompt in n8n/prompts/sales-agent.md (plain text, editable
   later)
5. Modify Phase 2 workflow to:
   a. Receive incoming message
   b. Look up conversation history in PostgreSQL by customer number
   c. Call Claude API with system prompt + history + new message
   d. Save response to history
   e. Send response to customer via WhatsApp Cloud API
   f. If detects [ORDER_CONFIRMED], save order in `orders` table 
      (status: "pending_payment")
6. Create `conversations` table (id, phone, messages_json, created_at) and
   `orders` table (id, phone, data_json, status, created_at) in Postgres
7. Export updated workflow to n8n/workflows/03-conversational-agent.json
8. Test with me: I send "Hi, I saw your headphones ad" from MY WhatsApp to the
   test number, and the bot must respond coherently

IMPORTANT for saving Claude API tokens:
- Limit history to last 20 messages
- Use Claude Haiku 4.5 (model: claude-haiku-4-5), NOT Sonnet or Opus
- Max output tokens: 300
- Caching: if Anthropic offers prompt caching for the system prompt, use it

When done:
- Show logs of a successful back-and-forth conversation
- docs/phase-3-agent.md with the flow and system prompt
- Commit "phase-3: Claude Haiku conversational agent"
- Stop
Phase 4 Prompt — Catalog and product logic
Execute Phase 4: Product catalog + suppliers.

Goal: the agent no longer makes up prices — consults a real DB of products
with their suppliers, costs, suggested prices, margins, and priority.

Steps:
1. Design SQL schema:
   - `products` table: id, sku, name, description, sale_price_cop, category, active
   - `variants` table: id, product_id, name (ex "Black", "Size M"),
     available_stock, variant_sku
   - `suppliers` table: id, name, whatsapp, phone, city, reliability_score (1-5), notes
   - `product_supplier` table: product_id, supplier_id, cost_price_cop,
     priority (1=first contact), average_response_time_min
2. Create schema in PostgreSQL
3. Create catalog/products-seed.json with 3 example products:
   - "XYZ Bluetooth headphones" - 3 color variants - 2 suppliers
   - "USB mini fan" - 2 color variants - 1 supplier
   - "LED desk lamp" - 1 variant - 2 suppliers
4. Node.js or Python script in scripts/seed-catalog.js that loads that JSON to DB
5. Modify conversational agent (Phase 3) so that:
   a. At start of conversation, load catalog from DB
   b. Pass catalog as context to Claude (short list, not giant JSON)
   c. If customer asks about something in catalog, Claude quotes with real price
   d. If customer asks about something outside, Claude says "let me check"
6. Create simple admin endpoint to view/edit catalog:
   - n8n workflow with webhook GET /admin/products → list
   - n8n workflow with webhook POST /admin/products → create/edit
   - Protected with header `X-Admin-Token` that you put in .env
7. Tests:
   - I ask about "headphones" → bot must mention catalog ones
   - I ask about "wedding dress" → bot says it doesn't handle that product
   - I confirm order → must save in orders table with real product_id

DO NOT do:
- Don't build admin UI with HTML/React (only API endpoints)
- Don't load product photos yet (later phase)
- Don't connect with real suppliers yet (Phase 6)

When done:
- docs/phase-4-catalog.md with schema and examples
- catalog/products-seed.json and catalog/suppliers-seed.json in git
- Commit "phase-4: catalog and suppliers in DB"
- Stop
Phase 5 Prompt — Wompi payment gateway
Execute Phase 5: Wompi payment gateway.

Prerequisite: I confirm to you that I have Wompi Sandbox keys
(pub_test_xxx and prv_test_xxx) and events secret.

If I don't have them, give me exact guidance to get them from comercios.wompi.co.

Goal: when the agent confirms an order, generate Wompi payment link, send it to
the customer, and when the customer pays, n8n receives webhook and changes
order status to "paid".

Steps:
1. Add WOMPI_PUBLIC_KEY, WOMPI_PRIVATE_KEY, WOMPI_EVENTS_SECRET to infra/.env
2. New workflow: "generate-payment-link"
   - Trigger: when order changes to "awaiting_payment_link"
   - Calls Wompi API: POST https://sandbox.wompi.co/v1/payment_links
   - Body: amount_in_cents, currency=COP, name, description, single_use=true,
     redirect_url, expires_at (24h)
   - Save link in orders table (column: payment_link_url)
   - Send link to customer via WhatsApp with friendly message
3. New workflow: "wompi-webhook"
   - Public endpoint (via ngrok): /webhook/wompi
   - Validate HMAC SHA256 signature with events secret — CRITICAL, without this
     anyone could fake payments
   - If event=transaction.updated and status=APPROVED:
     - Look up order by reference
     - Change status to "paid"
     - Trigger next workflow (Phase 6: contact supplier)
   - If status=DECLINED or VOIDED:
     - Change status to "payment_failed"
     - Send WhatsApp to customer: "Your payment didn't process, want to try again?"
4. Modify conversational agent (Phase 3) so when it detects [ORDER_CONFIRMED],
   it TRIGGERS the "generate-payment-link" workflow (don't send link itself)
5. Configure webhook URL in Wompi Sandbox dashboard
6. Tests:
   - I confirm an order via WhatsApp
   - Bot sends payment link
   - I open link, pay with Wompi test card (4242 4242 4242 4242 or whichever
     they provide)
   - Webhook arrives, n8n processes, order changes to "paid"
   - I receive WhatsApp confirmation

IMPORTANT:
- EVERYTHING in sandbox, NEVER test with production keys until we migrate to VPS
- Mandatory HMAC validation — don't skip it "to test faster"

When done:
- Logs of a successful end-to-end test payment
- docs/phase-5-wompi.md
- Commit "phase-5: Wompi sandbox gateway functional"
- Stop
Phase 6 Prompt — Supplier orchestrator
Execute Phase 6: Automatically contact suppliers with fallback.

Goal: when an order changes to "paid", the system contacts the priority 1
supplier via WhatsApp. If they don't respond in 15 minutes, escalates to
priority 2 supplier. And so on.

Important: suppliers are real humans (El Hueco warehouses, Instagram sellers).
They need to be able to respond to the bot in natural language.

Steps:
1. Create `supplier_attempts` table: id, order_id, supplier_id, attempt_num,
   sent_at, responded_at, status (sent/accepted/rejected/no_response),
   message_sent, message_received
2. Workflow "contact-priority-supplier":
   - Trigger: order changes to "paid"
   - Look up priority 1 supplier for that product
   - Generate message with Claude Haiku 4.5: friendly tone, complete order data
     (what, when, address, contact), clear question "do you have it available
     for shipping today?"
   - Send via WhatsApp Cloud API to supplier
   - Create record in supplier_attempts
   - Schedule escalation job in 15 minutes
3. Workflow "process-supplier-response":
   - Trigger: incoming message from a number that's in suppliers table
   - Call Claude Haiku 4.5 to classify: ACCEPTS / REJECTS / ASKS / OTHER
   - If ACCEPTS: change order to "preparing_shipment", notify customer
   - If REJECTS: cancel this attempt, trigger escalation to next supplier
   - If ASKS: notify admin (me) to respond manually
   - If OTHER: notify admin
4. Workflow "escalate-supplier":
   - Scheduled 15 min job
   - If attempt remains without response, mark as "no_response"
   - Look up next supplier by priority
   - If there's a next one, trigger "contact-priority-supplier" with them
   - If NO more next:
     - Change order to "no_supplier_available"
     - Notify admin URGENT
     - Notify customer with apology + refund option
5. Admin endpoint: see pending orders and be able to intervene manually
6. Tests:
   - Create test order
   - Mark as paid (manual in DB for this test)
   - Verify YOUR WhatsApp (acting as test supplier) receives the message
   - Respond "yes I have it" — verify it classifies as ACCEPTS and notifies customer
   - Another order: DON'T respond, wait 15 min, verify it escalates to supplier 2

When done:
- docs/phase-6-orchestrator.md with state diagram
- Commit "phase-6: orchestrator with supplier fallback"
- Stop
Phase 7 Prompt — Shipping carrier and tracking
Execute Phase 7: Generate automatic shipping labels and tracking.

Goal: when a supplier accepts an order, the system automatically generates
the shipping label with the selected carrier.

For Colombia/Medellín, options in priority order:
1. InterRapidísimo (has REST API available for business clients)
2. Servientrega (restricted API, possible via intermediaries like Mensajeros Urbanos)
3. Coordinadora (similar to Servientrega)
4. Manual fallback: bot sends message to admin with all data and admin generates
   label manually on carrier website

IMPORTANT: if I DON'T have business account with API on any carrier, implement
Plan B (assisted manual) and leave Plan A documented for future.

Plan B (assisted manual):
1. When supplier accepts, system assembles pre-formatted message with ALL data
   in copy-paste format for carrier website
2. Send to admin (me) via WhatsApp with all data
3. I generate label on web manually
4. I respond to the bot with the label (photo or label number)
5. Bot saves it and sends it to customer automatically

Workflows:
1. "generate-shipping-instruction": triggers when supplier accepts, sends to admin
2. "register-label": endpoint where I respond with the label, bot saves it
3. "notify-customer-shipment": when label registered, message to customer with number
4. "periodic-tracking" (scheduled every 6h): if carrier has public tracking API
   (InterRapidísimo does, no auth needed for basic queries), query status and
   notify customer if there are important changes

Steps:
1. Add tables: `labels` (id, order_id, carrier, label_number, tracking_url,
   shipping_date, estimated_delivery_date)
2. Implement Plan B completely (workflows 1-3)
3. Implement basic tracking (workflow 4) querying public API if it exists
4. Admin endpoint: see all orders in "preparing_shipment" status without label
5. COMPLETE end-to-end tests:
   a. I (customer) → "I want the black headphones"
   b. Bot takes data, generates Wompi link
   c. I pay with sandbox
   d. Bot contacts supplier (I'm test supplier)
   e. I (supplier) → "Yes I have it"
   f. Bot sends instruction to admin (me)
   g. I (admin) → respond with label number
   h. Bot sends label to customer
   i. Order remains in "shipped" status
6. Create minimal dashboard (can be n8n workflow returning simple HTML) with
   statistics: orders today, in process, completed, revenue

When done:
- docs/phase-7-shipments.md
- docs/USAGE-MANUAL.md with guide on how I operate the system day to day
- README.md updated with final usage instructions
- Final commit "phase-7: complete end-to-end system"
- Stop

🎉 On completing this, you have the functional system. Optional next steps
(another conversation): VPS migration, pretty dashboard, ads management.

🪙 Token-saving tips for Claude Code
These are the habits that save the most tokens in your daily usage:
1. One session per phase, not one session for everything
Close Claude Code when finishing each phase and open a new session for the next. This resets context. PROJECT.md + STATUS.md file give it enough continuity.
2. Use "plan" mode when unsure
Before executing a large phase, tell it: "First make me a plan of what you'll do in Phase X, without executing anything yet. I approve it and you start." This prevents bad execution and tokens spent on corrections.
3. Short commands for checkpoints
Create these mental shortcuts:

"Status summary" → Claude reads STATUS.md and tells you where you are
"Commit and push" → makes commit with message based on changes
"Stop" → halts immediately

4. DO NOT use Claude Code for tasks that don't require code
Creating accounts on Meta, Wompi, buying SIM, etc.: you do that in the browser. Don't ask Claude Code to guide you in the browser — use the Claude web chat (this one) for that, which does NOT consume Claude Code quota.
5. Limit context with --max-turns or close long sessions
If a session already has 50+ exchanges, close it. Start fresh. Tell it: "Let's continue from STATUS.md, I already completed Phase X."
6. When Claude Code asks you something already in PROJECT.md, remind it
Short response: "That's in PROJECT.md, check it." Saves it having to debate everything again.
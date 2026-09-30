# Luis Garate

Tech lead at a Latin American credit fintech. I design and ship Laravel systems for credit operations and payments: card tokenization, collections, disbursements, and portfolio assignment to creditors.

I lead a squad of four backend engineers. Controllers stay thin, business rules live in services, and batch jobs are idempotent. I work in Colombia (UTC-5) and overlap with US Eastern hours.

Day-to-day work lives in a private company organization under [@LuisGarateCM](https://github.com/LuisGarateCM), with 1,300+ commits in the last year. The repositories below are written from scratch as public samples of the same problems.

## Public code

| Repository | What it shows |
|---|---|
| [php-payment-application-engine](https://github.com/ingluisgarate/php-payment-application-engine) | A payment becomes installment credits in a configurable order, then settles to whoever holds the loan. Strategy per creditor: originator or endorsed creditor with a servicing fee. Includes the [entity relationship model](https://github.com/ingluisgarate/php-payment-application-engine/blob/main/docs/data-model.md) for creditors, endorsements, payments, and settlement batches. |
| [php-cash-network-integrations](https://github.com/ingluisgarate/php-cash-network-integrations) | One cash-in contract, two transports: a SOAP bank correspondent with WS-Security and XPath parsing, and a REST cash network with HMAC-signed headers and idempotency keys. Retries only on transport failures. Reconciles the provider's settlement file. |
| [laravel-payments-demo](https://github.com/ingluisgarate/laravel-payments-demo) | Gateway contract, HMAC-signed webhook, and a ledger that applies each event once. A later refusal does not downgrade an approved payment. |
| [fastapi-payment-intents](https://github.com/ingluisgarate/fastapi-payment-intents) | FastAPI service with `Idempotency-Key` on creation, signed provider webhooks, and the same "approved stays approved" rule. |
| [python-etl-portfolio-report](https://github.com/ingluisgarate/python-etl-portfolio-report) | Extract, transform, load with the standard library: bad rows are rejected with a reason, duplicate payments are dropped, balances are upserted into SQLite, and an aging report by creditor is written as CSV. |
| [vue-checkout-demo](https://github.com/ingluisgarate/vue-checkout-demo) | Vue 3 and Pinia card form against a fake tokenizer. An approved card stores a token and clears the number and CVV. A decline or a holder mismatch stores nothing. |

## Selected work

### Card tokenization

Several origination channels were each interpreting the payment gateway response in their own way. A refused charge saved a short status and dropped the acquirer payload, so operations could not see why the card failed. I defined one persistence contract: store the gateway answer on failure, and never overwrite a card that was already approved. The same rule now applies across in-store origination, ecommerce, and a revolving credit line, with unit tests for the approved path, the refused path, and the case that must not replace a paid card.

### Payment methods and bank integrations in LATAM

I have integrated card gateways, cash networks, and bank correspondents directly, without a third-party package: SOAP web services with WS-Security, REST APIs with signed requests, webhooks, and settlement file reconciliation, in Colombia and Peru. The hard part is not the happy path. It is a retry, a webhook that arrives twice, and a payload that does not match the documented contract.

### Collections and payment application

Payment application on a live loan book. A payment must land on the right loan and the right installment, a duplicate notification must not apply twice, and large batches have to finish without loading the whole table into memory. The allocation order is a policy, not a constant, so a new product does not need a new engine.

### Creditors and endorsed portfolios

Loans are assigned to creditors from operational files, in batches, with a record of which file changed which loan. From the effective date of an endorsement, every payment on that loan belongs to the creditor: it is settled to the creditor's account and reported in the creditor's remittance. I designed the data model and the strategy that routes each collection to the right creditor.

### Reports

Aging by creditor, remittance files, regulatory and tax exports. Built as pipelines that reject bad rows with a reason instead of failing the whole run, and that can be re-run for the same date without duplicating data.

### Disbursements

Outbound payments to customers and partners, with a strategy per destination and a clear record of what was sent and what the provider answered.

### Technical leadership

Before a change spreads across channels, I set the rule and the test cases. Engineers then implement it on their own flow instead of inventing a new variant. Reviews focus on business behavior, queries, and what happens when the external provider fails.

## Stack

PHP 8, Laravel, Python (FastAPI, ETL), REST and SOAP APIs, Vue.js, MySQL, MongoDB, Redis, Git, Docker, Linux. Daily use of Cursor and Claude, with tests and a human review of the diff before anything is merged.

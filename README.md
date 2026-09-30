# Luis Garate

Tech lead at a Latin American credit fintech. I design and ship Laravel systems for credit operations and payments: card tokenization, collections, disbursements, and portfolio assignment.

I lead a squad of four backend engineers. Controllers stay thin, business rules live in services, and batch jobs are idempotent. I work in Colombia (UTC-5) and overlap with US Eastern hours.

## Selected work

### Card tokenization

Several origination channels were each interpreting the payment gateway response in their own way. A refused charge saved a short status and dropped the acquirer payload, so operations could not see why the card failed. I defined one persistence contract: store the gateway answer on failure, and never overwrite a card that was already approved. The same rule now applies across in-store origination, ecommerce, and a revolving credit line, with unit tests for the approved path, the refused path, and the case that must not replace a paid card.

### Payment methods in LATAM

I have integrated card and cash gateways directly, without a third-party Laravel package: checkout, webhooks, signature checks, and reconciliation. The hard part is not the happy path. It is a retry, a webhook that arrives twice, and a payload that does not match the documented contract.

### Collections

Payment application on a live loan book. A payment must land on the right loan, a duplicate notification must not apply twice, and large batches have to finish without loading the whole table into memory.

### Disbursements

Outbound payments to customers and partners, with a strategy per destination and a clear record of what was sent and what the provider answered.

### Portfolio assignment

Bulk assignment of loans to creditors from operational files, with a batch record and a log of what changed. The design goal is traceability: which file, which batch, which loan.

### Technical leadership

Before a change spreads across channels, I set the rule and the test cases. Engineers then implement it on their own flow instead of inventing a new variant. Reviews focus on business behavior, queries, and what happens when the external provider fails.

## Stack

PHP 8, Laravel, REST APIs, Vue.js, MySQL, MongoDB, Redis, Git, Docker, Linux. Daily use of Cursor and Claude, with tests and a human review of the diff before anything is merged.

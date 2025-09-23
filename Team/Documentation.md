
# Developer Action Plan (Expanded: Purchase + Refund + Void)

## Goals

Build a console‑based **Local Companion App** for Ingenico Desk terminals and wire it to Dynamics 365 Customer Service via **Power Automate**. Support **Purchase**, **Refund**, and **Void**.

## Companion App – Technical Scope

**Endpoints** - POST /payment → { amount, refId } - POST /refund → { originalReference, amount, refId } - POST /void → { originalReference, refId }

**Responsibilities** - Maintain a **TCP** session per request to the terminal (Ethernet). - Build SI messages for **00 Purchase**, **03 Refund**, **05 Void**; parse responses. - Enforce **idempotency** with refId (ignore duplicate calls). - Log request/response summaries; never log PAN. Persist masked card and references only.

**JSON Response (all endpoints)**

{  
"status": "APPROVED|DECLINED|ERROR|TIMEOUT",  
"authCode": "A12345",  
"maskedCard": "************1234",  
"approvedAt": "2025-09-17T14:21:33Z",  
"reference": "TID-XYZ-001",  
"message": "Optional human-readable info"  
}

## Power Automate – Flows & Buttons

- **Process Payment** (Purchase): validate amount > 0 → call /payment → update Dataverse.
    
- **Refund**: ensure original txn is **settled & refundable**; optional partial amount policy → call /refund → write refund record and link to original.
    
- **Void**: ensure **same-day voidable** state → call /void → mark original as **Voided** and store void reference.
    

## Data Model (Dataverse)

- **Payment Transaction** table:
    

- Amount, Status, Auth Code, Masked Card, Approved At, Terminal Reference, External RefId.
    
- Relationships: **Refund ←→ Original Transaction**, **Void ←→ Original Transaction**.
    
- Flags: IsRefund, IsVoid, IsOriginal, OriginalTxnId.
    

## Validation Rules (in Flow)

- **Refund**: require original status = APPROVED, amount ≤ original amount, not already fully refunded.
    
- **Void**: require original status = APPROVED, within same-day window; not previously voided.
    
- Common: block when status is IN_PROGRESS.
    

## Testing Matrix (POC)

- Purchase: approve, decline, timeout.
    
- Refund: approve, decline (exceed amount), invalid reference.
    
- Void: approve, invalid window, already voided.
    

## Risks & Mitigations

- **Sandbox behavior**: align with processor on refund/void capabilities → prepare test cards.
    
- **Reference tracking**: persist terminal reference & acquirer data for traceability.
    
- **Edge cases**: minimal graceful errors now; expand in hardening sprint.
# POC Scope: Local Companion App (Expanded)

## Objective

Deliver a live, card‑present payment demo from Dynamics 365 Customer Service to an Ingenico Desk terminal, now covering **Purchase (00)**, **Refund (03)**, and **Void (05)**.

## Scope & Features (Lab POC)

- **Transactions**: Purchase, Refund (against prior auth/capture), and Void (same‑day reversal/cancel of prior txn).
    
- **Transport**: Ethernet TCP to terminal (skip Serial/USB for POC).
    
- **Receipts**: Printed by terminal (no CSA/ECR receipt relay in POC).
    
- **Companion App**: Console EXE exposing local HTTP endpoints:
    

- POST /payment → purchase
    
- POST /refund → refund by reference/amount
    
- POST /void → void by reference
    

- **Config**: Terminal IP/port in appsettings.json.
    
- **Security (POC)**: HTTP on localhost/VLAN; protect tunnel URL (if used) with bearer token.
    
- **CSA UX**: Buttons in command bar (Power Automate): **Process Payment**, **Refund**, **Void**.
    

## Pre‑requisites

- Terminal in **Semi‑Integrated** mode; reachable via LAN.
    
- Test cards & sandbox profile supporting approval/decline for **purchase, refund, void**.
    
- Selected **connectivity** to workstation (tunnel or on‑premises data gateway + custom connector).
    
- Dataverse table for Payment Transaction with fields for status, auth code, masked card, timestamps, and original reference.
    

## End‑to‑End Flow (Purchase)

1. Agent clicks **Process Payment** in CSA.
    
2. Instant Cloud Flow triggers with recordId, amount.
    
3. Flow **POST** → Companion App /payment.
    
4. Companion App → terminal (SI API) → cardholder taps/inserts → approval.
    
5. Companion App returns JSON; Flow updates Dataverse; terminal prints receipts.
    

## End‑to‑End Flow (Refund)

1. Agent selects a settled transaction → clicks **Refund**.
    
2. Flow fetches original reference, validates refundable state and amount (full or partial per POC config).
    
3. Flow **POST** → Companion App /refund with { originalReference, amount }.
    
4. Terminal processes refund; JSON response returned; Dataverse updates with refund status and linkage to original txn.
    

## End‑to‑End Flow (Void)

1. Agent selects a same‑day transaction → clicks **Void**.
    
2. Flow validates voidable state (same‑day/same batch per sandbox rules).
    
3. Flow **POST** → Companion App /void with { originalReference }.
    
4. Terminal processes void; JSON response returned; Dataverse marks original txn **Voided** and records the void reference.
    

## Risks & Mitigations

- **Processor sandbox rules** may differ for refund/void → align test cards and scenarios up front.
    
- **State validation** (void vs refund) → enforce with simple Flow checks and Dataverse fields.
    
- **Reference matching** → store and re‑use terminal transaction reference IDs reliably.
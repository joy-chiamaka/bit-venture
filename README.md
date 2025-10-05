# **BitVenture Protocol**

### *Decentralized Startup Funding on Bitcoin (via Stacks)*

---

## **Overview**

**BitVenture** is a **decentralized startup funding protocol** built on the **Stacks blockchain**, leveraging Bitcoin’s security to create a **trustless, milestone-driven venture funding system**. The protocol connects **startups** seeking capital with **investors** who gain **on-chain governance rights** through **tokenized equity**.

Unlike traditional venture models, BitVenture ensures capital release is **conditioned on milestone approval** by investors, reducing risk and enhancing transparency. It introduces a Bitcoin-native mechanism for **equity-backed fundraising**, **automated milestone verification**, and **decentralized capital management**.

---

## **System Overview**

The BitVenture ecosystem consists of three primary actors:

1. **Founders / Startups**

   * Create campaigns specifying funding goals, milestones, and timelines.
   * Receive funds progressively as investors approve milestone completions.

2. **Investors**

   * Contribute STX to campaigns.
   * Receive **equity tokens** proportional to investment size.
   * Vote on milestone completion to approve or deny fund releases.

3. **Protocol Operator (Platform Owner)**

   * Maintains platform-level configurations (e.g., fees, emergency controls).
   * Receives a **2.5% platform fee** on each investment.
   * Can pause the contract or intervene in case of emergency.

---

## **Contract Architecture**

### **1. Core Components**

| Component                       | Description                                                                   |
| ------------------------------- | ----------------------------------------------------------------------------- |
| **Campaign Management**         | Handles campaign creation, goal tracking, and completion logic.               |
| **Investment Flow**             | Manages STX transfers, equity distribution, and platform fees.                |
| **Milestone Governance**        | Supports on-chain milestone voting and fund release authorization.            |
| **Investor Portfolio Tracking** | Aggregates investment statistics for each investor.                           |
| **Administrative Controls**     | Enables protocol maintenance, platform fee management, and emergency actions. |

---

### **2. Data Structures**

| Data Map               | Purpose                                                                   |
| ---------------------- | ------------------------------------------------------------------------- |
| `campaigns`            | Stores core campaign metadata (founder, goal, deadline, milestones, etc.) |
| `campaign-investments` | Tracks investor contributions, timestamps, and equity tokens.             |
| `campaign-milestones`  | Defines milestone details and voting outcomes.                            |
| `milestone-votes`      | Records per-investor milestone votes and voting power.                    |
| `investor-portfolios`  | Aggregates user-level investment summaries.                               |
| `campaign-stats`       | Provides per-campaign metrics (investor count, average investment, etc.)  |

---

### **3. Key Variables**

| Variable                  | Description                                  |
| ------------------------- | -------------------------------------------- |
| `total-campaigns`         | Global campaign counter.                     |
| `platform-fee-percentage` | Default 2.5% (adjustable by owner, max 10%). |
| `total-platform-fees`     | Accumulates total platform fees collected.   |
| `paused`                  | Global pause flag for emergency stops.       |

---

### **4. Error Handling Constants**

| Error Code                    | Meaning                                |
| ----------------------------- | -------------------------------------- |
| `ERR-OWNER-ONLY`              | Function restricted to contract owner. |
| `ERR-NOT-AUTHORIZED`          | Caller lacks required permissions.     |
| `ERR-CAMPAIGN-NOT-FOUND`      | Campaign ID does not exist.            |
| `ERR-CAMPAIGN-ENDED`          | Campaign is inactive or closed.        |
| `ERR-INSUFFICIENT-FUNDS`      | Not enough STX for transaction.        |
| `ERR-MILESTONE-NOT-FOUND`     | Invalid milestone reference.           |
| `ERR-ALREADY-VOTED`           | Voter has already cast a vote.         |
| `ERR-VOTING-PERIOD-ENDED`     | Voting period has expired.             |
| `ERR-MILESTONE-NOT-COMPLETED` | Approval threshold not met.            |

---

## **Functional Overview**

### **1. Campaign Lifecycle**

1. **Creation**

   * Founder initializes campaign via `create-campaign`.
   * Defines title, description, funding goal, duration, and number of milestones.

2. **Investment Phase**

   * Investors fund campaign using `invest-in-campaign`.
   * Protocol deducts platform fee and transfers STX to founder.
   * Investor receives **equity tokens** proportional to investment size.

3. **Milestone Definition**

   * Founder defines milestones post-funding using `create-milestone`.
   * Each milestone includes a funding percentage, description, and voting duration.

4. **Voting Phase**

   * Investors vote via `vote-on-milestone` using their equity-weighted power.
   * If **>50% approval**, milestone is marked complete and funds are released.

5. **Closure**

   * Founder closes campaign via `close-campaign` once all goals are reached or deadline passes.

---

### **2. Equity and Governance**

* Equity tokens are **virtual** and represent voting weight in milestone decisions.
* Governance is **fully decentralized**, allowing investors to collectively determine milestone outcomes.
* Approval thresholds (>51%) enforce democratic control over fund disbursement.

---

### **3. Platform Fees**

* Default **2.5%** fee charged on each investment.
* Fees accumulate in `total-platform-fees` and can be withdrawn by the contract owner.
* Platform owner may update the fee up to a **10% maximum** via `set-platform-fee`.

---

### **4. Emergency Controls**

* `toggle-pause` — Suspends or resumes all contract operations.
* `emergency-close-campaign` — Forcefully deactivates a campaign.
* `force-milestone-completion` — Overrides milestone state in emergencies.

---

## **(Optional) System Data Flow**

```
[Investor]
   │
   ├─► invest-in-campaign()
   │     ├─► STX Transfer to Founder (minus fee)
   │     ├─► STX Fee → Platform Treasury
   │     └─► Update campaign-investments + investor-portfolios
   │
[Founder]
   │
   ├─► create-milestone()
   │     └─► Define milestone parameters
   │
[Investors Vote]
   │
   ├─► vote-on-milestone()
   │     └─► Weighted by equity tokens
   │
[Protocol]
   └─► complete-milestone()
         └─► Check approval >51%, release funds
```

---

## **Security & Governance Considerations**

* **Funds Custody:** STX are transferred directly between investors and founders, eliminating centralized custody.
* **Voting Power:** Calculated based on tokenized equity proportional to each investor’s funding share.
* **Emergency Pauses:** Ensures resilience during network instability or protocol vulnerability.
* **Immutable Transparency:** All milestones, votes, and fund flows are verifiable on-chain.

---

## **Future Extensions**

* NFT-based identity badges for verified founders/investors.
* DAO integration for platform governance.
* Off-chain oracle bridge for milestone validation using external proofs.
* Tokenized equity markets for secondary liquidity.

---

## **License**

MIT License © 2025 BitVenture Protocol
Developed for the **Stacks Bitcoin Layer**, empowering decentralized venture innovation.

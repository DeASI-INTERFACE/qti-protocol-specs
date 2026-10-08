# QTI Protocol — v4 Fully Robust Protocol Specification

**Version:** 4.0.0-draft  
**Classification:** Internal Technical Specification  
**Author:** De-ASI-INTERFACE Protocol Engineering  
**Date:** July 5, 2026  
**Status:** DRAFT — Pending v3 Finish Gate  
**Reference model:**
[qti-protocol-core](https://github.com/De-ASI-INTERFACE/qti-protocol-core)
(executable model, invariant and stress tests, per-gate status in
`docs/GATES.md`)

---

## Executive Summary

Version 4 is the terminal architecture of the QTI Protocol. It is no longer a
capital-allocation specialist. Version 4 is a fully robust, institutionally
credible on-chain protocol with native staking infrastructure, open strategy
marketplace, decentralized operator sets, formal treasury management,
progressive governance maturity, compliance-aware operating model, and long-term
financial sustainability as a self-governed protocol entity.

The transition is defined: versions 1 through 3 built and validated the system
under controlled operator authority. Version 4 expands the system's surface area
to external operators, third-party strategy adapters, validator-level
decentralization, and institutional compliance infrastructure — while
maintaining all safety properties proven in prior versions. The result is a
protocol that can survive key-person failure, regulatory scrutiny, institutional
due diligence, and multi-year operating continuity without founder dependency.

---

## 1. Objectives

### 1.1 In-Scope (v4)

- Open strategy marketplace: third-party adapter submission, curation, and
  activation via governance
- Decentralized operator set: multiple independent bots/operators with slashing
  and performance bonding
- Native validator-level staking infrastructure: direct Solana stake account
  management, delegation strategies
- Full treasury management module: diversified protocol reserves, buyback
  mechanisms, grant program
- Progressive governance maturity: protocol constitution, parameter governance,
  emergency council
- Compliance-aware operating model: KYC tiers, jurisdiction controls, regulatory
  monitoring hooks
- Long-term sustainability model: protocol-owned liquidity, fee sustainability
  analysis
- Formal upgrade framework: verified program upgrades, rollback procedures,
  upgrade governance
- Operator bonding and slashing: staked bond requirement for operator
  registration; slashable on misbehavior
- Public API and SDK: third-party integration layer for wallets, analytics
  platforms, institutional front-ends

### 1.2 Non-Objectives (v4 explicitly excludes)

- Becoming a centralized financial institution
- Removing community governance or reverting to admin-only control
- Compromising the 20% liquid buffer safety invariant
- Deploying capital to unaudited or unregistered strategy adapters

---

## 2. Architecture Overview

### 2.1 Full Protocol Program Suite

```text
programs/
  qti-vault-v4/                     # Upgraded vault (inherits v1-v3 logic)
  qti-governance/                   # Upgraded: parameter governance, protocol constitution
  qti-emissions-controller/         # Upgraded: epoch-based emission with governance
  qti-strategy-marketplace/         # NEW: adapter submission, review, activation
  qti-operator-registry/            # NEW: operator bonding, performance tracking, slashing
  qti-staking-infrastructure/       # NEW: native Solana stake account management
  qti-treasury/                     # NEW: protocol reserves, buyback, grant disbursement
  qti-compliance/                   # NEW: KYC tier management, jurisdiction flags
  qti-risk-dashboard/               # Upgraded: full public telemetry, historical snapshots
  qti-sdk/                          # TypeScript SDK for third-party integrations
```

### 2.2 Strategy Marketplace Architecture

| Stage           | Description                                                  | Governance Required  |
| --------------- | ------------------------------------------------------------ | -------------------- |
| Submission      | Third party submits adapter + documentation + audit evidence | No                   |
| Curation review | Protocol security committee reviews for minimum standards    | Committee vote       |
| Community vote  | QTI holders vote to activate adapter                         | Full governance vote |
| Activation      | Strategy enters registry with initial risk score             | Admin execution      |
| Monitoring      | Ongoing risk score updates; slashable if operator misbehaves | Automatic            |
| Deprecation     | Governance vote to remove; cooldown executed                 | Full governance vote |

### 2.3 Decentralized Operator Set

| Parameter                     | v4 Value                                                 |
| ----------------------------- | -------------------------------------------------------- |
| Minimum operators             | 3 independent parties                                    |
| Bond requirement per operator | 50,000 QTI (governance-adjustable)                       |
| Slash condition               | Unauthorized rebalance, policy violation, downtime > 48h |
| Slash amount                  | 10–100% of bond (graduated by severity)                  |
| Performance metric            | Risk-adjusted net yield vs. benchmark                    |
| Operator rotation             | Governance-controlled; minimum 30-day notice             |

### 2.4 Native Staking Infrastructure

The v4 staking module manages Solana stake accounts directly at the protocol
level:

- Protocol creates and manages stake accounts on behalf of vault TVL
- Validator selection policy: geographic diversity, commission cap, uptime SLA
- Delegation spread: no single validator > 15% of protocol stake
- Auto-restaking: rewards optionally compounded or distributed to vault NAV
- Unstaking queue management: protocol maintains rolling unstaking schedule to
  preserve liquidity

### 2.5 Treasury Management Module

| Reserve Type      | Target Allocation                      | Purpose                                         |
| ----------------- | -------------------------------------- | ----------------------------------------------- |
| Operating reserve | 6-month runway                         | Protocol maintenance, oracle costs, audit fees  |
| Insurance reserve | 5% of TVL                              | Emergency coverage for smart contract incidents |
| Liquidity reserve | Protocol-owned liquidity in key venues | Reduce slippage on reward token exits           |
| Grant reserve     | Governance-controlled                  | Ecosystem development, security research        |
| Buyback reserve   | Governance-controlled                  | QTI token support mechanism                     |

---

## 3. Protocol Constitution

Version 4 formalizes a protocol constitution as an on-chain governance parameter
set that constrains all future governance:

### 3.1 Immutable Protocol Rules

These parameters require a constitutional amendment process (80% supermajority +
6-month time-lock):

- Minimum liquid buffer: 20% TVL — cannot be reduced
- Maximum single-strategy allocation: 50% TVL — cannot be increased above this
  floor
- Operator bond requirement: cannot be reduced below 10,000 QTI
- Emergency pause: guardian veto power cannot be revoked by governance alone

### 3.2 Governance-Adjustable Parameters

Standard governance vote (60% approval, 48h time-lock):

- Management fee rate
- Performance fee rate
- Emission schedule
- Strategy allocation caps
- Operator bond amount (above constitutional floor)
- Voting quorum and approval threshold (non-constitutional)

---

## 4. Compliance-Aware Operating Model

### 4.1 KYC Tier Architecture (v4 expanded)

| Tier          | Access Level                                  | Verification        |
| ------------- | --------------------------------------------- | ------------------- |
| Unverified    | Read-only dashboard; no deposits              | None                |
| Basic         | Public vault; capped deposit (governance-set) | Email + wallet      |
| Institutional | All vaults; no cap; governance voting         | Full KYC/AML        |
| Operator      | Strategy execution; requires bonding          | Full KYC/AML + bond |

### 4.2 Jurisdiction Controls

- Jurisdiction flag per wallet enforced by compliance module
- Restricted jurisdiction list maintained by Protocol Council; updated by
  governance
- On-chain instruction rejects deposits from flagged jurisdiction if compliance
  flag set

### 4.3 Regulatory Monitoring Hooks

- Monthly NAV and fee reports formatted to institutional fund reporting
  standards
- Governance activity log exportable to compliance review format
- Audit trail: all operator actions logged with signed payload hashes and
  on-chain event anchors

---

## 5. Upgrade Framework

### 5.1 Program Upgrade Process

1. Engineering proposes upgrade: publishes new program binary hash + change log
2. Security review: minimum 7-day review period; third-party audit for critical
   changes
3. Governance vote: standard 60% approval, 48h time-lock
4. Staged deployment: devnet → testnet → mainnet with monitoring between stages
5. Rollback plan: previous verified binary retained; rollback executable by
   guardian within 24h post-deploy
6. Verification: `solana-verify` confirms deployed binary matches approved
   source

### 5.2 Emergency Upgrade

- Guardian can freeze program (not upgrade) within seconds
- Emergency upgrade requires Admin multisig 3-of-5 sign-off + 24h minimum delay
- Post-emergency review mandatory within 72h: publish incident report and root
  cause

---

## 6. Testing Matrix (v4)

### 6.1 System-Wide Invariant Tests

| Invariant                                       | Test Method                                          |
| ----------------------------------------------- | ---------------------------------------------------- |
| Liquid buffer ≥ 20% TVL under all conditions    | Fuzz test 100,000 rebalance scenarios                |
| No single strategy > 50% TVL                    | Invariant enforced in multi_rebalance instruction    |
| Total shares × NAV = total_assets               | Property-based test across all instruction sequences |
| Operator bond intact unless slash condition met | Simulate all slash conditions programmatically       |
| Governance cannot execute below time-lock       | Time manipulation attack test                        |

### 6.2 Stress Tests

| Scenario                                 | Target                                | Pass Criteria                                    |
| ---------------------------------------- | ------------------------------------- | ------------------------------------------------ |
| 50% TVL withdrawal in single epoch       | Liquidity buffer + unstaking queue    | No withdrawal fails; queue processed correctly   |
| All operators go offline simultaneously  | Vault continues; no funds at risk     | Allocation frozen; deposits/withdrawals continue |
| Strategy adapter exploit simulation      | Blast radius capped at allocation cap | Loss ≤ allocation cap × TVL                      |
| Governance attack: malicious proposal    | Guardian vetoes; time-lock blocks     | No unauthorized execution                        |
| Validator slashing event on native stake | Insurance reserve covers NAV impact   | NAV floor maintained                             |

### 6.3 Long-Run Simulation

- 365-day simulation across multiple APR regimes (bull, bear, flat)
- Measure: annualized net yield, max drawdown, Sharpe ratio vs. benchmark
- Operator performance scoring validated against simulation results
- Treasury sustainability model: confirm operating reserve survives 12-month fee
  drought

### 6.4 Third-Party Audit Requirements (v4)

- Full-scope audit by Tier-1 firm (OtterSec, Neodyme, or Zellic): all programs
- Economic security review: invariant model, tokenomics, emission sustainability
- Governance attack simulation: paid adversarial review
- Compliance architecture review: KYC tier logic and jurisdiction controls
- All critical and high severity findings closed before mainnet; medium severity
  accepted with documented risk acceptance

---

## 7. Public API and SDK

### 7.1 TypeScript SDK (qti-sdk)

```typescript
// Core SDK surface
QTIVault.deposit(amount, userKeypair, rpcUrl)
QTIVault.withdraw(shares, userKeypair, rpcUrl)
QTIVault.getNavPerShare(rpcUrl) => BN
QTIVault.getUserPosition(wallet, rpcUrl) => UserPosition
QTIStrategyRegistry.listStrategies(rpcUrl) => Strategy[]
QTIGovernance.listProposals(rpcUrl) => Proposal[]
QTIGovernance.castVote(proposalId, vote, keypair, rpcUrl)
QTIRiskDashboard.getSnapshot(rpcUrl) => ProtocolSnapshot
```

### 7.2 Public Telemetry Endpoints (Off-Chain Relay)

- `/v4/snapshot` — Latest protocol snapshot
- `/v4/strategies` — Registry with risk scores
- `/v4/governance/proposals` — Active and historical proposals
- `/v4/operators` — Operator performance stats
- `/v4/treasury` — Reserve balances and allocation
- `/v4/nav` — Historical NAV per share series

---

## 8. Validation Gates (v4)

| Gate   | Criteria                                                              | Owner             |
| ------ | --------------------------------------------------------------------- | ----------------- |
| V4-G1  | v3 fully finished and all governance live                             | Engineering       |
| V4-G2  | Strategy marketplace unit and integration tests pass                  | Engineering       |
| V4-G3  | Operator bonding and slashing tests pass                              | Engineering       |
| V4-G4  | Native staking infrastructure devnet validated                        | Engineering       |
| V4-G5  | Treasury module integration tests pass                                | Engineering       |
| V4-G6  | System-wide invariant fuzz tests: 100,000 scenarios pass              | Engineering       |
| V4-G7  | Stress tests pass: all 5 scenarios                                    | Engineering       |
| V4-G8  | 365-day simulation within accepted parameters                         | Quant             |
| V4-G9  | Full Tier-1 audit: all critical/high findings closed                  | External Auditor  |
| V4-G10 | Economic security review complete                                     | External Reviewer |
| V4-G11 | Governance attack simulation: no exploits                             | Security          |
| V4-G12 | Compliance architecture review signed off                             | Legal/Compliance  |
| V4-G13 | SDK published and integration-tested by 2 external parties            | Community         |
| V4-G14 | Protocol constitution ratified by governance vote                     | Protocol Council  |
| V4-G15 | Post-launch monitoring stack operational (Grafana, alerting, on-call) | Ops               |

---

## 9. Completion Criteria

Version 4 is **FINISHED** (protocol declared fully robust) when:

1. All validation gates V4-G1 through V4-G15 signed off
2. At least 3 independent operators actively running on mainnet
3. Strategy marketplace has at least 5 governance-activated adapters
4. Native staking infrastructure managing at least 10% of protocol TVL
5. Protocol constitution ratified and on-chain
6. Treasury operating reserve funded to 6-month target
7. Full-scope external audit published with zero open critical/high findings
8. Protocol operates without founder intervention for minimum 30 consecutive
   days
9. Monthly NAV and governance reports published on-chain and publicly accessible

---

## 10. Comparative Version Roadmap

| Dimension        | v1                     | v2                        | v3                        | v4                                 |
| ---------------- | ---------------------- | ------------------------- | ------------------------- | ---------------------------------- |
| Vault type       | Single, fixed strategy | Multi-strategy, adaptive  | Governed, composable      | Open marketplace                   |
| Operators        | Admin + bot            | Admin + bot (shadow mode) | Squads multisig set       | Decentralized bonded operators     |
| Governance       | None                   | None                      | On-chain QTI governance   | Constitutional protocol governance |
| Staking          | One adapter, capped    | Multi-adapter, scored     | Liquid staking integrated | Native Solana stake accounts       |
| Tokenomics       | None                   | None                      | QTI voting + fee split    | Full emissions + buyback + grants  |
| Compliance       | Whitelist only         | Whitelist + scoring       | KYC tiers                 | Full compliance module             |
| Audit scope      | Single vault           | Vault + scoring engine    | Full protocol             | Full protocol + economic review    |
| Decentralization | Centralized            | Centralized               | Semi-decentralized        | Progressively decentralized        |
| Treasury         | Fee accrual only       | Fee + haircut             | Governance treasury       | Full reserves + POL                |
| External use     | None                   | None                      | SDK (internal)            | Public SDK + API                   |

---

## 11. Risk Register (v4)

| Risk                                               | Severity | Probability | Mitigation                                                |
| -------------------------------------------------- | -------- | ----------- | --------------------------------------------------------- |
| Third-party adapter exploit                        | Critical | Medium      | Marketplace audit gate; allocation cap; insurance reserve |
| Governance takeover via token accumulation         | Critical | Low         | Constitutional supermajority + time-lock + guardian       |
| Operator cartel / collusion                        | High     | Low         | Slashing; performance bonding; governance rotation        |
| Regulatory action in key jurisdiction              | High     | Medium      | KYC tiers; jurisdiction controls; legal workstream        |
| Smart contract upgrade introduces regression       | High     | Medium      | Staged deploy; rollback plan; time-lock                   |
| Treasury reserve depleted in prolonged bear market | High     | Low         | 6-month operating reserve target; grant controls          |
| Native validator slashing cascade                  | Medium   | Low         | 15% validator cap; insurance reserve                      |
| SDK adoption failure (no external integrations)    | Medium   | Medium      | Developer grant program; SDK documentation                |

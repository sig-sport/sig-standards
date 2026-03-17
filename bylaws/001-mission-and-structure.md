# SIG — Sports Interoperability Group
## Mission, Structure and Governance
**Status:** Draft v0.1 | **Date:** March 2026

---

## Mission Statement

The Sports Interoperability Group (SIG) exists to advance data interoperability across the global sports ecosystem. It brings together federations, platform vendors, regional sports bodies, and technology practitioners to identify shared challenges, develop common solutions, and publish open standards that any organisation can adopt.

SIG produces Sports Standard Recommendations (SSRs) — open, implementation-agnostic specifications informed by real deployment experience. SSRs define the contracts that allow sports systems to exchange data reliably, without requiring organisations to surrender control of their data or lock themselves to a single vendor.

SIG does not tell organisations how to build their systems. It defines the surface where systems must agree in order to work together.

---

## Vision

A sports ecosystem where an athlete's identity, membership record, and competition history follow them across systems, borders, and sports — held by the organisations that are accountable for them, accessible to those who need them, and never captive to a single vendor.

---

## Principles

**Minimal by design.** SSRs specify only what systems must agree on. Everything else is an extension or a local decision.

**Adoption over mandate.** Value comes from real implementations, not enforcement. SIG has no regulatory authority and does not require any organisation to adopt any SSR.

**Vendor neutral.** No single vendor, federation, or chapter may control the direction of a specification unilaterally. Voting rights are held by member organisations, not by commercial interests.

**Stable contracts.** Once an SSR is accepted it does not break existing implementations. Evolution is handled through a defined deprecation path and a new SSR number.

**Open by default.** All specifications, discussions, and votes are conducted in public. Membership is by participation and contribution, not by paying for a seat.

**Data sovereignty.** SSRs are designed to reinforce — not undermine — the data ownership rights of federations, clubs, and athletes. SIG does not store, process, or act as custodian of any sports data.

---

## Structure

### Chapters

SIG operates through regional chapters, each responsible for driving adoption and contributing specifications within their geography. Chapters share the SIG name, governance model, and specification output. All accepted SSRs are published globally under the SIG identity, not the originating chapter.

| Chapter | Scope | Status |
|---|---|---|
| SIG-DE | Germany | Founding chapter |
| SIG-EU | Pan-European | Target state |
| SIG-APAC | Asia-Pacific | Future |

A chapter is formed when at least three independent member organisations within a region commit to active participation and nominate a chapter coordinator. New chapters are ratified by the SIG Steering Council. Chapter coordinators are non-voting members of the Steering Council, present in an advisory capacity when their chapter's work is under discussion.

---

### Member Organisations

SIG membership is open to any organisation with a material stake in sports data interoperability: national federations, regional sports confederations, Landessportbünde and equivalent regional bodies, platform vendors, and standards-adjacent bodies such as national Olympic committees.

Members are organisations, not individuals. Each member organisation nominates a single **Representative** — an individual authorised to vote and contribute on the organisation's behalf. Representatives may be replaced by the nominating organisation at any time without requiring SIG approval. No individual may represent more than one member organisation simultaneously.

#### Membership eligibility

An organisation applying for membership must:

- Demonstrate a material, active stake in sports data — through deployment, procurement, or direct federation responsibility
- Be nominated by an existing member organisation or Steering Council member
- Allow a public discussion period of at least two weeks before a membership vote is called
- Receive a simple majority vote of the Steering Council to be admitted

A former member organisation may reapply for membership at any time.

#### Active participation requirement

Membership requires active participation. An organisation that has not voted, contributed to a working group, or attended a chapter session within any rolling 12-month period may be placed on **inactive status** by the Secretary. Inactive members retain observer rights but lose voting rights until participation resumes. The Secretary must notify the organisation in writing before changing its status.

---

### Roles

#### Steering Council

The Steering Council is a seven-member board elected by and from the pool of active Representatives. It is the primary decision-making body of SIG, responsible for:

- Approving SSRs and SERs at final acceptance stage
- Ratifying new chapters
- Appointing Editors and Maintainers
- Resolving governance disputes
- Amending these bylaws (requires two-thirds majority)

Council members serve two-year terms, with elections staggered so that no more than four seats are contested in any single election cycle. No single chapter may hold more than three Council seats at any time. A Council member may simultaneously serve as a Representative, but may not serve as Secretary or as Editor of any active working group.

Quorum for Steering Council decisions is five members. Decisions require a simple majority unless otherwise specified.

#### Representative

Each member organisation is represented by a single Representative. Representatives:

- Hold the organisation's voting rights in all SIG votes
- Are eligible to participate in and lead working groups
- Act as the primary point of contact between SIG and the member organisation

A Representative may temporarily delegate their voting rights to another individual authorised by their organisation, for a defined period, by notifying the Secretary in writing. All rights revert automatically at the end of the stated period.

#### Editor

An Editor leads a working group and is responsible for the quality and progression of a specific SSR or SER. Editors are appointed by the Steering Council. An individual may edit more than one working group simultaneously. An Editor may not simultaneously serve as Secretary.

The Editor has final authority over the working group's output and process, subject to Steering Council review. If an Editor is absent for more than 45 days without notice, the Steering Council may appoint a replacement.

#### Secretary

The Secretary is the impartial administrator of SIG. There may be between one and three Secretaries serving concurrently to ensure continuity. Secretaries are elected by the Steering Council.

Secretaries may not concurrently serve as a Representative, Council member, or Editor. They must publicly declare all conflicts of interest. Secretaries act as the public face of SIG on procedural and administrative matters.

**Defined responsibilities:**

- Maintaining the SIG website and public repository
- Managing the voting system and tallying votes — a vote initiated by one Secretary must be tallied by a different Secretary
- Tracking member organisation activity and maintaining the membership register
- Moderating official SIG communication channels
- Ensuring bylaws are followed and clarifying bylaw text where unambiguous (contested clarifications go to a full vote)
- Publishing chapter records and working group status updates

---

### Working Groups

All specification work is carried out by working groups. A working group is formed by an **Entrance Vote** of the Steering Council, which simultaneously appoints the Editor and defines the working group's scope.

Every working group requires:

- A single Editor
- At least two additional active participants
- Optionally, a Steering Council sponsor (required for cross-chapter or high-complexity specifications)

Working groups are open. Any individual may apply to join, subject to the Editor's judgment that their experience would advance the group's work. A Representative may join any working group without requiring Editor approval, unless the Editor objects on grounds of disruption or irrelevance.

A working group is dissolved when its output reaches Accepted or Deprecated state, or by Steering Council decision if the group has shown no activity for six consecutive months.

---

## Output Artefacts

SIG produces three categories of output:

### Sports Standard Recommendation (SSR)

An SSR is a frozen interoperability specification — a contract between data providers and consumers. Once accepted, an SSR does not change. Additions or breaking changes require a new SSR number. Deprecations are permitted and must include a migration reference.

SSRs are developed by a working group and accepted by a two-thirds majority vote of the Steering Council.

### Sports Evolving Recommendation (SER)

An SER is a living best-practice specification that may be updated as the ecosystem evolves. SERs follow semantic versioning:

- **Patch releases** — the Maintainer may publish at any time
- **Minor releases** — require Steering Council notification and implicit approval
- **Major releases** — require an explicit Steering Council approval vote

SERs are used for guidelines, tooling standards, implementation patterns, and domain profiles that are expected to change as regulation, technology, or practice evolves.

### Reference Implementation (RI)

An RI is an open-source implementation of an SSR or SER, published to demonstrate conformance and lower the cost of adoption. RIs are not normative — the SSR or SER text is the authority. Each RI is maintained by a designated Maintainer appointed by the Steering Council.

---

## SSR Lifecycle

```
Idea → Draft → Review → Accepted
                      ↘ Deprecated
```

| State | Description |
|---|---|
| **Idea** | Informal proposal, not yet a specification. Anyone may raise an Idea on the public repository. |
| **Draft** | Specification text exists and is open for public comment and trial implementations. A working group has been formed. |
| **Review** | At least two independent confirmed implementations exist. The specification is stable; only editorial changes are permitted. |
| **Accepted** | Approved by two-thirds Steering Council majority. Frozen. |
| **Deprecated** | Superseded by a newer SSR. Remains published with a migration reference. |

---

## Voting Protocol

SIG uses two classes of vote:

**Representative vote** — all active Representatives participate. Used for Steering Council elections and membership decisions.

**Steering Council vote** — Council members only. Used for SSR acceptance, chapter ratification, Editor appointments, and bylaw amendments.

All votes are conducted publicly, with a minimum discussion period of two weeks before a vote is called. Results are published to the SIG public repository within 48 hours of closing.

Abstentions do not count toward quorum or toward the majority calculation.

---

## What SIG Does Not Do

- SIG does not regulate software vendors or mandate adoption of any SSR.
- SIG does not operate as a certification or accreditation body.
- SIG does not store, process, or act as custodian of any sports data.
- SIG does not represent its members in commercial negotiations or disputes.
- SIG does not endorse specific products, vendors, or implementations.

---

## Licensing

All SIG specifications, reference implementations, and governance documents are published under **CC0 1.0 Universal** — no rights reserved. Any organisation may adopt, implement, fork, or build upon SIG artefacts without restriction and without requiring attribution, though attribution is appreciated.

---

## Amendment Process

These bylaws may be amended by a two-thirds majority vote of the Steering Council, following a minimum public comment period of four weeks. Proposed amendments must be published to the SIG public repository before the comment period begins.

---

## Changelog

| Version | Date | Status | Notes |
|---|---|---|---|
| 0.1 | March 2026 | Draft | Initial publication. Authored by SIG-DE founding group. |
